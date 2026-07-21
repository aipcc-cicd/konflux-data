# konflux-data

Public repository containing [Tekton](https://tekton.dev/) pipelines used by [Konflux](https://konflux-ci.dev/) for post-release automation in the AIPCC productization ecosystem.

These pipelines are referenced via the Tekton `git` resolver in Konflux resource configurations (e.g., `finalPipeline` on ReleasePlans). The repository is public so that the Konflux platform can fetch pipeline definitions without additional authentication.

## Pipelines

### copy-clair-scan-results

Copies Clair CVE scan report blobs (OCI artifacts) from the Konflux build registry to the released product registry (e.g., `quay.io/aipcc/*`). This makes full CVE scan data accessible alongside released images without requiring Konflux cluster access.

**How it works:**
1. Reads the Konflux Snapshot and Release CR to identify the released component and its source image
2. Downloads the build attestation and extracts Clair `REPORTS` digests
3. Copies the report blobs from the build registry to the target registry using `skopeo`

**Usage:** Configured as a `finalPipeline` on Konflux ReleasePlans — runs automatically after a managed release pipeline succeeds.

## Contributing

Pipelines in this repository are shared across multiple AIPCC products. Changes should be tested end-to-end in a Konflux tenant before merging.
