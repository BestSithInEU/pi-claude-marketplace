# Requirements: Workflow Detection

**Defined:** 2026-08-29
**Milestone:** workflows-detection
**Core Value:** A Pi user can install each supported Claude plugin component as a working Pi artifact.

## Milestone Requirements

### Workflow Detection

- [ ] **WDET-01**: The schemas accept a `workflows` declaration in marketplace entries and `plugin.json` files.
- [ ] **WDET-02**: The resolver finds `<pluginRoot>/workflows/` without a manifest declaration. This includes the current `claude-security` and `code-modernization` layouts.
- [ ] **WDET-03**: A plugin with workflows resolves as `partially-available` and records `workflows` as an unsupported component.

### User Surfaces

- [ ] **WDET-04**: Each unsupported-reason surface shows the exact `{workflows}` reason.
- [ ] **WDET-05**: A normal install rejects the partial plugin. An install with `--partial` installs only its supported components.

### Scope Boundary

- [ ] **WDET-06**: The extension does not materialize or execute workflow files.

## Future Requirements

### Workflow Support

- Workflow discovery, validation, materialization, and execution remain future work.

## Out of Scope

| Feature | Reason |
| ------- | ------ |
| Workflow execution | Pi does not support Claude workflows yet. |
| Workflow script validation | Detection only identifies the unsupported component kind. |
| New network operations | Detection uses the resolved plugin root and existing cache rules. |

## Traceability

The roadmap maps each requirement to one phase.

| Requirement | Phase | Status |
| ----------- | ----- | ------ |
| WDET-01 | — | Pending |
| WDET-02 | — | Pending |
| WDET-03 | — | Pending |
| WDET-04 | — | Pending |
| WDET-05 | — | Pending |
| WDET-06 | — | Pending |

**Coverage:**

- Milestone requirements: 6 total
- Mapped to phases: 0
- Unmapped: 6

---

_Requirements defined: 2026-08-29_
_Last updated: 2026-08-29 after initial definition_
