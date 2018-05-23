# TSCC information

## Queues

- `home-gibbs` (shared among a few groups)

  - Number: About 24 nodes
  - Hardware: GTX Titan X (Maxwell), 2x 8-core Xeon E5-2630 2.4 GHz, 64 GB memory
  - Wall clock: No limit

  Each GPU node has 8 GPUs. Specific GPU resources can be requested in the PBS script with `-l nodes=1:ppn=2:gpuTitan` (for example).

- `home-mgilson` (owned by us)

  - Number: 10 CPU nodes, 7 GPU nodes
  - CPU Hardware: Each node has 2x 8-core Xeon E5-2670 2.6 GHz, 128 GB memory, Infiniband
  - GPU Hardware: 3 nodes with GeForce GTX 980 (2x 6-core Xeon 2.6 GHz, 64 GB), 2 nodes with GeForce GTX 780 (2x 6-core Xeon 2.6 GHz, 32 GB), 2 nodes with GeForce GTX 680 (2x 6-core Xeon 2.3 GHz, 64 GB)
  - Wall clock: No limit

  Each GPU node has 4 GPUs. Specific GPU resources can be requested in the PBS script with `-l nodes=1:ppn=2:gpu680,gpu780` (for example).

- `gpu-hotel` (owned by TSCC)

  - Number: 3
  - Hardware: GeForce GTX 680 2x 6-core Xeon E5-2630 2.3 GHz, 32 GB memory
  - Wall clock: 72 hours

  Each GPU node has 4 GPUs. Hotel are nodes that TSCC owns.

- `hotel` (owned by TSCC)
  - Number: About 51
  - Hardware: 2x 8-core Xeon E5-2670 2.6 GHz, 64 GB memory, Infiniband (but may not all be on the same switch)
  - Wall clock: 72 hours

  Can specify `ibswitch1` or `ibswitch2` (see [here](http://www.sdsc.edu/~hocks/FG/TSCC.torque.html)).

- `gpu-condo` (the collection of all the nodes owned by groups)

  - Number: About 55
  - Hardware: Mix of GTX Titan X, GTX 980, GTX 780, GTX 680, GTX 1080 Ti
  - Wall clock: 8 hours

  Each GPU node has 4 or 8 GPUs.

- `condo` (the collection of all the nodes owned by groups)

  - Number: About 232
  - Hardware: Mix (between 16 and 24 cores per node), 64-512 GB memory
  - Wall clock: 8 hours

- `glean` (whatever is available of the groups; often broken)

  - Number: About 232
  - Hardware: Mix
  - Wall clock: No limit

  Glean gives whatever is available. Jobs can be killed at any time.

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
