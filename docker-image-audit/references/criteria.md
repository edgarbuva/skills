# Optimization Criteria and Limits

## Contents and compatibility

Check whether a smaller base meets actual requirements. Alpine uses musl: native extensions or wheels may need compilation or fail compared with glibc. Distroless/scratch require checking dynamic linking, CA certificates, time zones, users, and shell availability, including startup scripts and healthchecks. Do not force migrations solely to save bytes.

Multi-stage builds exclude only what is neither copied nor inherited into the runtime. Copying the builder's entire `node_modules` may retain development dependencies. Check install scripts, workspaces, and native modules before recommending a lockfile-based production install or pruning. Compilation may require devDependencies. Verify ABI, architecture, and shared libraries across stages; avoid moving native environments across incompatible distributions. Another stage may not benefit an already minimal interpreted application.

Do not copy the entire builder into the final stage: identify required artifacts and resources. Ensure `.dockerignore` exclusions retain generated code, manifests, and necessary assets. Account for negation rules and Dockerfile-specific ignore files.

## Builds and layers

Copy stable dependency inputs before source when installation can run without source; workspaces and local scripts may require additional files. Preserve the project's package manager and lockfile.

Distinguish layer cache, package-manager cache, and external cache. Cache mounts can accelerate reinstalls without adding their contents to the runtime; `--no-cache-dir` may defeat that benefit. Cache consumes disk even when it saves time. For ephemeral CI, assess cache import/export overhead before recommending it.

Deleting files in a later layer does not recover bytes from earlier layers. Combine related installation and cleanup or isolate tools in the builder; do not merge every RUN at the expense of reuse. Instruction count is not a byte measurement: metadata instructions do not imply new filesystem layers. With APT, couple index refresh and installation; choose between cleaning lists and retaining them in an appropriate cache mount. Keep artifacts required at runtime.

## Reproducibility and operation

Tags, including version tags, can move. Digests identify content and need an update process; they do not alone guarantee reproducible builds or smaller images. Never invent digests or recommend obsolete article versions without checking current support.

For ordinary local files, COPY expresses intent. ADD may suit local extraction, Git sources, or verified downloads; preserve required semantics. HTTP downloads do not automatically behave like local archive extraction.

If ARG/ENV/COPY expose secrets, report without reproducing values and recommend secret/SSH mounts where appropriate. Later deletion does not revoke exposure. A non-root user improves security, not necessarily size or speed: check permissions and ports before changing it.

For runtime performance, investigate repeated startup work, late compilation, or development mode. Change these only with evidence and preserve signal handling and graceful shutdown. Smaller transfers may speed deployment but do not prove higher throughput or lower RAM.

## Sources

Requested inspiration: [Dockerfile Performance Optimization: Best Practices Explained, Vasanthan K](https://medium.com/@vasanthancomrads/dockerfile-performance-optimization-best-practices-explained-25d85877f12b). The original URL uses `@vasanthank29` with the same article identifier. This skill adapts its base, cache, context, layer, build/runtime separation, and measurement themes without adopting universal percentages or mandatory multi-stage builds.

Primary references for environment-specific syntax and behavior:

- [Dockerfile instructions, mounts, ADD, and COPY](https://docs.docker.com/reference/dockerfile/).
- [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/).
- [Build best practices](https://docs.docker.com/build/building/best-practices/).
- [Cache optimization](https://docs.docker.com/build/cache/optimize/).

Check official runtime/package-manager documentation for version-dependent recommendations; do not extrapolate Node commands to other stacks.
