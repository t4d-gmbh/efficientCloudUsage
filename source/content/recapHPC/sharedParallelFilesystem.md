---
sd_hide_title: true
---
## Shared Parallel Filesystem

```{figure} ./../_static/sharedFilysystem.png
:alt: Shared File System
:width: 100%
```
{% if page %}
Shared parallel file systems expose a single POSIX‑compatible namespace that is mounted on many compute nodes.
A shared file system splits the workload internally: one set of servers stores the actual file data, while a separate set of metadata servers maintains directory hierarchies, inodes, permissions and lock state.

All operations that change the namespace (creates, deletes, renames, permission checks) must go through the metadata layer, which therefore becomes the main scalability bottleneck in a Slurm cluster; the data layer can scale more easily because reads and writes are distributed across many storage nodes.
CephFS is a common implementation of this architecture, using object‑storage nodes for data and dedicated MDS nodes for metadata.

{% endif %}



