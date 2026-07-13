<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **github-community-projects--issue-metrics/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image pinned to a mutable tag (`v4`) instead of an immutable SHA digest. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` can be silently replaced by a different image at any time, enabling supply-chain attacks. It should be replaced with a SHA-digest reference such as `docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): The `${{ matrix.python-version }}` expression is interpolated directly inside `run:` shell command strings. The `matrix.*` context is workflow-controllable and any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted into the shell command before the shell parses it. Offending lines: `run: uv python install ${{ matrix.python-version }}` (line 42) and `run: uv sync --frozen --python ${{ matrix.python-version }}` (line 44). These should be moved to an `env:` block and referenced as a quoted shell variable, e.g. `"$PYTHON_VERSION"`.

Locations:

- `.github/workflows/python-package.yml:42`
- `.github/workflows/python-package.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned the Docker image from mutable tag 'docker://ghcr.io/github-community-projects/issue_metrics:v4' to immutable digest 'docker://ghcr.io/github-community-projects/issue_metrics@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98' with a '# v4' comment for readability.
2. .github/workflows/python-package.yml: Moved both ${{ matrix.python-version }} expressions out of run: shell strings and into env: blocks as PYTHON_VERSION, then referenced as "$PYTHON_VERSION" in the shell commands to prevent script injection.

