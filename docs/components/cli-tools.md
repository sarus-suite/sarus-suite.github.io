# Cluster tools & developer experience

Sarus Suite ships a set of tools that orchestrate EDF-driven container runs on HPC systems and support development workflows.

## sarusctl

`sarusctl` is a simple CLI that launches containers from an EDF. Users submit a single command and get a scheduler-aware, policy-compliant container without having to remember low-level Podman flags.

## Skybox

**Skybox** is a Slurm SPANK plugin. It ensures EDF jobs submit and run cleanly under Slurm, with the right cgroups, environment and node resources for the allocation. See [HPC scheduler plugins](hpc-scheduler-plugins.md) for more details.

## Raster

**Raster** is the EDF parser library: a single, versioned implementation of the EDF specification, shared across suite components so EDF semantics stay consistent.

## Podman-driver

The **podman-driver** crate provides a Rust API that executes Podman and Parallax commands with support for EDF files. It is the glue between EDF descriptions and concrete container runtime actions.

## Developer tooling

Two components help with development and CI:

- **deploy** – reproducible dev-to-prod environments and fast onboarding.
- **containerfiles-ci** – canonical build/test images with pinned dependencies, making CI behavior reproducible and easier to debug.
