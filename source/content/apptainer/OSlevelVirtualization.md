## The Principle of OS-Level Virtualization

{% if slide %}
**Core Concept:** Containers utilize OS-level virtualization.
* **Unlike VMs:** They do not emulate separate hardware or run heavy guest Operating Systems.
* **Shared Kernel:** They share the host machine's OS kernel.
* **Sandboxing:** Linux features create restricted environments (isolated filesystems, networks, and process trees).
* **Application-like Execution:** Manifests can define default commands (e.g., `ENTRYPOINT` or `%runscript`), enabling the container to execute as a single packaged application rather than a booted OS.
* **The Key Difference:** Docker and Apptainer differ fundamentally in *how* they configure this sandbox.
{% endif %}

{% if page %}
To understand container tools, the principle of OS-level virtualization must first be established. Unlike traditional Virtual Machines (VMs) that emulate entirely separate hardware and run their own heavy guest Operating Systems, containers share the host machine's OS kernel. They utilize Linux features to create restricted "sandboxes" where a group of processes operates as if it has its own independent filesystem, network, and process tree. 

Furthermore, because containers do not need to boot a full operating system, their build manifests (such as a Dockerfile or an Apptainer definition file) can declare a default executable. This allows a container to run immediately as a single, self-contained software package (an application) rather than behaving like a general-purpose computer waiting for user login.

The core difference between Docker (or Podman) and Apptainer lies in how they configure and manage this sandbox.

:::{admonition} Firmware → Kernel → OS
:class: note

When you power on a computer, three layers come into play in sequence:

- **Firmware** is the bare-minimum code stored in non-volatile memory (ROM/flash). It runs first at power-on, initializes the hardware, and bootstraps the next stage. Firmware is on many consumer products (headphones, dockingstations, RC-cars, ...), OS is not necessarily.
- **Kernel** is the core component of the operating system. It acts as the doorkeeper and translator between the OS and the hardware, mediating all access to the CPU, RAM, I/O devices, and system calls.
- **Operating System (OS)** is the full software stack built on top of the kernel. It adds a user interface (GUI or CLI), file management, and the environment in which applications run.

The boot sequence is: **Firmware → Kernel → Full OS**.

{.smaller} 
([Source](https://stackoverflow.com/a/39279496/6098024))
:::
{% endif %}
