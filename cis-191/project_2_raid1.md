In this project you'll create a RAID array with two disks on your VM. The you'll partition, format and mount it. 

## Introduction 

In the previous lab you used a spare disk to create mount points. In this lab you'll move the /var/www mount point onto a RAID disk. With your web content on RAID storage your webserver can suffer a disk failure without going down.

## Create The RAID1 Array 

In class you saw how to create a RAID1 array using the mdadm command. Assuming that you added disks last week you should be able to create an array with the command:

```
$ sudomdadm --create /dev/md0 --level 1 -n 2 /dev/sdc /dev/sdd
```

If you do it correctly you should be able to verify that your RAID is working using the command:

```
$ sudo mdadm --detail /dev/md0
```

Once you're convinced that your array is setup go to the next step. If you don't see proper output check for error messages in the output of the mdadm command.

## Partition, Format, Move 

Now that the RAID devices (/dev/md0) is setup follow the instructions in the previous project to do the following:
  - Partition /dev/md0. Create only one partition that uses the entire disk (name it www).
  - Format the partition you create (/dev/md0p1) with the btrfs filesystem.
  - Copy the contents of /var/www to your new filesystem.
  - Mount the new filesystem on /var/www
  - Update /etc/fstab

Reboot your VM and verify that your RAID device /dev/md0p1 is mounted on /var/www. If that worked go to the next step. 
## Degrade Your Array 

Use the mdadm command to mark one of your disks as failed. Use the manual page to discover how to do this. If you do it correctly the output of mdadm should look like this:

```
$ sudo mdadm --detail /dev/md0
/dev/md0:
    Version : 1.2
 Creation Time : Thu Oct 6 12:38:42 2016
  Raid Level : raid1
  Array Size : 20955136 (19.98 GiB 21.46 GB)
 Used Dev Size : 20955136 (19.98 GiB 21.46 GB)
 Raid Devices : 2
 Total Devices : 2
  Persistence : Superblock is persistent
  Update Time : Thu Oct 6 12:42:40 2016
     State : active, degraded
Active Devices : 1
Working Devices : 1
Failed Devices : 1
 Spare Devices : 0
     Name : cis191-20:0 (local to host cis191-20)
     UUID : e6f1b3cc:ed166277:fe174054:bd6fb971
    Events : 20
  Number  Major  Minor  RaidDevice State
   0    8    32    0   active sync  /dev/sdc
   2    0    0    2   removed
   1    8    48    -   faulty  /dev/sdd
```

Save your "failed" output with the command:

```
$ sudo mdadm --detail /dev/md0 > /tmp/failed_array.txt
```

Check /var/www. The files should still be there and accessible. RAID really works! Now fix your broken array by:
  - Removing the device you marked as faulty (in my case that was /dev/sdd)
  - Re-adding the device back into the array

Once you re-add the device the array will start rebuilding. This will take some time. During the rebuilding time the output of mdadm should look like this:

```
$ sudo mdadm --detail /dev/md0
/dev/md0:
    Version : 1.2
 Creation Time : Thu Oct 6 12:38:42 2016
  Raid Level : raid1
  Array Size : 20955136 (19.98 GiB 21.46 GB)
 Used Dev Size : 20955136 (19.98 GiB 21.46 GB)
 Raid Devices : 2
 Total Devices : 2
  Persistence : Superblock is persistent
  Update Time : Thu Oct 6 12:47:00 2016
     State : active, degraded, recovering
Active Devices : 1
Working Devices : 2
Failed Devices : 0
 Spare Devices : 1
Rebuild Status : 81% complete
     Name : cis191-20:0 (local to host cis191-20)
     UUID : e6f1b3cc:ed166277:fe174054:bd6fb971
    Events : 36
  Number  Major  Minor  RaidDevice State
   0    8    32    0   active sync  /dev/sdc
   2    8    48    1   spare rebuilding  /dev/sdd
```

Save the output of this command:

```
$ sudo mdadm --detail /dev/md0 > /tmp/rebuilding_array.txt
```

Submit your outputs for the project.

## Turn In 

  - /tmp/failed_array.txt
  - /tmp/rebuilding_array.txt
  - /etc/fstab

Submit your homework on canvas.

## Grading 

  * 5 points for fstab showing that your RAID is mounted.
  * 15 points for your mdadm output.

