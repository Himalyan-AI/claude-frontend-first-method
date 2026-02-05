---
name: ffm
description: This skill should be used when the user asks to "start a new project", "build an app", "create an application", "use frontend first methodology", "use FFM", "plan a product", "prevent frontend backend drift", or mentions needing a structured development process. Implements the Frontend First Methodology - an API-spec-driven development process that perfects UX with mocked backends before building real ones, preventing frontend-backend drift and eliminating costly rework.
version: 0.1.0
license: MIT
---

# Frontend First Methodology (FFM)

A stack-agnostic development methodology that uses the **API specification as a central contract** between frontend and backend. Perfect UX cheaply with mocked backends, then build the real backend once — to spec — with zero drift.

## Why FFM Exists

Traditional full-stack iteration is expensive. Change a UX flow and the backend changes too. Rebuild the backend and the frontend breaks. Every iteration touches everything. FFM breaks this cycle:

- **UX iteration with mocks costs ~1/5th** of full-stack iteration (no backend rebuilds, no data migrations, no API versioning)
- **The API spec is the contract** — frontend mocks FROM it, backend implements TO it. Drift is structurally impossible when both sides reference the same spec
- **Human feedback happens when changes are cheap** — iterate UX before committing to infrastructure

## Core Principles

### 1. The API Spec as Central Contract

The API specification (e.g., OpenAPI v3) is the **single source of truth**:

1. **Frontend** mocks all backend interactions from the spec
2. **Backend** implements endpoints to match the spec exactly
3. **Tests** validate both sides against the spec
4. **Drift detection** = spec validation. If frontend expects X and backend returns Y, the spec catches it

Generate the initial API spec during story creation. Refine it during UX iteration. Freeze it before backend development begins.

### 2. Docs-First, Code-Second

**Always update `/ffm-docs/` before changing code.** Documentation is the source of truth, not a byproduct of implementation.

- Before building a feature → update the user story in `/ffm-docs/STORIES/` first
- Before changing an API → update the API spec first
- Before fixing a bug → document the issue in `/ffm-docs/PRD/UserFeedback/` first
- Before every BTRI loop → update `/ffm-docs/current-status.md` first

This ensures Claude always has accurate context. The docs describe intent. The code implements it. If they disagree, the docs win and the code gets fixed.

### 3. Test Aggressively, Relax Deliberately

FFM defaults to **aggressive testing**. Every UX state is a test case. Coverage targets are high from the start. The user can always relax these during the PRD interview — but the methodology assumes quality matters until told otherwise.

For every user story, generate tests covering ALL states:
- **Loading**: Skeleton screens, spinners, disabled inputs during fetch
- **Success**: Correct data rendering, formatting, layout
- **Empty**: Zero-data states, first-run experience, cleared filters
- **Error**: Network failures, validation errors, server errors, timeouts
- **Edge cases**: Boundary values, long strings, special characters, concurrent actions
- **Permissions**: Unauthorized access, partial permissions, role-based UI differences

Unit tests cover every component with meaningful logic. Integration tests validate API contract compliance. E2E tests cover every user path through every state.

## Best Suited For

- **Enterprise development** where quality gates, traceability, and documented decisions are non-negotiable
- **v2/v3 rewrites** where the team learned that frontend-backend drift killed the last version and needs a methodology to prevent it structurally
- **AI-assisted development at scale** where token efficiency is budget — dragging the full stack through every UX iteration is unaffordable
- Can be relaxed for prototypes and hackathons by adjusting test gates during the PRD interview

## Two Entry Paths

### Greenfield (New Project)
Start at Phase 1: Interview → PRD → Stories → Build with mocks → Perfect UX → Build backend.

### Adopt (Existing Project)
1. Assess the current codebase — identify frontend, backend, and API boundaries
2. Generate an API spec from the existing backend (reverse-engineer from routes/controllers)
3. Identify drift between what the frontend expects and what the backend provides
4. Create stories to resolve drift and align both sides to the spec
5. Enter the FFM loop at the appropriate phase

## The BTRI Loop (Build → Test → Review → Integrate)

This is the reusable iteration cycle invoked at every phase. Each cycle tightens quality:

### Build
- **Update `/ffm-docs/` first** — stories, specs, and status BEFORE writing code
- Implement stories for the current phase
- Frontend uses mocked backend (early phases) or real backend (later phases)
- Generate/update artifacts per the document taxonomy (see `references/doc-taxonomy.md`)

### Test
- Run E2E tests against the real frontend + mock/real backend, covering **all UX states** (loading, success, empty, error, edge cases, permissions)
- Run unit tests for all components with meaningful logic — aim for configured coverage target
- Validate API interactions against the spec
- **Test gate**: Must meet the configured pass rate AND coverage target for the current phase before proceeding

### Adversarial Self-Review
- Adopt an adversarial stance and critique the code just built
- Assess: maintainability, modularity, scalability, security, deployability, test coverage
- Document findings with quantitative metrics in `/ffm-docs/ADVERSARIAL/`
- See `references/adversarial-review-guide.md` for the full checklist

### Human Review (with Structured Interview)
- Interview the user about the current phase's output using the AskUserQuestion tool
- Cover: epic progress, adversarial review findings, UX satisfaction, stack concerns, and edge cases
- Proceed question by question with multiple-choice options for non-obvious decisions
- Document feedback in `/ffm-docs/PRD/UserFeedback/`
- See `references/interview-questions.md` for structured templates per phase

### Integrate
- Analyze feedback from review + adversarial findings
- **Update docs first**: Generate PRD revision if needed (increment version), update stories, update status
- Create new/updated stories and epics to address issues — document them BEFORE implementing
- Loop back to Build if changes are required

## Major Phases

### Phase 1 — UX First (Mocked Backend)

**Goal**: A fully functional, demo-able frontend with perfect UX. Zero backend code.

1. **Interview** the user to refine the PRD (see `references/interview-questions.md` — PRD section)
2. **Decompose** PRD into epics and atomic user stories covering every UX state
3. **Generate** the API specification from story requirements
4. **Set up** mock backend from the API spec (any spec-compatible mocking tool)
5. **Build** the frontend against mocks — every interaction, loading state, error state, empty state, edge case, permission state
6. **Run BTRI loop** — iterate until user confirms UX satisfaction
   - Test gate: configurable (default 90% E2E pass / 80% unit coverage, tightening to 95%/85% per loop)
   - Every user story produces tests for ALL UX states, not just the happy path
   - Interview focus: UX flow, feature completeness, intuitiveness
7. **Freeze the API spec** once UX is approved — this becomes the backend contract

**What you're NOT spending tokens on**: Backend logic, database schemas, auth flows, deployment config. All of that comes later, built once to a frozen spec.

### Phase 2 — Backend to Spec

**Goal**: Real backend that passes all existing E2E tests without frontend changes.

1. **Interview** the user about backend preferences: hosting, auth, storage, external APIs
2. **Generate** backend architecture and low-level design docs from the frozen API spec
3. **Build** real backend endpoints matching the spec exactly
4. **Provide** a simple toggle between mock and real backend (e.g., environment variable) for debugging
5. **Run BTRI loop** — all existing frontend E2E tests must pass against the real backend
   - Test gate: configurable (default 95% E2E pass / 90% unit coverage)
   - Interview focus: backend reliability, performance, security
6. **Validate** zero drift: every API response matches the spec the frontend was built against

**What you're NOT rebuilding**: The frontend. It already works. The spec guarantees compatibility.

### Phase 3 — Harden for Deployment

**Goal**: Production-ready system passing UAT.

1. **Deploy** to a UAT environment mimicking production
2. **Run** structured acceptance testing with representative user scenarios
3. **Run BTRI loop** with hardened gates
   - Test gate: configurable (default 98% E2E pass, 90%+ unit coverage)
   - Interview focus: UAT results, edge cases, performance under load
4. **Deploy** to staging — full regression, load testing, security audit
5. **Final BTRI loop** — comprehensive adversarial review + human sign-off

### Phase 4 — Production and Monitor

**Goal**: Safe rollout with monitoring.

1. **Deploy** with rollback procedures (canary, blue-green, or as appropriate)
2. **Set up** monitoring for KPIs, error rates, and performance
3. **Monitor** for 30 days minimum — document findings
4. **Generate** hotfix stories as needed, following the BTRI loop

## Configurable Test Gates (Aggressive Defaults)

During the PRD interview, present these defaults and let the user confirm or relax:

| Phase | Default E2E Pass | Default Unit Coverage | Purpose |
|-------|------------------|-----------------------|---------|
| UX Iteration (Phase 1, first loop) | 90% | 80% | Prove UX works before human review |
| UX Iteration (Phase 1, subsequent) | 95% | 85% | Each iteration tightens, never loosens |
| Backend Build (Phase 2) | 95% | 90% | Backend must match spec with high confidence |
| UAT (Phase 3) | 98% | 90% | Production-grade reliability |
| Production (Phase 4) | 98% | 90% | Maintained post-launch |

**These are defaults, not mandates.** A rapid prototype might relax to 70%/60%. A regulated product might push to 100%/95%. Always ask during the PRD interview. The methodology starts strict — the user opts out of coverage, not in.

## Document Taxonomy

All documentation follows the `ff-m-tag` structure under `/ffm-docs/`. Full specification in `references/doc-taxonomy.md`. Core folders:

- `/ffm-docs/current-status.md` — 50-line phase/epic summary, always current
- `/ffm-docs/index.md` — Central index linking all documents
- `/ffm-docs/PRD/` — Refined PRD with versions, user feedback
- `/ffm-docs/TECH/` — Architecture, deployment docs
- `/ffm-docs/LLD/` — Low-level design per phase with 2-line reasoning per element
- `/ffm-docs/STORIES/` — Atomic user stories with frontend, backend, and E2E acceptance criteria
- `/ffm-docs/EPICS/` — Short epics with checklists linking to stories
- `/ffm-docs/ARTIFACTS/` — Phase-specific artifacts (diagrams, specs, results)
- `/ffm-docs/ADVERSARIAL/` — Adversarial review documents with metrics

## Token Efficiency Design

FFM is structured to minimize wasted work in AI-assisted development:

| Phase | What tokens buy | What tokens DON'T buy |
|-------|----------------|----------------------|
| Phase 1 (UX) | UI components, interactions, UX flows, mock data | Backend logic, DB schemas, auth, infra |
| Phase 2 (Backend) | API implementation to frozen spec | Frontend rewrites (it already works) |
| Phase 3 (Harden) | Test hardening, edge cases, security | Architectural rework (spec prevents drift) |

Every UX change in Phase 1 is cheap. The same change after Phase 2 would require backend updates, data migration, and re-testing. **FFM front-loads decisions to when changes cost the least.**
