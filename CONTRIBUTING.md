# Contributing

Thanks for your interest in contributing to NeuroNest. The project is actively evolving, and contributions — code, documentation, architectural feedback, and issue reports — are welcome as it grows.

Because the architecture is still being established, please read this document before opening a pull request, especially the sections on development philosophy and repository structure.

---

## Before You Start

Before making a change, please:

- Read [`README.md`](./README.md) to understand the project's goals and current architecture.
- Review [`SECURITY.md`](./SECURITY.md), especially if your change touches authentication, secrets, AI provider integration, file handling, or persistent data.
- Understand the module you're changing — read its existing `.controller.ts`, `.service.ts`, `.routes.ts`, `.types.ts`, and (where present) `.schema.ts` files before modifying them.
- Check existing issues and open pull requests to avoid duplicating work already in progress.
- Never commit secrets, API keys, credentials, or `.env` files. Use `.env.example` as a template only.

---

## Development Philosophy

NeuroNest's architecture is guided by a consistent set of engineering principles. Contributions are expected to follow them:

- **Correctness before speed.** A working, well-considered change is more valuable than a fast, fragile one.
- **Explicit architecture.** Module boundaries, contracts, and responsibilities should be clear from the code, not implied.
- **Strict typing.** TypeScript's strict mode is not optional. Prefer explicit types and interfaces over `any`; use `unknown` when a type is genuinely not known ahead of time.
- **Maintainability over cleverness.** Prefer clear, readable code to clever abstractions that are hard to follow.
- **Security by default.** Secrets stay out of source control and logs; untrusted input (user input, file uploads, AI model output) is treated as untrusted at every boundary.
- **Testable code.** Structure code so it can reasonably be tested — favor dependency inversion and clear boundaries over tightly coupled implementations.
- **Minimal unnecessary dependencies.** Adding a new dependency should be a deliberate decision, not a default.
- **Incremental implementation.** It's acceptable — and expected — for scaffolding to contain `TODO` markers for logic that isn't implemented yet. Do not implement fake or placeholder business logic just to make something *look* complete.
- **Do not implement functionality merely to make a test pass.** Tests should reflect real behavior, not be gamed.
- **Document architectural changes.** If a change alters how a module, provider, or subsystem is structured, update the relevant documentation alongside the code — don't let docs silently drift out of date.
- **Avoid unnecessary coupling between domains.** Modules under `src/modules/` should depend on shared abstractions, not reach directly into each other's internals.

---

## Repository Structure

```text
NeuroNest/
├── apps/
│   ├── backend/
│   │   ├── src/
│   │   │   ├── config/        Centralized environment & infrastructure config
│   │   │   ├── core/          Errors, logger, middleware, security, utils
│   │   │   ├── database/      Migrations, schema, seed scaffolding
│   │   │   ├── jobs/          Background job queues and workers
│   │   │   ├── modules/       Domain modules (ai, auth, users, memory, ...)
│   │   │   └── routes/        Route aggregation
│   │   └── tests/
│   │       ├── unit/
│   │       ├── integration/
│   │       └── e2e/
│   └── frontend/              Placeholder workspace (not yet implemented)
├── packages/
│   ├── api-client/            Typed client foundation for the backend API
│   └── shared/                 Framework-agnostic types, schemas, constants, utils
├── infrastructure/             Docker, Nginx, and deployment scaffolding
├── docs/                       Architecture, API, database, and development docs
├── scripts/
└── .github/                    Workflows and issue templates
```

- **`apps/backend`** — the Express/TypeScript API. Domain logic lives under `src/modules/`; shared application infrastructure (config, errors, logging, middleware) lives under `src/core/` and `src/config/`.
- **`apps/frontend`** — reserved for future frontend work. Currently a placeholder workspace.
- **`packages/shared`** — types, schemas, constants, and utilities usable by both backend and, eventually, frontend/api-client code. Should not depend on backend-specific implementation details.
- **`packages/api-client`** — a typed foundation for consuming the backend API from other workspaces.
- **`infrastructure/`** — Docker and Nginx scaffolding for local development. Not production-hardened.
- **`docs/`** — supplementary documentation beyond the top-level `README.md`.

---

## Development Environment

Baseline requirements:

- Node.js 20+
- npm 10+
- Git
- (Optional) [Ollama](https://ollama.com) running locally, for AI-related development

Install dependencies from the repository root:

```bash
npm install
```

Copy the environment templates and fill in local values (never commit the resulting files):

```bash
cp .env.example .env
cp apps/backend/.env.example apps/backend/.env
```

Start the backend in watch mode:

```bash
npm run dev
```

---

## Making Changes

### Working Within a Module

When adding or modifying a domain module under `apps/backend/src/modules/`, keep responsibilities separated:

- `<module>.controller.ts` — request/response handling only. No business logic here.
- `<module>.service.ts` — business logic and orchestration.
- `<module>.routes.ts` — route definitions, wired to the controller.
- `<module>.types.ts` — module-local types.
- `<module>.schema.ts` — input validation, where the module accepts external input.

Do not place business logic directly inside route definitions — routes should delegate to controllers, and controllers should delegate to services.

### Working on the AI Layer

AI provider implementations live under `apps/backend/src/modules/ai/providers/`. The AI service must depend on the provider interface (`provider.interface.ts`), never on a specific provider's implementation details directly. If you add a new provider, implement the existing interface rather than introducing a parallel code path in the AI service.

### Cross-Module Changes

Avoid reaching directly into another module's internals. If two modules need to share logic or types, consider whether that logic belongs in `packages/shared` instead.

### Scaffolding and TODOs

Some modules currently contain typed contracts with `TODO` markers rather than complete business logic. When implementing one of these:

- Replace the `TODO` with real logic scoped to that module's responsibility.
- Do not silently expand the module's scope or change its public contract without noting it in your PR description.
- If your change alters an established contract (a service's public method signatures, a provider interface, a shared type), call that out explicitly — this is an architectural change and should be reviewed as one.

---

## Commit and Pull Request Guidelines

- Keep changes scoped to a single module or concern where possible. Large, sprawling PRs are harder to review carefully.
- Write clear commit messages that describe *what* changed and *why*.
- In your PR description, note any architectural decisions, trade-offs, or intentionally deferred work (e.g. remaining `TODO`s).
- If your change affects environment configuration, update the relevant `.env.example` file(s) alongside the code.
- If your change affects documentation-worthy behavior (README, SECURITY, architecture docs), update the relevant document in the same PR rather than as a follow-up.

Before opening a PR, run:

```bash
npm run typecheck
npm run lint
npm run test
```

These are the same checks expected to run in CI. Please make sure they pass locally first.

---

## Code Review

All contributions are expected to go through review before merging. Reviewers will generally be looking at:

- Whether the change respects existing module boundaries and the AI provider abstraction.
- Whether TypeScript typing is explicit and strict-mode compliant.
- Whether secrets, credentials, or sensitive data are handled correctly.
- Whether the change is scoped clearly and documented where it affects architecture.

Review turnaround is best-effort — this is an actively developed project without a dedicated review team, so please be patient and open to discussion on your PR.

---

## Reporting Issues

When opening an issue, please include:

- A clear description of the problem or proposed change.
- The affected module or area of the codebase, if known.
- Steps to reproduce, for bugs.
- Any relevant context (Node.js version, environment, logs — with secrets removed).

For security vulnerabilities, please follow the process in [`SECURITY.md`](./SECURITY.md) instead of opening a public issue.

---

## Questions

If something about the architecture or contribution process is unclear, opening an issue to ask is welcome — clarifying questions often surface gaps in the documentation itself, which is useful in its own right.