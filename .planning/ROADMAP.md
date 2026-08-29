# workflows-detection — Workflow Detection

**Milestone:** `workflows-detection`

## Overview

Workflow Detection finds declared and conventional workflow components. Affected plugins become partially available and show the exact `{workflows}` reason. With `--partial`, the extension installs only supported components and never materializes or executes workflows.

The six requirements form one resolver-to-install workflow, so standard granularity keeps them in one phase.

## Phases

**Phase numbering:** Phase 106 continues the global sequence after the completed Phase 105.

- [ ] **Phase 106: Workflow Detection and Partial Install** - Users can identify workflow-bearing plugins and install only their supported components with explicit consent.

## Phase Details

### Phase 106: Workflow Detection and Partial Install

**Goal**: Users can identify plugins that contain unsupported workflows and install only their supported components with explicit `--partial` consent.
**Depends on**: Phase 105
**Requirements**: WDET-01, WDET-02, WDET-03, WDET-04, WDET-05, WDET-06
**Success Criteria** (what must be TRUE):

  1. Marketplace entries and `plugin.json` files that declare `workflows` load successfully and expose the declaration to the resolver.
  2. The resolver finds conventional `<pluginRoot>/workflows/` directories without declarations, including the current `claude-security` and `code-modernization` layouts.
  3. `list`, `info`, install rejection, and all other unsupported-reason outputs show affected plugins as `(partially-available) {workflows}`.
  4. A normal install rejects an affected plugin. `--partial` installs only its supported components.
  5. After `/reload`, supported artifacts work, but Pi has no materialized workflow files and does not execute workflows.

**Plans**: TBD
**UI hint**: yes

## Progress

**Execution Order:** Phase 106

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 106. Workflow Detection and Partial Install | 0/TBD | Not started | - |
