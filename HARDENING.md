<!-- markdownlint-disable -->

# Hardening Report: github-community-projects--issue-metrics/v4.2.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **github-community-projects--issue-metrics/v4.2.5** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (`v4`) instead of an immutable SHA digest. The reference `docker://ghcr.io/github-community-projects/issue_metrics:v4` can be silently updated to point to a different (potentially malicious) image without any change to the action definition, creating a supply-chain attack vector. It should be pinned to a specific SHA digest, e.g. `docker://ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest> # v4`.

Locations:

- `action.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from `ghcr.io/github-community-projects/issue_metrics:v4` to `ghcr.io/github-community-projects/issue_metrics@sha256:0a3825e9a0af5404b1e28885d48b77eb7f0e0ccbff20ec599db66d616d394119 # v4`. The comment is placed outside the YAML string quotes as required.

