# Storage & Podman integration

Sarus Suite is built around Podman and OCI runtimes, focusing on HPC-friendly storage and performance extensions.

## Transparent storage with Parallax

**Parallax** is a lightweight Go utility that optimizes Podman for HPC systems by:

- converting images into read-only SquashFS images on a shared parallel filesystem (e.g. NFS),
- enabling multiple nodes to reuse images without redundant pulls,
- remaining compatible with existing Podman workflows.

The **parallax-mount-program** is a helper program for Podman that presents these SquashFS images as normal rootfs layers so containers "just work" from the user's perspective.

## Performance extensions

The **performance-extensions** component bundles:

- **CDI** (Container Device Interface) configurations for GPUs, Networking, and other accelerators
- **Podman config modules** that tune namespaces and cgroups for native HPC performance
- **sarus-hooks**, which use EDF annotations to enable specific performance features at container start (e.g. CXI hooks, NCCL configurations, CUDA MPS) while keeping binaries static and easy to deploy.

Combined, these features allow EDF-described containers to reach native performance on HPC systems using Podman as the primary runtime.
