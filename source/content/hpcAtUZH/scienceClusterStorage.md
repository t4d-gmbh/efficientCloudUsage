## Science Cluster Storage

{% if slide %}

```{admonition} <i class="fa-solid fa-circle-info"></i> Details
:class: margin
[docs.s3it.uzh.ch/cluster/data](https://docs.s3it.uzh.ch/cluster/data/)
```

{% endif %}

{% if slide %}

:::::{grid} 1 1 2 2
:gutter: 2

::::{grid-item-card} <i class="fa-solid fa-house"></i> `/home`
:class-header: sd-bg-primary sd-bg-text-light

Persistent — CephFS

^^^

- Default login directory
- Backed by CephFS (distributed)
- Accessible from **all** nodes
- Every file operation = **network request**

+++
<i class="fa-solid fa-triangle-exclamation"></i> Not for heavy I/O
::::

::::{grid-item-card} <i class="fa-solid fa-bolt"></i> `/scratch`
:class-header: sd-bg-warning sd-bg-text-dark

Temporary — CephFS

^^^

- High-throughput staging area
- Also CephFS-backed, shared across nodes
- For **active computation** data
- Subject to **purging policies**

+++
<i class="fa-solid fa-broom"></i> Clean up after your jobs
::::

:::::

:::{admonition} CephFS and the Metadata Server
:class: warning

Both `/home` and `/scratch` sit on CephFS.
Every file operation — open, close, stat — triggers a network request to CephFS's Metadata Server.

Thousands of small file accesses (e.g., Python imports, `pip install`) can bottleneck the shared metadata layer.
:::

{% else %}

### Storage Architecture

The Science Cluster provides two main storage areas, both backed by CephFS (a distributed network filesystem):

**`/home` (Persistent):**
The user's home directory is the default entry point.
It resides on CephFS and is mounted across every node in the cluster (login and compute nodes alike).
This means files are available regardless of where a job runs.
However, every file operation (open, close, stat, read, write) results in a network request to the CephFS metadata server.
Heavy I/O workloads against `/home` degrade performance for all users on the shared filesystem.

**`/scratch` (Temporary):**
A shared scratch space is available for staging large datasets and intermediate results during active computation.
Like `/home`, it is backed by CephFS and accessible from all nodes.
Scratch storage is subject to automated purging policies: data should be moved elsewhere once processing is complete.

### Performance Considerations

Because both areas rely on CephFS, every file operation traverses the network and hits the shared metadata server.
Workloads that open thousands of small files (common during Python package imports or data loading with many individual files) can saturate the metadata layer.
Strategies to mitigate this include:

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
- Use [SquashFS](https://www.kernel.org/doc/html/latest/filesystems/squashfs.html) for read-only data.
- Using containers (Apptainer) to bundle dependencies into a single image file.
:::
:::{grid-item}
- Packing many small files into archives (e.g., `.tar`) before processing.
- Staging data to node-local storage (if available) before computation.
:::
::::

{% endif %}
