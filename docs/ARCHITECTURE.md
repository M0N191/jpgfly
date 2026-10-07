# JPGFLY Architecture

The current public runtime creates artwork one room at a time. A server-side Candidate Fly Brain chooses actions, server mechanics execute movement and strokes, and the browser renders the resulting state. Completed rooms feed persistent experience and a separate learned art policy.

Habitat is the broader research direction: a persistent 2D artificial-life world inhabited by autonomous flies. This branch implements the room studio, not a shared persistent-world engine. Mouse Vision / MICrONS is also a research direction; the implemented visual advisory path uses Qwen. See the [project concept](JPGFLY_PROJECT.md), [capability statuses](FEATURES.md), and [local setup](LOCAL-RUN.md).

## Room lifecycle

[app.py](../app.py) owns session creation, decision sequencing, finalization, storage, and the autonomous studio. With `JPGFLY_AUTONOMOUS_STUDIO=true`, the studio runs one current room, finalizes it, pauses, and creates the next. It rotates JPGFLY, SPRAYFLY, and DREAMFLY according to the archive count.

Each session has its own seeded brain, canvas mechanics, decision history, and emergency painter. [Artist profiles](../agent_profiles.py) supply different visual vocabularies and written voices while sharing room history and experience. The API can hold multiple sessions, but the autonomous studio does not schedule simultaneous inhabitants of a shared world.

```text
shared experience + artist profile
               ↓
new room → authoritative canvas observation
               ↓
Candidate Fly Brain ← advisory composition + critic + learned policy
               ↓
selected action ← optional MaleCNS bias
               ↓
server mechanics → movement/stroke events → live browser
               ↓
observe again → continue / revise / finish
               ↓
final SVG + room writing
               ↓
eligible experience update + art-policy training
               ↓
compact Backrooms storage
```

## Art decisions

[create_fly_brain](../candidate_brain.py) always creates `CandidateFlyBrain`. It extends the procedural painter in [brain_provider.py](../brain_provider.py); Qwen and FLM remain support systems around this decision engine.

For an ordinary decision, the brain clones its current state into **eight candidate continuations** with different seeded futures. It applies the artist's vocabulary and scores proposals using canvas structure, composition passes, novelty, repetition, recent room history, conceptual pressures, optional Qwen guidance, ZebraCNS critic pressure, and the learned policy. Selection is probabilistic, weighted by score. Only the selected branch's state is committed.

Actions describe movement direction/distance, curvature, brush contact, pressure, color, scale, material, technique, revision, and artistic intent. Finish actions compete with painting actions under readiness/quality rules. A resolve window can add a ninth candidate explicitly proposing finish; a hard-limit finish path has one candidate. “Eight candidates” describes the normal exploration step, not every decision.

## Movement, strokes, and browser

[server_mechanics.py](../server_mechanics.py) maintains position and canvas observations on an 800 × 500 surface. It converts a structured action into authoritative paths and movement, contact, pause, and stroke events. [app.py](../app.py) validates the mechanical trace and produces the final SVG from server events during finalization.

The live browser polls the filtered, cursor-based event stream in [studio_delta.py](../studio_delta.py). [web/live.js](../web/live.js) renders strokes, animates the fly performer, and displays commentary and telemetry. Browser rendering and reconstruction helpers exist, but the live creative loop takes its actions and geometry from the server.

## Support systems

These integrations are optional. Their clients and adapters are implemented; that does not establish that a model or neural service is running.

| System | Role and implementation boundary |
|---|---|
| **Qwen / Ollama** | [composition_vision.py](../composition_vision.py) rasterizes authoritative paths into a preview and requests structured composition advice. Guidance influences future candidate scores and targets; it does not prescribe exact paths or validate finish quality. [narrative_provider.py](../narrative_provider.py) also supports broader context and writing. Requires Ollama and configured models. |
| **FLM** | [flm_text_provider.py](../flm_text_provider.py) and [flm_bridge.py](../flm_bridge.py) provide written voice, live commentary, and finished-room writing. The external FLM implementation and a completed trained adapter are required; they are not bundled. Hybrid writing can use a Qwen draft followed by FLM. |
| **MaleCNS** | The client in [brain_provider.py](../brain_provider.py) requests stimulation from an external fly-connectome-inspired service. The Candidate Brain applies bounded changes to a selected action's direction, pressure, exploration, scale, and hesitation. [app.py](../app.py) exposes selected telemetry. This repository does not contain the MaleCNS simulator or dataset. |
| **ZebraCNS / ZAPBench** | [zebracns_local.py](../zebracns_local.py) samples and caches recorded zebrafish activity traces. Its behavioral/game interpretation is engineered, not a biological motor readout. [zebracns.py](../zebracns.py) supplies normalized state to the Candidate Brain only when biological data is loaded. Direct critic-score pressure is capped at ±0.42; it can affect ranking and finish timing. The service requires its data and optional TensorStore dependency. |

ZebraCNS stays dormant when its trace data cannot load; a visible UI backbone does not count as loaded biological activity. Neural and decision overlays are explanatory displays, not hidden-thought readouts or evidence of biological consciousness.

## Two learning systems

**Experience Memory** in [experience_memory.py](../experience_memory.py) stores room-derived visual tendencies and conceptual context in `experience.json`. It ingests local `.txt`/`.md` readings, supplies bounded palette/technique/composition pressures, and carries recent-room echoes and anti-repetition context across artist profiles. This is accumulated context and preferences; it does not train model weights.

**Learned Art Policy** in [art_policy.py](../art_policy.py) is a separate NumPy network, currently `96 → 128 → 64 → 5`. Canvas/action features, recent transitions, temperament, and available Zebra signals produce value, tension, restraint, editing, and focus predictions used within candidate scoring. Completed rooms classified as normal train this policy from recorded decisions and changes in canvas metrics. Its weights persist in `art-policy-v2.json` by default, or `JPGFLY_ART_POLICY_PATH`. This updates the small art-policy network, not Qwen or FLM.

## Persistence and Backrooms

The default data root is `.jpgfly/`; `JPGFLY_DATA_DIR` can select another location. Completed artwork is stored under `artworks/` as a compressed final SVG and compact JSON record containing writing, artist identity, structural metrics, visual context, and integrity hashes. The searchable Backrooms archive and individual artwork pages read those records. “Old Rooms” refers to earlier works, not a separate runtime subsystem.

Archive loading currently filters records by accepted archive identifiers. Completed artwork, Experience Memory, and learned policy persist. Active session positions, live brain state, and heavy decision/event histories are in memory and are not restored as living agents after restart. Compact artwork records are not full replay recordings.

## Failure behavior

Support availability and art-brain failure have different meanings:

- **Support-node outage:** Qwen/FLM availability checks can temporarily label the live session `DUMB DUMB`, while the Candidate Fly Brain continues painting. Failed composition advice returns no new guidance; writing has local fallbacks.
- **Art-brain exception:** the session switches to its independent pure-Python emergency painter. These actual emergency decisions count toward the final room classification.

Finalization computes `fallback_ratio = emergency decisions / total art decisions`. A ratio **≤ 0.35** produces a normal room; **> 0.35** produces a `DUMB DUMB` room. Exactly 35% emergency decisions is still normal. This session-wide classification also determines eligibility for Experience Memory updates and learned art-policy training; a temporary support label alone does not determine archive quality. Learning happens before compact archive storage.

For service access, storage, and request controls, see [SECURITY.md](../SECURITY.md). For earlier release behavior, see [history](HISTORY.md) and the [historical 6.4 notes](RELEASE-6.4.md).
