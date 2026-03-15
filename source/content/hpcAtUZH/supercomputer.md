## The Supercomputer (Alps)

{% if slide %}

```{admonition} <i class="fa-solid fa-circle-info"></i> Documentation
:class: margin
[docs.cscs.ch/clusters](https://docs.cscs.ch/clusters/)
[docs.s3it.uzh.ch/supercomputer](https://docs.s3it.uzh.ch/supercomputer/overview/)
```

{% endif %}

{% if slide %}

```{epigraph}
{.centered}
A national-scale HPC facility operated by CSCS (Swiss National Supercomputing Centre).
```

{% endif %}

{% if slide %}:::::{card}{% else %}###{% endif %} <i class="fa-solid fa-gears"></i> Architecture
{% if slide %}

::::{grid} 1 1 2 2
:gutter: 2

:::{grid-item-card} Orchestration
- **Kubernetes** (bare-metal) for service management
- **vCluster** for multi-tenant isolation
- **Slurm** as the workload manager (familiar user interface)
:::
:::{grid-item-card} Software Environments
- **uenv** — SquashFS-based software stacks
- Entire environment mounted as a read-only image
- Avoids hitting the filesystem metadata server on every import
:::
::::

:::::

{% else %}

Alps is the current-generation supercomputer at CSCS, available to Swiss researchers through institutional agreements.
UZH access is brokered by S3IT.

### Orchestration

The Alps infrastructure uses a modern multi-layer orchestration stack:

- **Kubernetes** manages services at the platform level (deployed on bare metal, possibly via [Rancher](https://www.rancher.com/))
- **vCluster** provides lightweight, isolated Kubernetes environments per tenant or project, so different research groups operate in separate virtual clusters without interference
- **Slurm** remains the user-facing workload manager. Researchers submit jobs with the same `sbatch`/`srun` commands used on the Science Cluster

### Software Environments (uenv)

Instead of traditional module systems or container-only workflows, Alps uses [uenv](https://docs.cscs.ch/software/uenv/) which is a SquashFS-based approach to software distribution.

A uenv packages an entire software stack (compilers, libraries, applications) into a compressed, read-only SquashFS image.
At job start, this image is mounted into the filesystem, providing all required software without thousands of individual file lookups against the cluster's metadata server.

This design directly addresses the metadata bottleneck that plagues shared filesystems when large numbers of nodes simultaneously load Python packages or shared libraries.

{% endif %}
