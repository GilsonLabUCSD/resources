# Local (common) machines


- `rosenbluth` **(Compute)**
  - 2x 64-core AMD EPYC Milan 7713 CPU @ 2.0GHz
  - 512 GB memory
  - 4x GeForce RTX 3090 (Ampere)
  - 480 GB SSD (RAID 1), 8 TB NVMe SSD `/scratch`
- `vulcan` **(Compute)**
  - 2x 10-core Intel(R) Xeon(R) Silver 4114 CPU @ 2.20GHz
  - 98 GB memory
  - 8x GeForce GTX 1080 Ti (Pascal)
  - 256 GB SSD, 2 TB `/data`
- `entropy` **(Compute)**
  - 2x 6-core Intel(R) Xeon(R) X5680 CPU @ 3.33GHz
  - 72 GB memory
  - 4x GeForce GTX 980
  - 1 TB `/`, 7.3 TB (RAID 5) `/data`
- `kirkwood` **(Storage)**
  - 2x 8-core Intel(R) Xeon(R) CPU E5-2687W 0 @ 3.10GHz
  - 128 GB memory
  - No GPUs
  - 180 GB `/`, 75 GB `/home`, 36 TB (RAID 6) `/data`, 44 TB (RAID 6) `/data2` and `/backup2`
  - GPFS mounted `/gpfs`
- `onsager` **(Storage)**
  - 2x 8-core Intel(R) Xeon(R) CPU E5-2687W 0 @ 3.10GHz
  - 128 GB memory
  - No GPUs
  - 180 GB `/`,  75 GB `/home`, 36 TB (RAID 6) `/data`, 87 TB (RAID 6) `/backup`