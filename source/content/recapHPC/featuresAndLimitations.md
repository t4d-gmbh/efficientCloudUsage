{% if build == "slides" %}
# Features & Limitations
{% else %}
## Features & Limitations
{% endif %}

{% if slide %}

:::{admonition} <i class="fa-solid fa-comments"></i> Discussion
:class: attention

**What are the main features *and* limitations of an HPC cluster?**
:::

---

:::::{grid} 2
:gutter: 2

::::{grid-item-card} <i class="fas fa-check-circle"></i> Features
- **Massive parallelism** — thousands of cores/GPUs
- **On-demand resources** — request exactly what you need
- **Fair usage** — priority-based scheduling
- **Large shared storage** — TB to PB scale
::::

::::{grid-item-card} <i class="fas fa-exclamation-triangle"></i> Limitations
- **Queuing** — jobs wait for resources
- **Limited configurability** — no root, modules/containers only
- **Shared environment** — "noisy neighbors"
- **Storage constraints** — quotas, purging, shared I/O
::::
:::::

{% endif %}

{% if page %}

| | |
|---|---|
| <i class="fas fa-check-circle"></i> **Massive Parallelism** | Thousands of CPU cores and GPUs available for simultaneous use. |
| <i class="fas fa-check-circle"></i> **On-Demand Resources** | Request specific CPUs, GPUs, memory, and wall-time per job. |
| <i class="fas fa-check-circle"></i> **Fair Usage** | Priority-based scheduling prevents monopolization. |
| <i class="fas fa-check-circle"></i> **Large Shared Storage** | Parallel filesystems (Lustre, GPFS, Ceph) accessible from all nodes. |
| <i class="fas fa-exclamation-triangle"></i> **Queuing** | Jobs wait in a priority queue; large requests wait longer. |
| <i class="fas fa-exclamation-triangle"></i> **Limited Configurability** | No root access; software managed via modules or containers. |
| <i class="fas fa-exclamation-triangle"></i> **Shared Environment** | Multi-tenant system; other workloads can affect I/O performance. |
| <i class="fas fa-exclamation-triangle"></i> **Storage Constraints** | Home quotas, scratch purging, shared filesystem I/O bottlenecks. |

{% endif %}
