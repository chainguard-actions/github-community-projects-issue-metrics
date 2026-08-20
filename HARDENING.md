<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v5.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **github-community-projects--issue-metrics/v5.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses a Docker image pinned to a mutable tag (`v4`) instead of an immutable SHA digest. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` can be silently replaced by a new image at any time, enabling supply-chain attacks. It should be replaced with a SHA-digest reference such as `docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): `${{ matrix.python-version }}` is interpolated directly inside two `run:` shell command strings in python-package.yml. The `matrix.*` context flows through YAML template substitution before the shell ever sees the value, meaning a specially crafted matrix value could inject arbitrary shell commands. Offending lines:
  - `run: uv python install ${{ matrix.python-version }}`
  - `run: uv sync --frozen --python ${{ matrix.python-version }}`
Fix: move the value into an `env:` variable and reference it as a quoted shell variable, e.g. `"$PYTHON_VERSION"`.

Locations:

- `.github/workflows/python-package.yml:39`
- `.github/workflows/python-package.yml:41`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned Docker image from mutable tag `v4` to immutable digest `docker://ghcr.io/github-community-projects/issue_metrics:v4@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98`, preserving the docker:// scheme and tag inline. 2. .github/workflows/python-package.yml: Fixed two script injection instances where `${{ matrix.python-version }}` was interpolated directly in `run:` shell strings. Moved the expression into an `env:` block as `PYTHON_VERSION` in both steps and referenced it as the quoted shell variable `"$PYTHON_VERSION"` in the run commands.

