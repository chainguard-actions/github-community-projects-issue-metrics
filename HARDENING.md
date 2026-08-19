<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v4.2.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **github-community-projects--issue-metrics/v4.2.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image referenced by a mutable tag (`v4`) rather than an immutable SHA digest. This means the image could be silently replaced with a different (potentially malicious) version. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` should be replaced with a SHA-digest reference such as `docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): The workflow directly interpolates `${{ matrix.python-version }}` inside `run:` shell command strings. Any `${{ ... }}` expression interpolated directly into a `run:` block is evaluated by the YAML template engine before the shell sees it, allowing shell metacharacters to be injected. The offending lines are:
  - `run: uv python install ${{ matrix.python-version }}`
  - `run: uv sync --frozen --python ${{ matrix.python-version }}`
These should be rewritten to pass the value via an `env:` variable and reference it as a quoted shell variable, e.g.:
```yaml
env:
  PYTHON_VERSION: ${{ matrix.python-version }}
run: |
  uv python install "$PYTHON_VERSION"
  uv sync --frozen --python "$PYTHON_VERSION"
```

Locations:

- `.github/workflows/python-package.yml:36`
- `.github/workflows/python-package.yml:38`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned the Docker image from `docker://ghcr.io/github-community-projects/issue_metrics:v4` to `docker://ghcr.io/github-community-projects/issue_metrics:v4@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98`, preserving the docker:// scheme and tag inline. 2. .github/workflows/python-package.yml: Fixed two script injection vulnerabilities on lines 36 and 38 by moving `${{ matrix.python-version }}` out of `run:` blocks into `env:` blocks as `PYTHON_VERSION`, then referencing it as `"$PYTHON_VERSION"` in the shell commands.

