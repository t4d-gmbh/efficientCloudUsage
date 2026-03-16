# System-Level Packages

{% if slide %}
**The Problem:** A lack of `sudo` privileges on clusters prevents the installation of system-level software (e.g., specific compilers, rendering engines).

**The Solution:** Build a container with its own Operating System and install the required tools inside during the build phase.
{% endif %}

{% if page %}
Perhaps the single greatest benefit of Apptainer on an HPC cluster is the ability to bypass system-level installation restrictions.

Without `sudo` (root) privileges on the cluster, users cannot run commands like `apt-get install` to add system-level software. If a workflow requires a specific C++ compiler, a video rendering engine like FFmpeg, or complex libraries not present on the cluster, waiting for IT support can cause significant delays.

**The Solution:** A container can be built with its own dedicated Operating System (e.g., Ubuntu), allowing required tools to be installed *inside* the container during the build phase.
{% endif %}

:::{admonition} Example
{.bigger}
**Installing GDAL**

{% if slide %}
**GDAL:** A complex C++ library for processing geospatial data.

**1. Definition File (`gdal_env.def`):**
```docker
Bootstrap: docker
From: ubuntu:24.04

%post
    apt-get update -y
    DEBIAN_FRONTEND=noninteractive \
    apt-get install -y \
    gdal-bin libgdal-dev
    apt-get clean

```

**2. Build & Use:**

```bash
apptainer build --ignore-fakeroot-command gdal_env.sif gdal_env.def
apptainer shell gdal_env.sif

```

{% endif %}

{% if page %}
GDAL is a heavy, complex C++ library used for processing geospatial and satellite data. It is notoriously difficult to install without root privileges. With Apptainer, a custom Ubuntu environment can be created that already includes it.

**`gdal_env.def`**

```docker
Bootstrap: docker
# A recent Ubuntu OS ensures compatibility with the cluster's kernel
From: ubuntu:24.04

%post
    apt-get update -y
    
    # Install the system-level packages as "root" inside the container
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        gdal-bin \
        libgdal-dev
        
    apt-get clean

```

**Building it on the cluster:**
*(Note: Using `--ignore-fakeroot-command` tells Apptainer to bypass strict cluster user-mapping policies that often break `apt-get` builds).*

```bash
apptainer build --ignore-fakeroot-command gdal_env.sif gdal_env.def

```

**Using it seamlessly:**
Once built, the `shell` command can be utilized. The container acts as a transparent overlay, providing access to the `gdalinfo` system command while allowing reads from satellite data residing on the cluster's shared network drive.

```bash
# On the cluster host (Fails because GDAL is not installed)
$ gdalinfo satellite_image.tif
-bash: gdalinfo: command not found

# Drop into the Apptainer shell
$ apptainer shell gdal_env.sif

# Inside the container (Works instantly, reading the host's file!)
Apptainer> gdalinfo satellite_image.tif
Driver: GTiff/GeoTIFF
Files: satellite_image.tif
Size is 512, 512

```

{% endif %}
:::
