# JPGFLY 6.4 release notes

**HISTORICAL** — these notes describe the 6.4 architecture milestone. The current source identifies its art brain as `JPGFLY-BRAIN/7.1-TEMPORAL-ART-POLICY`. Use [Architecture](ARCHITECTURE.md) and [Features](FEATURES.md) for current behavior and integration status.

## Art brain, body, and language

The 6.4 notes established the separation between art decisions, physical execution, and writing:

```text
canvas observation
    -> Candidate Fly Brain: propose and score candidate actions
    -> selected action, with optional MaleCNS bias
    -> server-authoritative movement and stroke
    -> browser rendering
```

Language services supplied studio notes and finished-room writing while the Python painter selected actions. In current source, FLM retains this language/voice role, and Qwen also supplies advisory visual composition and context that influence later candidate scoring. Candidate Fly Brain remains the main art-decision engine.

## Neural integration boundary

MaleCNS is an optional connectome-inspired action-bias and telemetry interface. The public source contains its client integration; running it requires an external service. Recorded or simulated neural activity and its engineered effect on artwork must be described separately from claims about biological cognition.

## Resilience and rendering

The release notes documented an independent procedural emergency painter for Candidate Fly Brain decision errors and a live `DUMB DUMB` degraded-mode indicator. Language-service outages alone do not constitute emergency art decisions.

Current archive classification counts actual emergency-fallback decisions: a finished room is classified `DUMB_DUMB` only when its fallback ratio exceeds 35%. A temporary live label and the final archive classification have different purposes. See [current fallback behavior](ARCHITECTURE.md).

The live renderer reconstructs strokes from authoritative server events. This keeps the displayed canvas aligned with executed mechanics rather than relying on browser animation timing.

For service exposure and credentials, follow the [security policy](../SECURITY.md). For current setup, use [Local run](LOCAL-RUN.md).
