# The HPC Cluster Environment

{% if slide %}
**Key Differences from Cloud:**
* **Multi-Tenancy & Security:** `root` access is a catastrophic security risk.
* **Shared Global Filesystems:** Read/write access requires an exact Unix user ID (UID) match.

**The Apptainer Advantage:**
* Executes without root privileges.
* Maps the UID 1:1 inside the container, ensuring files written to shared storage retain the correct ownership.
{% endif %}

{% if page %}
HPC clusters, managed by schedulers like Slurm, differ significantly from standard cloud servers:

1. **Multi-Tenancy & Security:** With hundreds of researchers sharing nodes, granting `root` access presents a catastrophic security risk.
2. **Shared Global Filesystems:** Data resides on networked storage. Reading and writing files requires the Unix user ID (UID) to match perfectly.

**Why Apptainer is suited for HPC:** It requires no root privileges to execute. Because the UID maps 1:1 inside the container, any file written to the shared storage is correctly owned by the actual user, rather than `root`.
{% endif %}
