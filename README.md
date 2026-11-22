# Oracle Health Check

A Windows desktop application for Oracle database health checks, Data
Guard synchronization checks, and database diagnostic report generation.

> **Status:** Active development. Verify commands, dependencies,
> supported Oracle versions, and packaging instructions against the
> current source before using this as a release manual.

## Overview

The application is built with Python and PySide6. Its GUI includes:

-   **Dashboard** -- database selection, run controls, health summary,
    check results, and recent reports.
-   **Oracle Health Check** -- runs SQL-based database health checks.
-   **Sync Check** -- checks Data Guard synchronization.
-   **All Checks** -- runs the combined health-check workflow.
-   **Database Reports** -- AWR, ASH, and ADDM report interfaces.
-   **Reports & Logs** -- browses generated HTML reports and execution
    logs.
-   **Settings** -- Oracle Home, database connections, retention period,
    and theme.

The application supports light and dark themes. A simulated-data mode is
used for development and UI testing; simulated results are not evidence
of live database connectivity or database health.

## Technology

  Area                   Technology
  ---------------------- ---------------------------------------------
  Language               Python
  GUI                    PySide6 / Qt
  Database               Oracle Database
  Oracle utilities       SQL\*Plus and Data Guard Manager (`dgmgrl`)
  Health-check queries   Bundled SQL scripts
  Packaging              PyInstaller
  Tests                  Python `unittest`

The development environment has used Python 3.14 (64-bit), PySide6, and
PyInstaller. Follow the versions and dependencies specified by the
current project files.

## Requirements

### Development

-   Windows 10 or Windows 11, 64-bit.
-   A supported 64-bit Python version.
-   Git, if using the source repository.
-   Access to package repositories when installing dependencies.

### Live Oracle operations

-   A configured Oracle Home containing `sqlplus.exe`.
-   `dgmgrl.exe` for Data Guard operations.
-   Network access to the target database listener.
-   A database account with the privileges required by the selected
    checks and reports.

**Licensing:** AWR, ASH, and ADDM are associated with Oracle Diagnostics
Pack functionality. Confirm the applicable Oracle licensing before using
these features.

## Project setup

The development project has been used at:

``` text
D:\tool_exe\OracleHC_PY
```

Open PowerShell:

``` powershell
cd D:\tool_exe\OracleHC_PY
```

Create a virtual environment if one does not already exist (adjust the
Python version to the one supported by the project):

``` powershell
py -3.14 -m venv .venv
```

Activate it:

``` powershell
.\.venv\Scripts\Activate.ps1
```

Install dependencies if the repository contains `requirements.txt`:

``` powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If no dependency file exists, use the dependency versions documented by
the project. Do not guess package versions.

## Run the application

Run from the project root:

``` powershell
& .\.venv\Scripts\python.exe -m ohc
```

Running from the project root ensures Python can resolve the `ohc`
package.

## Initial configuration

1.  Open **Settings**.
2.  Set **Oracle Home** to the Oracle home containing the required
    utilities.
3.  Use **Detect** or **Browse** to locate Oracle Home.
4.  Add or select a database connection.
5.  Enter the required database connection details and credentials.
6.  Select **Test Connection** to verify connectivity.
7.  Set the report/log retention period and preferred theme.
8.  Select **Save**.

The project has used Windows DPAPI to protect locally stored
credentials. Credential files are sensitive and should not be committed
to source control or treated as portable backups.

## Main workflows

### Oracle Health Check

1.  Select the target database.
2.  Open **Oracle Health Check**, or use the Dashboard's **Run Health
    Check** action.
3.  Start the run.
4.  Review each check's status, message, and execution time.
5.  Open the generated report from **Reports & Logs**.

A previous development checkpoint contained 51 SQL scripts under
`scripts/sql`. Verify the current script count against the repository
before release.

### Data Guard Sync Check

1.  Select a database configured for the relevant Data Guard
    environment.
2.  Open **Sync Check**.
3.  Run the synchronization check.
4.  Review the actual status and messages.

### All Checks

Use **All Checks** to run the combined health-check workflow. Review the
individual results and generated report.

### Database Reports

The Database Reports section provides:

-   **AWR** -- Automatic Workload Repository.
-   **ASH** -- Active Session History.
-   **ADDM** -- Automatic Database Diagnostic Monitor.

Choose the database and report parameters shown by the application, then
generate the report. Availability depends on database configuration and
account privileges. Confirm Oracle Diagnostics Pack licensing before
using these features.

### Reports & Logs

This page allows users to:

-   Refresh report and execution-log lists.
-   Filter entries by date.
-   Search filenames.
-   Open generated HTML reports.
-   View execution logs.

The application has a configurable retention period. Confirm the current
storage and retention implementation in source before manually deleting
files.

## Project structure

High-level structure (not a complete file inventory):

``` text
OracleHC_PY/
├── ohc/
│   ├── gui/                 # PySide6 pages, widgets, theme and main window
│   └── ...                  # Application and health-check modules
├── scripts/
│   └── sql/                 # SQL health-check scripts
├── tests/                   # Automated tests
├── tools/                   # Development and screenshot utilities
├── .venv/                   # Local virtual environment; do not commit
└── README.md
```

Important GUI modules have included:

-   `ohc/gui/main_window.py` -- main window and navigation.
-   `ohc/gui/dashboard_page.py` -- Dashboard.
-   `ohc/gui/run_page.py` -- health-check run pages.
-   `ohc/gui/reports_page.py` -- Reports & Logs.
-   `ohc/gui/settings_page.py` -- Settings.
-   `ohc/gui/awr_page.py` -- AWR interface and Database Reports
    navigation.
-   `ohc/gui/ash_page.py` -- ASH interface.
-   `ohc/gui/addm_page.py` -- ADDM interface.
-   `ohc/gui/run_skin.py` -- shared page styling and backdrop.

## Tests

The project uses `unittest`. Run commands from the project root.

Run one test module:

``` powershell
& .\.venv\Scripts\python.exe -m unittest tests.test_dashboard_page -v
```

Run focused Dashboard tests:

``` powershell
& .\.venv\Scripts\python.exe -m unittest tests.test_dashboard_skin tests.test_dashboard_page -v
```

Run the full test suite:

``` powershell
& .\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

Some GUI tests take longer because they create Qt widgets and test
page/layout behaviour. Run focused tests first, then the broader
regression suite before release.

### Screenshot capture

Some development batches provide screenshot utilities under `tools/`.
For example:

``` powershell
$env:PYTHONPATH = (Get-Location).Path
& .\.venv\Scripts\python.exe tools\capture_dashboard_shots.py
```

Run capture scripts from the project root and confirm that the named
script exists in the current checkout.

## Build and packaging

The project has used PyInstaller. The exact build command, SQL/resource
inclusion rules, and runtime configuration must be taken from the
current packaging configuration or build script.

Before release:

1.  Run focused tests and the full regression suite.
2.  Verify live connectivity against a suitable test database.
3.  Verify report generation and output locations.
4.  Test on a clean Windows environment without the development virtual
    environment.
5.  Confirm required SQL scripts and other resources are included in the
    executable.
6.  Confirm credentials, logs, reports, and settings are stored
    appropriately.

A successful development run does not prove that a packaged executable
is complete.

## Security and operational notes

-   Never commit database passwords, credential files, private keys, or
    production connection details.
-   Do not commit production logs, reports, or customer data.
-   Use least-privilege database accounts.
-   Confirm Oracle licensing before using AWR, ASH, or ADDM.
-   Distinguish live database results from simulated development data.
-   Back up files before applying source patches.
-   Never bypass patch baseline/hash checks.

## Development workflow

For each focused change:

1.  Identify the files and behaviour in scope.
2.  Preserve existing SQL, database, report-generation, and credential
    behaviour unless explicitly approved.
3.  Make the smallest practical change.
4.  Add or update focused tests.
5.  Run focused tests locally.
6.  Inspect the real application in light and dark themes where
    relevant.
7.  Run broader regression tests before release.
8.  Update this README and `PROJECT_STATUS.md` when decisions or
    behaviour change.
9.  Commit completed work with a clear Git message.

## Current design and development notes

-   The shared pink page wave in `ohc/gui/run_skin.py` has been visually
    approved for redesigned pages. Do not change it without explicit
    approval.
-   Reports & Logs uses distinct visual treatments for SUCCESS, WARNING,
    CRITICAL, and CANCELLED.
-   The Dashboard status summary should show a grey empty-state donut
    before checks run, then a data-driven green/amber/red donut with
    counts and percentages after results exist.
-   Settings and Reports & Logs have undergone GUI redesign work.
-   Keep approved GUI areas stable while fixing isolated defects.

## Troubleshooting

### `ModuleNotFoundError: No module named 'ohc'`

Run from the project root:

``` powershell
cd D:\tool_exe\OracleHC_PY
```

For standalone scripts under `tools`, set the project root on
`PYTHONPATH`:

``` powershell
$env:PYTHONPATH = (Get-Location).Path
```

### Oracle utilities are not found

-   Recheck Oracle Home in Settings.
-   Confirm `sqlplus.exe` exists under that path.
-   Confirm `dgmgrl.exe` exists if running Data Guard operations.
-   Use the Settings page's Detect or validation action.

### Database connection fails

-   Check host, listener port, service name/SID, and network access.
-   Confirm the database account and required privileges.
-   Use **Test Connection** and inspect the resulting message and
    execution log.

### Tests fail after applying a patch

-   Read the first failing assertion and traceback.
-   Check that the patch was applied to the expected baseline.
-   Do not force an incremental patch over unexpected file hashes.
-   Use the patch's documented rollback procedure if necessary.

## Maintainers

Update this README when dependencies, supported Oracle versions, setup,
SQL bundles, workflows, packaging, or security behaviour changes.

This README is a project-level development guide. It is not a substitute
for the current source code, Oracle documentation, or your
organization's database and licensing procedures.
