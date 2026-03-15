## At a Glance

{% if slide %}

:::::{grid} 1 1 2 2
:gutter: 3

::::{grid-item-card} <i class="fa-solid fa-cubes"></i> Science Cluster
:class-header: sd-bg-primary sd-bg-text-light

Local HPC at UZH

^^^

- Managed by **S3IT** (UZH)
- Slurm workload manager
- CephFS shared storage
- Containers via **Apptainer**

+++
<i class="fa-solid fa-link"></i> [docs.s3it.uzh.ch/cluster](https://docs.s3it.uzh.ch/cluster/overview/)
::::

::::{grid-item-card} <i class="fa-solid fa-rocket"></i> Supercomputer (Alps)
:class-header: sd-bg-info sd-bg-text-light

National HPC at CSCS

^^^

- Managed by **CSCS** (ETH Domain)
- Slurm + Kubernetes + vCluster
- Custom software distribution (**uenv**)
- Access brokered through S3IT

+++
<i class="fa-solid fa-link"></i> [docs.cscs.ch/clusters](https://docs.cscs.ch/clusters/)
::::
:::::

{% else %}

UZH researchers can draw on two complementary HPC systems:

| | **Science Cluster** | **Supercomputer (Alps)** |
|---|---|---|
| **Operator** | S3IT, University of Zurich | CSCS, ETH Domain |
| **Scale** | Departmental / university-wide | National facility |
| **Scheduler** | Slurm | Slurm (on vCluster / Kubernetes) |
| **Storage** | CephFS (`/home`, `/scratch`) | Site-specific parallel FS |
| **Software** | Apptainer containers, user modules | uenv (SquashFS-based environments) |
| **Access** | SSH (UZH network / VPN) | SSH (via S3IT brokered accounts) |
| **Documentation** | [docs.s3it.uzh.ch/cluster](https://docs.s3it.uzh.ch/cluster/overview/) | [docs.cscs.ch/clusters](https://docs.cscs.ch/clusters/) |

Both systems use Slurm as the workload manager, so the core workflow of writing job scripts, submitting, and monitoring jobs is largely the same.
The key differences lie in scale, available hardware, and how software environments are managed.

{% endif %}
