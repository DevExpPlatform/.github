# PRD: Add Shared Octo STS PR Write Policy for Project GCIO Repositories

**Created**: 2026-04-13  
**Status**: In Progress  
**Related**: workforce-management-backend automated PR unblock

## Problem Statement

The repository `project-workforce-management-backend` will be renamed to `project-gcio`, and future GCIO repositories may follow the same naming pattern. Those repositories need to open and update automated pull requests from GitHub Actions workflows without relying on per-repository PAT secrets.

Managing a separate PAT per repository does not scale, and requiring the platform team to update trust policy every time GCIO adds a new workflow or repository is not acceptable.

## Proposed Solution

Treat the renamed `project-gcio` repository, and future `project-gcio-*` repositories, as a shared trust zone for automated PR operations.

Add a single org-level Octo STS policy in `DevExpPlatform/.github/.github/chainguard/` that allows federation only from repositories matching the `project-gcio` naming family and grants the minimum permissions needed for automated PR flows.

## Key Requirements

- [x] The policy **MUST** live in `DevExpPlatform/.github/.github/chainguard/`
- [x] The policy **MUST** restrict federation to repositories matching `project-gcio` and future `project-gcio-*` names
- [x] The policy **MUST** grant `contents: write`
- [x] The policy **MUST** grant `pull_requests: write`
- [x] The policy **MUST** grant `metadata: read`
- [x] The PRD **MUST** document that GCIO is treated as a shared trust zone

## Risk Assessment

- **Accepted Risk - Shared Trust Zone**: Any authorized workflow in a `project-gcio-*` repository can obtain a token usable across the GCIO repository perimeter if the Octo STS app installation also covers that perimeter.  
  **Mitigation**: This is an explicit governance decision. GCIO is treated as one trust zone and owns the policy consequences within that perimeter.

- **Operational Risk - App Installation Scope**: If the Octo STS app installation is wider than the GCIO perimeter, `repositories: []` may allow a broader token scope than intended.  
  **Mitigation**: Preferred operating model is to keep the app installation limited to the GCIO repository perimeter, or later evolve this policy to a generated `repositories:` list if needed.

## Success Validation

- Policy file added at `.github/chainguard/project-gcio-pr-write.sts.yaml`
- Policy expresses the GCIO `project-gcio` trust boundary and the expected naming family for future repos
- PRD captures the governance rationale for a single shared policy

## Work Log

### 2026-04-13 - Initial Implementation

**Summary**: Added a shared org-level Octo STS policy for GCIO repositories and documented the trust model.
