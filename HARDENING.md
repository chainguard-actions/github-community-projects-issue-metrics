<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **github-community-projects--issue-metrics/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image with a mutable tag (`v4`) instead of an immutable SHA digest. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` can be silently replaced by a new image at any time, enabling supply-chain attacks. It should be pinned to a SHA digest, e.g. `ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): `${{ matrix.python-version }}` is directly interpolated inside `run:` shell command strings in two steps. GitHub Actions performs YAML template substitution before the shell ever sees the value, so any special characters in the matrix value are parsed by the shell. Although `matrix.python-version` is defined in the same workflow file, it is still a workflow-controllable context and must not appear directly in `run:` blocks. Offending lines: `run: uv python install ${{ matrix.python-version }}` and `run: uv sync --frozen --python ${{ matrix.python-version }}`. Fix by routing through an env var and double-quoting: `env: PY_VER: ${{ matrix.python-version }}` then `run: uv python install "$PY_VER"`.

Locations:

- `.github/workflows/python-package.yml:38`
- `.github/workflows/python-package.yml:40`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned Docker image from `docker://ghcr.io/github-community-projects/issue_metrics:v4` to `docker://ghcr.io/github-community-projects/issue_metrics:v4@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98`, preserving the docker:// scheme and tag. 2. .github/workflows/python-package.yml: Fixed two script injection instances by moving `${{ matrix.python-version }}` into `env: PY_VER:` blocks and referencing as `"$PY_VER"` in the run commands for both the 'Set up Python' and 'Install dependencies' steps.

