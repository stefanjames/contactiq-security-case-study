# Security Controls

## Authentication and authorization

- Server-side authentication context established through Clerk.
- Application routes require an authenticated user.
- Verified primary email is required before protected application routes proceed.
- Admin authorization uses a server-side role record.
- Resource queries and mutations apply authenticated-user ownership conditions.
- Admin authorization failures are audited.

## Application and browser security

- Unified Express serving for frontend and API.
- Content Security Policy.
- Framing denied.
- MIME sniffing prevention.
- Referrer and permissions policies.
- Production origin restrictions.
- Origin/same-origin referrer checks for non-safe browser requests.
- Request-body limits.
- Private frontend routes marked non-indexable.
- API responses configured no-store.
- Hashed frontend assets use immutable long-lived caching.
- HSTS supplied once at the hosting edge.

## API and abuse controls

- General API rate limit.
- Dedicated AI assistant rate limit.
- Dedicated email rate limit.
- Dedicated import and merge rate limits.
- Current limiter is application-memory based and is not represented as a distributed quota system.

## Input and import controls

- Server-side validation for application input.
- CSV import row limits and expected-field validation.
- Formula-like input rejection.
- Original import files are not retained as executable uploads.
- Import preview and commit operations are separately constrained.

## Data isolation

Contacts, tags, reminders, interactions, imports, exports, and assistant-context queries use authenticated ownership constraints. Client-supplied visibility is not treated as authorization.

## Secrets

Provider credentials and application secrets are supplied through managed runtime configuration rather than committed source. Browser code receives only public client configuration required for the application.

## AI / LLM controls

- Server-side Gemini integration.
- Trusted system instruction separated from user/application data.
- Structured untrusted application context and user request.
- Ownership-scoped data selection.
- Context minimization and PII exclusions.
- Limits on contacts, reminders, note lengths, total serialized context, user input, and provider output.
- No conversation-history persistence by ContactIQ.
- No autonomous tools/actions.
- Generic provider-failure response with minimized logging.

See [AI Security](ai-security.md).

## Privacy

- Authenticated application-data export.
- Account deletion with transactional local deletion.
- Audit actor anonymization for retained security history.
- Reminder and interaction deletion.
- Public privacy notice.
- Provider-side retention/deletion behavior is not asserted beyond what was independently established.

## Logging and audit

Request logging records operational metadata such as request identifier, method, path without query parameters, response status, and timing. Sensitive authorization/cookie fields are redacted.

Selected security-relevant operations write minimal audit events including actor, action, entity type/identifier, success state, request identifier, and limited operation metadata. Reminder creation is included.

Audit writes are best-effort; this tradeoff is documented.

## Dependency and supply-chain review

- Supported dependency audit reported zero known vulnerabilities.
- Supported SAST reported no findings.
- Supported HoundDog scan reported no vulnerabilities.
- Lockfile integrity metadata and minimum package release age were reviewed.
- Available dependency updates were identified but not automatically upgraded.
- No SBOM or separate provenance attestation was produced.

## Operational controls

Documentation was created for incident response, vulnerability management, secret rotation, security maintenance, backup/recovery, and production readiness.

These documents define operating expectations; they do not imply 24/7 monitoring, verified restore execution, certification, or complete production readiness.
