# Comparable Measurements and Storage

## Inspection without running the application

Use actual image references and record commands and environment. Local-image examples (replace `IMAGE`):

```sh
docker image inspect --format '{{.Id}} {{.Os}}/{{.Architecture}} {{.Size}}' IMAGE
docker image history --no-trunc IMAGE
docker system df -v
docker buildx du
```

History/configuration may contain old build credentials: redact sensitive values before reporting. `buildx du` describes the selected builder and may be unavailable. Do not require installing tools or switching builders for a static audit.

Record Docker/BuildKit version, builder, platform, target, commit, resolved base, and relevant parameters. `.Size` is local logical size, not compressed transfer size or reclaimable physical space. Identify the image store when it affects interpretation.

## Before and after

Use new baseline/proposal tags and separate contexts or temporary copies for incremental changes. Keep platform, arguments, target, and host resources equivalent. Do not overwrite deployment tags for measurement.

When building is in scope, adapt this example to the context and capture wall time and logs:

```sh
docker buildx build --load --progress=plain -f Dockerfile -t docker-audit:baseline CONTEXT
```

`--load` loads a single-platform image locally; do not assume it loads a multiarch matrix. Repeat with the proposal and its own tag. Keep exporters identical: exporting/loading takes time too.

Compare distinct scenarios:

- **Cold layer cache:** `--no-cache`; note that it does not empty cache mounts or remove local bases. For fully cold builds, use an authorized isolated builder and record downloads without purging shared builders.
- **Warm:** unchanged context and instructions.
- **Incremental:** representative source-only changes, then dependency changes separately. Observe rebuilt steps without modifying the user's working files.

Do not apply `--pull` to only one variant or change digests silently. Repeat enough to characterize variability when affordable; report median/range and sample count. State the limitation of a single run. Calculate percentage reductions only for comparable metrics with a positive baseline.

## Functionality and runtime performance

Verify actual startup commands, required resources/dependencies, user permissions, and graceful shutdown. For services, measure functional readiness rather than `docker run` return time. Separate download, creation, and startup. Use identical workloads and CPU/memory limits to compare latency, throughput, and resources; `docker stats --no-stream` is a sample, not a benchmark.

Use existing tests or relevant smoke tests in temporary containers when application execution is authorized, with isolated ports/resources and no production credentials or volumes. Do not invent health endpoints. Report failures and do not claim runtime validation when execution tests are missing.

## Disk consumption

Separate shared/exclusive layers, builder cache, containers, volumes, and logs. A smaller new image does not remove old images or accumulated data. With only a Dockerfile, mark host-storage diagnosis as pending.

Base retention and GC recommendations on observed usage and build frequency. Removing cache may slow the next build. Do not remove volumes to reduce image size. Authorized cleanup must identify resources and preserve required data; never use `docker system prune -a --volumes` as a generic fix.

Reference: [Prune unused Docker objects](https://docs.docker.com/engine/manage-resources/pruning/). Check installed-version flags before recommending deletion commands.
