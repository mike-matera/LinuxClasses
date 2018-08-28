In this lab you will add disks to your VM and begin to manage the storage on them. 

## Introduction 

You have a new disk. Yay! Now your job is to add the storage on the disk to the system. In this lab you will perform the tasks to bring the new storage into the filesystem. Your Cabrillo VMs have extra disks for you to use. 

## Partitioning with Parted 

Check to be sure you have three additional disks on your VM:

```
$ ls -la /dev/sd?
brw-rw---- 1 root disk 8, 0 Sep 12 11:43 /dev/sda
brw-rw---- 1 root disk 8, 16 Sep 12 11:43 /dev/sdb
brw-rw---- 1 root disk 8, 32 Sep 12 11:43 /dev/sdc
brw-rw---- 1 root disk 8, 48 Sep 12 11:43 /dev/sdd
```

You should see device entries for four SCSI disks (sda, sdb, sdc and sdd). If you don't go back and be sure that you've added disks to your VM and reboot. We will use the sdc and sdd next week. This week you will partition /dev/sdb and use it's storage. Start GNU parted to partition the disk. Be careful with parted, mistakes made here may destroy your VM.

```
$ sudo parted /dev/sdb
GNU Parted 3.2
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)                         
```

Use the parted commands you learned in class to perform the following tasks:

  - Initialize the disk with a GUI Partition Table (GPT).
  - Create a new partition that is roughly 10G in size with the name "Homes"
  - Create a new partition that is roughly 10G in size with the name "Webserver"

### Verify Your Work 

Verify that you have created the partitions correctly using the print free command in parted. 

```
(parted) print free
```

Answer the following questions:

  - What are the exact sizes of the partitions you created?
  - Is there free space at the end of the disk? How much?

## Creating Filesystems 

Verify that you have completed the previous step. If you have successfully partitioned /dev/sdb you should now see partition numbers when you use ls in the /dev directory:

```
$ ls -la /dev/sdb?
brw-rw---- 1 root disk 8, 17 Sep 29 10:34 /dev/sdb1
brw-rw---- 1 root disk 8, 18 Sep 29 11:08 /dev/sdb2
```

The partitions are freshly created and contain no filesystem. The next task is to format the filesystems. Create filesystems on /dev/sdb1 and /dev/sdb2 with the following specifications:

  * /dev/sdb1
    * ext4 filesystem
    * Volume label: home
  * /dev/sdb2
    * btrfs filesystem
    * Volume label: web

Verify that you have correctly formatted your two partitions using the commands:

```
dumpe2fs
```

This command shows you ext2/3/4 parameters.

```
btrfs filesystem show
```

This command shows you btrfs parameters.

The commands will also show you the UUID of each filesystem. Make a note of the UUIDs, you will need them in a subsequent step.

## Mount Your New Filesystems 

Your filesystems are ready for use. Each of them belongs in a specific place. The "homes" filesystem will be used for home directories and the "web" partition will be used to store webpages that are served by your VM. Home directories are (of course) stored in /home and the webserver data is stored in /var/www. There is already information in both places. You must copy the existing information into its new volume. That is a multi-step procedure. Here's a walk through for how to do this for /var/www. Before you begin be sure to install the webserver:

```
$ sudo apt-get install apache2
```

### Step 1: Mount The New Volume in a Temporary Directory 

Mount /dev/sdb2 onto /mnt. This is only temporary.

```
$ sudo mount /dev/sdb2 /mnt
```

Verify that the mount succeeded using the mount command with no arguments. If you don't see your /dev/sdb2 partition mounted check your work.

### Step 2: Copy the contents of /var/www into the new partition. 

This command copies the existing contents of /var/www into your new filesystem (which should be mounted on /mnt).

```
$ sudo cp -Rp /var/www/* /mnt
```

Be sure to use the "p" option. That preserves file ownership and permissions. Things will go wrong without it. Use ls to verify that the data is copied.

### Step 3: Unmount and Move the Filesystem 

The new filesystem is ready to be put into service. Unmount it from its temporary mount:

```
$ sudo umount /mnt
```

Now re-mount it in the place where it belongs:

```
$ sudo mount /dev/sdb2 /var/www
```

Verify that your device is properly mounted using the df command.

### Step 4: Repeat! 

Repeat this procedure for /dev/sdb1 and /home. Be careful! Breaking your home directory can cause problems logging in to your machine so be sure that you fully verify each step before you move on.

## Make your Changes Permanent 

The mounts you made in the previous parts are only temporary because they're only held in memory. If you want the mounts to persist after a reboot you must add the mount points to the /etc/fstab file. That file tells Linux what to mount on startup. Here's the default /etc/fstab on your VM:

```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>  <type> <options>    <dump> <pass>
/dev/mapper/ubuntu--vg-root /        ext4  errors=remount-ro 0    1
# /boot was on /dev/sda1 during installation
UUID=dcedaaf6-80c8-4ef2-96c5-13aed6a93aa6 /boot      ext2  defaults    0    2
/dev/mapper/ubuntu--vg-swap_1 none      swap  sw       0    0
/dev/fd0    /media/floppy0 auto  rw,user,noauto,exec,utf8 0    0
```

Add lines like the following to the file:

```
UUID=<put-the-UUID-here> /home    ext4  defaults    0    3
UUID=<put-the-UUID-here> /var/www   btrfs  defaults    0    3
```

Be absolutely sure that you entered the UUIDs that match the ones in the filesystems you created. If you did this step correctly you should be able to verify that fstab is correct by unmounting and remounting the filesystems:

```
$ sudo umount /home
$ sudo umount /var/www
$ sudo mount /home
$ sudo mount /var/www
```

If you get "device is busy" errors when you umount be sure that your working directory is not in one of the mounts. Notice you can now mount /home and /var/www without passing in a device parameter. That's because the device is listed in /etc/fstab! When you are convinced that your mounts are working reboot the system:

```
$ sudo reboot
```

## Turn In 

When your machine is rebooted and working run the following commands:

```
$ mount > /tmp/mounts.txt
$ sudo parted /dev/sdb print free > /tmp/parted-free.txt
```

Turn in the files:
  - /tmp/mounts.txt
  - /tmp/parted-free.txt
  - /etc/fstab
  - Answers to the questions

Submit your homework on Canvas.

## Grading 

  * 15 points for correct files
  * 5 points for your answers

