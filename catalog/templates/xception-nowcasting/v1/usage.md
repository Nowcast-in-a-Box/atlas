# Xception Nowcasting — provisional usage notes (template v1)

Status: provisional NiB Hub v0 catalog template. This is a packagable
declaration template for configuration delivery, not verified upstream
deployment material and not a claim that the model has been executed.

## What this entry provisions

Reference to the AINPP nowcasting containers upstream: an Xception U-Net
precipitation nowcasting container covering Latin America. The Hub Package
records the reference; it does not embed model weights, source code, or
container images.

## Configuration

The v0 configuration schema exposes no Product parameters; internal model and
runtime parameters stay Product-owned and are not invented here. The
deterministic configuration for this ProductVersion is the empty object `{}`.

## Delivery state (unresolved for deployment)

- Upstream repository: https://github.com/SInApSE-INPE/ainpp-nowcasting-containers
  at short commit `2b38214c`. The commit was inspected (Dockerfile, Compose
  file, crontab, model definition, checkpoint, data acquisition, inference,
  and output paths), but the full immutable revision SHA is not resolved, so
  this is declared evidence, not verified immutable evidence.
- No OCI image digest, release tag, or reproducible build source is verified.
- The upstream Compose configuration depends on a `config.env` file and FTP
  data access that were not portable in inspection; the required environment
  variable names are not yet inspected, and the Hub does not collect
  credentials or secrets.
- License and resource requirements (CPU/RAM/GPU/storage) are unverified and
  not declared.
- Deployment from a Package built from this entry is not supported until the
  unresolved items above are resolved by the Product maintainer.
