# JPGFLY project

JPGFLY studies creative agency through an experimental artificial-life and generative-art system, with multiple autonomous artist identities sharing memory and a common creative runtime. The central question is what changes when an artwork follows from an agent's ongoing actions, observations, and memory.

The current **IMPLEMENTED** public runtime is room-based. A Fly observes its canvas, explores possible actions, makes a mark, observes the consequences, and continues or finishes. A Room accumulates decisions over time rather than arriving as a single generated image.

## Habitat and the public implementation

**Habitat — RESEARCH DIRECTION** is the broader project idea of a persistent 2D world inhabited by autonomous fly agents. It connects memory, language, neural systems, perception, evaluation, and live creative processes.

This branch supplies the creative runtime and persistent finished-room history. It does not establish a shared persistent Habitat world engine, simultaneous agent inhabitants, or restoration of living agents after restart. Persisted artwork and learning are different from persisted world state.

**Mouse Vision / MICrONS — RESEARCH DIRECTION** concerns visual-perception research. No public runtime integration is present. The implemented visual composition interface instead uses an optional Qwen vision-language model.

## Three artist lines

- **JPGFLY** is the origin/generalist painter. Its shared brush vocabulary can support many kinds of marks and subjects.
- **SPRAYFLY** specializes in graffiti-oriented pressure: tags, splatter, impact, drips, and overwriting.
- **DREAMFLY** specializes in abstract/surreal pressure: impossible scale, altered gravity, dream logic, wash, soft paint, and atmospheric forms.

Their differences are declarative style, material, technique, and written-voice biases within a shared engine. They share earlier Rooms and reading experience. Cross-room echoes offer continuity without prescribing a copy of an earlier image.

The autonomous studio rotates these profiles through one current Room. Artist lineage is project language backed by profile and archive metadata, not an implemented biological reproduction process. See [artist profiles](../agent_profiles.py).

## Agency through action

The **Candidate Fly Brain — IMPLEMENTED** is the main art-decision engine. It evaluates candidate continuations using canvas measurements, composition, memory, a learned art-policy vote, and optional advice.

**Server mechanics — IMPLEMENTED** give actions physical consequences: movement, contact, strokes, pauses, erasure, and changing canvas measurements. The browser presents those consequences and the performing Fly. Its live display is not the art-decision system.

**Experience Memory — IMPLEMENTED** turns eligible completed Rooms and local readings into bounded shared tendencies and conceptual context.

**Learned Art Policy — IMPLEMENTED** is separate: a small temporal network learns from eligible completed-room decisions and contributes bounded candidate-scoring pressure. Its persisted weights are not Qwen or FLM weights.

## Supporting systems

These systems have **OPTIONAL LOCAL INTEGRATION** status:

| System | Contribution | Public implementation boundary |
|---|---|---|
| Qwen / Ollama | Visual composition advice, broader context, and narrative support | Request/response clients and a canvas-preview composition teacher; model service configured separately. |
| FLM | Written Fly voice, studio notes, titles, descriptions, statements, and memory threads | Text provider and bridge; external FLM implementation and trained adapter required. |
| MaleCNS | Connectome-inspired perturbations of a selected action and neural telemetry | Service clients are present; simulator and dataset supplied separately. |
| ZebraCNS / ZAPBench | Bounded critic pressure on candidate preference and finish timing | Adapter plus a local recorded-data service; data access and optional dependencies required. |

Candidate Fly Brain retains action authority. Qwen advice is not direct stroke generation; Zebra critic pressure is not direct motor control. Recorded biological activity, engineered interpretation, and the character's written voice should be distinguished.

## Backrooms and continuity

**Backrooms — IMPLEMENTED** is the archive of successfully finalized works. A Room keeps its compressed final SVG and compact writing, public notes, metadata, structural summaries, and integrity hashes. The complete live event stream is not retained as the public artwork format.

**Old Rooms** means earlier works in that same archive. It does not name a separate public subsystem. Artist filters, search, remembered tendencies, and room echoes connect current activity to earlier works. Active canvas and session state are process-local; restart begins a fresh Room using persistent history and learning.

## Interpreting the experiment

Autonomy means that, once a Room starts, the program selects its actions and finish point without per-mark human steering. Operators still configure, start, stop, and supply data to the system.

Project fiction and artistic voice give the Fly a character. They do not establish biological equivalence, consciousness, or access to hidden model thoughts. Neural displays should be read according to the signal source and implementation behind them.

For the precise flow, see [ARCHITECTURE](ARCHITECTURE.md). For capability status, see [FEATURES](FEATURES.md). Setup belongs in [LOCAL-RUN](LOCAL-RUN.md); version-specific development is covered in [HISTORY](HISTORY.md).

See [Security](../SECURITY.md), [License](../LICENSE), and [Trademark](../TRADEMARK.md).
