# Security

NeuroNest is an AI-powered personal intelligence and learning ecosystem, and it is being built with security as a first-class architectural concern rather than an afterthought. This document describes NeuroNest's current security posture, the principles guiding its development, how to report vulnerabilities, and what still needs to happen before the project should be trusted with sensitive or production data.

NeuroNest is an actively developed project. Security controls described here reflect a mix of what is implemented today and what is planned as the system matures.

---

## Security Status

NeuroNest is currently under **active development** and has **not** undergone a complete, independent production security audit.

The project should **not** be considered production-ready. Core subsystems — authentication, database access, persistent memory, file processing, and AI provider integration — are still evolving, and their security controls will evolve alongside them.

Until a formal security review has taken place, NeuroNest should be treated as a development-stage architectural foundation, not a hardened platform suitable for real user data or sensitive workloads.

---

## Reporting a Vulnerability

If you believe you have found a security vulnerability in NeuroNest, please report it responsibly.

**Preferred method:** If this repository has GitHub's private vulnerability reporting feature enabled (under the "Security" tab), please use it to submit your report privately.

**If private reporting is not available:** Please contact the maintainers directly and privately rather than opening a public GitHub issue that contains exploit details, proof-of-concept payloads, or other sensitive information. Public issues are appropriate for general security *hardening suggestions* that do not disclose an exploitable weakness, but not for active vulnerability reports.

A useful vulnerability report should include, where possible:

- A clear description of the issue
- The affected component or module
- Step-by-step reproduction instructions
- Expected behavior
- Actual (vulnerable) behavior
- The security impact (what an attacker could achieve)
- A proof of concept, if one can be shared safely
- A suggested mitigation, if you have one

**Please do not publicly disclose** exploit details, credentials, personal information, or working attack payloads before the issue has been triaged and addressed.

---

## Supported Versions

NeuroNest is early-stage and does not yet have tagged releases or a formal versioning scheme. Active development happens primarily on the main development branch.

| Branch | Security support |
|---|---|
| `main` (default branch) | Actively maintained |
| Older snapshots / forks | Not guaranteed |

Because there are no formal releases yet, there is no long-term support commitment for any prior state of the code. Always prefer the latest `main` branch when evaluating or reporting security issues.

---

## Secret Management

- `.env` files are for **local configuration only** and must never be committed to version control.
- `.env.example` and `apps/backend/.env.example` contain **placeholder templates only** — they must never contain real values.
- API keys, database credentials, JWT secrets, and other sensitive values must **never be hard-coded** in source files.
- In production, credentials should be stored in an appropriate secret-management solution (e.g. a cloud provider's secrets manager or a dedicated vault), not in plain environment files on disk.
- Secrets must not appear in logs, error responses, source code, tests, fixtures, screenshots, documentation, or commit history.
- If a secret is accidentally committed at any point, it must be treated as **compromised** and **rotated immediately** — removing it from a future commit is not sufficient, since it remains in Git history.

---

## Authentication and Authorization

NeuroNest's `auth` module defines the intended contract for authentication and authorization; the underlying enforcement logic is under active development and should not be assumed to be fully implemented.

Security requirements guiding this module include:

- Strong authentication before access to protected resources is granted.
- Authorization checks that enforce least privilege — a user or token should only be able to act on resources it legitimately owns or has been granted access to.
- Secure handling of sessions and/or tokens, including appropriate expiration and invalidation.
- If password-based authentication is introduced, passwords must be hashed with a modern, purpose-built algorithm — never stored in plaintext or reversible form.
- Secrets used to sign or verify tokens (e.g. `JWT_SECRET`) must be sufficiently random, kept out of source control, and rotated if ever exposed.
- Strict user/resource isolation, so one user's data or actions cannot affect another's.

These are design requirements the project is being built toward, not a claim that every control is complete today.

---

## AI and Model Security

The AI subsystem is a particularly important attack surface for NeuroNest and warrants explicit treatment.

Key risks the architecture must account for:

- **Prompt injection** — untrusted input attempting to override system instructions or intended behavior.
- **Indirect prompt injection** — malicious instructions embedded in retrieved content (files, memory, web content, tool output) rather than the user's direct message.
- **Sensitive information disclosure** — model output inadvertently revealing secrets, internal configuration, or another user's data.
- **Malicious or untrusted model output** — treating anything the model generates as data, not as a trusted command.
- **Tool abuse** — a model directing tools or actions beyond what was intended or authorized.
- **Context poisoning and memory poisoning** — an attacker manipulating stored context or memory to influence future responses.
- **Insecure handling of retrieved context** — pulling in external or user-supplied content without adequate isolation from system instructions.
- **Model hallucination** — the model presenting fabricated information as fact.
- **Excessive AI permissions** — granting the AI layer more access to data, tools, or actions than a given task requires.

Guiding principle: **AI-generated content is treated as untrusted input.** Model output must never be automatically executed, treated as an authoritative command, or used to bypass validation and authorization checks that would otherwise apply to that action.

The provider abstraction (`AI Controller → AI Service → AI Provider Interface → Ollama Provider`) exists partly to keep provider credentials and provider-specific behavior isolated from the rest of the application, reducing the blast radius of a compromised or misbehaving provider.

---

## Memory and Personal Data

NeuroNest is intended to eventually store and use personal context — conversations, learning activity, preferences, and related data — to provide more useful assistance over time. This makes the `memory` module a sensitive part of the system.

Guiding principles:

- **Data minimization** — store only what is genuinely needed to provide the intended functionality.
- **Purpose limitation** — use stored data only for the purpose it was collected for.
- **Access control** — ensure only the intended user (and authorized system components) can read or write their own memory.
- **User isolation** — prevent any possibility of one user's stored context leaking into another user's session or responses.
- **Secure storage** — protect stored memory and conversation data at rest, appropriate to its sensitivity.
- **Secure deletion / lifecycle management** — support removing data when it is no longer needed or when a user requests deletion.
- **Avoiding unnecessary retention** — avoid retaining data indefinitely without a clear reason.
- **Careful handling of retrieved memory** — treat memory retrieved for use in AI prompts with the same caution as other untrusted/sensitive context (see [AI and Model Security](#ai-and-model-security)).

NeuroNest does **not** currently claim compliance with any regulatory framework (e.g. GDPR, HIPAA, SOC 2, ISO 27001). Such compliance, if pursued, would require dedicated review and is not represented by this document.

---

## File Upload and Processing Security

The `files` module handles user-supplied files, which must always be treated as **untrusted input**.

Security considerations for this domain include:

- File type validation (not relying solely on file extension or client-supplied MIME type).
- File size limits to prevent resource exhaustion.
- Filename and path sanitization to prevent path traversal.
- Malware/content scanning where appropriate for the deployment context.
- Safe, isolated temporary storage during processing.
- Preventing arbitrary code execution via file parsing or processing.
- Avoiding unsafe or overly permissive parsing libraries.
- Isolating file-processing workloads from core application logic where feasible (e.g. via background jobs).
- Access control on stored and downloadable files, so users can only access files they own or are authorized to access.

These are the intended controls for this domain. As with other subsystems, the extent of current implementation should not be assumed — check the `files` module directly for its current state.

---

## API and Web Security

Planned and in-progress controls for the HTTP/API layer include:

- Input validation at module boundaries (`*.schema.ts`).
- Rate limiting to reduce abuse and brute-force risk.
- CORS configuration scoped to legitimate origins.
- CSRF protections where applicable (e.g. cookie-based sessions).
- Secure HTTP headers (via middleware such as `helmet`).
- TLS/HTTPS enforced at the infrastructure layer in production.
- Request size limits to reduce resource-exhaustion risk.
- Authentication and authorization middleware applied consistently across protected routes.
- Centralized error handling that avoids leaking stack traces or internal implementation details.
- Request IDs to aid traceability and incident investigation.
- General abuse-prevention measures appropriate to a public-facing API.

Not every control listed here is fully implemented today; this section describes the target posture for the API layer as it matures.

---

## Database and Infrastructure Security

Considerations for the data and infrastructure layer include:

- Database credentials supplied via environment configuration, never hard-coded.
- Least-privilege database users/roles, scoped to what the application actually needs.
- Encryption in transit for database and cache connections.
- Encryption at rest where supported by the hosting environment.
- Regular backups with tested restore procedures, once the project reaches a stage where this is applicable.
- Safe, reviewable migration practices.
- Redis secured appropriately (authentication, network restriction) rather than exposed openly.
- Container security practices for any Docker images built from this repository.
- Network isolation between services where the deployment topology allows it.
- Reverse proxy (Nginx) configuration hardened before any production use.
- Clear separation between development and production configuration.

**Note:** The current `infrastructure/docker/` and `infrastructure/nginx/` configurations represent **development-oriented scaffolding**. They are not production-hardened and should be reviewed and strengthened before any production deployment.

---

## Dependency and Supply-Chain Security

- Dependencies are kept reasonably current; updates should be reviewed rather than applied blindly.
- Security advisories for direct and transitive dependencies should be reviewed periodically.
- Lockfiles are used to ensure reproducible, verified dependency trees.
- New dependencies should be evaluated before adoption — avoid adding packages for trivial functionality that could reasonably be implemented directly.
- Automated dependency/security scanning tools (e.g. `npm audit`, GitHub Dependabot alerts) should be used where available in CI.
- Third-party packages are treated as part of the application's attack surface, not as inherently trusted code.

---

## Secure Development Practices

- Strict TypeScript configuration across the codebase, favoring explicit types over `any`.
- Code review for all non-trivial changes, with extra scrutiny for security-sensitive code (auth, AI providers, file handling, database access).
- Automated tests, expanded over time to cover security-relevant behavior.
- Static analysis and linting as part of the development workflow.
- Dependency and secret scanning integrated into CI where available.
- Small, focused changes that are easier to review carefully.
- Explicit security review encouraged for any change touching authentication, authorization, secrets, AI provider integration, or file handling.

Not all of the automated tooling described above is necessarily wired into CI yet; this section describes the practices the project is working toward as much as what exists today.

---

## Security Checklist

### Development

- [ ] No secrets committed to the repository
- [ ] `.env` files excluded via `.gitignore`
- [ ] Input validated at module/schema boundaries
- [ ] New dependencies reviewed before adoption
- [ ] AI-generated output treated as untrusted data, not executable instructions
- [ ] Errors handled without leaking internal details

### Pre-Production

- [ ] Authentication and authorization enforced on all protected routes
- [ ] Secrets moved to a proper secret-management solution (not `.env` files)
- [ ] Database users scoped to least privilege
- [ ] Rate limiting and request size limits configured
- [ ] Secure HTTP headers and CORS configured
- [ ] TLS/HTTPS configured at the infrastructure layer
- [ ] Docker/Nginx configuration reviewed and hardened
- [ ] Dependency and secret scanning enabled in CI
- [ ] File upload validation and storage isolation verified

### Production

- [ ] Independent security review completed
- [ ] Backups configured and restore procedures tested
- [ ] Logging/monitoring in place without exposing secrets
- [ ] Incident response process defined
- [ ] Regular dependency and vulnerability review scheduled

---

## Disclosure and Security Updates

Confirmed vulnerabilities will generally go through:

1. Investigation and reproduction
2. Development of a mitigation or patch
3. Documentation updates where relevant
4. A security advisory, where appropriate, once a fix is available

There is currently no established formal response-time SLA for security reports. Reports will be addressed as promptly as the maintainers' capacity allows, prioritized by severity and impact.

---

## Scope

Security reports should focus on NeuroNest's own code and infrastructure as maintained in this repository (backend application, shared packages, and the infrastructure scaffolding included here).

Please do **not** attempt to test against third-party AI infrastructure (e.g. an Ollama deployment or other external AI service), other users' data, or any systems or services you have not been explicitly authorized to test. Reports involving third-party infrastructure outside this project's control are out of scope.

---

## Disclaimer

NeuroNest is an actively developed, early-stage project. It should **not** be assumed suitable for sensitive or production workloads — including real user data, personal information, or anything requiring regulatory compliance — until the security controls, independent review, and infrastructure hardening described in this document have actually been completed.