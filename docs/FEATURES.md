# JPGFLY Capabilities

This inventory describes the current public source. **IMPLEMENTED** means code is present in the room runtime. **OPTIONAL LOCAL INTEGRATION** means an implemented bridge requires additional models, services, or data. **RESEARCH DIRECTION** identifies the broader project concept without claiming a public runtime implementation. **HISTORICAL** identifies version-specific documentation.

## Creative runtime

| Capability | Status | Scope and source |
|---|---|---|
| Autonomous room studio | IMPLEMENTED | One current room at a time, with artist rotation and successive completed works. [app.py](../app.py) |
| JPGFLY | IMPLEMENTED | Origin/generalist painter using shared movement, brush, and technique pools. [Profiles](../agent_profiles.py) |
| SPRAYFLY | IMPLEMENTED | Spray/graffiti identity expressed through tags, splatter, marker bleed, dry brush, charcoal, neon, and overwrite techniques. [Profiles](../agent_profiles.py) |
| DREAMFLY | IMPLEMENTED | Surreal/abstract identity with wash, soft paint, fog, ribbon, and neon biases. [Profiles](../agent_profiles.py) |
| Candidate Fly Brain | IMPLEMENTED | Main art-decision engine: eight ordinary continuations, weighted selection, revision and finish rules. Finish paths can have a different candidate count. [candidate_brain.py](../candidate_brain.py) |
| Composition and materials | IMPLEMENTED | Spatial regions, passes, focal structure, negative space, subjects, palettes, brush/material effects, erasure, and overpainting. [Composition](../composition_engine.py), [materials](../art_materials.py), [subjects](../subject_catalog.py) |
| Authoritative movement/strokes | IMPLEMENTED | Server executes physical actions, observes canvas changes, and produces the final SVG. [server_mechanics.py](../server_mechanics.py), [app.py](../app.py) |
| Live browser and fly performer | IMPLEMENTED | Renders server events, animates the artist, and displays commentary and decision/neural telemetry. [web/live.js](../web/live.js), [studio_delta.py](../studio_delta.py) |
| Backrooms / Old Rooms | IMPLEMENTED | Searchable completed-work archive and artwork pages; compressed SVG, compact writing, metrics, and hashes. Old Rooms is a name for earlier works. [app.py](../app.py) |
| Resilience | IMPLEMENTED | Local writing fallback and an independent emergency painter; final `DUMB DUMB` classification requires more than 35% actual emergency art decisions. [Architecture](ARCHITECTURE.md) |

## Memory and intelligence support

| Capability | Status | Scope and source |
|---|---|---|
| Experience Memory | IMPLEMENTED | Persistent room tendencies, reading context, shared cross-profile echoes, and recent-room anti-repetition. It accumulates context rather than training weights. [experience_memory.py](../experience_memory.py) |
| Learned Art Policy | IMPLEMENTED | Separate `96 → 128 → 64 → 5` network trained from normal-classified completed rooms; persists weights and contributes to candidate scoring. [art_policy.py](../art_policy.py) |
| Procedural room writing | IMPLEMENTED | Local writing for titles, descriptions, statements, anomalies, and memory threads without a model service. [flm_text_provider.py](../flm_text_provider.py) |
| Qwen / Ollama | OPTIONAL LOCAL INTEGRATION | Advisory composition, reasoning/context, and narrative support. Structured visual guidance affects future candidates; server mechanics retain stroke authority. Requires configured models. [Vision](../composition_vision.py), [narrative](../narrative_provider.py) |
| FLM | OPTIONAL LOCAL INTEGRATION | Primarily written voice, commentary, and room-writing; hybrid mode can use Qwen context. Requires an external FLM implementation and trained adapter. [Provider](../flm_text_provider.py), [bridge](../flm_bridge.py) |
| MaleCNS | OPTIONAL LOCAL INTEGRATION | Fly-connectome-inspired action bias and selected telemetry. Client is present; simulator and dataset require an external service. [brain_provider.py](../brain_provider.py), [app.py](../app.py) |
| ZebraCNS / ZAPBench | OPTIONAL LOCAL INTEGRATION | Recorded-activity loader/service and bounded critic/evaluation pressure on candidates and finish timing. Requires trace data and optional TensorStore; dormant without loaded data. [Adapter](../zebracns.py), [service](../zebracns_local.py) |

Neural recordings, engineered critic signals, and browser visualizations have distinct roles. Displayed activity does not establish biological consciousness or expose hidden model thoughts. See [architecture](ARCHITECTURE.md) for the exact decision flow and failure distinctions.

## Project directions and history

| Area | Status | Scope |
|---|---|---|
| Habitat | RESEARCH DIRECTION | Persistent shared 2D artificial-life world for autonomous agents. This branch provides the room-based creative runtime; it does not restore a shared world or living agents after restart. [Project](JPGFLY_PROJECT.md) |
| Mouse Vision / MICrONS | RESEARCH DIRECTION | Visual-perception research component; no implementation is present in this public runtime. The current implemented visual advisory integration is Qwen. [Project](JPGFLY_PROJECT.md) |
| Release 6.4 | HISTORICAL | Version-specific resilience/mechanics notes. Current Candidate Brain identifies itself as 7.1 temporal art policy. [6.4 notes](RELEASE-6.4.md), [history](HISTORY.md) |

Start with [local setup](LOCAL-RUN.md). See [SECURITY.md](../SECURITY.md) for actual application/service controls and [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidance.
