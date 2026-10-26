---
id: TKT-001
title: Fix sort icon references on clean consumer imports
status: closed
done: true
blocked: false
priority: high
requirements:
  - DEPS-01
owner: gpt-6-luna (high)
thread_id: 01a176b5-e413-75c3-9a9f-a6a9c729de3c
branch: codex/tkt-001-sort-icon-uids
worktree: C:/Users/Alex/.codex/worktrees/2853/table-view
created: 2026-10-26
updated: 2026-10-26
---

# TKT-001 — Fix sort icon references on clean consumer imports

## Goal and requirement coverage

Keep the existing TableView theme/icons and public behavior while removing two
stale external-resource UID hints that warn on a fresh consumer import/load.
This is the owned-fork source prerequisite for NetworkedGameProject TKT-211.
DEPS-01 is the consuming project's existing reproducible-install/source-graph
contract, defined at
https://github.com/chalecampb/NetworkedGameProject/blob/main/docs/reqs/REQUIREMENTS_DEPENDENCIES.md.
No new table feature or duplicate product requirement is introduced.

## Authority and ownership

- Alex explicitly requested on 2026-10-26: “make a fork under chalecampb and
  make the necessary changes there.”
- Owning fork: https://github.com/chalecampb/table-view, fork of
  https://github.com/4d49/table-view. Fork base is the already reviewed upstream
  commit `101c56bf0a624820ffe6de606ecf947593fda13d`.
- Original Sol orchestrator/reviewer: `01a17198-0d85-7e82-8cd0-cacc8ef4b768`
  on local. The existing TKT-211 Luna/High chat
  `01a176b5-e413-75c3-9a9f-a6a9c729de3c` is the sole implementation owner for
  this companion repository ticket as well; do not create a third Luna chat.
- One independently reviewed fork PR, then the existing parent PR #135 consumes
  the exact reviewed/merged fork revision. No PR/issue to 4d49 is authorized.
- Preserve the MIT license, notices, all upstream distribution files and paths.

## Diagnosis and baseline evidence

`resources/table_view.tres` references its two sort SVGs by valid resource paths
and obsolete optional UIDs `uid://clfgc3v3th3uu` and `uid://2pb3n8g6ox3n`.
The source package does not ship SVG import metadata owning those IDs. A clean
Godot 4.8.dev.custom_build.63b7e0272 import generates the SVG metadata but leaves
the original theme declarations unchanged. A subsequent theme load succeeds
through the file paths while producing two invalid-UID fallback warnings.

Canonical baseline diagnostic (before any TableView source correction) is in
the parent owner worktree's ignored
`build/test-results/tkt211-addon-diagnostic-summary.json` and log. It imported
then loaded pristine pinned packages. Direct inspection after that import
confirmed both SVG .import files exist and both original theme UID attributes
remain. Do not replace this evidence with a claim that normal import repairs
the theme. Repeating normal import/load should also be checked as requested by
Alex, recording any actual source changes and warnings rather than assuming.

## Approved implementation plan — Sol, 2026-10-26

1. Move this ticket to tickets/inprog, fill owner/branch/worktree/PR metadata,
   preserve the baseline above and execute the canonical validation table in
   docs/TESTING.md before changing the resource. This is a metadata compatibility
   correction; existing parent canonical consumer behavior tests remain
   tests/ui/developer/test_entity_view.gd. Do not add a test framework or source
   scanner merely to assert these two lines.
2. Change only the two Texture2D ext_resource declarations in
   resources/table_view.tres: remove each optional uid attribute; retain type,
   res:// path, external ID, theme's own UID, all styles and references.
   Do not add generated .import files, modify SVGs/scripts or rename resources.
3. No generic source patching, byte manifest, Git submodule or package manager
   change belongs in this fork. Normal Godot import is responsible for cache;
   the author-owned theme is fixed once in Git and consumed without a parent
   overlay. Do not create a nested project.godot in the distributable root.
4. Run the canonical native clean/repeated-import validation from this fork's
   completed source using a disposable ignored consumer host under build/.
   Keep generated imports, logs, summaries and temporary scripts uncommitted.
   If needed add only build/ and .godot/ ignore rules to .gitignore in the PR;
   preserve existing upstream rules and all licensed content.
5. Commit source, push codex/tkt-001-sort-icon-uids and open exactly one PR to
   chalecampb/table-view:main, explicitly setting the base repository so no
   PR is opened to the third-party upstream. Link this ticket, parent TKT-211,
   DEPS-01, tested implementation SHA and compact native packet.
6. Move ticket to review only after native checks pass; append exact commands,
   Godot version, source before/after import observation, both icon-load results,
   console warning/error counts and ignored artifact paths. Distinguish tested
   implementation from later ticket-only evidence. This fork has no existing
   CI or full suite; record that limit and do not invent green required checks.
7. Original Sol alone reviews and approves exact head, merges through GitHub,
   compares reviewed/merged trees and closes this ticket audit-only. Sol then
   supplies the immutable fork merge revision for parent integration. The Luna
   owner does not merge or pick an unreviewed parent pin.

## Acceptance criteria and canonical validation

- [x] The two optional stale UID hints are removed; stable paths and complete
  upstream resource/code/license contents remain intact.
- [x] Native clean import and subsequent theme/icon load succeed with zero
  warnings/errors; both sort icons are non-null Texture2D resources.
- [x] A second normal import/load also succeeds without warnings or authoring
  source changes. Record whether baseline import repaired anything.
- [x] Bounded source diff and whitespace checks pass; generated metadata and
  test evidence remain untracked/ignored.
- [x] Exact-head Sol approval, GitHub merge and tree equivalence/mismatch checks
  are recorded before audit-only closure and immutable parent-pin handoff.

Canonical test table: docs/TESTING.md, rows TV-CLEAN, TV-REPEAT, TV-SCOPE.
The actual Game's TableView consumer/full acceptance stays in the separate
NetworkedGameProject integration ticket. No new standalone gameplay behavior
or serialization contract is introduced by this source metadata correction.

## Compatibility, failure and recovery

- Icons already load via stable-path fallback in the baseline; preserve output
  and sorting behavior. This change removes invalid references and diagnostics.
- A failed import or missing icon remains a failure. Do not suppress warnings,
  weaken the parent source gate, reintroduce local parent patches, regenerate
  authored metadata on setup, or upgrade unrelated TableView code.
- If the two UID removals do not produce the specified result, return exact
  compact diagnostics to original Sol. Do not broaden scope.

## Task continuity

- Parent TKT-211 / PR: https://github.com/chalecampb/NetworkedGameProject/pull/135.
- Sol / local: 01a17198-0d85-7e82-8cd0-cacc8ef4b768.
- Persistent Luna / local: 01a176b5-e413-75c3-9a9f-a6a9c729de3c.
- Branch: codex/tkt-001-sort-icon-uids.
- Planned companion worktree: C:/Users/Alex/.codex/worktrees/2853/table-view.
- Fork PR: [chalecampb/table-view #1](https://github.com/chalecampb/table-view/pull/1),
  current head `f296902df85c73fe96d909c502b853b677ee9139`.
- Tested implementation SHA: `f296902df85c73fe96d909c502b853b677ee9139`.
- Review/merge SHA and consumed parent pin: pending Sol approval/merge.
- Reuse this exact owner/branch/PR for corrections. Final Sol review input is
  current approved ticket/test table, exact PR head/delta, compact evidence and
  unresolved findings; no historical narrative or full raw logs.

## Implementation dispatch

- Started 2026-10-26 under the existing TKT-211 Luna/High owner chat
  `01a176b5-e413-75c3-9a9f-a6a9c729de3c`; original Sol
  `01a17198-0d85-7e82-8cd0-cacc8ef4b768` retains review/merge authority.
- Fork: `chalecampb/table-view`, branch
  `codex/tkt-001-sort-icon-uids`, worktree
  `C:/Users/Alex/.codex/worktrees/2853/table-view`; base planning commit
  `0c17e2b` from reviewed `main`.
- Fork PR: [chalecampb/table-view #1](https://github.com/chalecampb/table-view/pull/1);
  current head `f296902df85c73fe96d909c502b853b677ee9139`. Do not merge or
  update the parent pin before Sol's exact-head review and merged immutable
  revision handoff.

## Confirmed test execution

- 2026-10-26, pristine fork baseline before source change:
  `rtk python "C:\Users\Alex\.codex\worktrees\2853\NetworkedGameProject\tools\testing\opaque.py" --result-stem tkt001-baseline --summary build/test-results/tkt001-baseline-summary.json --log build/test-results/tkt001-baseline.log -- rtk python build/tkt001-native-diagnostic.py --reuse-host`
- Result: diagnostic process PASS (all four native commands exit 0), baseline
  acceptance FAIL as expected because each theme load emits two UID fallback
  warnings. On Godot `4.8.dev.custom_build.63b7e0272`, the clean editor import
  and repeated editor import each had zero warning/error diagnostics. The first
  and repeated theme loads succeeded; both `sort_ascending` and `sort_descending`
  were non-null `Texture2D` values, with exactly two warnings per load.
- Source observation: `resources/table_view.tres` SHA-256 was
  `d23a1d094681f62b643a7c95f86e96fbafba75522e6b133dbd65fb26c5b01481`
  before clean import, after clean import, after first load, after repeated
  import, and after repeated load. Import does not rewrite either UID attribute.
- Requirements covered: `DEPS-01` consumer compatibility diagnostic; this is
  the recorded pre-fix baseline, not candidate acceptance. Summary and raw log
  remain under ignored `build/test-results/`; disposable consumer host and
  probe remain under ignored `build/`.
- Planning-only upload `0c17e2b` contained ticket and validation specification;
  no executable source/resource/test changes were present then.

### Candidate validation — tested implementation `f296902df85c73fe96d909c502b853b677ee9139`

- Exact command:
  `rtk python "C:\Users\Alex\.codex\worktrees\2853\NetworkedGameProject\tools\testing\opaque.py" --result-stem tkt001-candidate --summary build/test-results/tkt001-candidate-summary.json --log build/test-results/tkt001-candidate.log -- rtk python build/tkt001-native-diagnostic.py --host-name tkt001-candidate-host`
- Result: PASS, selected Godot `4.8.dev.custom_build.63b7e0272`. TV-CLEAN and
  TV-REPEAT editor imports and resource loads all exited 0 with zero
  `WARNING`, `ERROR`, or `SCRIPT ERROR` diagnostics. Both
  `sort_ascending` and `sort_descending` returned non-null `Texture2D` icons on
  both loads. Source SHA-256
  `e71b9b21a840e884e870abb1056f72b5432bad9fd7f17851ef89c24c880844b3`
  was unchanged before/after each import and load.
- TV-SCOPE: `rtk git diff --check` PASS. The tested implementation diff is
  exactly the two requested UID attribute removals in
  `resources/table_view.tres`; no generated import files or unrelated package
  source changed. `.gitignore` adds only root `/.godot/` and `/build/` ignores
  for disposable validation output.
- Fork has no existing CI or standalone full suite; none was invented or
  claimed. Candidate summary and raw log remain under ignored
  `build/test-results/`; the consumer project and probe remain under ignored
  `build/`.
- Requirements covered: `DEPS-01` consumer compatibility. Parent consumer/full
  acceptance remains in TKT-211 after Sol merges and supplies the immutable
  fork revision.

- GitHub PR: [chalecampb/table-view #1](https://github.com/chalecampb/table-view/pull/1)
- Source branch: `chalecampb/table-view:codex/tkt-001-sort-icon-uids`
- Tested implementation commit: `f296902df85c73fe96d909c502b853b677ee9139`
- Review/evidence commit: HEAD after this ticket-only update (not tested)
- Tested range: `0c17e2b..f296902df85c73fe96d909c502b853b677ee9139`
- Correction range: `none; first implementation review`
- Review/evidence range: `f296902df85c73fe96d909c502b853b677ee9139..HEAD`
- Focused: PASS — exact OPAQUE command above; summary
  `build/test-results/tkt001-candidate-summary.json`; raw log and clean/repeat
  host icon assertions retained under ignored `build/`.
- Candidate: PASS — same exact OPAQUE command and summary; fork criteria
  TV-CLEAN, TV-REPEAT, and TV-SCOPE all pass.
- Python production utilities: NOT APPLICABLE — only authored resource
  metadata and ignore rules changed.
- Annotations: NOT APPLICABLE — no GDScript source changed.
- Full: NOT APPLICABLE — no fork CI or standalone full suite exists.
- Editor/import: PASS — exact wrapped command above; Godot
  `4.8.dev.custom_build.63b7e0272`; diagnostic source: OPAQUE console capture;
  clean import 0, first load 0, repeat import 0, repeat load 0 warnings/errors.
- Failures: None in candidate. The pre-fix baseline's two warnings per load
  remain recorded above as diagnosis evidence.

## Blocker

- Blocked: No. Fork creation and source correction explicitly authorized.

## Review History

- 2026-10-26 — Original Sol approves this independently executable plan and
  the tightly scoped metadata fix. Candidate source approval remains pending.

## Verified closure — 2026-10-26 03:23 EDT

- Exact-head Sol approval for `77959c8b8cb530f23149661539d5c19f3cf2c763`:
  https://github.com/chalecampb/table-view/pull/1#issuecomment-6410517222.
- GitHub PR #1 merged using squash and expected-head protection to
  `4fd8aba754c05791fe67cb31d04896808f4fe071`.
- Reviewed head and merged primary trees both:
  `9716206606eecad274ceb902dde811369604e39c`.
- Exact comparison commands from fork root:
  `rtk git show -s --format=%T 77959c8b8cb530f23149661539d5c19f3cf2c763` and
  `rtk git show -s --format=%T 4fd8aba754c05791fe67cb31d04896808f4fe071`.
- Result: TREE_EQUIVALENT; confirmed native evidence carries forward without
  duplicate runs. Sol read the exact resource/ignore diff, canonical validation
  table, compact packet, existing diagnostic driver and named phase markers.
- TV-CLEAN/TV-REPEAT verified: four native phases exit 0, zero diagnostic
  severities, both named icons non-null on both loads, authored theme unchanged
  across each import/load. Baseline retains two warnings per load even after
  repeat import, demonstrating that import does not repair these source hints.
- TV-SCOPE: two stale UID attributes only, with root build/cache ignore rules;
  tested-to-reviewed changes are ticket-only. No generated metadata, public
  upstream report, parent overlay or unrelated source change.
- No standalone CI/full suite existed in this fork. Parent TKT-211 owns its
  independent source/style/startup/full fresh-clone acceptance after consuming
  the exact immutable fork merge `4fd8aba754c05791fe67cb31d04896808f4fe071`.
- This closure changes only ticket state/checklist and append-only audit prose,
  moving review to closed. Source, worktrees and ignored evidence are preserved.
