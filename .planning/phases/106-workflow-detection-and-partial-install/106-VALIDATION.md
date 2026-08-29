---
phase: 106
slug: workflow-detection-and-partial-install
# status lifecycle: draft (seeded by plan-phase) -> validated (set by validate-phase)
status: draft
nyquist_compliant: false
wave_0_complete: true
created: 2026-08-29
---

# Phase 106 - Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Node.js built-in `node:test` |
| **Config file** | None. `package.json` defines commands and test scopes. |
| **Quick run command** | `node --test tests/domain/manifest.test.ts tests/domain/resolver-strict.test.ts tests/domain/resolver-loose.test.ts tests/shared/probe-classifiers.test.ts tests/orchestrators/plugin/cross-surface-reason-parity.test.ts tests/architecture/notify-closed-set-locks.test.ts tests/architecture/catalog-uat.test.ts` |
| **Full suite command** | `npm run check` |
| **Estimated runtime** | About 10 seconds for focused tests. Full-suite time varies by environment. |

---

## Sampling Rate

- **After every task commit:** Run the smallest command from the verification map.
- **After every plan wave:** Run `npm test && npm run test:integration`.
- **Before `$gsd-verify-work`:** Run `npm run check` and `npm run test:e2e`.
- **Max feedback latency:** 30 seconds for task-level checks.

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Threat Ref | Secure Behavior | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|------------|-----------------|-----------|-------------------|-------------|--------|
| 106-01-01 | 01 | 1 | WDET-01 | T-106-01 | Treat declarations as opaque presence signals. | unit | `node --test tests/domain/manifest.test.ts` | ✅ | ⬜ pending |
| 106-01-02 | 01 | 1 | WDET-02, WDET-03 | T-106-01, T-106-03 | Stat only the fixed `workflows/` directory and keep structural failure precedence. | unit | `node --test tests/domain/resolver-strict.test.ts tests/domain/resolver-loose.test.ts` | ✅ | ⬜ pending |
| 106-02-01 | 02 | 2 | WDET-04 | T-106-05 | Use one closed classifier for exact `{workflows}` output on all surfaces. | unit and architecture | `node --test tests/shared/probe-classifiers.test.ts tests/orchestrators/plugin/cross-surface-reason-parity.test.ts tests/architecture/notify-closed-set-locks.test.ts tests/architecture/catalog-uat.test.ts` | ✅ | ⬜ pending |
| 106-03-01 | 03 | 3 | WDET-05 | T-106-03 | Require explicit `--partial` consent before supported artifacts are staged. | integration unit | `node --test tests/orchestrators/plugin/install.test.ts` | ✅ | ⬜ pending |
| 106-03-02 | 03 | 3 | WDET-06 | T-106-02, T-106-04 | Never copy, record, discover, or execute a workflow file. | boundary integration | `node --test tests/orchestrators/plugin/install.test.ts tests/orchestrators/discover.test.ts` | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠ flaky*

---

## Wave 0 Requirements

Existing infrastructure covers all phase requirements. The listed test files and fixture helpers already exist. Implementation tasks extend them with workflow cases.

---

## Manual-Only Verifications

All phase behaviors have automated verification.

---

## Validation Sign-Off

- [ ] All tasks have an automated check or a Wave 0 dependency.
- [ ] No three consecutive tasks omit an automated check.
- [x] Wave 0 covers all missing references.
- [x] Commands do not use watch mode.
- [x] Task-level feedback latency is less than 30 seconds.
- [ ] Set `nyquist_compliant: true` after execution evidence is green.

**Approval:** Pending execution.
