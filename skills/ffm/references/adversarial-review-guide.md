# Adversarial Self-Review Guide

When performing an adversarial code review, adopt a critical stance toward the code just produced. The goal is to find weaknesses before they reach production.

## When to Perform

Execute an adversarial self-review during the **Review** step of every BTRI loop. Document findings in `/ffm-docs/ADVERSARIAL/Phase<X>-Completion-Adversarial-Code-Review.md`.

## The Six Dimensions

### 1. Maintainability

Assess how easily the code can be understood, modified, and debugged.

**Checklist:**
- [ ] Functions/methods have clear, single responsibilities
- [ ] Naming is consistent and descriptive
- [ ] Complex logic has inline comments explaining WHY (not what)
- [ ] No dead code, commented-out blocks, or TODO debris
- [ ] Error messages are actionable (tell the developer what went wrong and where)
- [ ] File organization follows project conventions

**Metrics**: Cyclomatic complexity per function (flag >10), average function length (flag >50 lines), nesting depth (flag >3 levels).

### 2. Modularity / Expandability

Assess how well the code supports change and growth.

**Checklist:**
- [ ] Components have clear boundaries and interfaces
- [ ] Dependencies flow inward (business logic does not depend on UI/framework details)
- [ ] Adding a new feature does not require modifying unrelated modules
- [ ] Shared state is minimized and explicitly managed
- [ ] API spec changes can propagate to mocks and tests without code surgery

**Metrics**: Coupling between modules (count cross-module imports), cohesion within modules, number of files touched per story.

### 3. Scalability

Assess performance characteristics and resource efficiency.

**Checklist:**
- [ ] No N+1 query patterns or unbounded data fetches
- [ ] Lists/tables handle large datasets (pagination, virtualization, or lazy loading)
- [ ] API calls are batched or debounced where appropriate
- [ ] No synchronous blocking operations on critical paths
- [ ] Asset sizes are reasonable (images optimized, bundles split)

**Metrics**: Bundle size, largest contentful paint, API response times under simulated load.

### 4. Security

Assess vulnerability surface and data protection.

**Checklist:**
- [ ] User input is validated and sanitized at system boundaries
- [ ] Authentication/authorization checks exist on protected routes and endpoints
- [ ] Sensitive data is not exposed in logs, URLs, or client-side storage
- [ ] API keys, tokens, and secrets are not hardcoded
- [ ] CORS, CSP, and other security headers are configured
- [ ] Dependencies have no known critical vulnerabilities

**Metrics**: Count of unvalidated inputs, count of hardcoded secrets (must be 0), dependency audit results.

### 5. Deployability

Assess readiness for reliable deployment.

**Checklist:**
- [ ] Application starts and stops cleanly
- [ ] Configuration is externalized (environment variables, config files)
- [ ] Health check endpoints exist (backend)
- [ ] Build process is reproducible (lock files, pinned versions)
- [ ] Mock/real backend toggle works correctly
- [ ] No environment-specific hardcoding

**Metrics**: Build time, startup time, number of environment-specific config values.

### 6. Test Coverage

Assess testing completeness and quality.

**Checklist:**
- [ ] Every user story has corresponding E2E tests
- [ ] Every UX state (loading, success, error, empty) is tested
- [ ] API interactions are validated against the spec
- [ ] Critical business logic has unit tests
- [ ] Tests are deterministic (no flaky tests)
- [ ] Test gate percentage is met for the current phase

**Metrics**: Line coverage %, branch coverage %, E2E scenario count vs. story count, flaky test count (must be 0).

## Review Document Template

```markdown
# Phase X — Adversarial Code Review

**Date**: YYYY-MM-DD
**Phase**: [Phase name]
**Test Gate**: [Configured percentage]
**Actual Pass Rate**: [Measured percentage]

## Summary
[2-3 sentence overall assessment]

## Dimension Scores

| Dimension | Score (1-5) | Key Finding |
|-----------|-------------|-------------|
| Maintainability | X | [one-liner] |
| Modularity | X | [one-liner] |
| Scalability | X | [one-liner] |
| Security | X | [one-liner] |
| Deployability | X | [one-liner] |
| Test Coverage | X | [one-liner] |

## Critical Issues
[Issues that MUST be resolved before proceeding]

## Recommendations
[Prioritized list of improvements, tagged by severity: critical/major/minor]

## Metrics
[Quantitative measurements collected during review]

## Gate Decision
- [ ] PASS — proceed to next step
- [ ] FAIL — return to Build with the following issues: [list]
```

## Adversarial Mindset Guidelines

When reviewing, actively look for:
- **The happy path trap**: Does the code only handle the ideal scenario?
- **The scaling cliff**: What happens with 10x the current data/users?
- **The security shortcut**: Where did speed-of-development trade off against safety?
- **The coupling creep**: Which changes would cascade across multiple files?
- **The test gap**: Which user story states have no test coverage?
- **The deploy surprise**: What could go wrong in a production environment that works locally?
