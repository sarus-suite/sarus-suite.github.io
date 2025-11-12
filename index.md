---
layout: default
title: Home
---

# Sarus Suite

**sarus-suite** is a modular and open-source toolkit that layers on top of Podman to run containers efficiently on HPC clusters. The suite aims to deliver **native HPC performance** with the **developer productivity** of cloud-native tools by building on Podman’s rapid innovation and enhancing it with domain-specific HPC expertise.

At its core is a **consistent user experience** driven by the **Environment Definition File (EDF)**. EDF lets users describe environments once while the suite abstracts the tooling that runs on the HPC system, allowing system administrators to adjust runtimes, policies, and configuration as needed by the different platforms and user community needs.

The components of the suite are interoperable and usable independently, and they plug cleanly into existing stacks (**Podman, conmon, crun, Slurm**, etc.).

# Cluster tools

* **sarusctl**. Simple CLI to launch from an EDF. One command to get a scheduler-aware, policy-compliant container without knowing Podman flags.
* **skybox**. Slurm integration (SPANK). EDF jobs submit/run cleanly under Slurm with the right cgroups, env, and node resources.
* **raster**. EDF parser library. A single, versioned specification library implementation that drives all tool.
* **podman-driver**. Rust library crate to execute Podman and Parallax commands with support for EDF files.
* **EDF**. Environment Definition File. A portable, declarative description of image, mounts, devices, and env; decouples UX from underlying system tools.

# Transparent storage

* **parallax**. Lightweight Go utility that optimizes Podman for HPC systems by enabling fast, read-only container image storage using SquashFS on parallel filesystems (e.g., NFS). It allows seamless migration of images to shared locations, avoiding redundant pulls across cluster nodes while maintaining compatibility with existing workflows.
* **parallax-mount-program**. Helper program for Podman. Presents the read-only squashed images as normal rootfs layers, so existing workflows “just work.”

# Performance extensions

* **CDI**. Vendor device injection (GPUs/IB/others). Expert configuration that enables access to accelerators with correct device files and libraries.
* **Podman config modules**. Podman configurations including namespace and cgroup management tuned to achive native HPC performance.
* **sarus-hooks**. Annotation-driven Performance Extensions for Sarus-suite that unlock HPC on Podman. Static binaries cut operational overhead, standardize installs, and speedup time-to-science.


# Developer experience

* **deploy**. Reproducible development to production environments for the suite. Enabling fast onboarding and consistent local/cluster behavior.
* **containerfiles-ci**. Canonical build/test images. Provides reliable CI builds, pinned dependencies, and easy reproduction of issues.

Sarus-suite provides a consistent EDF-based user-experience, Podman compatibility, faster starts, vendor-grade performance, and cleaner operations with components you can adopt individually or as a full stack.

