<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v4.2.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **github-community-projects--issue-metrics/v4.2.8** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action's Docker image reference uses a mutable tag (`v4`) instead of a SHA digest. `image: "docker://ghcr.io/github-community-projects/issue_metrics:v4"` can be silently replaced with a different (potentially malicious) image at any time. It should be pinned to an immutable SHA digest, e.g. `image: "ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest>"`.

Locations:

- `action.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag `ghcr.io/github-community-projects/issue_metrics:v4` to the immutable digest `ghcr.io/github-community-projects/issue_metrics@sha256:8a38489d7fcd68792af2956a1da35d56582a09b754bc94b4809eb79ae699de98` with `# v4` comment for readability.

