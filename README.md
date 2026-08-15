# NeuroNest

**An AI-powered academic and learning intelligence platform designed to help learners understand, organize, plan, and continuously improve their learning journey.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)
[![Status](https://img.shields.io/badge/status-early--stage%20active%20development-orange)](#development-status)
[![Node](https://img.shields.io/badge/node-20%2B-339933)](#getting-started)
[![TypeScript](https://img.shields.io/badge/TypeScript-Monorepo-3178C6)](#technology-stack)

---

## 📖 Overview

NeuroNest is built for **learners**, not just students in the traditional sense. It is designed to adapt across school education, higher education, competitive examinations, professional certifications, self-directed learning, and lifelong learning.

Most tools around learning — calendars, to-do lists, grade trackers, chatbots — tell you **what exists**. They show a list of assignments, a set of deadlines, a table of scores. What they don't do is help you understand what any of it *means* for you, right now.

NeuroNest is built around a different question: given everything a learner is working on, what actually matters most, and what should happen next?

The product is designed as an intelligent layer around a learner's existing academic and learning information — not a replacement for how they already work, but a system that understands context and turns it into direction.

**Core philosophy:**

```
INPUT → UNDERSTAND → PLAN → ACT → MEASURE → IMPROVE
```

NeuroNest is intended to continuously learn from a learner's context — their workload, goals, progress, and performance — and use that understanding to guide better decisions over time.

---

## 🎯 Who NeuroNest Is For

NeuroNest is designed to adapt across different learning environments, including:

- **School learners** — managing homework, assignments, exams, and revision
- **University learners** — managing coursework, projects, assessments, and study schedules
- **Competitive-exam learners** — managing preparation plans and structured revision
- **Professional learners** — managing certifications and structured upskilling
- **Self-directed learners** — managing personal learning goals
- **Lifelong learners** — building knowledge and skills outside formal structures

NeuroNest is not exclusively school-oriented — it is designed to grow with the learner as their learning context changes.

---

## 🧭 Core Objectives

| Objective | What It Means |
|---|---|
| **Understand** | Build context around a learner's goals, workload, and performance |
| **Organize** | Bring learning activities, assessments, and schedules into one coherent structure |
| **Prioritize** | Determine what actually matters most, right now |
| **Assist** | Provide learning support that goes beyond simple Q&A |
| **Improve** | Turn performance data into concrete, actionable next steps |

---

## ✨ Core Features

### 📚 Learning & Academic Management

NeuroNest treats academic and learning work as a set of **configurable learning activities** — not every learner will use every type, and the system is designed to flex around whichever apply.

Supported activity types include:

- Homework
- Classwork
- Assignments
- Projects
- Coursework
- Practical / lab work
- Exams
- Tests
- Revision
- Research
- Practice
- Learning goals

Each learning task can carry:

- Subject or learning area
- Task type
- Description
- Due date
- Priority
- Estimated time
- Completion status
- Associated assessment
- Notes
- Resources

### 🗓️ Intelligent Planner

NeuroNest's planner is designed to go beyond a static calendar. It can consider:

- Deadlines
- Available time
- Estimated workload
- Priority
- Upcoming assessments
- Learning difficulty
- Previous performance
- Current workload
- Long-term goals

...and generate a prioritized action plan rather than a flat list of dates.

**Example:**

```
Input:
- Physics test in 3 days (weak topic: thermodynamics)
- History essay due in 5 days (on track)
- Math practice set due tomorrow (light workload)

Generated Plan:
1. Today    → Finish math practice set (30 min, low effort)
2. Today    → Begin thermodynamics revision (60 min, high priority)
3. Tomorrow → Continue thermodynamics + practice problems
4. Day 3    → Final physics review before test
5. Day 4–5  → History essay drafting and review
```

### 🤖 AI Learning Assistant

The AI is designed to function as a **learning copilot, not merely a chatbot**.

It can help with:

- Concept explanations
- Different explanation levels (beginner → advanced)
- Answering questions
- Worked examples
- Practice questions
- Revision material
- Mistake analysis
- Summaries
- Exam preparation
- Personalized learning plans

### 📈 Performance Intelligence

NeuroNest is designed to analyze:

- Assessment scores
- Performance trends
- Learning progress
- Improvement rate
- Weak areas
- Strong areas
- Topic-level performance
- Historical performance

**Example — from raw data to actionable insight:**

```
Raw data:
- Algebra: 92%, 88%, 95% (last 3 tests)
- Geometry: 61%, 58%, 65% (last 3 tests)

Insight:
Geometry performance is consistently ~30 points below Algebra
and shows a slower improvement rate.

Recommendation:
Increase geometry practice frequency and revisit foundational
concepts before the next assessment.
```

### 🧩 Personalized Learning

NeuroNest is designed to build an evolving understanding of a learner's:

- Goals
- Strengths
- Weaknesses
- Learning progress
- Workload
- Study patterns
- Completed work
- Unfinished work
- Upcoming assessments

This context is intended to improve the relevance of future recommendations over time.

### 🔔 Proactive Intelligence

This is a core differentiator for NeuroNest.

> **The learner shouldn't always have to ask.**

NeuroNest is designed to proactively surface relevant information and recommend action when it detects situations such as:

- An upcoming deadline
- Multiple assessments clustered close together
- Increasing overall workload
- Repeated weak performance in a topic
- Insufficient preparation time before an assessment
- A large unfinished workload

### 📅 Schedule Awareness

Schedules and calendars are treated as an input to planning, not restricted to school timetables. This can include:

- Classes
- Lectures
- Labs
- Study sessions
- Exams
- Work commitments
- Personal availability

### 🎯 Goal Management

NeuroNest is designed to connect long-term ambition to daily execution:

```
LONG-TERM GOAL
      ↓
MONTHLY TARGET
      ↓
WEEKLY TARGET
      ↓
TODAY'S ACTION
```

---

## 🔁 The NeuroNest Intelligence Loop

This feedback loop is central to the product vision:

```
Learner
   ↓
Goals
   ↓
Context
   ↓
Workload
   ↓
Learning
   ↓
Performance
   ↓
Insights
   ↓
Recommendations
   ↓
Action
   ↓
Improvement
   └──────────────→ Context
```

Every action a learner takes feeds back into their context, refining future insights and recommendations.

---

## 🌟 What Makes NeuroNest Different

NeuroNest combines several things that typically exist as separate tools into one system:

- Academic management
- Learning assistance
- Context awareness
- Planning
- Productivity
- Performance intelligence
- Personalized recommendations
- Proactive intelligence

The goal is ambitious but grounded: not to be the only tool a learner ever uses, but to be the intelligence layer that makes their existing effort more effective.

---

## 🏗️ System Architecture

NeuroNest is built as a modular **TypeScript/Node.js monorepo**.

```
Learner Client
      ↓
Backend API
      ↓
Domain Modules
      ↓
Services
      ↓
Infrastructure
 ┌────┼────┬──────┐
 DB  Redis  AI   Jobs
      ↓
Context / Intelligence
      ↓
Actionable Output
```

The backend is structured as a **modular monolith**: a single deployable service organized into clearly bounded domain modules, rather than a tangle of shared logic.

---

## ⚙️ Backend Architecture

Current backend modules:

| Module | Responsibility |
|---|---|
| `auth` | Authentication and session handling |
| `users` | User accounts and profiles |
| `ai` | AI provider integration and orchestration |
| `conversations` | AI conversation handling |
| `memory` | Learner context and memory management |
| `learning` | Learning tasks, subjects, and activities |
| `tasks` | Task and workload management |
| `analytics` | Performance and progress analytics |
| `notifications` | Proactive alerts and reminders |
| `files` | File storage and processing |

Each module is organized around:

- **Controllers** — handle incoming requests and responses
- **Services** — contain business logic
- **Routes** — define API endpoints
- **Types** — shared type definitions
- **Schemas** — validation, where applicable

### Application vs. Server

The backend separates application construction from process lifecycle:

**`app.ts`**
- Express application construction
- Middleware registration
- Security configuration
- Routing
- Error handling

**`server.ts`**
- HTTP server startup
- Process lifecycle management
- Graceful shutdown
- Signal handling

---

## 🧠 AI Provider Architecture

NeuroNest uses a **provider-agnostic AI architecture**, built around dependency inversion:

```
AI Controller
      ↓
AI Service
      ↓
AI Provider Interface
      ↓
Ollama Provider
```

The AI service depends on a **provider interface**, not directly on any specific AI provider implementation. This allows additional providers to be integrated in the future without rewriting the application layer.

**Current AI provider:** [Ollama](https://ollama.com)

Current provider architecture includes:

- A defined provider interface
- An Ollama implementation of that interface
- Provider-specific configuration through environment variables

> No additional providers are implemented at this time — the abstraction exists to make adding them straightforward later.

---

## 📁 Repository Structure

```
NeuroNest/
├── apps/
│   ├── backend/
│   │   ├── src/
│   │   │   ├── config/
│   │   │   ├── core/
│   │   │   ├── database/
│   │   │   ├── jobs/
│   │   │   ├── modules/
│   │   │   └── routes/
│   │   └── tests/
│   │       ├── unit/
│   │       ├── integration/
│   │       └── e2e/
│   └── frontend/
├── packages/
│   ├── api-client/
│   └── shared/
├── infrastructure/
│   ├── deployment/
│   ├── docker/
│   └── nginx/
├── docs/
├── scripts/
├── .github/
├── package.json
├── README.md
├── LICENSE
├── SECURITY.md
├── CONTRIBUTING.md
└── CODE_OF_CONDUCT.md
```

- **`apps/backend`** — the Express/TypeScript API and domain modules
- **`apps/frontend`** — the learner-facing client application
- **`packages/api-client`** — shared client for calling the backend API
- **`packages/shared`** — shared types and utilities across apps
- **`infrastructure`** — Docker, Nginx, and deployment configuration
- **`docs`** — project documentation
- **`scripts`** — repository tooling and automation

---

## 🛠️ Technology Stack

**Backend**
- TypeScript
- Node.js
- Express
- REST API
- npm workspaces

**AI**
- Ollama
- Large Language Models
- Provider abstraction layer

**Infrastructure**
- Redis
- Docker
- Nginx

**Development**
- Git / GitHub
- Visual Studio Code
- GitHub Actions

> The stack above reflects the current foundation. Some pieces are scaffolded and under active development rather than fully implemented — see [Development Status](#development-status).

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+
- npm 10+
- Git

**Optional:**
- [Ollama](https://ollama.com) (for local AI provider functionality)

### Installation

```
npm install
```

### Environment Setup

```
cp apps/backend/.env.example apps/backend/.env
```

> ⚠️ Never commit real credentials or secrets. Always keep `.env` files out of version control.

### Development

```
npm run dev
```

### Available Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start the development environment |
| `npm run build` | Build all workspaces |
| `npm run typecheck` | Run TypeScript type checking |
| `npm run lint` | Run linting |
| `npm run test` | Run test suites |
| `npm run clean` | Clean build artifacts |

---

## 🔐 Environment Configuration

| Variable | Description | Notes |
|---|---|---|
| `NODE_ENV` | Application environment | `development` / `production` |
| `PORT` | Backend server port | Has a safe default |
| `DATABASE_URL` | Database connection string | Required in production |
| `REDIS_URL` | Redis connection string | Required for caching/jobs |
| `JWT_SECRET` | Secret for signing auth tokens | **Production-required secret** |
| `OLLAMA_BASE_URL` | Base URL for the Ollama service | Optional if AI features unused |
| `OLLAMA_API_KEY` | API key for Ollama, if applicable | Optional |
| `OLLAMA_MODEL` | Model identifier used by the AI provider | Optional, has a default |
| `CORS_ORIGIN` | Allowed origin(s) for CORS | Required in production |
| `MAX_FILE_SIZE_MB` | Maximum upload size | Has a safe default |
| `LOG_LEVEL` | Logging verbosity | Has a safe default |

> **Real credentials must never be committed to the repository.** Use `.env` files locally and a proper secrets manager in production.

---

## 🗄️ Database

The database layer is currently **intentionally abstract and scaffolded**. No final ORM or database provider has been locked in yet.

```
apps/backend/src/database/
├── connection boundary
├── schema/
├── migrations/
└── seeds/
```

This abstraction allows the concrete database implementation to evolve without restructuring the domain layer above it.

---

## ⏱️ Background Jobs

```
apps/backend/src/jobs/
├── queues/
└── workers/
```

Potential future workloads for the job system include:

- Notifications
- File processing
- Analytics aggregation
- AI processing
- Memory processing

> Workers are part of the planned architecture and are not fully implemented yet.

---

## 🧪 Testing

```
apps/backend/tests/
├── unit/          # Isolated logic and function-level tests
├── integration/   # Module and service interaction tests
└── e2e/           # End-to-end API behavior tests
```

> The project does not yet have comprehensive test coverage — the structure above reflects the intended testing strategy as the codebase matures.

---

## 🐳 Docker

```
infrastructure/docker/
├── Dockerfile.backend
└── docker-compose.yml
```

Nginx configuration is available at:

```
infrastructure/nginx/nginx.conf
```

> Current infrastructure is development/scaffolding-oriented and is not automatically production-hardened.

---

## 🔒 Security

NeuroNest's security approach covers:

- Secret management
- Authentication
- Authorization
- Input validation
- Secure API communication
- Least privilege access
- Learner data protection

For details and reporting a vulnerability, see [SECURITY.md](./SECURITY.md).

> No formal security audit has been conducted at this stage.

---

## 📌 Development Status

NeuroNest is currently an **early-stage, actively developed project**.

The repository currently establishes:

- Monorepo structure
- Backend architecture
- Domain module boundaries
- AI provider abstraction
- Infrastructure scaffolding
- Development tooling

Many business features described in this README are **planned or in progress**, not yet fully implemented. This document aims to reflect that distinction accurately.

---

## 🗺️ Roadmap

> Roadmap items are planned and subject to change as the project evolves.

### Phase 1 — Foundation
- Repository and monorepo setup
- Backend foundation
- Configuration system
- Database boundary
- Authentication foundation
- AI provider abstraction

### Phase 2 — Learning Infrastructure
- Learning tasks
- Subjects / learning areas
- Assessments
- Schedule
- Goals
- Performance records

### Phase 3 — Intelligence
- AI assistant
- Context engine
- Memory
- Task prioritization
- Intelligent planning
- Performance intelligence

### Phase 4 — Proactive Intelligence
- Workload analysis
- Risk detection
- Personalized recommendations
- Adaptive planning
- Automated revision planning

### Phase 5 — Ecosystem
- Web application
- Mobile applications
- Cross-device synchronization
- Integrations
- Advanced analytics

---

## 🤝 Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before submitting anything.

This covers expectations around:

- Opening issues
- Proposing features
- Submitting pull requests
- Writing and running tests
- Documentation standards
- Maintaining architectural consistency

---

## 📄 License

NeuroNest is licensed under the **Apache License 2.0**.

See [LICENSE](./LICENSE).

---

## 💡 Philosophy

Learners don't need more information. They need better intelligence around the information they already have.

```
Understand.
Organize.
Prioritize.
Learn.
Improve.
```
