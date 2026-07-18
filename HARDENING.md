<!-- markdownlint-disable -->

# Hardening Report: python-semantic-release--python-semantic-release/v10.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **python-semantic-release--python-semantic-release/v10.6.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: ${{ }} expressions are directly interpolated inside run: shell command strings in ci.yml. The step 'Evaluate | Detect if any of the combinations of file sets have changed' uses ${{ steps.core-changed-files.outputs.build_any_changed }}, ${{ steps.ci-changed-files.outputs.ci_any_changed }}, and other steps.*.outputs.* expressions directly inside a bash if-condition within a run: block. These values flow through YAML template substitution before the shell parses them, enabling shell metacharacter injection.

Locations:

- `.github/workflows/ci.yml:64`

### script-injection (severity: high)

Rule (a) violation: ${{ }} expressions are directly interpolated inside run: shell command strings in cicd.yml. (1) The step 'Evaluate | Detect if any of the combinations of file sets have changed' uses ${{ steps.core-changed-files.outputs.build_any_changed }}, ${{ steps.ci-changed-files.outputs.ci_any_changed }}, and other steps.*.outputs.* expressions directly inside a bash if-condition within a run: block. (2) The step 'Setup | Force release branch to be at workflow sha' uses `git reset --hard ${{ github.sha }}` directly in a run: block. Any ${{ ... }} expression inside a run: script is a script-injection risk.

Locations:

- `.github/workflows/cicd.yml:48`
- `.github/workflows/cicd.yml:113`

### script-injection (severity: high)

Rule (a) violation: Multiple ${{ }} expressions are directly interpolated inside run: shell command strings in validate.yml. (1) 'Setup | Force correct release branch on workflow sha' uses `git checkout -B ${{ github.ref_name }}` in a run: block. (2) 'Build | Annotate next version' uses `printf '%s\n' "::notice::Next release will be '${{ steps.version.outputs.tag }}'"` in a run: block. (3) 'Test | Run pytest -m e2e --comprehensive' (Linux) uses `--cov=${{ steps.install.outputs.PKG_INSTALLED_DIR }}` in a run: block. (4) 'Test | Run pytest -m e2e' (Windows) uses `` `--cov=${{ steps.install.outputs.PKG_INSTALLED_DIR }} `` in a run: block.

Locations:

- `.github/workflows/validate.yml:100`
- `.github/workflows/validate.yml:129`
- `.github/workflows/validate.yml:248`
- `.github/workflows/validate.yml:358`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection vulnerabilities in three workflow files:

1. **ci.yml** (line 64): Moved 7 `${{ steps.*.outputs.* }}` expressions (build_any_changed, ci_any_changed, docs_any_changed, src_any_changed, tests_any_changed, gha_src_any_changed, gha_tests_any_changed) from the bash if-condition into the step's `env:` block, referencing them as plain env vars ($BUILD_ANY_CHANGED, etc.).

2. **cicd.yml** (line 48): Same fix as ci.yml for the eval-changes job's detect step. (line 113): Moved `${{ github.sha }}` from `git reset --hard ${{ github.sha }}` into `env: WORKFLOW_SHA: ${{ github.sha }}` and used `git reset --hard "$WORKFLOW_SHA"`.

3. **validate.yml** (line 100): Moved `${{ github.ref_name }}` from `git checkout -B ${{ github.ref_name }}` into `env: REF_NAME:` and used `git checkout -B "$REF_NAME"`. (line 129): Moved `${{ steps.version.outputs.tag }}` from the printf command into `env: RELEASE_TAG:` and used `'$RELEASE_TAG'`. (line 248): Moved `${{ steps.install.outputs.PKG_INSTALLED_DIR }}` from `--cov=` argument into `env: PKG_INSTALLED_DIR:` and used `--cov="$PKG_INSTALLED_DIR"` (Linux). (line 358): Same fix for Windows PowerShell step, using `"--cov=$env:PKG_INSTALLED_DIR"` syntax.

