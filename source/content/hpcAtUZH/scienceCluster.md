## The Science Cluster

{% if slide %}

```{admonition} <i class="fa-solid fa-circle-info"></i> Full specs
:class: margin tip
[docs.s3it.uzh.ch/cluster/resources](https://docs.s3it.uzh.ch/cluster/resources/)
```

{% endif %}

{% if slide %}:::::::{card}{% else %}###{% endif %} <i class="fa-solid fa-microchip"></i> Hardware
{% if slide %}

::::::{grid} 1 2 2 2
:gutter: 2

:::::{grid-item-card} CPUs
AMD EPYC & Intel Xeon

- 8 - 288 CPU's per node
- 30 - 2014 GB Ram per node
:::::
:::::{grid-item-card} Scale
- ~60 CPU-only servers
- ~30 GPU-nodes (15 are single-GPU)
:::::
:::::{grid-item-card} GPUs (NVIDIA)
:columns: 12

::::{grid} 2 2 2 2
:gutter: 2

:::{grid-item}
_nvlink (no nvswitch)_
- **A100** (_80GB_) **5x8** _(node)x(GPU per node)_
- **H100** SXM (_80GB_) **2x8** / NVL (_96GB_) 4x2
- **H200** (_140GB_) **1x8**
:::
:::{grid-item}
_PCIe Gen 4_  
- **L4** (_24GB_) **15x1**

_PCIe Gen 3_  
- **V100** (_32GB_) **5x8**
:::
::::
:::::
::::::
:::::::

{% else %}

The Science Cluster is operated by S3IT (Service and Support for Science IT) at UZH.
It aggregates compute nodes of varying hardware generations into a single Slurm-managed system.

Hardware highlights:
- **CPUs**: AMD EPYC and Intel Xeon processors across different node generations
- **GPUs**: NVIDIA accelerators (various models) for GPU-accelerated workloads
- **Memory**: Ranges from standard to high-memory nodes depending on the partition

The exact list of available partitions, node counts, and GPU models changes as hardware is procured and retired.
Check the [resources page](https://docs.s3it.uzh.ch/cluster/resources/) for the current inventory.

{% endif %}

{% if slide %}
---
{% else %}
### Software Stack
{% endif %}

{% if slide %}::::{card}{% else %}{% endif %} <i class="fa-solid fa-layer-group"></i> Software
{% if slide %}

- **Slurm** — workload manager (job scheduling, resource allocation)
- **Apptainer** — OS-level virtualization (containers without root)
- **Environment Modules** — pre-installed compilers, libraries, tools
- **No root access** — software installation limited to user-space

:::{admonition} Containers on HPC
:class: note
Docker requires root privileges and is therefore not available on shared clusters.
Apptainer (formerly Singularity) fills this gap: it runs containers in user space and integrates with Slurm.
:::

::::

{% else %}

The software stack of the Science Cluster builds on three pillars:

**Slurm:**
The workload manager handles all job scheduling and resource allocation.
Users interact with it through the standard Slurm commands (`sbatch`, `srun`, `squeue`, `scancel`, etc.).

**Apptainer (formerly Singularity):**
Since users do not have root access on shared clusters, Docker cannot be used directly.
Apptainer provides OS-level virtualization (containers) that runs entirely in user space.
It can pull and convert Docker images, so existing Dockerfiles remain useful as a starting point.

**Environment Modules:**
A selection of pre-installed compilers, libraries, and tools is available through the `module` command.
This allows loading specific software versions without manual installation (e.g., `module load gcc/12.2.0`).

Any additional software must be installed in user space (e.g., via `pip install --user`, conda environments, or Apptainer containers).

{% endif %}
