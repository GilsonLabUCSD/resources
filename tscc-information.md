# TSCC information

## Queues

- `home-gibbs` (shared among a few groups)

  - Number: 24 nodes
  - Hardware: 8x GTX Titan X (Maxwell), 2x 8-core Xeon E5-2630 @ 2.4 GHz, 64 GB DDR3 memory
  - Wall clock: No limit
  
    Two notes about walltime limit that also apply to other queues: 
    - when maintenance is scheduled, it will prevent jobs whose walltime limit is longer than the time left until maintenance from running. 
    - When walltime limit is not set in a job, e.g. `qsub -I -l nodes=1:ppn=2:gpu -q home-gibbs -A mgilson-gibbs`, the limit defaults to 1 hours which is often too short.

Each GPU node has 8 GPUs and 16 cores (in multiples of 21 CPU:GPU ratio). Specific GPU resources can be requested in the PBS script with `-l nodes=1:ppn=2:gpuTitan` (for example).

- `home-hopper` (shared among a few groups)
  - Number: 4 CPU nodes, 17 GPU nodes
  - GPU Hardware: 8x RTX 3090 (Ampere), 2x 16-core AMD EPYC Rome 7282 @ 2.8 GHz, 256 GB DDR4 memory
  - CPU Hardware: 2x 32-core AMD EPYC Rome, 256 GB DDR4 memory
  - Wall clock: No limit 
    
Each GPU node has 8 GPUs and 32 cores (in multiples of 4:1 CPU:GPU ratio). Specific GPU resources can be requested in the PBS script with `-l nodes=1:ppn=4:gpu3090` (for example).

- `home-mgilson` (owned by us)

  - Number: 7 CPU nodes
  - CPU Hardware: Each node has 2x 8-core Xeon E5-2670 @ 2.6 GHz, 128 GB memory, Infiniband
  - Wall clock: No limit

  Each GPU node has 4 GPUs. Specific GPU resources can be requested in the PBS script with `-l nodes=1:ppn=3:gpu980` (make sure you specify 3 processors for every GPU). To request all 4 GPUs in a GPU node, use `-l nodes=1:ppn=12:gpu`.

- `gpu-hotel` (owned by TSCC; we get charged for this)

  - Number: 3
  - Hardware: GeForce GTX 680 2x 6-core Xeon E5-2630 @ 2.3 GHz, 32 GB memory
  - Wall clock: 72 hours

  Each GPU node has 4 GPUs. Hotel are nodes that TSCC owns.

- `hotel` (owned by TSCC; we get charged for this)
  - Number: About 51
  - Hardware: 2x 8-core Xeon E5-2670 @ 2.6 GHz, 64 GB memory, Infiniband (but may not all be on the same switch)
  - Wall clock: 72 hours

  Can specify `ibswitch1` or `ibswitch2` (see [here](http://www.sdsc.edu/~hocks/FG/TSCC.torque.html)).

- `gpu-condo` (the collection of all the nodes owned by groups)

  - Number: About 51
  - Hardware: Mix of GTX Titan X, GTX 980, GTX 780, GTX 680, GTX 1080 Ti
  - Wall clock: 8 hours

  Each GPU node has 4 or 8 GPUs.

- `condo` (the collection of all the nodes owned by groups)

  - Number: About 232. One user can only occupy 512 condo processors at the same time.
  - Hardware: Mix (between 16 and 24 cores per node), 64-512 GB memory
  - Wall clock: 8 hours

- `glean` (whatever is available of the groups; often broken)

  - Number: About 232. One user can only occupy 1024 glean processors at the same time.
  - Hardware: Mix
  - Wall clock: 8 hours (used to be unlimited but they changed in April 2020)

  Glean gives whatever is available. Glean is free of charge to our lab account. Jobs can be killed at any time when another user requests the same resource using non-glean queue.

### Scratch

From the [documentation](http://www.sdsc.edu/support/user_guides/tscc-quick-start.html), the home directories for all users are NFS mounted through a single server and thus, they are slow. Don't run jobs in your home directory. `/oasis/tscc/scratch` is optimized for efficient handling of large files (you can create a directory below that, e.g., `/oasis/tscc/scratch/$USER`) for your jobs. This works well with 10-200 files open simultaneously. Local storage -- disks directly connected to each node -- can be used too and is available as `$TMPDIR`, but the files are purged when the job completes, so make sure to `rsync` these somewhere else! Size is limited to 200 GB for `home-gilson` GPU nodes and 2 TB for `home-gibbs` GPU nodes.


## Useful commands

- `pbsnodes`: show all the nodes and their properties
- `lsjobs gpu`: list jobs running on the GPU nodes
- `lsjobs --property=mgilson-node`: list all jobs (including `glean` running on `home-mgilson` nodes)
- `qsub -I -X -q home-gibbs -l nodes=1:ppn=2 -l walltime=0:50:00`: request an interactive job on the `home-gibbs` queue
- `showq` / `qstat`: show status of jobs
- `showq -i`: show jobs under consideration by the scheduler (only about 10 jobs at a time for each user are under consideration)
- `python3 /home/henrikse/pe/tsccqstat.py <username>`: detailed listing of running jobs and their nodes (should be python 2.7+ compatible)
