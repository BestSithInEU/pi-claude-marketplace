---
phase: 106-workflow-detection-and-partial-install
reviewed: 2026-08-29T20:40:36Z
depth: standard
files_reviewed: 16
files_reviewed_list:
  - docs/output-catalog.md
  - extensions/pi-claude-marketplace/domain/components/plugin.ts
  - extensions/pi-claude-marketplace/domain/resolver.ts
  - extensions/pi-claude-marketplace/shared/notify-reasons.ts
  - extensions/pi-claude-marketplace/shared/notify.ts
  - extensions/pi-claude-marketplace/shared/probe-classifiers.ts
  - tests/architecture/catalog-uat.test.ts
  - tests/architecture/compat-01-no-expansion.test.ts
  - tests/architecture/notify-closed-set-locks.test.ts
  - tests/domain/manifest.test.ts
  - tests/domain/resolver-loose.test.ts
  - tests/domain/resolver-strict.test.ts
  - tests/orchestrators/discover.test.ts
  - tests/orchestrators/plugin/cross-surface-reason-parity.test.ts
  - tests/orchestrators/plugin/install.test.ts
  - tests/shared/probe-classifiers.test.ts
findings:
  critical: 0
  warning: 1
  info: 0
  total: 1
status: issues_found
---

# Phase 106: Code Review Report

**Reviewed:** 2026-08-29T20:40:36Z
**Depth:** standard
**Files Reviewed:** 16
**Status:** issues_found

## Summary

The workflow declaration and directory signals enter the existing unsupported-component pipeline without adding a materialization path. Strict and loose resolution, structural precedence, partial-install persistence, and the shared reason mapping are internally consistent. One new catalog contract is inconsistent with the real install command.

## Narrative Findings (AI reviewer)

### Warnings

#### WR-01: The catalog locks a rejection row that the install command never emits

**Classification:** WARNING
**Files:** `/home/acolomba/pi-claude-marketplace/.worktrees/workflows-detection/docs/output-catalog.md:624`; `/home/acolomba/pi-claude-marketplace/.worktrees/workflows-detection/tests/architecture/catalog-uat.test.ts:1328`; `/home/acolomba/pi-claude-marketplace/.worktrees/workflows-detection/tests/orchestrators/plugin/install.test.ts:5491`

**Issue:** The new `workflow-install-rejection` catalog state includes `v1.0.0`. The real install path rejects a partially available plugin before it resolves the version. The end-to-end install test explicitly locks the resulting row as `⊖ helper (partially-available) {workflows}` with no version. The catalog UAT passes because its synthetic message supplies a version and compares that synthetic output with the same incorrect documentation. It does not validate the command output. This leaves two incompatible byte contracts for one user-visible state and can mislead future changes that rely on the catalog as the output authority.

**Fix:** Remove `version: "1.0.0"` from the `workflow-install-rejection` fixture and remove `v1.0.0` from the matching catalog row:

```text
● official [user]
  ⊖ helper (partially-available) {workflows}
    Re-run with --partial to install the supported components.
```

Then keep the existing command-level assertion as the authoritative regression check for this early rejection path.

---

_Reviewed: 2026-08-29T20:40:36Z_
_Reviewer: the agent (gsd-code-reviewer)_
_Depth: standard_
