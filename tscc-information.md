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


## Useful commands

- `pbsnodes`: show all the nodes and their properties
- `/usr/bin/python /opt/sdsc/bin/lsjobs`: list jobs running on the nodes (can also use the `gpu` flag to see GPU jobs, e.g., `lsjobs gpu`)
- `qsub -I -q home-gibbs -l nodes=1:ppn=2 -l walltime=0:50:00`: request an interactive job on the `home-gibbs` queue
 /usr/bin/python /opt/sdsc/bin/lsjobs gpu
- `showq` / `qstat`: show status of jobs
