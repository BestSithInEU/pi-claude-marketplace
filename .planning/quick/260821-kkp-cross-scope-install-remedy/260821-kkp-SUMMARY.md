---
gsd_summary_version: 1.0
quick_id: 260821-kkp
status: complete
date: 2026-08-21
branch: features/cross-scope-install-remedy
commits:
  - eaa82289 feat(completion) offer --local on the six write-target verbs
  - 300b25dd fix(install) name both remedies when the marketplace is in the other scope
---

# Summary -- 260821-kkp

Supersedes the install half of PR #142. That PR's completion half was
sound and is carried over here; its install half is replaced.

## What shipped

**Remedy trailer.** `MarketplaceNotAddedMessage` gained
`presentInOtherScope?: boolean`. `shared/notify.ts` owns the bytes via
`crossScopeRemedyTrailerFor`, appended below the row after one blank line at
column 0. `crossScopeRemedyApplies` (`orchestrators/plugin/shared.ts`) makes
the decision; `install.ts` wires it into the standalone `marketplaceAbsent`
arm only. Orchestrated (import cascade) mode is untouched.

**Completion.** The shared `--local` catalog entry is `complete: true` with a
description, renamed `NON_COMPLETED_SCOPE_TARGET` ->
`WRITE_TARGET_FLAG_ENTRY`.

## Decisions honored

- Stays a FAILURE. D-29 Locked / CMP-4 / PI-16: no user->project source
  fallback, no retarget. Brace, severity, and summary unchanged.
- `marketplace add` is the FIRST remedy -- it makes the command as typed
  succeed. A bare `--scope` means the user chose nothing, not that they chose
  the other scope.
- `--local` appears nowhere in the remedy. It selects the file within a scope
  and cannot resolve a scope miss. An assertion in the CMP-4 test pins its
  absence.
- Boolean field, not `hint?: string`. A caller-composed string would return
  user-visible prose to the construction site, which
  `docs/messaging-style-guide.md` retired.
- No invented requirement ID. Cites CMP-4 / SCOPE-01 / D-29 only; PR #142's
  fabricated `ATTR-11` is not carried over.

## Two defects in PR #142 that this avoids

1. **Unguarded `loadState`.** The probe sits after
   `withLockedStateTransaction` returns and no edge handler catches, so an
   unreadable other-scope `state.json` replaced the failure row with an
   unhandled rejection and ZERO `ctx.ui.notify` calls. Reproduced on both
   branches before writing the fix. `crossScopeRemedyApplies` catches and
   degrades to "no remedy". Pinned by a test that was mutation-checked: with
   the try/catch removed it fails, with it restored it passes.
2. **Unreachable project-target arm.** A project-target `marketplace-absent`
   means the CMP-3 fallback already consulted user scope, so the probe returns
   `false` without reading and no unreachable "registered at user scope"
   message arm exists.

## Verification

- `npm run check` GREEN (typecheck, lint, fallow, prettier, 3608 unit /
  3607 pass + 1 pre-existing skip, 21 integration).
- `pre-commit run --all-files`: every hook passes except trufflehog, which
  fails structurally in a linked worktree (documented in CLAUDE.md). Cleared
  by the filesystem route: `verified_secrets: 0`, `unverified_secrets: 0`.
- Catalog UAT byte-equality holds; annotated-example count 173 -> 174.
- 10 new tests: 4 completion exact-set (uninstall/reinstall/enable/disable),
  4 `crossScopeRemedyApplies` unit, 2 install byte-form (bare row, corrupt
  other-scope state), plus a grammar-invariant fixture and the updated CMP-4
  byte assertion.

## Not done (deliberate)

- No PR opened -- operator reviews the diff first.
- No version bump.
- `SCOPE_TARGET_FLAG` (the exported name for `--local`) is still a misnomer
  now that the scope-vs-file axis distinction is explicit. Left alone as a
  pre-existing exported symbol consumed by `edge/handlers/shared.ts`; flagged
  rather than renamed.
