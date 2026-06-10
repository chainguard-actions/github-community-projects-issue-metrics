# Hardening Report: github-community-projects--issue-metrics/v4.2.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **github-community-projects--issue-metrics/v4.2.6** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses a Docker image referenced by a mutable tag rather than an immutable SHA digest. In action.yml, `image: "docker://ghcr.io/github-community-projects/issue_metrics:v4"` uses the tag `:v4`, which can be silently replaced with different (potentially malicious) content at any time. It should be pinned to a full SHA256 digest, e.g. `image: "ghcr.io/github-community-projects/issue_metrics@sha256:<64-hex-char-digest> # v4"`.

Locations:

- `action.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag `ghcr.io/github-community-projects/issue_metrics:v4` to the immutable digest `ghcr.io/github-community-projects/issue_metrics@sha256:0a3825e9a0af5404b1e28885d48b77eb7f0e0ccbff20ec599db66d616d394119 # v4`. The comment preserving the original tag is placed outside the YAML string quotes.

