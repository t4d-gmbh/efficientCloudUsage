## 📝 Homework: Embarrassingly Parallel HelloWorld 📝

:::{admonition} Due in 1 week
:class: important

1. **Create 3 greeting files for 3 different courses**  
   We want to run the HelloWorld pipeline once for each of three courses (e.g. `pSciComp`, `math1`, `linalg3`), producing a separate greeting file per course.

2. **Adapt the configuration**  
   The input data already contains people enrolled in multiple courses. We need a config that selects which course to greet for.

   _Checkout the branch that contains this already:_
   ```bash
   git checkout parallel_config
   ```

3. **Introduce an environment variable to select the course**  
   Instead of hardcoding the course name, the script should read it from an environment variable (e.g. `COURSE_NAME`).

   _Checkout the branch that contains this already:_
   ```bash
   git checkout parallel_env
   ```

4. **Adapt the Slurm script for multi-node execution**  
   Write (or adapt) the `.sh` submission script so that it uses **multiple nodes** (or array jobs), setting the `COURSE_NAME` environment variable appropriately for each.

   A Slurm job array is a clean way to do this:

   ```bash
   #!/bin/bash
   #SBATCH --job-name=hello-parallel
   #SBATCH --array=0-2
   #SBATCH --cpus-per-task=1
   #SBATCH --mem=4G
   #SBATCH --time=00:10:00
   #SBATCH --output=logs/%A_%a.out

   COURSES=("pSciComp" "math1" "linalg3")
   export COURSE_NAME=${COURSES[$SLURM_ARRAY_TASK_ID]}

   module load apptainer

   apptainer exec \
     --env COURSE_NAME=$COURSE_NAME \
     --env-file .env \
     --bind /scratch/$USER/exoHelloWorld/data/raw:data/raw \
     --bind /scratch/$USER/exoHelloWorld/data/final:data/final \
     env-sif_latest.sif \
     python scripts/say_hello.py
   ```

5. **Invite us to your repository**  
   Add [@matteodelucchi](https://github.com/matteodelucchi) and [@j-i-l](https://github.com/j-i-l) as collaborators so we can see that you at least tried :-)
:::
