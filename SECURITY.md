# Security policy

## Reporting

Report vulnerabilities through GitHub's private vulnerability reporting or security advisory features when available. Do not put working credentials, personal data, private endpoints, or exploit details in public issues. If private reporting is unavailable, open an issue asking for a private reporting channel without disclosing the vulnerability.

## Application controls

The [HTTP middleware](app.py) requires `Authorization: Bearer <JPGFLY_CONTROL_TOKEN>` for control methods when that credential is configured. Without it, control requests are limited to loopback clients. Public deployment mode requires a control credential of at least 32 characters and validates writable storage outside the public web directory.

The application applies security headers and rejects declared request bodies above its configured size limit. These controls do not replace deployment-level authentication, TLS, request limits, and careful proxy configuration.

- Bind local development to `127.0.0.1`.
- Use HTTPS for remote access and restrict control routes to authorized callers.
- Generate unique credentials for each deployment and keep `.env` files and service credentials outside Git.
- Review how the reverse proxy supplies client addresses and which routes it exposes.

## Optional model and neural services

Keep Qwen/Ollama, FLM, MaleCNS, and ZebraCNS services on loopback unless remote access has an authenticated boundary. Expose only the needed routes, with TLS and request-size limits. A separately configured gateway is deployment infrastructure, not a bundled JPGFLY service.

The [FLM bridge](flm_bridge.py) defaults to loopback access. Enabling remote bridge access requires `JPGFLY_FLM_AUTH_TOKEN` with at least 32 characters. The application also supports bearer credentials for FLM and Ollama clients; verify client configuration before exposing a service.

Treat model outputs and ingested reading material as untrusted input. Review prompt construction, generated public text, and any new filesystem or network access together. External model implementations, adapters, checkpoints, and neural datasets need their own access controls.

## Persistent data

Completed rooms, Experience Memory, and Learned Art Policy contain creative history and influence later behavior. Store them outside `web/`, restrict filesystem access, and back them up before maintenance. Use a persistent data directory for deployments that must retain this history.

Avoid ingesting confidential reading material or personal data into an installation that exposes room writing and metadata publicly. Do not commit runtime archives, local model artifacts, or machine-specific credentials.

See [Local run](docs/LOCAL-RUN.md) for setup and [Architecture](docs/ARCHITECTURE.md) for runtime and fallback behavior.
