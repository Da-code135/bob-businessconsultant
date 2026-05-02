# Architecture Prompt Template

Run only after summary acceptance. Substitute placeholders with collected answers.
Execute all steps in order. Output feeds directly into `05-output_format.md`.

---

## INPUTS
- Problem: [Q1]
- Users: [Q2]
- Platform: [Q3]
- Scale: [Q4]
- Offline: [Q5]
- Data sensitivity: [Q6]
- Integrations: [Q7]
- Budget: [Q8]

---

## STEP 0 — REQUIREMENTS

### Functional Requirements
Atomic, testable, one action each.
```
FR-001: The system shall ...
FR-002: The system shall ...
```

### Non-Functional Requirements
- Performance: concurrent users, response time
- Reliability: uptime, offline handling
- Security: data protection level
- Usability: target user skill level

---

## STEP 1 — SYSTEM CLASSIFICATION
One precise sentence defining the system type.

---

## STEP 2 — CONSTRAINTS
Numbered list:
1. Platform
2. Scale
3. Offline requirement
4. Data sensitivity
5. Integrations
6. Budget

---

## STEP 3 — ARCHITECTURE OPTIONS

**Option A:** [Pattern] — Pros / Cons
**Option B:** [Pattern] — Pros / Cons

---

## STEP 4 — ARCHITECTURE SELECTION
- Chosen pattern + justification (reference constraints)
- Trade-offs
- Risks
- Scalability path

---

## STEP 5 — COMPONENTS
For each component:
- Name
- Responsibility (2 sentences max)
- Connects To
- Supports (FR-IDs)

---

## STEP 6 — TECHNOLOGY STACK
For each component:
- Technology
- Reason (1 sentence, context-specific)
- Cost: Free / Open Source / Low Cost / Paid

Priority: well-documented, offline-capable where needed, junior-developer learnable, low-cost where budget is constrained.

---

## STEP 7 — MERMAID DIAGRAM
```
graph TD
  [all components, labelled flows, subgraphs where logical]
```

---

## STEP 8 — PRODUCT BACKLOG

### Epics
Group related features.

### User Stories
`As a [user], I want [action], so that [value].`

### Tasks
Actionable. Map to components. Every FR must appear.

---

## STEP 9 — SPRINT PLAN

| Sprint | Focus | Tasks | Duration |
|--------|-------|-------|----------|
| 1 | Setup + environment | ... | 1 week |
| 2 | Core functionality | ... | 2 weeks |
| 3 | Features + integrations | ... | 2 weeks |
| 4 | Testing + deployment | ... | 1 week |