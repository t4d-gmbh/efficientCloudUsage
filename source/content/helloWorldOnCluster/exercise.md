## ✏️ Exercise: HelloWorld on the Cluster ✏️

```{epigraph}
**End-to-end HPC workflow with containers, object storage, and Slurm**
```

Head over to the [HelloWorld](https://github.com/pSciComp/exoHelloWorld) repository and work through [`HPC - Exo 2`](https://github.com/pSciComp/exoHelloWorld/blob/main/exercises/hpc/Exo_2.md).

{% if slide %}
:::{admonition} ORAS
:class: note margin
[ORAS](https://oras.land/) (OCI Registry As Storage) is a standardized storage service for container images and artifacts, adhering to the [Open Container Initiative (OCI)](https://opencontainers.org/) Distribution Specification. It allows storing and managing container images, ensuring interoperability across platforms. 
:::

:::{admonition} Your tasks
:class: tip

1. **Track input data with git lfs**  
   Add `data/raw/*.json` (or your input data) to git LFS tracking:
   ```bash
   git lfs install
   git lfs track "data/raw/*.json"
   git add .gitattributes data/raw/
   ```

2. **Get the container from object storage**  
   Pull the pre-built container from the GitHub Container Registry:
   ```bash
   apptainer pull oras://ghcr.io/pscicomp/env-sif:latest
   ```
   Or from UZH's Swift object storage (if configured).

3. **Write the submission script**  
   Complete the template below — fill in the gaps marked `___`:

   ```bash
   #!/bin/bash
   #SBATCH --job-name=helloworld
   #SBATCH --cpus-per-task=___
   #SBATCH --mem=___
   #SBATCH --time=___
   #SBATCH --output=logs/%j.out

   module load apptainer

   apptainer exec \
     --env-file .env \
     --bind ___:data/raw \
     --bind ___:data/final \
     env-sif_latest.sif \
     ___ scripts/say_hello.py
   ```

4. **Upload results to object storage**  
   After the job completes, push output data to object storage:
   ```bash
   # Using swift CLI (UZH Science Cloud)
   swift upload <container-name> data/final/greeting.txt
   # Or using mc / rclone / boto3
   ```
:::

{% else %}

### Step-by-step

#### 1. Track input data with git LFS

Large or binary input files should not be tracked directly by git. Use git LFS instead:

```bash
git lfs install
git lfs track "data/raw/*.json"
git add .gitattributes data/raw/
git commit -m "Track input data with LFS"
```

#### 2. Get the container from object storage

If you built and pushed the container in a previous exercise (see [`Struct E3`](https://github.com/pSciComp/exoHelloWorld/blob/main/exercises/structure/Exo_3.md)), you can pull it directly:

```bash
apptainer pull oras://ghcr.io/pscicomp/env-sif:latest
```

Alternatively, download from UZH's Swift object storage if the container was uploaded there.

#### 3. Write the Slurm submission script

Create a file `scripts/slurm/submit.sh` based on the template:

```bash
#!/bin/bash
#SBATCH --job-name=helloworld
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=00:10:00
#SBATCH --output=logs/%j.out

module load apptainer

apptainer exec \
  --env-file .env \
  --bind /scratch/$USER/exoHelloWorld/data/raw:data/raw \
  --bind /scratch/$USER/exoHelloWorld/data/final:data/final \
  env-sif_latest.sif \
  python scripts/say_hello.py
```

Submit with `sbatch scripts/slurm/submit.sh` and monitor with `squeue -u $USER`.

#### 4. Upload results to object storage

Once the job completes, push the output to persistent storage outside the cluster:

```bash
# Using swift CLI
swift upload my-results data/final/greeting.txt

# Or using rclone
rclone copy data/final/greeting.txt remote:my-bucket/results/
```

This closes the loop: input data comes from version control (LFS) or object storage, the container comes from a registry, computation happens on the cluster, and results go back to object storage.

{% endif %}
