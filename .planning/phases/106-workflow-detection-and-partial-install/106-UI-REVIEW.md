# Phase 106 - UI Review

**Audited:** 2026-08-29
**Baseline:** `106-UI-SPEC.md`
**Screenshots:** Not captured. This phase has no graphical frontend, and no dev server was available on ports 3000, 5173, or 8080.

---

## Pillar Scores

| Pillar | Score | Key Finding |
|--------|-------|-------------|
| 1. Copywriting | 3/4 | The exact workflow token and hints are correct, but the rejection example in the UI spec still includes a version that the implemented early-rejection path omits. |
| 2. Visuals | 4/4 | The terminal row reuses the closed partial-state glyphs and adds no workflow-specific visual element. |
| 3. Color | 4/4 | The extension adds no color rule. Severity and adjacent status text preserve meaning without color. |
| 4. Typography | 4/4 | The renderer leaves typography to the host and adds no ANSI or markdown styling. |
| 5. Spacing | 4/4 | Shared composers preserve the 0/2/4/6-space ladder and blank-line contracts. |
| 6. Experience Design | 3/4 | Consent, rollback, retry, and structural precedence are covered, but some workflow-specific update rows rely on shared parity rather than direct byte fixtures. |

**Overall: 22/24**

---

## Top 3 Priority Fixes

1. **Correct the rejection example** - The current UI spec and implemented catalog disagree about the version token. Remove `v1.0.0` from the normal-rejection example in `106-UI-SPEC.md`; early rejection is intentionally versionless, as it is for the other unsupported components.
2. **Add workflow-specific update byte fixtures** - The surface matrix promises targeted decline, bulk decline, and partial update or autoupdate output. Add catalog fixtures for these rows so their severity, hint, and reload behavior are direct contracts.
3. **Approve the corrected UI contract** - `106-UI-SPEC.md` still has `status: draft`, unchecked sign-off boxes, and `Approval: pending`. Mark it approved after the example and fixture coverage are aligned.

---

## Detailed Findings

### Pillar 1: Copywriting (3/4)

**WARNING:** The implemented vocabulary is precise. `workflows` is a dedicated tail member of `REASONS`, and the shared classifier maps only that typed kind to the exact lowercase plural token. `composeReasons` keeps all reasons in one comma-separated brace block. The catalog also preserves the existing install hint and error summary.

The baseline rejection example at `106-UI-SPEC.md:198` says `helper v1.0.0`. The executable catalog at `docs/output-catalog.md:624` says `helper` without a version. The latter matches the intentional early-rejection behavior recorded by the clean code review. Align the spec with the implementation and with the established unsupported-component behavior.

### Pillar 2: Visuals (4/4)

**PASS:** This phase has no HTML, CSS, React, JSX, or TSX surface. The terminal renderer uses the existing `⊖` partially-available and `◉` partially-installed glyphs. It adds no workflow-specific icon, heading, prompt, or trailer. The status text and `{workflows}` reason make the degraded state distinct from a clean install.

### Pillar 3: Color (4/4)

**PASS:** No CSS, hard-coded color, ANSI escape, or workflow color token was added. The row carries a textual status and reason, so color is not the only state signal. The catalog fixtures pin info severity for inventory and partial success, and error severity for normal rejection.

### Pillar 4: Typography (4/4)

**PASS:** Font family, size, weight, and line height remain host-controlled. The implementation adds plain terminal text only. Workflow states use the same renderer and token composition as all other plugin states.

### Pillar 5: Spacing (4/4)

**PASS:** `notify.ts:2640` documents the byte grammar, including two-space plugin rows, four-space details, six-space nested details, and one blank line between blocks. `notify.ts:4035` and `notify.ts:4043` implement the plugin and hint indentation. The three workflow catalog states bind the inventory, rejection, and success bytes in both catalog walk directions.

### Pillar 6: Experience Design (3/4)

**WARNING:** The implemented flow is coherent. Normal install rejects with an actionable `--partial` hint. Explicit partial consent installs only supported components. Structural defects still win, failed staging rolls back, retry succeeds, and workflow files never enter resources or reload discovery.

The shared classifier and cross-surface parity test support the broader surface matrix. However, the workflow-specific catalog directly binds only inventory, normal rejection, and partial-install success. Add direct byte fixtures for targeted update decline, bulk update decline, and partial update or autoupdate success to lock their different severity and trailer rules.

---

## Registry Safety

No `components.json` file exists, shadcn is not initialized, and the UI spec lists no third-party registries. The registry audit does not apply.

---

## Files Audited

- `.planning/phases/106-workflow-detection-and-partial-install/106-UI-SPEC.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-CONTEXT.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-01-PLAN.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-01-SUMMARY.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-02-PLAN.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-02-SUMMARY.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-03-PLAN.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-03-SUMMARY.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-04-PLAN.md`
- `.planning/phases/106-workflow-detection-and-partial-install/106-04-SUMMARY.md`
- `extensions/pi-claude-marketplace/domain/components/plugin.ts`
- `extensions/pi-claude-marketplace/domain/resolver.ts`
- `extensions/pi-claude-marketplace/shared/notify-reasons.ts`
- `extensions/pi-claude-marketplace/shared/notify.ts`
- `extensions/pi-claude-marketplace/shared/probe-classifiers.ts`
- `tests/shared/probe-classifiers.test.ts`
- `tests/orchestrators/plugin/cross-surface-reason-parity.test.ts`
- `tests/orchestrators/plugin/install.test.ts`
- `tests/architecture/catalog-uat.test.ts`
- `tests/architecture/notify-closed-set-locks.test.ts`
- `docs/output-catalog.md`
