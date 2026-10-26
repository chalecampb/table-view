# Fork compatibility validation

This fork retains upstream TableView's public behavior. The consuming project's
TKT-211/DEPS-01 needs valid theme references after a clean Godot import; the
two-stale-UID correction is owned here. There is no existing fork CI/full test
suite. Use the native validation below and retain actual commands/results in
the active ticket. The parent separately runs its source, startup, canonical
consumer and complete native integration gates.

| Canonical case | Procedure | Expected result |
| --- | --- | --- |
| TV-CLEAN | Copy the selected complete fork source to addons/table-view in a disposable consumer host with no .godot cache or prior SVG .import files. Run normal native import, then load the TableView theme and its two named sort icons. | Import/load exit 0; zero Godot WARNING, ERROR or SCRIPT ERROR; theme and both Texture2D icons are non-null. |
| TV-REPEAT | Run normal import and the same load again; compare the authored resources/table_view.tres before/after each import. | Exit 0, zero diagnostics, icons present, authored theme unchanged. Record the pristine pre-fix baseline separately rather than claiming import repaired its old hints. |
| TV-SCOPE | Review Git diff/whitespace and status. | Only the approved two UID attribute removals plus deliberate ticket/docs/ignore configuration; no generated imports, cache, logs or unrelated source edits. |

Use the explicitly selected native Windows executable in GODOT_BIN. Commands
from the disposable consumer project are equivalent to:

```powershell
& $env:GODOT_BIN --headless --editor --path <consumer-root> --import
& $env:GODOT_BIN --headless --path <consumer-root> --script res://theme_probe.gd
```

The disposable probe extends SceneTree, loads
res://addons/table-view/resources/table_view.tres as Theme and checks
get_icon("sort_ascending", "TableView") and
get_icon("sort_descending", "TableView") are non-null Texture2D values. Exit
nonzero on a missing theme/icon. Capture console severities as well as exit
code; an exit-zero fallback warning is a failed clean-import criterion.

Run native commands through the already available NetworkedGameProject OPAQUE
wrapper in this coordinated task, using distinct fork result stems and ignored
artifact paths. The temporary host/driver may live under ignored build/;
these diagnostic fixtures are not a second production implementation and are
not committed. Record the exact outer wrapper command and inner Godot argument
arrays in the fork ticket so the result is independently reviewable.

The baseline two warnings and successful path-fallback load were observed on
Godot 4.8.dev.custom_build.63b7e0272. Do not infer results for untested engines
or platforms. Native icon/theme validation is programmatic, without screenshots
or human visual acceptance.
