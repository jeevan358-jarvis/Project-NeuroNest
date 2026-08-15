# NeuroNest

> **An AI-powered personal intelligence and learning ecosystem.**

NeuroNest is designed to bring AI assistance, persistent memory, personalized learning, productivity, analytics, file processing, and intelligent user context together into one modular platform.

The project is built as a **TypeScript/Node.js monorepo** with a domain-oriented backend architecture and an extensible AI provider layer.

---

## Table of Contents

- [Overview](#overview)
- [Vision](#vision)
- [Core Principles](#core-principles)
- [Architecture](#architecture)
- [System Architecture](#system-architecture)
- [Repository Structure](#repository-structure)
- [Backend Architecture](#backend-architecture)
- [Domain Modules](#domain-modules)
- [AI Provider Architecture](#ai-provider-architecture)
- [Memory Architecture](#memory-architecture)
- [Learning Architecture](#learning-architecture)
- [Analytics](#analytics)
- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [Available Scripts](#available-scripts)
- [Database](#database)
- [Background Jobs](#background-jobs)
- [Testing](#testing)
- [Docker](#docker)
- [Security & Privacy](#security--privacy)
- [Development Status](#development-status)
- [Roadmap](#roadmap)
- [Development Workflow](#development-workflow)
- [Contributing](#contributing)
- [Team](#team)
- [License](#license)

---

## Overview

NeuroNest is being engineered as more than a conventional chatbot.

The long-term goal is to create a **personal intelligence layer** capable of understanding a user's context, retaining useful information, assisting with learning, supporting productivity, analyzing patterns, and progressively providing more personalized assistance.

The architecture brings together several interconnected domains:

- AI assistance
- Persistent contextual memory
- Personalized learning
- Conversations
- Tasks and productivity
- Analytics
- Notifications
- File processing
- Authentication
- User management
- AI provider abstraction

The current repository establishes the architectural foundation for these systems. Domain functionality is being implemented incrementally behind explicit contracts rather than being represented as completed functionality before it exists.

---

## Vision

The long-term vision for NeuroNest is to evolve from an AI assistant into a **personal intelligence ecosystem**.

The system is being designed around four major pillars:

### 1. Intelligence

Provide an AI layer capable of reasoning over relevant user context and interacting with the rest of the NeuroNest ecosystem.

### 2. Memory

Build a persistent contextual memory system that can retain useful information and retrieve relevant context when needed.

### 3. Learning

Create an adaptive learning environment capable of understanding progress, identifying weaknesses, and supporting personalized study.

### 4. Personalization

Use accumulated context, preferences, activity, learning patterns, and goals to make the system progressively more useful to each individual user.

The ultimate objective is not simply to generate better answers.

It is to build a system that can **understand context, remember what matters, learn how the user works, and assist intelligently over time.**

---

## Core Principles

### Modularity

Each major domain is isolated behind clear boundaries.

This allows individual systems such as memory, learning, AI, tasks, and analytics to evolve without creating unnecessary coupling throughout the application.

### Provider Independence

The AI subsystem depends on an abstract provider contract rather than directly depending on one AI implementation.

This allows future AI providers to be introduced without redesigning the application layer.

### Type Safety

NeuroNest uses strict TypeScript throughout the backend.

The architecture favors:

- Explicit types
- Interfaces
- `unknown` over unnecessary `any`
- Clear module boundaries
- Explicit error handling
- Dependency inversion

### Honest Scaffolding

The repository distinguishes between:

- Implemented functionality
- Architectural foundations
- Work in progress
- Planned capabilities

Unimplemented business logic is not represented as completed functionality.

---

# Architecture

NeuroNest follows a **domain-oriented modular monolith** architecture.

The backend is currently organized as one application containing independently structured domain modules.

This provides the organizational benefits of modular services without introducing unnecessary distributed-system complexity at the current stage.

---

## System Architecture

At a high level, requests follow this path:

```text
                         ┌─────────────────────┐
                         │       Client        │
                         │ Frontend / API User │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Express App       │
                         │      app.ts         │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
            Security Middleware              Route Layer
                    │                               │
                    └───────────────┬───────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Controllers      │
                         │ Request/Response    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      Services       │
                         │ Business Logic      │
                         └──────────┬──────────┘
                                    │
             ┌──────────────────────┼──────────────────────┐
             │                      │                      │
             ▼                      ▼                      ▼
       ┌───────────┐         ┌───────────┐         ┌─────────────┐
       │ Database  │         │   Redis   │         │ Background  │
       │           │         │           │         │    Jobs     │
       └───────────┘         └───────────┘         └─────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │  AI Provider  │
                            │    Layer      │
                            └───────┬───────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ Ollama / AI   │
                            │   Provider    │
                            └───────────────┘
```
---

## Repository Structure

```text
NeuroNest/
├── apps/
│   ├── backend/                  Node.js/TypeScript API
│   │   ├── src/
│   │   │   ├── config/           Centralized env, constants, database, redis config
│   │   │   ├── core/             Errors, logger, middleware, security, utils
│   │   │   ├── database/         Migrations, schema, seed scaffolding
│   │   │   ├── jobs/             Queues and workers scaffolding
│   │   │   ├── modules/          Domain modules (auth, ai, memory, learning, ...)
│   │   │   ├── routes/           Route aggregation
│   │   │   ├── app.ts            Application construction
│   │   │   └── server.ts         HTTP server startup + lifecycle
│   │   └── tests/
│   │       ├── unit/
│   │       ├── integration/
│   │       └── e2e/
│   │
│   └── frontend/                 Placeholder workspace (not yet implemented)
│
├── packages/
│   ├── api-client/                Typed client foundation for the backend API
│   └── shared/                    Framework-agnostic types, schemas, constants, utils
│
├── infrastructure/
│   ├── docker/                    Development-oriented Docker setup
│   ├── nginx/                     Reverse-proxy scaffolding
│   └── deployment/                Deployment scaffolding (not production-hardened)
│
├── docs/
│   ├── api/
│   ├── architecture/
│   ├── database/
│   └── development/
│
├── scripts/
│
└── .github/
    ├── workflows/
    └── ISSUE_TEMPLATE/
```

---

## Backend Architecture

The backend (`apps/backend`) is organized as a modular, domain-oriented monolith:

```text
apps/backend/src/
├── config/       env.ts, constants.ts, database.ts, redis.ts
├── core/
│   ├── errors/       AppError model + centralized error handler
│   ├── logger/         Structured logger interface
│   ├── middleware/      Request ID, auth/authz boundaries
│   ├── security/         Security middleware composition
│   └── utils/               Common utility types and helpers
├── database/
│   ├── migrations/
│   ├── schema/
│   └── seed/
├── jobs/
│   ├── queues/
│   └── workers/
├── modules/
│   ├── ai/
│   ├── analytics/
│   ├── auth/
│   ├── conversations/
│   ├── files/
│   ├── learning/
│   ├── memory/
│   ├── notifications/
│   ├── tasks/
│   └── users/
├── routes/
├── app.ts
└── server.ts
```

`app.ts` is responsible for constructing the Express application: middleware registration, security middleware, JSON parsing, route mounting, and centralized error handling.

`server.ts` is responsible for process lifecycle: loading configuration, starting the HTTP server, graceful shutdown, signal handling, and startup-failure handling.

This separation means the application object can be constructed and tested (e.g. via `supertest`) without ever binding a port.

---

## Domain Modules

Each domain module under `apps/backend/src/modules/` follows a consistent file layout:

| File | Responsibility |
|---|---|
| `<module>.controller.ts` | Request/response handling |
| `<module>.service.ts` | Business logic and orchestration |
| `<module>.routes.ts` | Route definitions for the module |
| `<module>.types.ts` | Module-local types |
| `<module>.schema.ts` | Input validation (where applicable) |

Current modules:

| Module | Responsibility |
|---|---|
| `auth` | Authentication and session/token issuance |
| `users` | User account and profile management |
| `ai` | AI orchestration via the provider interface |
| `conversations` | Conversation/thread persistence and retrieval |
| `memory` | Persistent user memory and context |
| `learning` | Personalized learning content and progress |
| `tasks` | Productivity and task management |
| `analytics` | Usage and behavioral analytics |
| `notifications` | User-facing notifications |
| `files` | File upload, storage, and processing |

Modules currently expose strongly typed interfaces and documented service boundaries with explicit `TODO` markers where domain logic will be implemented — this establishes the architectural contract before business logic is written.

---

## AI Provider Architecture

The AI module (`src/modules/ai/`) never depends on a specific AI backend's implementation details. Instead:

- `providers/provider.interface.ts` defines a provider-agnostic contract (request shape, response shape, error handling expectations).
- `providers/ollama.provider.ts` implements that contract for [Ollama](https://ollama.com), reading its base URL, API key, and model from environment configuration — never hard-coded.
- `ai.service.ts` depends only on the interface, not on the Ollama provider directly.

```text
AI Controller
      │
      ▼
AI Service
      │
      ▼
AI Provider Interface   ← stable contract, provider-agnostic
      │
      ▼
Ollama Provider         ← concrete implementation, swappable
```

This means a future provider (another local, self-hosted, or cloud-based AI system) can be added by implementing `AIProvider` and wiring it in at the composition point — with zero changes to the service layer or anything above it.

The Ollama provider:

- Performs asynchronous HTTP requests with explicit request/response types.
- Handles HTTP failures, malformed responses, and timeouts.
- Normalizes provider-specific output into the shape the AI service expects.
- Never logs API keys or other secrets.

---

## Memory Architecture

The `memory` module is responsible for persistent, retrievable context about a user — the foundation that lets NeuroNest behave consistently across sessions rather than treating each conversation as isolated.

Planned responsibilities (behind the module's typed contract):

- Storing durable facts, preferences, and context supplied by the user or inferred from interactions.
- Retrieving relevant memory for a given conversation or task, rather than replaying the entire history.
- Separating short-lived conversational context from long-term durable memory.
- Providing the `ai` and `conversations` modules with a stable interface for reading and writing memory, without exposing storage internals.

As with other modules, the memory subsystem currently exposes its service boundary and types; storage backend and retrieval strategy are implemented incrementally.

---

## Learning Architecture

The `learning` module is responsible for personalized, adaptive learning support.

Planned responsibilities:

- Tracking learning progress and topics across sessions.
- Identifying weak areas based on interaction patterns and explicit feedback.
- Structuring learning content and study plans tailored to the individual user.
- Coordinating with `memory` (for context) and `analytics` (for progress signals).

This module is scaffolded with explicit contracts; the adaptive logic itself is planned, incremental work.

---

## Analytics

The `analytics` module aggregates usage and behavioral signals across the platform to support:

- Understanding how learning and productivity features are actually used.
- Feeding progress signals back into the `learning` module.
- Surfacing patterns that could inform future personalization.

Analytics processing is designed to run partly through background jobs (see [Background Jobs](#background-jobs)) rather than blocking the request/response cycle.

---

## Getting Started

### Prerequisites

- Node.js 20 or later
- npm 10 or later
- (Optional, for AI features) [Ollama](https://ollama.com) running locally

### Installation

Clone the repository, install dependencies, and configure your environment:

```bash
npm install
cp apps/backend/.env.example apps/backend/.env
```

Edit `apps/backend/.env` and fill in real values for your local setup.

### Running the backend

```bash
npm run dev
```

This starts the backend in watch mode via the root `dev` script, which delegates to the `apps/backend` workspace.

---

## Environment Configuration

Environment variables are centralized and validated in `apps/backend/src/config/env.ts`. Missing critical configuration fails fast at startup in production rather than surfacing as an unclear runtime error later.

| Variable | Description | Required |
|---|---|---|
| `NODE_ENV` | `development` \| `production` \| `test` | No (defaults to `development`) |
| `PORT` | HTTP port for the backend | No (defaults to `4000`) |
| `DATABASE_URL` | Database connection string | Yes, in production |
| `REDIS_URL` | Redis connection string | No |
| `JWT_SECRET` | Secret used for signing auth tokens | Yes, in production |
| `OLLAMA_BASE_URL` | Base URL for the Ollama server | No (defaults to `http://localhost:11434`) |
| `OLLAMA_API_KEY` | API key for Ollama, if required | No |
| `OLLAMA_MODEL` | Model name to use | No (defaults to `llama3`) |

See `apps/backend/.env.example` for the full template. **Never commit a real `.env` file.**

---

## Available Scripts

Run from the repository root, these fan out to workspaces where applicable:

| Script | Description |
|---|---|
| `npm run dev` | Start the backend in watch mode |
| `npm run build` | Build all workspaces |
| `npm run typecheck` | Type-check all workspaces |
| `npm run lint` | Lint all workspaces |
| `npm run test` | Run tests across all workspaces |
| `npm run clean` | Remove build output across all workspaces |

The backend workspace (`apps/backend`) exposes the same lifecycle scripts individually.

---

## Database

Database access is intentionally not tied to a specific ORM yet. `apps/backend/src/database/` provides:

- `index.ts` — connection lifecycle interface (`connect`, `disconnect`, `isConnected`)
- `schema/` — schema definition scaffolding
- `migrations/` — migration scaffolding
- `seed/` — seed data scaffolding

`src/config/database.ts` centralizes connection configuration (connection string, SSL policy by environment).

---

## Background Jobs

`apps/backend/src/jobs/` provides typed scaffolding for asynchronous workloads that will eventually run outside the request/response cycle, such as file processing, notification delivery, analytics aggregation, AI processing, and memory processing.

- `queues/` — queue definitions
- `workers/` — worker process scaffolding

Redis (`src/config/redis.ts`) is the shared infrastructure boundary expected to back caching, sessions, rate limiting, and these queues.

---

## Testing

```text
apps/backend/tests/
├── unit/
├── integration/
└── e2e/
```

Run all tests with:

```bash
npm run test
```

A basic health-check test is included as a compatibility baseline — no test in this repository claims functionality that hasn't been implemented.

---

## Docker

Development-oriented Docker configuration lives in `infrastructure/docker/`:

- `Dockerfile.backend` — backend container build
- `docker-compose.yml` — local multi-service orchestration (backend + supporting infrastructure)

Reverse-proxy configuration is scaffolded in `infrastructure/nginx/nginx.conf`. `infrastructure/deployment/` holds deployment scaffolding — **this is not production-ready deployment infrastructure** and should be reviewed and hardened before real-world use.

---

## Security & Privacy

- No real API keys, passwords, tokens, or credentials are ever committed to this repository.
- `.env` files are gitignored; only `.env.example` templates are tracked.
- Secrets are never written to logs, including within the Ollama provider.
- External input is validated at module boundaries (`*.schema.ts`).
- Error responses avoid leaking stack traces or internal details in production.
- This project has **not** undergone a formal security review and should not be used with real user data in its current state. See [`SECURITY.md`](./SECURITY.md) for the responsible disclosure process.

---

## Development Status

This is an early architectural scaffold. The structure, contracts, and extension points are established; most domain logic is not yet implemented.

| Area | Status |
|---|---|
| Monorepo structure | ✅ Established |
| Backend app/server lifecycle | ✅ Established |
| AI provider abstraction | ✅ Established |
| Ollama provider | 🟡 Baseline implementation |
| Domain module contracts | ✅ Established |
| Domain business logic | 🔴 Not yet implemented |
| Database/ORM selection | 🔴 Not yet decided |
| Background jobs | 🟡 Scaffolded only |
| Frontend | 🔴 Placeholder only |
| Security review | 🔴 Not started |

---

## Roadmap

- [ ] Select and wire in a concrete database client/ORM
- [ ] Implement domain logic in each module's `.service.ts`
- [ ] Enforce authentication/authorization in `core/middleware/`
- [ ] Implement queue/worker logic in `src/jobs/`
- [ ] Add a second AI provider to validate provider-interface extensibility
- [ ] Build out `packages/api-client` as the frontend workspace comes online
- [ ] Formal security review before any real-user deployment

---

## Development Workflow

1. Fork and branch from `main`.
2. Keep changes scoped to a single module or concern.
3. Run `npm run typecheck && npm run lint && npm run test` before opening a PR.
4. Document architectural decisions in code comments where they aren't obvious.
5. Do not commit `.env` files or secrets.

---

## Contributing

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for full guidelines on branching, scope, and pre-PR checks.

---

## Team

NeuroNest is designed and developed by **Jeevan M**.

---

## License

See [`LICENSE`](./LICENSE).