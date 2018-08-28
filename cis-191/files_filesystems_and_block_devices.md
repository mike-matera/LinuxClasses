This lecture will help you understand the interlinked topics of Files, Filesystems and Block Devices. Collectively they create the way that Linux interacts with storage.
The lecture slides can be found [here](https://docs.google.com/presentation/d/1rpuae6Xpa0eZANFK9XHWSnxIM5p8ixkV8B2N7FBel38/edit?usp=sharing).

## Topics 

  * [Navigating the Filesystem](navigating_the_filesystem)
  * [Filesystems and Mount](filesystems_and_mount)
  * [Finding things with Find](finding_things_with_find)

## Using TAR  

  * The ''tar'' program packages files and directories into a single file 
    * Similar to a zip, but uncompressed 
    * Tar files are optionally compressed
  * The tar format preserves important file attributes 
  * Tar is good for backups (that's what it's designed for)
    * When you're making backups run ''tar'' as root. 

Create a tar file: 

<code bash>
$ tar -cvf <tar-file-name> <stuff-to-tar>
```

Extract a tar file:

<code bash>
$tar -xvf <tar-file-name>  
```

  * The ''c'' option is for "create"
  * The ''x'' option is for "extract"
  * The ''v'' option prints the file names as they are added/extracted (slows down tar)
  * The ''f'' option indicates that you want to use a file 
    * The file name **must** come right after the ''f'' 
    * If the file name is ''-'' stdin/stdout is used. 
    * Without the ''f'' option tar will try to find a tape device in ''/dev/rmt0'' or similar

## Disk Devices on your VM  

  * Your VMs at Cabrillo have extra disks 
    * You can add disks to local VMs too 
    * Check the documentation of your hypervisor for how. 
  * The vm disks are accessible using /dev directory entries 

| /dev/sda | System disk, Linux is installed here | 
| /dev/sdb | Extra disk 1 | 
| /dev/sdc | Extra disk 2 | 
| /dev/sdd | Extra disk 3 | 
| /dev/sde | Extra disk 4 | 

  * In this class you will format and use some of your extra disks. 

## Examining Mounts and Disk Space  

  * Linux doesn't use drive letters 
  * You have to ask the OS where data in the filesystem is actually stored. 
  * The mount command does that

```
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=1003940k,nr_inodes=250985,mode=755)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,noexec,relatime,size=204820k,mode=755)
/dev/mapper/ubuntu--vg-root on / type ext4 (rw,relatime,errors=remount-ro,data=ordered)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/lib/systemd/systemd-cgroups-agent,name=systemd)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=25,pgrp=1,timeout=0,minproto=5,maxproto=5,direct)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime)
mqueue on /dev/mqueue type mqueue (rw,relatime)
debugfs on /sys/kernel/debug type debugfs (rw,relatime)
fusectl on /sys/fs/fuse/connections type fusectl (rw,relatime)
/dev/sda1 on /boot type ext2 (rw,relatime,block_validity,barrier,user_xattr,acl)
lxcfs on /var/lib/lxcfs type fuse.lxcfs (rw,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other)
tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,size=204820k,mode=700,uid=1000,gid=1000)
```

  * There are a lot of //virtual// filesystems that mount tells you about. 
  * Can you find /dev/sda in the list above? 
  * What disk is the root filesystem ''/'' mounted on? 
    * Don't work too hard, you need to understand LVM to answer that question. 
  * The ''df'' command is a bit more useful.

```
$ df
Filesystem                  1K-blocks    Used Available Use% Mounted on
udev                          1003940       0   1003940   0% /dev
tmpfs                          204820    5892    198928   3% /run
/dev/mapper/ubuntu--vg-root  60266580 2553248  54628840   5% /
tmpfs                         1024088       0   1024088   0% /dev/shm
tmpfs                            5120       0      5120   0% /run/lock
tmpfs                         1024088       0   1024088   0% /sys/fs/cgroup
/dev/sda1                      482922  206104    251884  46% /boot
tmpfs                          204820       0    204820   0% /run/user/1000
```

  * The ''df'' command doesn't show you most virtual filesystems 
  * It shows you how much disk space is free, which is important. 
  * You can ask ''df'' about a particular place 

```
$ df /home
Filesystem                  1K-blocks    Used Available Use% Mounted on
/dev/mapper/ubuntu--vg-root  60266580 2553248  54628840   5% /
```


