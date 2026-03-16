### Docker & Podman

```{compound}
{.centered}
{.bigger}
**The "Isolation First" Philosophy**
```

{% if slide %}
**Concept:** Treats a container like an **independent mini-computer**.

* **Philosophy:** Isolation by default (separate network stack, isolated filesystem, internal root user).
* **Strengths:** Microservices, web infrastructure, and cloud environments.
* **HPC Challenges:** * Tedious configuration required to pass user permissions and mount host directories.
  * Historically requires `root` privileges (a critical security risk on shared clusters).
{% endif %}

{% if page %}
Docker, and its daemonless alternative Podman, treat a container as an independent mini-computer.

* **The Philosophy:** Isolation by default. A Docker container is designed to be completely walled off from the host system. It possesses its own network stack, a completely separate filesystem, and its own internal root user. Interacting with the host or the outside world requires explicitly configuring access (punching holes in the sandbox).
* **Strengths:** This strict isolation makes Docker and Podman ideal for microservices and cloud environments. They are the foundation of modern web infrastructure, excelling at running web servers or databases where a compromised container must not be allowed to affect the rest of the server.
* **Weaknesses:** They struggle in environments where the goal is simply to execute a script on existing data rather than running a separate OS environment. Passing current user permissions, home directories, and network access into a Docker container requires tedious configuration. Furthermore, Docker historically requires `root` privileges, creating significant security vulnerabilities on shared machines like HPC clusters.
{% endif %}

