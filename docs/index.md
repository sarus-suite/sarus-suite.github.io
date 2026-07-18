---
tags:
  - HPC
  - Containers
  - Podman
  - OCI
title: Sarus Suite
---

# Sarus Suite

## Run HPC containers at scale - the cloud-native way

Sarus Suite keeps standard Podman and adds HPC capabilities through scheduler integration, scalable image access, and runtime host injection.

- **Built on upstream Podman and OCI interfaces**
- **Scales to large synchronized launches with shared SquashFS image access**
- **Matches strong HPC baseline performance on real workloads**

!!! tip "Try the CLI locally"
    Download the portable bundle, enter the Sarus Suite shell, inspect an EDF, and run a container locally without setting up Slurm or Skybox.

    [Get started locally](user/getting-started-local.md)

<div class="grid cards" markdown>

- :material-help-rhombus-outline:{ .lg .middle } **Why this approach**

    ---

    See why HPC containers need a different architecture than standard single-node container workflows.

    [:octicons-arrow-right-24: Why HPC containers need a different architecture](#why-hpc-containers-need-a-different-architecture)

- :material-chart-line:{ .lg .middle } **Proven at scale**

    ---

    Understand how the suite handles synchronized launches, shared image access and strong baseline performance.

    [:octicons-arrow-right-24: Proven at scale](#proven-at-scale)

- :material-file-tree-outline:{ .lg .middle } **How it works**

    ---

    Explore how Sarus Suite combines Podman, Parallax, EDF and runtime hooks into an HPC-ready stack.

    [:octicons-arrow-right-24: High-level architecture](architecture/overview.md)

- :material-rocket-launch-outline:{ .lg .middle } **Get started**

    ---

    Download the portable bundle, enter the Sarus Suite shell, inspect an EDF and run a container locally.

    [:octicons-arrow-right-24: Try Sarus Suite locally](user/getting-started-local.md)

- :material-server-network:{ .lg .middle } **For administrators**

    ---

    Understand the site installation model: runtime dependencies, Sarus binaries, Podman configuration, Parallax image storage and deployment packaging.

    [:octicons-arrow-right-24: Installation guide](admin/installation.md)

</div>

## Why HPC containers need a different architecture

Standard OCI runtimes like Podman are excellent at launching containers, but HPC environments impose additional requirements.

Large HPC jobs often start many containers concurrently, across multiple nodes, for this the ideal is to use shared resources efficiently when loading the same image at scale. Moreover, clusters also need scheduler-aware placement, deployment startup and cleanup policy, GPU device setup, high-performance interconnect support and controlled host injection at runtime.

Sarus Suite addresses those HPC-specific concerns without replacing the OCI engine. Instead, it keeps Podman and OCI interfaces intact, then adds the cluster-facing capabilities around them: EDF-driven configuration, scheduler integration, scalable image access and runtime hooks.

## Proven at scale

Sarus Suite is designed for the realities of HPC operations, where startup behavior and runtime integration matter as much as container compatibility.

- **Shared SquashFS image access** reduces redundant image distribution and supports large synchronized launches.
- **Scheduler integration** ensures containers fit existing Slurm-based workflows instead of bypassing them.
- **Runtime host injection and hooks** enable GPUs, MPI, libfabric and other system capabilities to be configured where HPC workloads actually need them.

Validated on real HPC and AI workloads:

- **MPI CFD and SPH applications**, benchmarked against MPICH and OpenMPI stacks
- **NCCL-based LLM training**, using distributed Megatron-LM workloads
- **Metadata-heavy startup benchmarks**, reflecting container launch behavior in production workflows

### Key results

- **Comparable performance to SOTA Enroot+Pyxis** on distributed HPC workloads
- **Comparable Megatron-LM throughput** using an upstream NGC image
- **Faster per-node startup** in warm-start production workflows

<div class="grid cards" markdown>

- :material-expansion-card:{ .lg .middle } **Up to 1024 GPUs tested**

    ---

    Evaluation included large-scale distributed AI runs across up to 1024 GPUs.

- :material-speedometer:{ .lg .middle } **Up to 2.47× faster per-node startup**

    ---

    Warm-run startup measurements showed substantially faster per-node startup in production-style workflows.

- :material-chart-box-outline:{ .lg .middle } **Comparable throughput**

    ---

    Distributed ML training and HPC application performance remained comparable to established production baselines.

</div>

## Why Sarus Suite

### For users

- **Describe the runtime environment once with EDF.** Define images, mounts, devices, environment variables and hooks in one place, then reuse that definition across jobs and projects.
- **Reuse workflows across clusters without rewriting launcher logic.** EDF-driven workflows reduce cluster-specific differences so users can keep a more consistent experience across systems.

### For administrators

- **Keep Slurm control, accounting, cgroups, and startup-cleanup container policy.** Sarus Suite integrates with the scheduler model instead of bypassing it, so existing operational job controls remain in place.
- **Integrate with existing Podman, Slurm, and site infrastructure.** The suite builds around standard OCI tooling and existing cluster services rather than replacing them with a custom system-specific runtime stack.

!!! info "Installing on a cluster?"
    Start with the manual installation guide to understand the stack layout, then use Deploy where openSUSE Leap 15.5 packaging fits your site.

    [Read the installation guide](admin/installation.md)

### For performance

- **Shared SquashFS image access via Parallax.** Large job launches can access the same image, making efficient use of parallel filesystem resources.
- **Runtime injection of GPUs, MPI/libfabric, NCCL, and host-tuned libraries.** HPC-specific capabilities are added at runtime so containers can use optimized host resources while keeping the OCI engine standard.

Sarus Suite provides an EDF-based user experience, Podman compatibility, faster container starts, vendor-grade performance and cleaner operations, with components you can deploy individually or as a full stack.

## Why EDF?

Running an HPC container involves more than selecting an image. Users may need to coordinate runtime settings, storage configuration, mounts, devices, environment variables, and cluster-specific hooks—often through a long collection of command-line options.

A typical container launch can look like this:

```bash
podman \
  --ipc host \
  --network host \
  --pid host \
  --uts host \
  --userns keep-id \
  --cgroupns host \
  --cgroups no-conmon \
  --tz local \
  --root "$PODMAN_ROOT" \
  --runroot "$PODMAN_RUNROOT" \
  --storage-opt "additionalimagestore=$RO_STORAGE" \
  --storage-opt "mount_program=/usr/bin/parallax-mount-program" \
  run \
  --mount "type=bind,src=/scratch/$USER/hf-models,dst=/opt/hf" \
  --mount "type=bind,src=/scratch/$USER/data,dst=/data" \
  --mount "type=bind,src=/scratch/$USER/output,dst=/output" \
  --workdir "/scratch/$USER" \
  --entrypoint "" \
  --device "nvidia.com/gpu=all" \
  --env "HUGGINGFACE_HUB_CACHE=/opt/hf/hub" \
  --env "TRANSFORMERS_CACHE=/opt/hf/transformers" \
  --annotation "com.hooks.cxi.enabled=true" \
  --annotation "com.hooks.aws_ofi_nccl.enabled=true" \
  --annotation "com.hooks.aws_ofi_nccl.variant=cuda12" \
  --annotation "com.hooks.nvidia_cuda_mps.enabled=true" \
  ghcr.io/cscs/ml-workflows/transformers:latest-arm64 \
  /usr/bin/myapp
```

HPC users must assemble these options correctly for each deployment. Missing or incorrect settings can cause the container to fail, prevent access to required resources, or result in reduced performance.

An Environment Definition File, or EDF, provides a declarative boundary between the workload a user wants to run and the site-specific mechanisms used to execute it. Instead of exposing the complete runtime command, Sarus Suite translates the environment description into the appropriate Podman, storage, and runtime configuration for the deployment.

The same environment can be described with EDF as follows:

```toml
image = "ghcr.io/cscs/ml-workflows/transformers:latest-arm64"

mounts = [
  "/scratch/$USER/hf-models:/opt/hf",
  "/scratch/$USER/data:/data",
  "/scratch/$USER/output:/output"
]

workdir = "/scratch/$USER"
entrypoint = false

devices = ["nvidia.com/gpu=all"]

[env]
HUGGINGFACE_HUB_CACHE = "/opt/hf/hub"
TRANSFORMERS_CACHE = "/opt/hf/transformers"

[annotations]
com.hooks.cxi.enabled = "true"
com.hooks.aws_ofi_nccl.enabled = "true"
com.hooks.aws_ofi_nccl.variant = "cuda12"
com.hooks.nvidia_cuda_mps.enabled = "true"
```

The application can then be launched through the SLURM scheduler with a concise command:

```bash
srun --edf transformers /usr/bin/myapp
```

EDF does not require every workload detail to be identical across systems. Paths, devices, and runtime extensions reflect the target environment. Its purpose is to separate the environment being requested from the lower-level command details and configuration used to realize it.

[:octicons-arrow-right-24: Learn more about EDF](user/edf)


## Project links

- [:octicons-mark-github-24: sarus-suite on GitHub](https://github.com/sarus-suite)
- [:octicons-law-24: License](license.md)
