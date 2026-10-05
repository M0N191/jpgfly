# JPGFLY

## Current work

### JPGFLY / Habitat

JPGFLY explores autonomous painting, memory, local models, and creative environments. Habitat is a persistent-world research direction.

[Project docs →](./docs/JPGFLY_PROJECT.md)

---

## Building around

```text
AI / AGENTS
├── persistent identity
├── multi-agent worlds
├── memory + agency
└── local / open models

GENERATIVE SYSTEMS
├── autonomous art
├── procedural worlds
└── live creative processes
```

---

### Philosophy

> Mechanics create facts. Intelligence creates meaning.

I’m interested in systems that **continue existing between prompts** — agents with memory, environments with history, and software where state has consequences.

## Run the portfolio locally

With Python 3.12 installed, open PowerShell in the repository folder:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
$env:JPGFLY_AUTONOMOUS_STUDIO="false"
$env:JPGFLY_AUTONOMOUS_TERMINAL="false"
$env:JPGFLY_HABITAT_SERVER="false"
$env:JPGFLY_TEXT_PROVIDER="procedural"
$env:JPGFLY_BRAIN_PROVIDER="procedural"
.\.venv\Scripts\python.exe -m uvicorn app:app --host 127.0.0.1 --port 4673
```

Open `http://127.0.0.1:4673` on the same computer. Stop the server with Ctrl+C. These defaults show the portfolio without starting autonomous creative workers; optional neural/model features require their separately configured local stack.
