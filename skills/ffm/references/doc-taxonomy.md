# FFM Document Taxonomy

All project documentation lives under `/ffm-docs/` and follows the `ff-m-tag` naming structure for grep/ls discoverability. Each document is atomic, reusable, and maintains full-context readability.

## Folder Structure

```
/ffm-docs/
├── current-status.md
├── index.md
├── PRD/
│   ├── <ProjectName>-Refined-PRD-v1.md
│   └── UserFeedback/
│       └── Feedback-<topic>-PRD<version>-<revision>.md
├── TECH/
│   ├── <ProjectName>-Architecture-v1.md
│   └── <ProjectName>-Deployment-v1.md
├── LLD/
│   └── Phase_<i>_lld.md
├── STORIES/
│   └── UserStory-<id>.md
├── EPICS/
│   └── Phase-<i>-Epic-<id>.md
├── ARTIFACTS/
│   └── Phase<X>-<ArtifactTopic>-<userstory-id>.md
└── ADVERSARIAL/
    └── Phase<X>-Completion-Adversarial-Code-Review.md
```

## Document Specifications

### current-status.md
- Maximum 50 lines
- Always reflects the current phase, active epic, and in-progress documents
- Update at every phase transition and BTRI loop completion

### index.md
- Central index linking every project document
- Organized by category (PRD, TECH, LLD, STORIES, EPICS, ARTIFACTS, ADVERSARIAL)
- Update whenever a new document is created

### PRD Documents

**Refined PRD** (`PRD/<ProjectName>-Refined-PRD-v1.md`):
- Product overview and vision
- Target users and personas
- Feature list with priorities
- Non-functional requirements (performance, security, accessibility, scalability)
- Success metrics and KPIs
- Version history tracking all revisions
- Increment version (v2, v3...) for each revision — never overwrite

**User Feedback** (`PRD/UserFeedback/Feedback-<topic>-PRD<version>-<revision>.md`):
- Progress summary on all epics (completed vs. pending)
- Adversarial review summary (key findings, metrics)
- Issues list with severity (critical/major/minor) and suggested fixes
- Feedback scaffold sections: UX, stack, performance, security, other

### TECH Documents

**Architecture** (`TECH/<ProjectName>-Architecture-v1.md`):
- Current FFM phase indicator
- High-level component diagram (frontend, backend/mocks, API spec)
- Design principles with rationale
- Technology decisions (stack-agnostic descriptions, specific choices documented here)

**Deployment** (`TECH/<ProjectName>-Deployment-v1.md`):
- Created during Phase 3+
- Deployment topology and procedures
- Environment configurations
- Rollback procedures
- Monitoring setup

### LLD Documents

**Low-Level Design** (`LLD/Phase_<i>_lld.md`):
- Detailed component designs for the phase
- Each design element includes a **2-line reasoning** justifying inclusion
- Reasoning references clean architecture principles: separation of concerns, dependency inversion, modularity, testability
- Example:
  ```
  ## UserAuthService
  Handles authentication token management and session lifecycle.
  **Reasoning**: Isolates auth logic from business rules per single responsibility;
  enables mock replacement during Phase 1 UX testing without touching UI components.
  ```

### STORIES Documents

**User Stories** (`STORIES/UserStory-<id>.md`):
- Title
- Format: "As a [user] I want [feature] so that [benefit]"
- Acceptance criteria split into three sections:
  - **Frontend**: All UI states (loading, success, error, empty, edge cases)
  - **Backend**: API requirements (endpoints, payloads, status codes) referencing the API spec
  - **E2E Testing**: Test scenarios covering every UX state
- Stories must be atomic — one testable behavior per story

### EPICS Documents

**Epics** (`EPICS/Phase-<i>-Epic-<id>.md`):
- Short, atomic scope
- Checklist linking to constituent user stories
- Required context for implementation (dependencies, assumptions)
- Phase indicator

### ARTIFACTS Documents

**Artifacts** (`ARTIFACTS/Phase<X>-<ArtifactTopic>-<userstory-id>.md`):
- Phase-specific outputs: diagrams, code snippets, test results, UAT reports
- Always note the FFM phase and linked user story ID
- Examples: API spec snapshots, performance benchmarks, UAT session results

### ADVERSARIAL Documents

**Adversarial Code Reviews** (`ADVERSARIAL/Phase<X>-Completion-Adversarial-Code-Review.md`):
- Six assessment dimensions:
  1. **Maintainability**: Code readability, refactoring ease, documentation
  2. **Modularity/Expandability**: Loose coupling, extensibility, dependency management
  3. **Scalability**: Performance under load, resource efficiency
  4. **Security**: Input validation, auth checks, data protection
  5. **Deployability**: Containerization readiness, config management, CI/CD compatibility
  6. **Test Coverage**: Quantitative metrics, gap analysis, gate compliance
- Each dimension includes quantitative metrics where measurable
- Overall pass/fail against configured test gate
- Recommendations prioritized by severity

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Project name | PascalCase | `MyApp` |
| Phase number | Numeric with underscore | `Phase_1`, `Phase_2` |
| Story ID | Hierarchical with dashes | `1a-b-c` |
| Epic ID | Phase-prefixed hierarchical | `Phase-1-Epic-1a-b-c` |
| Feedback | Topic-version-revision | `Feedback-UX-PRDv1-r1.md` |
| Artifacts | Phase-Topic-StoryID | `Phase1-APISpec-1a.md` |

## ff-m-tag System

Every document filename and internal heading uses the `ff-m-tag` prefix pattern for discoverability:

```bash
# Find all Phase 1 documents
ls /ffm-docs/*/Phase1-* /ffm-docs/*/Phase_1_*

# Find all adversarial reviews
ls /ffm-docs/ADVERSARIAL/

# Find feedback for a specific PRD version
ls /ffm-docs/PRD/UserFeedback/Feedback-*-PRDv2-*

# Find all artifacts for a specific story
grep -rl "userstory-1a" /ffm-docs/ARTIFACTS/
```
