# To-do
- ~~Inventory hard drives in Jane's office~~
- ~~Change `cron` scripts from Niel's email address to mine~~
- Install AMBER18 on `entropy`
- ~~Come up with longterm plan for Amanda's data.~~

# Questions
- Why are some users' home directories mounted on drives labeled as `backup`?
  - Because originally it was a backup of `/data` but when we ran out of space, before or around the time we got GPFS, some new users' home directories were placed on `/backup2`.
- ~~Is AMBER18 installed on `entropy`?~~

# Log
- 2018-06-19 17:33 Added `mschauperl` as a user on `onsager`
  - Look up existing user ID and group ID from `/etc/passwd` on `kirkwood`
  - `/usr/sbin/useradd -u 520237 -g 7195 -c 'Michael Schauperl' -d /backup/mschauperl mschauperl`
  - Copy existing entry from `/etc/shadow`
  - Copy existing entry from `/etc/group`
  - Copy existing entry from `/etc/gshadow`
  - Make `data_kirkwood` directory
  - Add `137.110.139.94:/backup2/mschauperl     /backup/mschauperl/data_kirkwood nfs   auto,hard,intr,tcp,timeo=100,bg    0 0
` to `/etc/fstab`
- 2018-06-19 17:33 Added `jiyin` as a user on `entropy`

- 2018-06-20 12:29 Added `jlomibao` as a user on `entropy`
  - `passwd -e jlomibao` forces a password change on login
  - John's home directory is on `/home/` on `entropy`. This should be migrated to `/data`.

- 2018-06-20 13:23 Setup `mouskouri` (Ido) to automount `onsager` via NFS with `/etc/fstab`

- 2018-07-05 09:47 Updated `cmake` on `vulcan` to 3.11.0 https://gist.github.com/1duo/38af1abd68a2c7fe5087532ab968574e
- 2018-07-05 09:47 Installed Developer Toolset-7 https://www.softwarecollections.org/en/scls/rhscl/devtoolset-7/
  - Previously gcc was 4.8.5, now gcc 7.3.1 (after calling `scl enable devtoolset-7 bash`)
  - Turns out this version of `gcc` doesn't work with `nvcc`, so don't use it.
- 2018-07-05 11:56 Compile `gromacs` 2018.2 on `vulcan`
  -  https://mailman-1.sys.kth.se/pipermail/gromacs.org_gmx-users/2018-May/120194.html
  ```
  cmake ..   -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=ON -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda -DGMX_SIMD=AVX2_256
```
- 2018-07-06 09:28 Added `lifeng` as a user on `deltag`
- 2018-07-11 16:00 Added `lifeng` as a user on `entropy` in group 7195 with home directory on `/backup`
- 2018-07-16 11:42 Added `benshalom` as a user on `t-rex` in group 7195 with home directory on `/home`
- 2018-09-26 16:38 Added `jlomibao` to `kirkwood`.
- 2018-10 The motherboard on `t-rex` has died. Data is fine.
- 2018-11 Went downstairs to discover multiple disk failures in both JBODs attached to `kirkwood` and `onsager`, half of `vulcan`'s GPUs not being detected, and a dead UPS battery.
   - Replaced two failed drives on `kirkwood`
   - Replaced two failed drives on `onsager`, leaving 1 failed disk in the JBOD
   - `kirkwood` rebuilt both of its RAID arrays over night
   -  `onsager` rebuild the single RAID array overnight, remaining array is in "Degraded" status
   - Two _additional_ disks failed on `kirkwood` during the rebuild, this caused the RAID array to go into "Failed" status, even though I was under the impression the RAID6 array could handle two simultaneous drive failures at any moment (...?)
   - Initially Niel and I thought "Failed" just meant the RAID controller took the disks offline, but now I think it indicates a serious hardware fault, the ARC1882 series manual is not very useful.
   - I took the failed array offline using `/root/Microway/areca-MWAY9/cli64`. This made the RAID array disappear in `vsf info` and `rsf info`.  `rsf activate raid=2` did not work.
   - We rebooted both `kirkwood` and the JBOD and were greeted with [this screen]("images/5cfa265b-02a9-4500-a30e-10b17d8483bb.jpg"). RHEL would not boot after that screen.
   - I then tried (again) to manually reactive all the RAID arrays to no avail, and tried "Create Raid Set" because the internet suggested recreating the missing RAID set without initialization (`noinit` option, I think) but no devices were available.
   - I used the "Rescue Raid Set" option, following some [unsubstantiated](http://johnrey.es/index.php/2018/10/31/areca-raid-raid-configuration-disappears/) [suggestions](http://www.softwareguru.com/TheGurusBlog/tabid/214/EntryId/6/ARECA-RAID-Rescue.aspx),  to bring the array back online using something called a `LeVeL2ReScUe`. Specifically, I did `RESCUE`, reboot, `SIGNAT, `LeVeL2ReScUe`, reboot, `SIGNAT`, reboot. This brought the array back [online](images/1bd604c5-83be-4400-81d8-b9bf191f0529.jpg).
   - An [alternative](https://serverfault.com/questions/369825/areca-1280ml-raid6-volume-set-failed) may have been trying to get the data off disk-by-disk. Didn't look into it.
   - After booting, there were still problems with `/backup2`
   ```
   [root@kirkwood ~]# mount -a
   mount: Structure needs cleaning
   mount: special device /backup2/mschauperl does not exist
   ```
   - Tried various [work-arounds](https://serverfault.com/questions/777299/proper-way-to-deal-with-corrupt-xfs-filesystems), by copying the metadata to `/data`  but `xfs_metadump` would crash with serious-looking `glibc` core dump.

   - Eventually did `xfs_repair -L /backup2` (https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/xfsrepair) which unfortunately resulted in most or all the data recovered, but shoved into `/backup2/lost+found` with scrambled file names.
