<!-- markdownlint-disable -->

# Hardening Report: python-semantic-release--python-semantic-release--/v10.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **python-semantic-release--python-semantic-release--/v10.6.1** was hardened automatically. 7 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Evaluate | Detect if any of the combinations of file sets have changed' step interpolates ${{ steps.core-changed-files.outputs.build_any_changed }}, ${{ steps.ci-changed-files.outputs.ci_any_changed }}, and other steps.*.outputs.* expressions directly inside a run: shell command string.

Locations:

- `.github/workflows/ci.yml:64`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Evaluate | Detect if any of the combinations of file sets have changed' step interpolates ${{ steps.core-changed-files.outputs.build_any_changed }}, ${{ steps.ci-changed-files.outputs.ci_any_changed }}, and other steps.*.outputs.* expressions directly inside a run: shell command string.

Locations:

- `.github/workflows/cicd.yml:48`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Setup | Force release branch to be at workflow sha' step uses 'git reset --hard ${{ github.sha }}' directly inside a run: shell command string.

Locations:

- `.github/workflows/cicd.yml:124`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Setup | Force correct release branch on workflow sha' step uses 'git checkout -B ${{ github.ref_name }}' directly inside a run: shell command string.

Locations:

- `.github/workflows/validate.yml:100`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Build | Annotate next version' step uses ${{ steps.version.outputs.tag }} directly inside a run: shell command string: printf '%s\n' "::notice::Next release will be '${{ steps.version.outputs.tag }}'"

Locations:

- `.github/workflows/validate.yml:129`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Test | Run pytest -m e2e --comprehensive' step (test-linux job) uses '--cov=${{ steps.install.outputs.PKG_INSTALLED_DIR }}' directly inside a run: shell command string.

Locations:

- `.github/workflows/validate.yml:261`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. The 'Test | Run pytest -m e2e' step (test-windows job) uses '--cov=${{ steps.install.outputs.PKG_INSTALLED_DIR }}' directly inside a run: shell command string.

Locations:

- `.github/workflows/validate.yml:335`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed all 7 script injection findings across 3 workflow files:

1. ci.yml (line 64): Moved 7 `steps.*.outputs.*` expressions from the run: block into an env: block (BUILD_ANY_CHANGED, CI_ANY_CHANGED, DOCS_ANY_CHANGED, SRC_ANY_CHANGED, TESTS_ANY_CHANGED, GHA_SRC_ANY_CHANGED, GHA_TESTS_ANY_CHANGED). Shell script now references plain env vars.

2. cicd.yml (line 48): Same fix as ci.yml - moved 7 `steps.*.outputs.*` expressions into an env: block.

3. cicd.yml (line 124): Moved `${{ github.sha }}` into env: block as WORKFLOW_SHA; `git reset --hard "$WORKFLOW_SHA"` now uses the env var.

4. validate.yml (line 100): Moved `${{ github.ref_name }}` into env: block as REF_NAME; `git checkout -B "$REF_NAME"` now uses the env var.

5. validate.yml (line 129): Moved `${{ steps.version.outputs.tag }}` into env: block as NEXT_TAG; printf now uses `$NEXT_TAG`.

6. validate.yml (line 261, test-linux): Moved `${{ steps.install.outputs.PKG_INSTALLED_DIR }}` into env: block as PKG_INSTALLED_DIR; pytest `--cov` argument now uses `"$PKG_INSTALLED_DIR"`.

7. validate.yml (line 335, test-windows): Moved `${{ steps.install.outputs.PKG_INSTALLED_DIR }}` into env: block as PKG_INSTALLED_DIR; PowerShell pytest `--cov` argument now uses `"$env:PKG_INSTALLED_DIR"`.

