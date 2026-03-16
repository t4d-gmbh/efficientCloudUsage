### Apptainer

```{compound}
{.centered}
{.bigger}
**The "Integration First" Philosophy**
```

{% if slide %}
**Concept:** Treats a container like a **software package**.

* **Philosophy:** Integration by default ("Mobility of Compute").
* **Behavior:** The user outside the container remains the exact same user inside. 
* **Strengths:** Scientific computing; automatically uses the host network and mounts the current working directory.
* **Challenges:** Poorly suited for hosting background services (e.g., web servers) due to the lack of default isolation.
{% endif %}

{% if page %}
Apptainer (formerly known as Singularity) adopts a fundamentally different approach, treating a container more like a standard software package.

* **The Philosophy:** Integration by default, often referred to as "Mobility of Compute." Apptainer operates on the principle that the user executing the container on the host system remains the exact same user inside the container. It automatically mounts the current working directory and utilizes the host's network natively.
* **Strengths:** Apptainer shines in scientific computing and High-Performance Computing (HPC). It allows complex software stacks to be packaged and run seamlessly over existing data without encountering permission errors or requiring administrative rights.
* **Weaknesses:** Due to its deliberate lack of default network and process isolation, Apptainer is poorly suited for hosting background services, such as web servers or isolated databases.
{% endif %}


