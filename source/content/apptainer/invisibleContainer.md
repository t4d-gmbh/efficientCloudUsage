# The "Invisible" Container

```{compound}
{.bigger}
**...and its hidden cost**
```

{% if slide %}
**Core Philosophy:** Integration over Isolation.
* Functions similarly to a local Python virtual environment.
* **Shell Overlay:** `apptainer shell`  overlays the container's internal filesystem onto the current session.
* Automatically mounts the current working directory (`$PWD`) and home directory (`$HOME`).
{% endif %}

{% if page %}
Apptainer’s core philosophy is **Integration over Isolation**.
Consequently, it can be used to create an "invisible" container that behaves almost exactly like a local Python virtual environment, but without generating a massive metadata footprint on the cluster's shared filesystem.

When the `apptainer shell` command is executed, Apptainer does not boot up an isolated mini-computer.
Instead, it takes the container's internal filesystem (which contains the required executable and installed libraries) and seamlessly overlays it onto the current session. 

* The current working directory (`$PWD`) and home directory (`$HOME`) are automatically mounted.
* By prepending the virtual environment to the `$PATH` within the container recipe, the system intercepts commands (like `python`) and utilizes the containerized version instead of the host cluster's default.

As a result, projects can be navigated, code edited, and scripts executed normally.
The container acts as a transparent lens that simply provides the correct software dependencies.
{% endif %}

## The Hidden Cost
```{compound}
{.bigger}
**Reproducibility vs. Convenience**
```

{% if slide %}
*⚠️ **The Danger:** Loss of strict containment.*

* The container inherits `$HOME`, `$PWD`, and host environment variables.
* Scripts might accidentally rely on hidden host files (e.g., `~/.local/` or `~/.bashrc`), causing failures when shared with others.
* **The Solution:** Use `--containall` or `--cleanenv` flags for strict, publication-grade reproducibility.
{% endif %}

{% if page %}
While this "invisible" integration is incredibly convenient for active development and debugging, it comes at the cost of strict containment. Because the container automatically inherits the `$HOME` directory, the `$PWD`, and many of the host's environment variables, the environment is not perfectly isolated.

* **The Danger:** A script might accidentally read a hidden configuration file in the host's `~/.local/` directory or rely on a custom environment variable set in a personal `~/.bashrc`.
* **The Result:** The script may execute perfectly for the original author on the cluster. However, if the `.sif` file and the script are shared with a colleague or executed on a different system, failures may occur because the host environment differs.

For strict, publication-grade reproducibility, Apptainer execution must eventually be restricted. Utilizing execution flags such as `--containall` or `--cleanenv` forces Apptainer to behave more like Docker, ensuring the container relies *only* on the code and data explicitly passed into it during batch submissions.

{% endif %}

```{admonition} Integration reduces containment
:class: warning
Apptainers "Integration First" approach bares the risk of not fully contained and documented environments, which can hamper reproducibility.
```
