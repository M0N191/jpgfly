# Contributing to JPGFLY

JPGFLY is an artwork and a software experiment. Contributions should make the creative system easier to understand, run, and develop while keeping its artistic voice distinct from implementation claims.

## Principles

- Keep Candidate Fly Brain responsible for art decisions and the server responsible for authoritative movement and strokes.
- The browser renders and replays server state; document any change to that boundary explicitly.
- Distinguish recorded biological data, simulated neural dynamics, and engineered artistic interpretations.
- Describe implemented capabilities, optional integrations, and research directions separately. Do not claim consciousness or sentience.
- Keep credentials, private user data, and local model artifacts out of commits.

## Setup and validation

Use [Local run](docs/LOCAL-RUN.md) for Python 3.12 setup and the autonomous studio. Use Node.js 22 for browser-source tests, matching [CI](.github/workflows/ci.yml).

For runtime changes, run the relevant checks locally:

```powershell
$env:JPGFLY_AUTONOMOUS_STUDIO="false"
$env:JPGFLY_TEXT_PROVIDER="procedural"

.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\python.exe -m py_compile app.py brain_provider.py candidate_brain.py composition_engine.py composition_vision.py experience_memory.py flm_bridge.py flm_text_provider.py narrative_provider.py server_mechanics.py studio_delta.py agent_profiles.py art_materials.py art_policy.py room_theme.py subject_catalog.py zebracns.py zebracns_local.py
.\.venv\Scripts\python.exe -m unittest discover -s tests -p "test_*.py" -v
npm.cmd test
```

For behavior changes, add or update a regression test that checks the affected behavior. For documentation changes, check relative links and verify technical claims against current source.

## Pull requests and releases

Treat `main` as release-only. Submit changes on a separate branch through a reviewed pull request. Require green `ci` and `Release audit` checks before merging; these are project review expectations, not a statement that branch protection enforces them.

[CODEOWNERS](.github/CODEOWNERS) identifies the default reviewer. The [Release audit](.github/workflows/release-audit.yml) checks dependencies, compiles the runtime, checks source hygiene, and runs Python and browser tests. Explain the concrete change, its validation, and any remaining limitations in the pull request.

## Documentation and UI copy

Keep documentation concise and source-backed. Artistic language can be poetic, strange, funny, or philosophical; technical documentation should identify where metaphor ends and implementation begins. See [Architecture](docs/ARCHITECTURE.md) and the [capability matrix](docs/FEATURES.md).

The repository uses LF for source and documentation and CRLF for PowerShell scripts through `.gitattributes`.

## Security-sensitive changes

Changes to HTTP controls, archive integrity, prompt construction, model bridges, credentials, or network exposure should include a short threat-model note. Follow the [security policy](SECURITY.md) when reporting vulnerabilities.
