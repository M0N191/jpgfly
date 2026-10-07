# Local run

This guide starts the implemented room-based creative runtime with procedural text. Python 3.12 is the repository's CI baseline. Use one server worker so the process-local studio and sessions have one owner.

## Windows PowerShell

Use a fresh shell with Git and Python 3.12 installed:

```powershell
git clone https://github.com/M0N191/jpgfly.git
cd jpgfly
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt

$env:JPGFLY_AUTONOMOUS_STUDIO="true"
$env:JPGFLY_TEXT_PROVIDER="procedural"
$env:JPGFLY_VISION_COMPOSITION="false"
$env:JPGFLY_MALECNS_ENABLED="false"
$env:JPGFLY_ZEBRACNS_ENABLED="false"
$env:JPGFLY_OLLAMA_URL=""
$env:JPGFLY_FLM_URL=""
$env:JPGFLY_MALECNS_URL=""
$env:JPGFLY_ZEBRACNS_URL=""
.\.venv\Scripts\python.exe -m uvicorn app:app --host 127.0.0.1 --port 4673 --workers 1
```

## macOS / Linux

With Git and Python 3.12 installed:

```bash
git clone https://github.com/M0N191/jpgfly.git
cd jpgfly
python3.12 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt

export JPGFLY_AUTONOMOUS_STUDIO=true
export JPGFLY_TEXT_PROVIDER=procedural
export JPGFLY_VISION_COMPOSITION=false
export JPGFLY_MALECNS_ENABLED=false
export JPGFLY_ZEBRACNS_ENABLED=false
export JPGFLY_OLLAMA_URL=
export JPGFLY_FLM_URL=
export JPGFLY_MALECNS_URL=
export JPGFLY_ZEBRACNS_URL=
.venv/bin/python -m uvicorn app:app --host 127.0.0.1 --port 4673 --workers 1
```

The Python invocation is platform-specific; the application settings and runtime flow are the same.

## Open and check

- [Live Room](http://127.0.0.1:4673/live) — autonomous painting and current state
- [About](http://127.0.0.1:4673/about) — project UI
- [Backrooms](http://127.0.0.1:4673/archive) — completed works
- [Health](http://127.0.0.1:4673/api/health) — studio enabled/state/decision count

Allow the first Room to begin, then check that health reports the studio enabled and a changing decision count. Completed works appear after finalization. Stop the server with Ctrl+C.

The source default is `JPGFLY_AUTONOMOUS_STUDIO=false`. The commands explicitly enable it. Setting it false starts the server without its autonomous worker; viewing the live page does not enable the worker.

## Environment files

Copying [.env.example](../.env.example) to `.env` does not make the application load it. Uvicorn can load it explicitly:

```powershell
.\.venv\Scripts\python.exe -m uvicorn app:app --env-file .env --host 127.0.0.1 --port 4673 --workers 1
```

Before using an environment file, set the studio true and select the desired services. For a self-contained run, use the settings in the quick start, including empty optional service URLs. Both MaleCNS and ZebraCNS attempt connections when their URLs are nonempty, even if their enabled flags are false. Configured Qwen/FLM URLs also participate in support health checks.

Existing shell variables take precedence over values loaded from the file. Use a fresh shell when changing configurations. Keep `.env` and credentials out of Git.

## Data and restart

The default data directory is `.jpgfly/` beside [app.py](../app.py). Set `JPGFLY_DATA_DIR` to a writable directory for a different location; the configured data directory must remain outside `web/`.

| Path under the data directory | Contents |
|---|---|
| `artworks/` | Compact Room JSON and compressed final SVG files |
| `experience.json` | Shared bounded experience |
| `art-policy-v2.json` | Learned Art Policy weights and training state |
| `readings/` | Local `.txt` / `.md` conceptual reading inputs |

Back up this directory to preserve completed work and learning. `JPGFLY_ART_POLICY_PATH` can separately override policy storage. Active canvas, movement, decision stream, and unfinished sessions remain in memory; restart starts a fresh Room.

## Optional local services

The main runtime does not require these services for the quick start.

| Integration | Typical endpoint | Prerequisites and settings |
|---|---|---|
| Qwen / Ollama | `http://127.0.0.1:11434` | Run an Ollama service with a compatible vision model. Configure `JPGFLY_OLLAMA_URL`, `JPGFLY_VISION_MODEL`, and enable `JPGFLY_VISION_COMPOSITION`. Current default model identifier is `qwen3-vl:8b-instruct-q4_K_M`. |
| FLM | `http://127.0.0.1:4680` | External FLM implementation and trained adapter. [flm_bridge.py](../flm_bridge.py) uses `JPGFLY_FLM_ROOT` and optional `JPGFLY_FLM_RUN`; configure `JPGFLY_FLM_URL` and select text provider `flm` or `hybrid`. |
| MaleCNS | `http://127.0.0.1:4690` | Supply a compatible external neural service and configure `JPGFLY_MALECNS_URL`. Public source contains the client integration. |
| ZebraCNS / ZAPBench | `http://127.0.0.1:4770` | [zebracns_local.py](../zebracns_local.py) needs TensorStore to fetch the recorded dataset, or a valid local cache. Configure `JPGFLY_ZEBRACNS_URL` after starting that service. |

TensorStore is not in `requirements.txt`; installing the base requirements alone does not provide dataset fetching for local ZebraCNS. FLM and MaleCNS services are also not supplied by the base install. Optional service startup and data availability must be checked independently.

Text providers are `procedural`, `ollama`, `flm`, and `hybrid`. Qwen composition advice is configured separately from text generation.

Keep local endpoints on loopback. Authenticated remote endpoints require appropriate service credentials. Public HTTP controls require `JPGFLY_CONTROL_TOKEN`; see [SECURITY](../SECURITY.md) before exposing the application.

## Troubleshooting

- **Live page waits:** confirm `JPGFLY_AUTONOMOUS_STUDIO=true` in the server's environment, then inspect health and terminal logs.
- **Port is already in use:** stop your previous instance or choose another port and use matching browser URLs.
- **Support status shows DUMB DUMB:** check configured model URLs. The Candidate Brain can continue during support outages; final Room classification uses actual emergency-painter decisions.
- **Archive is empty:** a Room may still be painting, or the selected data directory may be new. Only compatible, successfully finalized records are visible.
- **Write errors:** check the data directory and permissions.

Container setup is not a verified run path for this branch. These instructions use Python directly.
