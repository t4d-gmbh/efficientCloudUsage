{% if build == "slides" %}
# Interacting with the Cluster
{% else %}
## How to Interact
{% endif %}

{% if slide %}

:::::{grid} 2
:gutter: 2

::::{grid-item-card} <i class="fas fa-terminal"></i> SSH
```bash
ssh user@cluster.example.com
```
- Submit & monitor jobs (`sbatch`, `squeue`)
- Scriptable, automatable
::::

::::{grid-item-card} <i class="fas fa-globe"></i> Open OnDemand
Browser-based portal:
- File management, job composer
- Interactive apps (Jupyter, VS Code)
::::
:::::

{% endif %}

{% if page %}

**SSH** — the standard CLI interface. Connect, submit jobs, and monitor the queue via Slurm commands (`sbatch`, `squeue`, `sinfo`).

**Open OnDemand** — a web portal with file browser, job composer, and interactive apps (Jupyter, RStudio, VS Code) running on cluster resources.

Most users combine both: SSH for batch workflows, OnDemand for interactive sessions.

{% endif %}
