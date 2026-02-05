<h1 align="center">Frontend First Methodology</h1>

<p align="center">
  <strong>A Claude Code skill that eliminates frontend-backend drift</strong>
</p>

<p align="center">
  <a href="#install">Install</a>&ensp;&bull;&ensp;
  <a href="#invoke">Invoke</a>&ensp;&bull;&ensp;
  <a href="#the-core-idea">Core Idea</a>&ensp;&bull;&ensp;
  <a href="#how-it-works">How It Works</a>&ensp;&bull;&ensp;
  <a href="#token-efficiency">Token Efficiency</a>&ensp;&bull;&ensp;
  <a href="#design-principles">Principles</a>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="skills/ffm/SKILL.md"><img src="https://img.shields.io/badge/version-0.1.0-green.svg" alt="Version"></a>
  <a href="https://claude.ai/claude-code"><img src="https://img.shields.io/badge/built_for-Claude_Code-blueviolet.svg" alt="Claude Code"></a>
</p>

---

## Install

**One-liner** (copy skill directly):

```bash
git clone https://github.com/Himalyan-AI/claude-frontend-first-method.git ~/.claude/skills/ffm-repo && \
  cp -r ~/.claude/skills/ffm-repo/skills/ffm ~/.claude/skills/ffm
```

**Or manually:**

```bash
git clone https://github.com/Himalyan-AI/claude-frontend-first-method.git
cp -r claude-frontend-first-method/skills/ffm ~/.claude/skills/ffm
```

Restart Claude Code. Done.

## Invoke

In any Claude Code session:

```
> /ffm
```

Or just say it naturally:

- *"Start a new project using FFM"*
- *"Use the frontend first methodology"*
- *"Build an app with FFM"*
- *"I want to prevent frontend backend drift"*

Claude will begin with a structured interview to understand your project, then drive the full methodology.

---

## The Core Idea

```
  Traditional                        FFM
  ───────────                        ───

  Frontend ←→ Backend            Frontend ←→ API Spec ←→ Backend
  (build both, hope they agree)  (spec is the contract, drift = 0)

  UX change → rebuild backend    UX change → update spec + mocks
  Backend change → fix frontend  Backend builds TO the frozen spec
  Iterate everything together    Iterate UX cheap, build backend once
```

The **API specification** is the single source of truth. Frontend mocks from it. Backend implements to it. Both sides reference the same contract. Drift is structurally impossible.

## Who This is For

**Enterprise teams** — where quality gates, traceability, and documented decisions are non-negotiable.

**Teams building v2 or v3** — who learned that frontend-backend drift killed the last version and need a methodology that prevents it structurally this time.

**AI-assisted development at scale** — where token efficiency is budget. Every UX iteration costs real money. You can't afford to drag the full stack along for every design change.

Can be relaxed for prototypes by adjusting test gates during the PRD interview.

---

## How It Works

Four phases. One repeating loop.

### The BTRI Loop

Every phase uses the same cycle: **Build → Test → Review → Integrate**

| Step | What happens |
|------|-------------|
| **Build** | Update `/ffm-docs/` first, then implement stories |
| **Test** | E2E + unit tests covering all UX states against configured gates |
| **Review** | Adversarial self-review (6 dimensions) + structured user interview |
| **Integrate** | Analyze feedback, revise docs, create stories for fixes, loop if needed |

### The Phases

**Phase 1 — Perfect the UX** (mocked backend)
Build a fully working frontend against API mocks. Iterate with interviews and adversarial reviews until every UX flow is right. No backend code written. Changes are fast and cheap.

**Phase 2 — Build Backend to Spec**
Freeze the API spec. Build the real backend to match it. All existing E2E tests must pass without touching the frontend. If they don't — the backend has a bug.

**Phase 3 — Harden for Deployment**
UAT, staging, load testing, security audits. Progressive quality gates tighten toward production.

**Phase 4 — Ship and Monitor**
Production rollout with rollback procedures, monitoring, and 30-day observation.

---

## Token Efficiency

FFM spends tokens where changes are cheapest:

| Phase | Tokens buy | NOT rebuilding |
|-------|-----------|---------------|
| UX (mocks) | UI, interactions, flows, states | Backend, DB, auth, infra |
| Backend (to spec) | API implementation | Frontend (already works) |
| Hardening | Edge cases, security, perf | Architecture (spec prevented drift) |

### vs. Specify-Everything Approaches

Most methodologies specify the entire stack upfront, then build everything. When UX changes (it always does), the cascade hits every layer:

```
Specify-Everything:
  spec full stack → plan → tasks → implement
  UX changes → re-spec → re-plan → re-task → re-implement  ← cascade

FFM:
  spec UX only → build UX + mocks → iterate cheap → freeze
  UX changes → update mock + rebuild UI only                ← contained
  backend built ONCE to frozen spec                         ← no rework
```

**The math:**
- Specify-everything: `(full spec + plan + tasks + build) * UX_iterations`
- FFM: `(UX spec + UI build) * UX_iterations + (backend spec + build) * 1`

3-5 UX iterations is normal. FFM spends backend tokens exactly once.

### Why This Hits Harder for Enterprise and v2/v3

- UX requirements shift during stakeholder reviews — each shift cascades through a full-stack spec
- Large API surfaces are interconnected — one endpoint shape change can break dozens of components
- Quality gates are mandatory — FFM's `/ffm-docs/` taxonomy provides the audit trail by design
- Budget is a line item — 3-5x token savings is the difference between finishing and running out

---

## Three Core Principles

### 1. API Spec as Central Contract

The spec is the single artifact both sides must agree on. Frontend mocks FROM it. Backend implements TO it. Tests validate against it. Drift detection = spec validation.

### 2. Docs-First, Code-Second

**Update `/ffm-docs/` before changing code. Always.**

```
Traditional:       Code first  → docs rot    → context lost
Spec-everything:   Spec first  → specs drift → same problem
FFM:               Docs first  → code second → docs ARE the truth
```

| Before you... | First update... |
|--------------|----------------|
| Build a feature | User story in `/ffm-docs/STORIES/` |
| Change an API | API spec in `/ffm-docs/ARTIFACTS/` |
| Fix a bug | Feedback in `/ffm-docs/PRD/UserFeedback/` |
| Start a BTRI loop | Status in `/ffm-docs/current-status.md` |

When Claude reads the docs, it has ground truth. No stale comments. No code that contradicts the spec.

### 3. Test Aggressively, Relax Deliberately

High defaults. The user opts *out*, not in.

**Every user story produces tests for all states:**

| State | Tested |
|-------|--------|
| Loading | Skeletons, spinners, disabled inputs |
| Success | Data rendering, formatting, layout |
| Empty | Zero-data, first-run, cleared filters |
| Error | Network, validation, server, timeout |
| Edge | Boundaries, long strings, concurrency |
| Permissions | Unauthorized, partial, role-based |

**Default quality gates:**

| Phase | E2E Pass | Unit Coverage |
|-------|----------|--------------|
| Phase 1 (first loop) | 90% | 80% |
| Phase 1 (subsequent) | 95% | 85% |
| Phase 2 (backend) | 95% | 90% |
| Phase 3 (UAT) | 98% | 90% |
| Phase 4 (production) | 98% | 90% |

Adjust during the PRD interview. Prototype? Drop to 70/60. Medical device? Push to 100/95.

---

## Document Taxonomy

FFM creates `/ffm-docs/` in your project — the single source of truth:

```
/ffm-docs/
├── current-status.md       50-line phase summary, always current
├── index.md                Links to everything
├── PRD/                    Product requirements + user feedback
├── TECH/                   Architecture + deployment
├── LLD/                    Low-level design per phase
├── STORIES/                Atomic user stories (frontend + backend + E2E)
├── EPICS/                  Phase-scoped work packages
├── ARTIFACTS/              Specs, diagrams, test results, UAT reports
└── ADVERSARIAL/            Self-review documents with metrics
```

## Adversarial Self-Review

At every review gate, Claude critiques its own code across six dimensions:

| Dimension | Catches |
|-----------|---------|
| Maintainability | Unreadable code, dead code, poor naming |
| Modularity | Tight coupling, cascading change risk |
| Scalability | N+1 queries, unbounded fetches, blocking ops |
| Security | Unvalidated input, hardcoded secrets, missing auth |
| Deployability | Environment coupling, missing health checks |
| Test Coverage | Untested states, flaky tests, coverage gaps |

Results documented with quantitative metrics and a pass/fail gate decision.

## What's Included

```
skills/ffm/
├── SKILL.md                          Core methodology
└── references/
    ├── phases-detailed.md            Full phase breakdown with gates
    ├── doc-taxonomy.md               /ffm-docs/ structure spec
    ├── adversarial-review-guide.md   Self-review checklist + template
    ├── interview-questions.md        Interview templates per phase
    └── adopt-existing-project.md     Migration path for existing codebases
```

**Stack-agnostic.** No frameworks, tools, or platforms mandated.
**Interview-driven.** Structured interviews at every review gate.
**Greenfield or existing.** New projects or adopt into existing codebases.

---

## Design Principles

| Principle | In practice |
|-----------|------------|
| API spec as contract | Single artifact both sides reference |
| Docs-first, code-second | `/ffm-docs/` updated before any code change |
| Iterate cheap, build once | UX changes ~5x cheaper with mocks |
| Test aggressively, relax deliberately | High defaults, user opts out not in |
| Every UX state is a test case | Loading, success, empty, error, edge, permissions |
| Structured feedback | Interviews replace "looks good" approvals |
| Self-critical builds | Adversarial reviews catch optimistic assumptions |
| Stack agnostic | Works with any framework or language |
| Token efficient | Phased specification minimizes wasted computation |

## Contributing

Contributions welcome. Propose changes via issues or PRs. Major methodology changes should include rationale for how they improve drift prevention, token efficiency, or developer experience.

## License

MIT

---

<p align="center">
  Built by <a href="https://github.com/Himalyan-AI">Himalyan AI</a><br>
  Designed for Claude Code. Applicable to any AI-assisted development workflow.
</p>
