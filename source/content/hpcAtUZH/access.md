## Accessing the Systems

{% if slide %}

::::{tab-set}

:::{tab-item} Science Cluster
**Prerequisites**:
1. UZH account with cluster access (request via [S3IT](https://docs.s3it.uzh.ch/cluster/overview/))
2. UZH network or VPN connection

**Connect**:
```bash
ssh <shortname>@cluster.s3it.uzh.ch
```

<i class="fa-solid fa-book"></i> [Cluster How-To](https://docs.s3it.uzh.ch/cluster/overview/)
:::

:::{tab-item} Supercomputer (Alps)
**Prerequisites**:
1. Account brokered through S3IT
2. Project allocation at CSCS

**Connect**:
```bash
ssh <username>@alps.cscs.ch
```

<i class="fa-solid fa-book"></i> [Supercomputer How-To](https://docs.s3it.uzh.ch/supercomputer/overview/)
:::

::::

{% else %}

### Science Cluster

Access to the Science Cluster requires a UZH account with cluster privileges and a connection to the UZH network (on campus or via VPN).

```bash
ssh <shortname>@cluster.s3it.uzh.ch
```

Requesting access and detailed setup instructions are documented in the [Cluster How-To](https://docs.s3it.uzh.ch/cluster/overview/).

### Supercomputer (Alps)

Access to Alps is brokered through S3IT and requires a project allocation at CSCS.
This typically involves an application process tied to a research project.

```bash
ssh <username>@alps.cscs.ch
```

Setup instructions and account management are covered in the [Supercomputer How-To](https://docs.s3it.uzh.ch/supercomputer/overview/).

### Common Workflow

Regardless of which system you use, the day-to-day workflow follows the same pattern:

1. **SSH** into the login node
2. **Prepare** your job script (resource requests, commands)
3. **Submit** via `sbatch job.sh`
4. **Monitor** with `squeue -u $USER`
5. **Retrieve** results from the shared filesystem

The Slurm commands are identical on both systems.
Differences lie primarily in available partitions, hardware, and how software environments are loaded.

{% endif %}
