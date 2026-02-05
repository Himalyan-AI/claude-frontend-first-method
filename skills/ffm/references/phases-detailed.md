# FFM Phases — Detailed Reference

This document provides the full phase breakdown with specific gates, deliverables, and loop conditions. The SKILL.md contains the summary; this is the comprehensive reference.

## Phase Overview

```
Phase 1   — UX First (Mocked Backend)
Phase 1.1 — Build + Auto Test (frontend + mocks)
Phase 1.2 — Adversarial Self-Review
Phase 1.3 — Human Review (interview)
Phase 1.4 — Integrate Feedback
Phase 1.5 — Build + Test Revised Epics
Phase 1.6 — Loop until UX satisfaction → Backend spec interview
Phase 1.7 — Build Real Backend to Spec
Phase 1.8 — Adversarial Self-Review (backend)
Phase 1.9 — Human Review (backend)
Phase 2.0 — Integrate Backend Feedback
Phase 2.1 — Build + Test Backend Revisions
  ... BTRI loops until backend satisfaction ...
Phase 2.6 — Adversarial Review (UAT readiness)
Phase 2.7 — Human Review (UAT readiness)
Phase 2.8 — Build + Test to UAT gate (98% pass, 80%+ coverage)
Phase 2.9 — Final pre-UAT Human Review
Phase 3.0 — User Acceptance Testing
Phase 3.1 — Integrate UAT Feedback
Phase 3.2 — Staging Deployment + Regression
Phase 3.3 — Final Adversarial + Human Review
Phase 4.0 — Production Deployment
Phase 4.1 — Post-Launch Monitoring (30+ days)
```

## Phase 1: UX First

### Phase 1.1 — Build and Auto Test

**Input**: Refined PRD, epics, user stories, API spec, mock backend setup
**Activities**:
- Build fully functional frontend against mock backend
- Every user story's UX states are implemented (loading, success, error, empty, edge cases)
- E2E tests cover every UX interaction and state in user paths
- Unit tests for critical frontend components
- Mock backend simulates all states and interactions defined in the API spec

**Deliverables**:
- Functional frontend (demo-able)
- E2E test suite (100% story coverage)
- Mock backend configuration
- Artifacts in `/ffm-docs/ARTIFACTS/Phase1-*`
- Architecture doc updated to reflect Phase 1 state

**Gate**: 100% of demo-critical tests must pass

### Phase 1.2 — Adversarial Self-Review

**Input**: All code from Phase 1.1
**Activities**:
- Review all six dimensions (see `adversarial-review-guide.md`)
- Quantitative metrics collection
- Gap analysis against test gate

**Deliverables**:
- `/ffm-docs/ADVERSARIAL/Phase1-Completion-Adversarial-Code-Review.md`

**Gate**: Configured test pass rate (default 80%) before human review

### Phase 1.3 — Human Review

**Input**: Working demo, adversarial review results
**Activities**:
- Structured interview using UX Review template (see `interview-questions.md`)
- Question-by-question with multiple-choice for non-obvious items
- Document all feedback

**Deliverables**:
- `/ffm-docs/PRD/UserFeedback/Feedback-UX-PRDv1-r1.md`

**Gate**: User feedback documented, all critical issues identified

### Phase 1.4 — Integrate Feedback

**Input**: Human review feedback + adversarial findings
**Activities**:
- Analyze combined feedback
- Generate PRD revision if scope changes (increment version)
- Create new/updated stories and epics for all identified issues

**Deliverables**:
- Updated PRD (if revised): `/ffm-docs/PRD/<ProjectName>-Refined-PRD-v<X>.md`
- New/updated stories and epics

**Gate**: All feedback items have corresponding stories

### Phase 1.5 — Build and Test Revised Epics

**Input**: New epics from Phase 1.4
**Activities**:
- Implement revisions
- Full E2E test suite (existing + new tests)
- Mock backend updated if API spec changed

**Deliverables**:
- Updated frontend
- Updated test suite
- Updated artifacts

**Gate**: Configured test pass rate (default 90% — tighter than initial)

### Phase 1.6 — Loop Until UX Satisfaction

**Input**: Revised frontend from Phase 1.5
**Activities**:
- Repeat adversarial review (Phase 1.2 criteria)
- Human review interview — emphasis on UX satisfaction gate
- Integrate any remaining feedback
- **Loop condition**: Continue BTRI until user confirms UX satisfaction
- When satisfied: interview about backend stack preferences (see Backend Stack Interview template)
- Generate complete backend documents in `/ffm-docs/TECH/` and `/ffm-docs/LLD/`
- Freeze API spec

**Deliverables**:
- UX-approved frontend
- Frozen API spec
- Backend architecture documents
- Backend LLD with unit testing requirements

**Gate**: User explicit confirmation of UX satisfaction + frozen API spec

---

## Phase 2: Backend to Spec

### Phase 1.7 — Build Real Backend

**Input**: Frozen API spec, backend architecture, backend LLD
**Activities**:
- Implement real API endpoints matching the spec
- Replace mock responses with real logic
- Build mock/real toggle (environment variable or config switch)
- Run ALL existing E2E tests against real backend — they must pass without frontend changes

**Deliverables**:
- Working backend
- Mock/real toggle mechanism
- Backend unit tests
- Updated LLD with implementation details

**Gate**: Configured test pass rate (default 95%)

### Phase 1.8–1.9 — Backend Review Cycle

Standard BTRI adversarial + human review, focused on:
- API spec compliance (zero drift)
- Backend reliability and performance
- Security implementation
- Mock/real toggle functionality

### Phase 2.0–2.5 — Backend Iteration Loops

BTRI loops continue until backend satisfaction, with test gates at configured rate (default 95%).

### Phase 2.6–2.9 — UAT Readiness

Final BTRI loops with hardened gates:
- Test pass: configured (default 98%)
- Code coverage: configured (default 80%+)
- Adversarial review focuses on edge cases and performance
- Human review focuses on deployment readiness

---

## Phase 3: Harden for Deployment

### Phase 3.0 — User Acceptance Testing

**Input**: UAT-ready system
**Activities**:
- Deploy to isolated UAT environment mimicking production
- Structured testing with representative users covering all stories
- Capture feedback: functionality, UX, performance, bugs
- Load testing if applicable
- User satisfaction survey

**Deliverables**:
- UAT results in `/ffm-docs/ARTIFACTS/Phase3-UAT-Results-*.md`
- User satisfaction score

**Gate**: 100% critical issue resolution, configured user satisfaction score (default 95%)

### Phase 3.1 — Integrate UAT Feedback

Standard integration phase. BTRI loops until UAT re-test confirms approval.

### Phase 3.2 — Staging Deployment

**Activities**:
- Deploy to production-mirroring staging environment
- Full E2E regression
- Load testing (simulate peak users)
- Security audit (vulnerability scans)
- Performance benchmarking
- Compatibility checks (browsers, devices)

**Deliverables**:
- `/ffm-docs/ARTIFACTS/Phase3-Staging-Deployment.md`
- `/ffm-docs/TECH/<ProjectName>-Deployment-v1.md`

**Gate**: Zero high-severity issues, 100% regression pass, performance within targets

### Phase 3.3 — Final Reviews

Comprehensive adversarial review (all system aspects) + final human review. Last chance for changes before production.

---

## Phase 4: Production

### Phase 4.0 — Production Deployment

**Activities**:
- Execute rollout plan (canary, blue-green, or appropriate strategy)
- Set up monitoring (metrics, logs, alerting)
- Configure rollback procedures
- Post-deployment smoke tests
- Verify KPIs and success metrics

**Deliverables**:
- `/ffm-docs/TECH/<ProjectName>-Deployment-v1.md` (updated)
- Live production system

### Phase 4.1 — Post-Launch Monitoring

**Activities**:
- Monitor for 30+ days
- Track KPIs, error rates, user engagement
- Collect user feedback (in-app, analytics)
- Log anomalies
- Generate hotfix stories as needed (BTRI loop)
- Periodic reviews and doc updates

**Deliverables**:
- `/ffm-docs/ARTIFACTS/Phase4-PostLaunch-Monitoring.md`
- Updated `current-status.md`
- Hotfix stories/epics as needed
