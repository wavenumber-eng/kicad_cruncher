+++
type = "plan"
id = "kicad-cruncher-large-board-profile-api-plan"
status = "active"
title = "KiCad Cruncher Large-Board Profile And API Decision Plan"
created = "2026-07-17"

[[steps]]
id = "branch-and-plan-bootstrap"
title = "Create profiling branch and active dev-std plan"
status = "done"

[[steps]]
id = "kicad-monkey-floor-refresh"
title = "Repin kicad-monkey floor to the latest released dependency and refresh the lock"
status = "pending"
depends_on = ["branch-and-plan-bootstrap"]

[[steps]]
id = "command-surface-inventory"
title = "Inventory Cruncher commands and APIs that load PCB, design-review, SVG, STEP, DRC-adjacent, BOM, PnP, or project data"
status = "pending"
depends_on = ["branch-and-plan-bootstrap"]

[[steps]]
id = "large-board-baseline-harness"
title = "Capture reproducible public or synthetic large-board baselines for candidate Cruncher workflows"
status = "pending"
depends_on = [
  "kicad-monkey-floor-refresh",
  "command-surface-inventory",
]

[[steps]]
id = "project-api-fit-research"
title = "Research whether Cruncher workflows should use KiCadDesign, projection, targeted readers, or existing full-model APIs"
status = "pending"
depends_on = [
  "command-surface-inventory",
  "large-board-baseline-harness",
]

[[steps]]
id = "cheap-window-candidate-selection"
title = "Select low-risk Cruncher optimizations or document rejection with measured rationale"
status = "pending"
depends_on = [
  "large-board-baseline-harness",
  "project-api-fit-research",
]

[[steps]]
id = "independent-research-review"
title = "Have an independent agent reproduce the research and check for missed cheap windows before implementation"
status = "pending"
depends_on = [
  "large-board-baseline-harness",
  "project-api-fit-research",
  "cheap-window-candidate-selection",
]

[[steps]]
id = "implementation-scope-decision"
title = "Approve, reject, or defer each implementation candidate after independent review"
status = "pending"
depends_on = ["independent-research-review"]

[[steps]]
id = "cruncher-cheap-window-implementation"
title = "Implement accepted low-risk Cruncher optimizations, or close this step with a rejection log if none qualify"
status = "pending"
depends_on = ["implementation-scope-decision"]

[[steps]]
id = "api-guidance-doc-updates"
title = "Update Cruncher CLI/API guidance for large-board read-path choices and any accepted behavior"
status = "pending"
depends_on = ["implementation-scope-decision"]

[[steps]]
id = "monkey-followup-backlog"
title = "Record upstream kicad-monkey follow-up work for library-level or native/pull-parser improvements"
status = "pending"
depends_on = ["implementation-scope-decision"]

[[steps]]
id = "behavior-performance-signoff"
title = "Verify behavior preservation and record measured performance impact for accepted changes"
status = "pending"
depends_on = [
  "cruncher-cheap-window-implementation",
  "api-guidance-doc-updates",
  "monkey-followup-backlog",
]

[[steps]]
id = "design-doc-intent-audit"
title = "Audit ADRs, design docs, requirements, CLI docs, and release notes against the accepted outcome"
status = "pending"
depends_on = ["behavior-performance-signoff"]

[[steps]]
id = "test-runtime-impact-audit"
title = "Audit test coverage, signoff wiring, and runtime impact for accepted changes"
status = "pending"
depends_on = ["behavior-performance-signoff"]

[[steps]]
id = "external-review"
title = "Obtain external review before release preparation or publish authorization"
status = "pending"
depends_on = [
  "design-doc-intent-audit",
  "test-runtime-impact-audit",
]

[[steps]]
id = "release-candidate-prep"
title = "Prepare PR/release-candidate artifacts only after external review and explicit authorization"
status = "pending"
depends_on = ["external-review"]

[[steps]]
id = "closeout-artifacts"
title = "Move durable results out of the active plan and delete the plan before release"
status = "pending"
depends_on = ["release-candidate-prep"]

[[exit_criteria]]
id = "ec-kicad-monkey-floor-current"
title = "kicad-cruncher depends on kicad-monkey >= 2026.7.17 and the lock resolves a final release"
status = "pending"

[[exit_criteria]]
id = "ec-command-surface-inventoried"
title = "Every PCB, design-review, SVG, STEP, BOM, PnP, and project read path has an owner and cost model"
status = "pending"

[[exit_criteria]]
id = "ec-public-large-board-baselines"
title = "Large-board timing evidence is reproducible from public corpus data or synthetic fixtures"
status = "pending"

[[exit_criteria]]
id = "ec-project-api-decision"
title = "The plan records where Cruncher should use project/projection/targeted APIs and where full-model APIs remain correct"
status = "pending"

[[exit_criteria]]
id = "ec-cheap-windows-implemented-or-rejected"
title = "Accepted cheap Cruncher optimizations are implemented and tested, or rejected with measured rationale"
status = "pending"

[[exit_criteria]]
id = "ec-monkey-followups-recorded"
title = "Library-level improvements that belong in kicad-monkey are recorded as future upstream work"
status = "pending"

[[exit_criteria]]
id = "ec-behavior-preserved"
title = "Accepted changes preserve public command output contracts, mutation safety, and rendering semantics"
status = "pending"

[[exit_criteria]]
id = "ec-signoff-green"
title = "Relevant focused tests, L99 signoff, and full dev-std audit pass before release preparation"
status = "pending"

[[exit_criteria]]
id = "design-doc-intent-audit"
title = "ADRs, design docs, requirements, CLI docs, and release notes match the accepted outcome"
status = "pending"

[[exit_criteria]]
id = "test-runtime-impact-audit"
title = "Test additions and runtime impact are reviewed with recorded evidence"
status = "pending"

[[exit_criteria]]
id = "external-review"
title = "External review approves research, implementation scope, and release-facing artifacts before release authorization"
status = "pending"

[[exit_criteria]]
id = "ec-no-release-without-authorization"
title = "No tag, GitHub Release, PyPI publish, or public issue response occurs without explicit user authorization"
status = "pending"
+++

# KiCad Cruncher Large-Board Profile And API Decision Plan

Status: active; branch created, execution not started.

This plan tracks a `kicad_cruncher` performance and API-fit effort for large
board workflows. It is a working artifact only. Per ADR-0003, durable results
must move into ADRs, design docs, CLI docs, requirements, release notes,
contracts, or tests before release, and this plan must be deleted at closeout.

## Strategy

Start by repinning `kicad-monkey` to the latest released floor,
`kicad-monkey>=2026.7.17`, and refreshing `uv.lock` so every timing and API
decision uses the current Monkey parser/projection/rendering behavior.

Then inventory and profile the Cruncher workflows that touch large PCB or
project data. The goal is to find cheap, low-risk command-level improvements
inside Cruncher and to decide which slow paths instead belong in future
`kicad_monkey` work.

No implementation slice should start until the command inventory, large-board
baselines, project-API fit research, candidate selection, and independent
research review are complete. Reviewer approval of research or implementation
is not release authorization; PR push, issue comments, tags, GitHub Releases,
and PyPI publishing require explicit user authorization.

## Known Inputs

`kicad-monkey` `2026.7.17` is the required input release. Its
`docs/guides/project-workflows.html` guide is the durable upstream guidance for
choosing between full design objects, PCB projection, targeted schematic
readers, IR/SVG rendering, and project-level workflows.

First-pass source reconnaissance found these Cruncher surfaces to profile:

- `design`, `design-review`, and `dr`: load `KiCadDesign`, write design JSON,
  netlist JSON, KiCad S-expression netlist, schematic SVGs, and one PCB review
  SVG per copper layer. Current PCB review rendering calls `pcb.to_svg(...)`
  per copper layer.
- `pcb-svg`: loads `KiCadPcb` and already has a command-scoped
  `PcbSvgCompositionRenderCache` that caches a base board IR and layer-filtered
  root SVGs for configured views.
- `pcb clean`, daemon, and plugin mutation requests: load `KiCadPcb` and scan
  layer definitions, footprint-local graphics, board graphics, generated items,
  and value fields. Apply mode requires a mutable full model.
- `pcb-layer-step`: loads full PCB geometry, derives board regions and copper
  bodies, then sends planar STEP requests to Geometer.
- BOM, PnP, JLC, and manufacturing helpers: load `KiCadDesign`, call Monkey
  assembly/variant APIs, and may load the board for placement data.

## Goals

- Establish honest large-board baselines for user-visible Cruncher workflows:
  design-review/DR, PCB SVG composition, PCB clean dry-run/apply planning,
  PCB layer STEP generation, and manufacturing outputs where board load matters.
- Determine where Cruncher should use `KiCadDesign` project APIs, PCB
  projection, targeted readers, or existing full-model APIs.
- Find cheap wins such as reusing a parsed board/design object, reusing cached
  IR/SVG render state, avoiding repeated broad renders, respecting config
  feature selections before expensive work, or documenting cheaper read paths.
- Record cases where projection is not a win because the workflow needs broad
  or mutable board state.
- Update CLI and API guidance so users understand which commands necessarily
  parse the full board and which workflows can be made narrow.
- Produce a future upstream Monkey backlog for parser, projection, IR/SVG,
  pull-parser, native, or API additions that Cruncher should not own.

## Non-Goals

- Do not change `kicad_monkey` in this plan.
- Do not introduce native extensions, Cython, C/C++, or new packaging
  complexity in Cruncher. Native and pull-parser acceleration belongs to
  upstream Monkey evaluation unless research proves a Cruncher-only wrapper
  need.
- Do not hide partial parsing behind existing full-output APIs when the output
  contract requires full design JSON, full netlist data, full mutable PCB
  state, or full SVG rendering.
- Do not use private board timings as the only evidence for a decision or
  release note.
- Do not publish a release from this branch without explicit authorization.

## Research Questions

- For `design/dr`, how much wall time is spent in `KiCadDesign.from_file()`,
  `to_json()`, netlist generation, schematic SVG rendering, and repeated PCB
  copper-layer SVG rendering?
- Can the existing PCB SVG compositor cache or a small shared helper eliminate
  repeated PCB IR/render setup in `design/dr` without changing output contracts?
- For `pcb-svg`, after the Monkey 2026.7.17 improvements, is the bottleneck
  board parse, base IR creation, XML pruning/copying, virtual layer synthesis,
  HLR/STEP projection, or file output?
- For `pcb clean`, can dry-run or mutation-request planning use projection or
  targeted inventory safely, or does apply/mutation safety require the full
  mutable `KiCadPcb` model?
- For `pcb-layer-step`, can feature/config selection avoid collecting unused
  geometry families before Geometer handoff, or is full board hydration the
  correct cost for the configured output?
- For BOM/PnP/JLC/manufacturing commands, are there cases where project APIs
  can avoid board load when only schematic or netlist output is requested?
- Which improvements are stable enough to test as regressions, and which
  should remain measured guidance only?

## Candidate Cheap Windows

Research must at least evaluate these candidates:

- Use the PCB SVG compositor cache or an equivalent command-scoped render cache
  in `design/dr` for the per-copper-layer PCB review SVG loop.
- Ensure `pcb-svg` only asks Monkey/Geometer for families needed by selected
  views and configured virtual layers.
- Avoid repeated board/project path resolution and repeated parse work across a
  single command invocation.
- Add progress or timing hooks where users currently see long silent work on
  large boards.
- Use projection or targeted readers only for narrow, read-only inventory
  workflows where measured baselines beat full parse.
- Keep full-model APIs for mutable commands, broad render outputs, design JSON,
  netlist generation, and STEP generation unless measured evidence says
  otherwise.
- Move deeper parser, projection, IR/SVG, pull-parser, or native acceleration
  needs to a Monkey follow-up backlog.

## Implementation Gates

Each candidate selected for implementation must have:

- baseline timing evidence from at least three rounds where practical;
- public corpus or synthetic reproduction instructions;
- behavior-preserving tests focused on the changed path;
- no output contract drift unless design docs and tests approve the change;
- an explicit accept/reject/defer decision in the plan log.

Conditional steps such as `cruncher-cheap-window-implementation` can be marked
done by implementation or by a rejection log if research shows no Cruncher-local
change should land. That convention keeps the dependency chain honest without
pretending rejected candidates were implemented.

## Validation Plan

Research validation:

- record command-surface inventory in a plan log;
- capture fresh baselines on this branch after `kicad-monkey>=2026.7.17`;
- compare full-model, project-level, projection, and targeted-reader choices
  only where the output contract makes those choices plausible;
- have an independent agent reproduce the research before implementation.

Implementation validation:

- focused tests for each accepted changed path;
- representative command smoke tests for `design/dr`, `pcb-svg`, `pcb clean`,
  and `pcb-layer-step` when affected;
- `uv run dev-std audit . --format json`;
- `uv run --extra test python tests/rack.py run L99_signoff`;
- broader L0/L3/Rack checks when command behavior, docs, or output contracts
  change.

## Closeout Expectations

Before release preparation:

- move durable decisions into ADRs, design docs, CLI docs, requirements,
  release notes, or tests;
- record public performance evidence and any caveats in release-facing docs;
- record deferred Monkey work in durable requirements or issues;
- delete this active plan and any transient plan logs that should not ship;
- obtain external review and explicit release authorization.
