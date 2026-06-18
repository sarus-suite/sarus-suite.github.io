---
tags:
  - HPC
  - Containers
  - Podman
  - OCI
---

# Sarus Suite

## Run HPC containers at scale - the cloud-native way

Sarus Suite keeps standard Podman and adds HPC capabilities through scheduler integration, scalable image access, and runtime host injection.

- **Built on upstream Podman and OCI interfaces**
- **Scales to large synchronized launches with shared SquashFS image access**
- **Matches strong HPC baseline performance on real workloads**

<div class="grid cards" markdown>

- :material-help-rhombus:{ .lg .middle } **Why this approach**

    ---

    See why HPC containers need a different architecture than standard single-node container workflows.

    [:material-arrow-right: Why HPC containers need a different architecture](#why-hpc-containers-need-a-different-architecture)

- :material-chart-line-variant:{ .lg .middle } **Proven at scale**

    ---

    Understand how the suite handles synchronized launches, shared image access and strong baseline performance.

    [:material-arrow-right: Proven at scale](#proven-at-scale)

- :material-sitemap:{ .lg .middle } **How it works**

    ---

    Explore how Sarus Suite combines Podman, Parallax, EDF and runtime hooks into an HPC-ready stack.

    [:material-arrow-right: High-level architecture](architecture/overview.md)

- :material-rocket-launch:{ .lg .middle } **Get started**

    ---

    Jump into installation and the quick start path for deploying the suite and running your first workloads.

    [:material-arrow-right: Installation quick start](admin/installation.md#quick-start)

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

- :material-gpu:{ .lg .middle } **Up to 1024 GPUs tested**

    ---

    Evaluation included large-scale distributed AI runs across up to 1024 GPUs.

- :material-speedometer:{ .lg .middle } **Up to 2.47x faster per-node startup**

    ---

    Warm-run startup measurements showed substantially faster per-node startup in production-style workflows.

- :material-chart-box:{ .lg .middle } **Comparable throughput**

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

### For performance

- **Shared SquashFS image access via Parallax.** Large job launches can access the same image, making efficient use of parallel filesystem resources.
- **Runtime injection of GPUs, MPI/libfabric, NCCL, and host-tuned libraries.** HPC-specific capabilities are added at runtime so containers can use optimized host resources while keeping the OCI engine standard.

Sarus Suite provides an EDF-based user experience, Podman compatibility, faster container starts, vendor-grade performance and cleaner operations, with components you can deploy individually or as a full stack.

## Project links

- [:simple-github: sarus-suite on GitHub](https://github.com/sarus-suite)
- [:material-scale-balance: License](license.md)
