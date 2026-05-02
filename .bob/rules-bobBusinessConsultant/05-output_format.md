# Output Format

All documents go in: `{project-slug}/docs/`
Each document: complete, concise, no fluff, directly usable by a dev team.

---

## FILE STRUCTURE
```
{project-slug}/
└── docs/
    ├── overview.md
    ├── requirements.md
    ├── architecture.md
    ├── components.md
    ├── stack.md
    ├── diagram.plantuml
    ├── backlog.md
    └── sprints.md
```

---

## overview.md
**System name + one-line description**

Para 1 — What it does and what problem it solves (technical tone).
Para 2 — Key constraints: platform, scale, offline, data sensitivity, integrations, budget.

---

## requirements.md
### Functional Requirements
```
FR-001: The system shall ...
FR-002: The system shall ...
```
Rules: atomic, testable, no ambiguity.

### Non-Functional Requirements
| Category | Requirement |
|----------|-------------|
| Performance | [concurrent users, response time] |
| Reliability | [uptime, offline handling] |
| Security | [data protection level] |
| Usability | [target user skill level] |

---

## architecture.md
**Pattern:** [chosen pattern]

**Justification:** 3–4 sentences referencing constraints and FRs.

**Trade-offs:**
- [limitation or risk]
- [limitation or risk]

---

## components.md
| Component | Responsibility | Connects To | Supports (FR-IDs) |
|-----------|---------------|------------|-------------------|

Rules: all components listed, every component maps to at least one FR.

---

## stack.md
| Layer | Technology | Reason | Cost |
|-------|-----------|--------|------|

Cost values: Free / Open Source / Low Cost / Paid

---

## diagram.plantuml
PlantUML or Mermaid (graph TD).
All components. Labelled flows. Subgraphs where logical.

---

## backlog.md
### Epics
Group features logically.

### User Stories
`As a [user], I want [action], so that [value].`
Tag each with relevant FR-IDs.

### Tasks
Actionable. Mapped to components. All FRs covered.

---

## sprints.md
| Sprint | Focus | Key Tasks | Duration |
|--------|-------|-----------|----------|
| 1 | Environment setup | ... | 1 week |
| 2 | Core build | ... | 2 weeks |
| 3 | Features + integrations | ... | 2 weeks |
| 4 | Testing + deployment | ... | 1 week |

---

## FOLLOW-UP OPTIONS
After delivering all documents, offer naturally:

"Your full engineering delivery pack is ready. I can also:
- Generate starter folder structure and boilerplate for this stack
- Answer technical questions about any part of the design
- Do both

What would be most useful right now?"

### If starter code requested:
Generate folder structure with comments + boilerplate matching the stack.

### If questions:
Answer at appropriate technical level. Flag if answer requires architecture change.

### If both:
Folder structure first, then open for questions.