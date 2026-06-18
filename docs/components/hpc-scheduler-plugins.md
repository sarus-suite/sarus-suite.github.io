# HPC scheduler plugins

Sarus Suite integrates cleanly with cluster schedulers rather than replacing them.

## Skybox (Slurm SPANK)

**Skybox** is a SPANK plugin for Slurm that understands EDF. It ensures that jobs launched with `sarusctl` or other suite tooling: 

- inherit the correct cgroups and accounting information
- get the right environment and node resources for the allocation
- remain compliant with site policies around container use

Future integrations can follow the same pattern, adding EDF-aware plugins for other schedulers while keeping the EDF and CLI stable.

