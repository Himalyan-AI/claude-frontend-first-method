# Adopting an Existing Project into FFM

Guide for applying the Frontend First Methodology to a codebase that already exists. The goal is to establish the API spec as the central contract and align both sides to it.

## When to Use This Path

- Existing project with frontend-backend drift
- Legacy codebase that needs structured iteration
- Project mid-development that wants to adopt FFM principles
- Any codebase where the frontend and backend have diverged from a shared understanding

## Step 1: Assess the Current State

Explore the codebase and document:

1. **Frontend inventory**: What components exist? What states do they handle? What API calls do they make?
2. **Backend inventory**: What endpoints exist? What do they return? What data models are in use?
3. **API boundary**: Where does the frontend talk to the backend? REST, GraphQL, RPC, websockets?
4. **Test coverage**: What tests exist? What's covered, what's not?
5. **Documentation**: Is there an existing API spec, README, or architecture doc?

Output: An assessment document in `/ffm-docs/TECH/<ProjectName>-Assessment-v1.md`

## Step 2: Generate API Spec from Reality

Reverse-engineer the API specification from what actually exists:

1. **Extract routes/endpoints** from the backend code (controllers, route files, handlers)
2. **Extract API calls** from the frontend code (fetch calls, API client usage, SDK calls)
3. **Compare** what the frontend expects vs. what the backend provides
4. **Document drift**: Where do they disagree? Missing fields, different types, inconsistent status codes

Output: Generated API spec (OpenAPI v3 or equivalent) + drift report

## Step 3: Create the Drift Resolution Plan

For each drift point:

1. **Decide the truth**: Does the frontend's expectation or the backend's implementation represent the desired behavior?
2. **Create user stories** to resolve each drift point — either fix the frontend, fix the backend, or meet in the middle
3. **Prioritize**: Critical drift (broken features) first, then inconsistencies, then polish

Output: Stories in `/ffm-docs/STORIES/`, epics in `/ffm-docs/EPICS/`

## Step 4: Establish the Mock Layer

Even for existing projects, set up mocking:

1. **Configure a mock server** from the newly generated API spec
2. **Verify** the frontend works against the mocks (it should, if the spec was generated from frontend expectations)
3. **Fix** any frontend issues discovered during mock testing

This gives a clean baseline: the frontend works against the spec. Now make the backend match.

## Step 5: Enter the FFM Loop

With the spec established and mocks working:

- If **frontend needs significant UX work**: Enter at Phase 1 (UX iteration with mocks)
- If **frontend UX is acceptable, backend needs alignment**: Enter at Phase 1.7 (build backend to spec)
- If **both sides mostly work, need hardening**: Enter at Phase 2.6 (UAT readiness)

From this point forward, follow standard FFM phases.

## Key Differences from Greenfield

| Aspect | Greenfield | Adopt |
|--------|-----------|-------|
| API spec source | Generated from stories | Reverse-engineered from code |
| First BTRI focus | Building new UX | Resolving existing drift |
| Mock purpose | Enable frontend development | Validate frontend expectations |
| Phase entry | Always Phase 1 | Depends on current state |
| PRD | Created fresh | May need to be reconstructed from existing features |

## Common Adoption Challenges

**"The frontend and backend use completely different data shapes"**
Create a mapping layer. Document both shapes in the spec. Decide which to standardize on and create stories to migrate.

**"There are no tests at all"**
Start by writing E2E tests against the current behavior (characterization tests). These become the regression safety net for all future changes.

**"The API has undocumented behavior"**
Run the frontend through all its flows while logging API calls. The actual network traffic becomes the initial spec draft.

**"Parts of the backend are third-party services"**
Include third-party APIs in the spec as external dependencies. Mock them during frontend testing. Document their expected behavior.
