# Oracle Health Check --- Project Status

**Last updated:** 2026-11-08\
**Project:** `OracleHC_PY`\

## 1. Project overview

A Windows desktop Oracle database health-check and reporting application
built with Python and PySide6.

The GUI must preserve existing database checks, SQL execution, report
generation, and deployment behaviour while allowing the interface to be
redesigned independently.

## 2. Current technology

-   Python
-   PySide6 / Qt
-   Oracle database connectivity and SQL health checks
-   PyInstaller for Windows executable packaging

Use the existing project environment and dependencies. Do not add
dependencies unless necessary and justified.

## 3. Current GUI pages and design scope

The current redesign scope is limited to:

1.  **Option 1 --- Oracle Health Check**
2.  **Option 2 --- Sync Check**
3.  **Option 3 --- All Checks**

Other pages are out of scope for this redesign. Their visual design will
be discussed separately.

### Shared visual requirement

All three pages must use **one identical shared wave design**, based on
the supplied reference screenshot.

The target wave should match the reference as closely as possible: -
Same curve and position. - Two soft, overlapping layers. - Pale pink /
Oracle-red tint. - Subtle transparency and smooth fading. - No hard edge
or solid-looking colour block.

Do not create different wave variants for the three pages. Do not simply
reuse the current wave if it does not match the reference.

Light and dark themes must both work. Preserve readability of headings,
subtitles, cards, and controls.

## 4. Completed work

### Batch 5A.4 --- Oracle Health Check page wave refinement

-   Applied on the user's Windows development machine.
-   Files replaced:
    -   `ohc/gui/run_skin.py`
    -   `tests/test_run_page_health.py`
-   Backup created: `backup_batch5a4_20261108_205525`
-   Focused Windows test result: **25 tests passed**.
-   The user launched the application and reviewed the resulting page
    screenshot.

**Important:** Batch 5A.4 was applied and its focused tests passed, but
the current wave has subsequently been judged unsatisfactory. It is not
the final approved shared wave design. Preserve a rollback path until
the replacement is accepted.

## 5. Next task

Redesign **Option 2 --- Sync Check**, while also correcting the shared
wave across Options 1, 2, and 3.

Before implementation: 1. Inspect the current source and understand page
construction, theme propagation, navigation, and shared styling. 2.
Identify the smallest set of files that must change. 3. Confirm the
implementation plan before editing.

Implementation constraints: - Keep one shared wave implementation for
Options 1, 2, and 3. - Keep the three pages visually consistent. -
Preserve all existing page functionality and execution logic. - Preserve
existing Sync Check progress-counting behaviour. - Do not redesign other
pages. - Avoid unrelated refactoring and unnecessary dependencies.

## 6. Relevant source files

Start by inspecting:

-   `ohc/gui/run_page.py` --- page UI for health check, sync check, and
    all checks.
-   `ohc/gui/run_skin.py` --- shared page skin, background, and wave
    painting.
-   `ohc/gui/main_window.py` --- page creation and theme propagation.
-   `ohc/gui/sidebar.py` --- navigation and page selection.
-   `tests/test_gui_run.py` --- GUI behaviour tests.
-   `tests/test_layout.py` --- layout tests.
-   `tests/test_run_page_health.py` --- health-check page and skin
    tests.

Request additional files only when a concrete dependency requires them.
The full project ZIP is not required if relevant source files are
supplied.

## 7. Validation requirements

Do not claim visual equivalence based only on numeric pixel samples or a
few selected pixels.

Before packaging: - Render the actual PySide6 application. - Capture all
three pages at comparable dimensions. - Compare the shared wave against
the supplied reference image. - Check both light and dark themes. -
Confirm the wave does not obstruct text, cards, or controls. - Run
relevant existing tests and new focused tests. - Report exactly which
tests were run and where. - Provide before/after screenshots for visual
review.

The user's Windows environment is the final validation environment.
Linux-only test results do not replace Windows testing.

## 8. Patch and rollback rules

-   Deliver changes as a small, reversible patch ZIP.
-   Include clear apply and rollback instructions.
-   Verify baseline file hashes before applying patches.
-   Never recommend `--force` to bypass a baseline mismatch without
    investigating it.
-   Keep backups until the user approves the visual result.
-   Do not modify unrelated files.

## 9. Current status / open items

-   [x] Batch 5A.4 applied.
-   [x] Focused Windows tests passed: 25/25.
-   [ ] Final shared wave design approved.
-   [ ] Sync Check redesign implemented.
-   [ ] All Checks page verified with the shared wave.
-   [ ] Light and dark theme visual checks completed for all three
    pages.
-   [ ] Relevant regression tests completed.
-   [ ] Final patch approved by the user.
