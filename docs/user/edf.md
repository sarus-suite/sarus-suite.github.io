# Environment Definition File (EDF)

The **Environment Definition File (EDF)** is the central abstraction in Sarus Suite. An EDF is a small, declarative file that describes:

- Which image to run
- Which paths to mount into the container
- The working directory and entrypoint behavior
- Environment variables
- Annotations that control hooks and performance extensions

EDF lets users define their runtime environment once. The suite then maps that description onto Podman, Parallax, scheduler integrations and device configuration on each cluster.

## Example EDF

```toml
image = "ghcr.io#sarus-suite/containerfiles-ci/transformers:latest-arm64"
mounts = ["/iopsstor/scratch/models/hf:/opt/hf",
          "/iopsstor/scratch/data:/data",
          "/iopsstor/scratch/output:/output"]
workdir = "${SCRATCH}"
entrypoint = false

[env]
HUGGINGFACE_HUB_CACHE = "/opt/hf/hub"
TRANSFORMERS_CACHE = "/opt/hf/transformers"
DEBUG = "false"

[annotations]
com.hooks.cxi.enabled = "true"
com.hooks.aws_ofi_nccl.enabled = "true"
com.hooks.aws_ofi_nccl.variant = "cuda12"
com.hooks.nvidia_cuda_mps.enabled = "true"
```

This EDF describes an ML workload based on the transformers library, pulling a container image from GitHub Container Registry, mounting model/data/output paths from a shared filesystem, and configuring caches via environment variables. The annotations enable specific hooks (e.g. CXI, NCCL, NVIDIA CUDA MPS) so that the container can leverage the underlying HPC system.
