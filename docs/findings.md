# Security Findings Summary

This document summarizes the most important findings from the ContactIQ security-engineering review. It is not a penetration-test report.

## 1. Deployment controls were not on the frontend request path

**Finding:** Express security middleware existed in the application, but the deployed static frontend was served through a separate path.

**Risk:** Source-level review could create false confidence that browser security controls were applied to frontend responses.

**Remediation:** Production serving was redesigned so the React frontend and API pass through a unified Express service.

**Validation:** Representative public, private, authentication, asset, health, and unknown API/static paths were checked after deployment. Browser security headers, route-aware indexing, API no-store behavior, asset caching, API/fallback separation, and edge HSTS behavior were observed as intended.

## 2. Authorization required consistent server-side ownership enforcement

**Finding:** Client-visible identity or routing state cannot establish authorization.

**Remediation:** Application queries and mutations use authenticated server identity and resource-ownership conditions. Admin authorization uses a server-side role check.

**Remaining validation:** Two-user authorization/IDOR runtime testing is still required before customer production.

## 3. AI context needed explicit trust boundaries

**Finding:** Contact notes, reminders, and user prompts can contain sensitive data or instruction-like content.

**Remediation:** Gemini access is server-side. System instructions are separated from structured application context and user input. Application/user content is marked untrusted, ownership-scoped, minimized, truncated, and bounded.

**Remaining validation:** Authenticated prompt-injection testing remains required.

## 4. Logs and audit events required minimization

**Finding:** Request bodies, credentials, contact content, recipient data, and raw provider responses were unnecessary for the intended operational evidence.

**Remediation:** Structured request logging is metadata-focused, sensitive headers are redacted, and selected security audit events retain minimal operation metadata.

**Limitation:** Audit writes are best-effort and audit coverage is selected rather than universal.

## 5. Account deletion crossed multiple trust boundaries

**Finding:** Local application deletion and identity-provider deletion are separate operations with different failure modes.

**Remediation:** Local application data is removed transactionally before external identity deletion is attempted. Retained security audit history is anonymized rather than retaining the deleted actor identifier.

**Remaining validation:** Full authenticated export/deletion and partial-failure runtime testing remains required.

## 6. Backup capability was not equivalent to verified recovery

**Finding:** Database connectivity and provider backup features do not demonstrate project-specific retention or successful restoration.

**Action:** Recovery documentation records backup/restore verification and an observed recovery exercise as customer-production requirements.

## Supporting validation

The supported dependency audit reported zero known vulnerabilities. Supported SAST and HoundDog scans reported no findings. API/frontend builds and workspace typechecking passed after final cleanup.

These results support the assessed controls but do not establish complete security, certification, compliance, or penetration-test coverage.
