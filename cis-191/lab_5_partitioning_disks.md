Before you can do this lab you will need to add disks to your VM. Instructions on how to do that can be found on the page[Adding Disks to your VM](adding_disks_to_your_vm).In this lab you will follow the instructions on the page[Disk Partitioning](disk_partitioning).

  - Install the btrfs-tools package. You'll need it to be able to format a btrfs filesystem:

  
```
 
sudo apt-get install btrfs-tools
```

  - Create a GPT disk label on /dev/sdb

  - Make five 2GB partitions on /dev/sdb with types

  - ext2
  - ext3
  - ext4
  - btrfs
  - msdos

  - Create a MSDOS disk label on /dev/sdc

  - Make five 2GB partitions on /dev/sdc with any type

  - ext2
  - ext3
  - ext4
  - btrfs
  - linux-swap

  - Format and mountall of the partitions you created (except for the swap partitions)

When you have done the above answer the following questions:
  - What are the partition numbers that were created on both disks?
  - Why are they different?

Also, print the partition tables into text files and submit them with your lab:

```
$ sudo parted /dev/sdb print free > partitions-sdb.txt
$ sudo parted /dev/sdc print free > partitions-sdc.txt
$ mount > mounts.txt

```

