<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v4.2.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **github-community-projects--issue-metrics/v4.2.8** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses a Docker image referenced by a mutable tag (`v4`) instead of an immutable SHA digest. The line `image: "docker://ghcr.io/github-community-projects/issue_metrics:v4"` can be silently replaced with a different image at any time, enabling supply-chain attacks. It should be pinned to a specific `sha256:` digest, e.g. `image: "docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>"`.

Locations:

- `action.yml:6`

### script-injection (severity: high)

Sub-rule (a): Two `run:` steps in python-package.yml directly interpolate `${{ matrix.python-version }}` into shell commands. Because GitHub Actions performs YAML template substitution before the shell ever sees the string, a malicious matrix value could inject arbitrary shell commands. The offending lines are:
- `run: uv python install ${{ matrix.python-version }}`
- `run: uv sync --frozen --python ${{ matrix.python-version }}`
Fix: move the value into an `env:` variable and reference it as a quoted shell variable, e.g. `env: { PY_VER: "${{ matrix.python-version }}" }` then `run: uv python install "$PY_VER"`.

Locations:

- `.github/workflows/python-package.yml:38`
- `.github/workflows/python-package.yml:40`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned Docker image from `docker://ghcr.io/github-community-projects/issue_metrics:v4` to `docker://ghcr.io/github-community-projects/issue_metrics:v4@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98`, preserving the docker:// scheme and tag inline. 2. .github/workflows/python-package.yml: Fixed two script injection vulnerabilities by moving `${{ matrix.python-version }}` out of both `run:` shell strings and into `env:` blocks as `PY_VER`, then referencing as `"$PY_VER"` in the shell commands.

