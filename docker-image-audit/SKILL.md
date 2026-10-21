---
name: docker-image-audit
description: Audit Dockerfiles and Docker images and recommend evidence-based improvements to image size, build time, runtime performance, and storage. Use when reviewing or optimizing container images and their builds, not for general Kubernetes administration.
---

# Docker Image Audit

Explain what is wrong, its effect, and why each proposed alternative fits the project better. Respond in the user's language. Prioritize evidence and compatibility over universal recipes or unmeasured savings.

## Scope and evidence

- Locate Dockerfiles, actual build contexts, effective `.dockerignore` files, manifests, lockfiles, and Compose/CI build configuration. Follow repository discovery instructions. Identify platform, target, non-secret arguments, runtime, and startup command before proposing changes.
- Scope multiple services using the request and available configuration. Ask only for information needed for a correct recommendation; continue independent analysis.
- Read [references/criteria.md](references/criteria.md) for decision criteria and common pitfalls. Read [references/measurement.md](references/measurement.md) when a local image exists or verification is requested.
- Without Docker, daemon access, or a built image, perform a static audit. State measurement gaps and provide concrete improvements without inventing sizes or timings.
- With only an image, inspect available configuration and history. Do not claim to reconstruct its exact Dockerfile or attribute original instructions without evidence. Request the Dockerfile to complete that analysis.

## Diagnosis

Tie each finding to file/lines or measurements and to stages that contribute to the selected target. Distinguish confirmed defects, conditional opportunities, and missing information. Verify contents before calling a dependency, library, or tool unnecessary.

Evaluate four dimensions separately:

- **Size:** runtime contents, base, dependencies, and bytes retained in layers.
- **Build:** expensive steps, cache invalidation, downloads, and context transfer.
- **Runtime:** readiness time, CPU, memory, and representative workload behavior. A Dockerfile alone cannot demonstrate runtime gains.
- **Storage:** images, shared layers, builder cache, writable layers, logs, and volumes. Do not sum image sizes as if every layer were exclusive.

## Recommendations and implementation

Explain each change's causal mechanism, cost, and preconditions. Prioritize likely benefit against effort and regression risk. Preserve versions and behavior unless a change requires otherwise; verify support and compatibility before recommending another base or version.

An audit delivers recommendations and a proposed diff or Dockerfile. When implementation is requested, edit relevant files within the authorized scope and validate. Preserve ENTRYPOINT/CMD, paths, static assets, permissions, certificates, native libraries, and runtime dependencies. Keep tests needed to verify the artifact.

Image, cache, and volume deletion is not implicit in optimization. Before deleting, identify affected resources and impact and verify authorization for that specific deletion. Do not publish images or export registry caches merely to audit. Review build commands before execution: they can run code and download dependencies.

## Deliverable

Lead with the main diagnosis and scope. Use a findings table when helpful:

`Priority | Evidence | Problem and consequence | Change and rationale | Expected impact | Risk/validation`

Include a project-specific diff or proposal and `.dockerignore` changes where relevant. Label partial snippets and assumptions; do not present placeholders as a buildable Dockerfile. If the original is sound, say so without manufacturing findings.

Finish with before/after measurements or a reproducible plan covering size, cold/warm/incremental builds, startup/resources, and relevant storage. Separate measured results from hypotheses and pending tests; briefly explain inapplicable dimensions. Do not equate logical size reduction with actual reclaimed disk space.
