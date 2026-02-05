# FFM Interview Templates

Structured interview questions for each phase gate. Use the AskUserQuestion tool to proceed question by question. Provide multiple-choice options for non-obvious decisions. Continue until the topic is fully explored.

## General Guidelines

- Ask one topic at a time
- Use multiple-choice for subjective or non-obvious questions
- Probe deeper on answers that reveal uncertainty or misalignment
- Summarize findings after each interview section
- Document all responses in the appropriate UserFeedback file

---

## PRD Kickoff Interview (Phase 1 Entry)

### 1. Product Vision and Scope
"What is the core purpose of this product? Describe the high-level problem it solves, target users, key outcomes, and unique value proposition."

### 2. Viability Assessment
Multiple choice:
- A: High viability — strong market need, low competition, feasible resources
- B: Moderate viability — some need, moderate competition, minor resource adjustments
- C: Low viability — uncertain market, high competition, significant challenges
- D: Not viable at present — major barriers, consider pivoting

### 3. Target Users and Personas
"Who are the primary user personas? Details on demographics, behaviors, pain points, and how the product addresses them."

### 4. Key Features and Prioritization
"List must-have features. For each, indicate priority (high/medium/low) and dependencies."

### 5. Complexity Assessment
Multiple choice:
- A: Low — basic CRUD, standard UI, no real-time or integrations
- B: Moderate — some custom UI, basic integrations, moderate data handling
- C: High — advanced UI/UX, real-time features, multiple integrations, scalability needs
- D: Very high — AI/ML components, high-security needs, large-scale data processing

### 6. Non-Functional Requirements
"Specify performance targets, security needs, accessibility standards, and scalability expectations."

### 7. Completeness Check
Multiple choice:
- A: Fully complete — all scenarios addressed
- B: Mostly complete — core paths covered, minor edge cases missing
- C: Partially complete — major paths outlined, significant detail gaps
- D: Incomplete — high-level only, needs substantial expansion

### 8. Constraints and Risks
"Budget, timeline, tech stack preferences, and potential risks (regulatory, technical, organizational)."

### 9. Success Metrics
"How will success be measured? Specific KPIs: user engagement, conversion rates, performance targets."

### 10. Test Gate Configuration
"FFM defaults to aggressive testing — high coverage, every UX state tested. Here are the defaults. Confirm or adjust:"

| Phase | Default E2E Pass | Default Unit Coverage |
|-------|------------------|-----------------------|
| Phase 1 first loop | 90% | 80% |
| Phase 1 subsequent | 95% | 85% |
| Phase 2 (Backend) | 95% | 90% |
| Phase 3 (UAT) | 98% | 90% |
| Phase 4 (Production) | 98% | 90% |

Multiple choice:
- A: Keep aggressive defaults — quality is critical for this project
- B: Slightly relax — drop Phase 1 to 80%/70%, keep later phases as-is
- C: Rapid prototype mode — 70%/60% across the board, tighten later
- D: Custom — specify exact values per phase

"Every user story will produce tests for ALL UX states: loading, success, empty, error, edge cases, and permissions. Want to adjust which states are mandatory?"

### 11. Integrations and External Dependencies
"Any external services, data sources, third-party APIs, or specific UI/UX guidelines to follow?"

---

## UX Review Interview (Phase 1 BTRI — Human Review)

### Progress Check
"Here is the progress on all epics: [summary]. And the adversarial review found: [key findings]. Starting the UX review."

### 1. Overall UX Impression
Multiple choice:
- A: Excellent — intuitive, delightful, no confusion points
- B: Good — mostly clear, minor friction in some flows
- C: Needs improvement — several confusing or frustrating interactions
- D: Major rework needed — fundamental UX issues

### 2. Feature Completeness
"For each implemented feature, does it match your expectations? Any missing behaviors or states?"

### 3. UI States Coverage
"Do loading states, error states, and empty states feel appropriate and informative?"

### 4. Flow and Navigation
"Is the navigation between screens intuitive? Any dead ends or confusing transitions?"

### 5. Edge Cases
"Any scenarios you can think of that aren't handled? Unusual inputs, connection loss, permissions?"

### 6. UX Satisfaction Gate
Multiple choice:
- A: Satisfied — ready to proceed to backend development
- B: Almost — a few specific items need fixing first: [list them]
- C: Not yet — significant UX changes needed before backend work begins

---

## Backend Stack Interview (Phase 1 → Phase 2 Transition)

### 1. Hosting Environment
Multiple choice:
- A: Cloud provider (specify: AWS, GCP, Azure, etc.)
- B: Platform-as-a-Service (Vercel, Railway, Render, Fly.io, etc.)
- C: Self-hosted / on-premise
- D: Undecided — recommend based on project needs

### 2. Authentication Strategy
Multiple choice:
- A: Token-based (JWT)
- B: OAuth 2.0 / OpenID Connect (social login)
- C: Session-based
- D: API key based (service-to-service)
- E: Multiple / Custom

### 3. Data Storage
Multiple choice:
- A: Relational SQL (PostgreSQL, MySQL, etc.)
- B: Document NoSQL (MongoDB, Firestore, etc.)
- C: Key-value / Cache (Redis, DynamoDB, etc.)
- D: Multiple storage types
- E: Undecided — recommend based on data model

### 4. External API Providers
"Any third-party services the backend must integrate with? Payment, email, SMS, analytics, etc."

### 5. Performance Requirements
"Expected concurrent users, response time targets, data volume expectations."

### 6. Backend Complexity Preferences
Multiple choice:
- A: Minimal — simple API layer, keep it lean
- B: Standard — proper service layer, validation, error handling
- C: Full architecture — domain-driven design, event sourcing, message queues
- D: Match the API spec complexity — let the spec drive the architecture

---

## Backend Review Interview (Phase 2 BTRI — Human Review)

### Progress Check
"Backend implementation progress: [summary]. Adversarial review findings: [key findings]."

### 1. API Compliance
"All E2E tests from Phase 1 are now running against the real backend. Pass rate: [X%]. Any specific endpoints or behaviors to verify manually?"

### 2. Mock/Real Toggle
"The mock-to-real backend toggle is operational via [mechanism]. Verified working?"

### 3. Performance Observations
"Initial performance measurements: [metrics]. Within acceptable range?"

### 4. Security Concerns
"Any specific security requirements beyond what the adversarial review covers?"

### 5. Backend Satisfaction Gate
Multiple choice:
- A: Satisfied — backend matches spec, tests pass, ready for hardening
- B: Almost — specific issues to address: [list]
- C: Not yet — significant backend work needed

---

## UAT Review Interview (Phase 3 BTRI — Human Review)

### 1. UAT Test Results
"Here are the UAT results: [summary]. [X] scenarios passed, [Y] failed. Critical issues: [list]."

### 2. User Satisfaction
Multiple choice:
- A: Users are satisfied — minor polish items only
- B: Users found issues — specific fixes needed: [list]
- C: Users found significant problems — another UAT round required

### 3. Performance Under Load
"Load test results: [metrics]. Acceptable for launch?"

### 4. Deployment Readiness
Multiple choice:
- A: Ready for production deployment
- B: Ready after resolving: [specific blockers]
- C: Not ready — needs another hardening cycle

---

## Post-Launch Interview (Phase 4)

### 1. Monitoring Review
"30-day metrics: [KPIs, error rates, uptime]. Any anomalies?"

### 2. User Feedback Themes
"Top user feedback themes: [list]. Priority items to address?"

### 3. Iteration Priority
Multiple choice:
- A: Fix reported bugs (stability focus)
- B: Add requested features (growth focus)
- C: Performance optimization (scale focus)
- D: Security hardening (compliance focus)
