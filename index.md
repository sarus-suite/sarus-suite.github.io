---
layout: default
title: Home
---

# Sarus Suite

**sarus-suite** is a modular and open-source toolkit that layers on top of Podman to run containers efficiently on HPC clusters. The suite aims to deliver **native HPC performance** with the **developer productivity** of cloud-native tools by building on Podman’s rapid innovation and enhancing it with domain-specific HPC expertise.

At its core is a **consistent user experience** driven by the **Environment Definition File (EDF)**. EDF lets users describe environments once while the suite abstracts the tooling that runs on the HPC system, allowing system administrators to adjust runtimes, policies, and configuration as needed by the different platforms and user community needs.

The components of the suite are interoperable and usable independently, and they plug cleanly into existing stacks (**Podman, conmon, crun, Slurm**, etc.).

<div style="margin:1rem 0; display:flex; gap:.5rem; flex-wrap:wrap;">
  <a class="btn" href="#get-started">Get started</a>
  <a class="btn" href="#components">Components</a>
  <a class="btn" href="#component-index">Component index</a>
  <a class="btn" href="#edf-example">EDF example</a>
  <a class="btn" href="https://github.com/sarus-suite">GitHub organization</a>
</div>

## Sarus-suite Component Overview {#components}

### Cluster tools

* **sarusctl**. Simple CLI to launch from an EDF. One command to get a scheduler-aware, policy-compliant container without knowing Podman flags.
* **skybox**. Slurm integration (SPANK). EDF jobs submit/run cleanly under Slurm with the right cgroups, env, and node resources.
* **raster**. EDF parser library. A single, versioned specification library implementation that drives all tool.
* **podman-driver**. Rust library crate to execute Podman and Parallax commands with support for EDF files.
* **EDF**. Environment Definition File. A portable, declarative description of image, mounts, devices, and env; decouples UX from underlying system tools.

### Transparent storage

* **parallax**. Lightweight Go utility that optimizes Podman for HPC systems by enabling fast, read-only container image storage using SquashFS on parallel filesystems (e.g., NFS). It allows seamless migration of images to shared locations, avoiding redundant pulls across cluster nodes while maintaining compatibility with existing workflows.
* **parallax-mount-program**. Helper program for Podman. Presents the read-only squashed images as normal rootfs layers, so existing workflows “just work.”

### Performance extensions

* **CDI**. Vendor device injection (GPUs/IB/others). Expert configuration that enables access to accelerators with correct device files and libraries.
* **Podman config modules**. Podman configurations including namespace and cgroup management tuned to achive native HPC performance.
* **sarus-hooks**. Annotation-driven Performance Extensions for Sarus-suite that unlock HPC on Podman. Static binaries cut operational overhead, standardize installs, and speedup time-to-science.


### Developer experience

* **deploy**. Reproducible development to production environments for the suite. Enabling fast onboarding and consistent local/cluster behavior.
* **containerfiles-ci**. Canonical build/test images. Provides reliable CI builds, pinned dependencies, and easy reproduction of issues.

---

## EDF example {#edf-example}

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
com.pyxis.pytorch_remap_vars = "true"
com.pyxis.entrypoint_log = "true"
```

> The EDF is parsed by **raster** and consumed by **sarusctl/skybox**; admins can update runtime backends without changing this file.

---

## Component index {#component-index}

| Component              | Category            | Purpose                               | Repo                                                                                                   | Releases                                                     |
| ---------------------- | ------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| sarusctl               | Cluster tools       | EDF-driven CLI for launches           | [github.com/sarus-suite/sarusctl](https://github.com/sarus-suite/sarusctl)                             | [Releases](https://github.com/sarus-suite/sarusctl/releases) |
| skybox                 | Cluster tools       | Slurm (SPANK) integration             | [github.com/sarus-suite/skybox](https://github.com/sarus-suite/skybox)                                 | –                                                            |
| raster                 | Cluster tools       | EDF parser library                    | [github.com/sarus-suite/raster](https://github.com/sarus-suite/raster)                                 | –                                                            |
| podman-driver          | Cluster tools       | Podman bridge with Parallax and EDF support        | [github.com/sarus-suite/podman-driver](https://github.com/sarus-suite/podman-driver)                   | –                                                            |
| parallax               | Transparent storage | Read-only SquashFS store on shared Parallel Filesystem support | [github.com/sarus-suite/parallax](https://github.com/sarus-suite/parallax)                             | [Releases](https://github.com/sarus-suite/parallax/releases) |
| parallax-mount-program | Transparent storage | Overlay mount helper for Podman       | [github.com/sarus-suite/parallax-mount-program](https://github.com/sarus-suite/parallax-mount-program) | –                                                            |
| performance-extensions | Performance         | CDI, Podman config modules, OCI hooks  | [github.com/sarus-suite/performance-extensions](https://github.com/sarus-suite/performance-extensions) | –                                                            |
| deploy                 | Dev experience      | Reproducible development to production environments             | [github.com/sarus-suite/deploy](https://github.com/sarus-suite/deploy)                                 | –                                                            |
| containerfiles-ci      | Dev experience      | Canonical build/test images           | [github.com/sarus-suite/containerfiles-ci](https://github.com/sarus-suite/containerfiles-ci)           | –                                                            |

---



## Architecture (high level)

![sarus-suite architecture overview](/assets/sarus-suite-architecture.png)


---

## Contribute & community

* **GitHub organization:** [https://github.com/sarus-suite](https://github.com/sarus-suite)
* Open issues or discussions in the relevant repo.
* PRs welcome—please see each repo’s README for build/test instructions.

---

Sarus-suite provides a consistent EDF-based user-experience, Podman compatibility, faster starts, vendor-grade performance, and cleaner operations with components you can adopt individually or as a full stack.
