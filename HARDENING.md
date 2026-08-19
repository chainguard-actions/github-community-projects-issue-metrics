<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v4.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **github-community-projects--issue-metrics/v4.2.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image referenced by a mutable tag (`v4`) instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image on future runs. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` should be replaced with a SHA-digested form such as `docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): Two `run:` steps in python-package.yml directly interpolate `${{ matrix.python-version }}` into shell command strings. Although `matrix` values are typically developer-controlled, any `${{ ... }}` expression interpolated directly into a `run:` block is a script-injection risk because the value is substituted by the YAML template engine before the shell ever sees it, bypassing shell quoting. Offending lines: `run: uv python install ${{ matrix.python-version }}` and `run: uv sync --frozen --python ${{ matrix.python-version }}`. These should be moved to an `env:` block and referenced as a quoted shell variable (e.g., `"$PYTHON_VERSION"`).

Locations:

- `.github/workflows/python-package.yml:36`
- `.github/workflows/python-package.yml:38`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned the Docker image from `docker://ghcr.io/github-community-projects/issue_metrics:v4` to `docker://ghcr.io/github-community-projects/issue_metrics:v4@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98`, preserving the docker:// scheme and :v4 tag inline. 2. .github/workflows/python-package.yml: Fixed two script-injection instances at lines 36 and 38 by moving `${{ matrix.python-version }}` into `env: PYTHON_VERSION:` blocks and referencing it as `"$PYTHON_VERSION"` in the shell commands.

