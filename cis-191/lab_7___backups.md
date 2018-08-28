
=== The purpose of this lab is to
implement a backup schedule on your new cis191B VMs that will allow you to recover from a
hard disk disaster. ===

Part OneCreating an archve device
You will create a second disk for your virtual machine which you will use as an
archive device.  You will make partitions on the disk that will correspond to different sized
tapes onto which you will do the backups.

  - Log in to CISVLAB and right click your cis191B VM choosing the Edit Settings menu option.

  - In the Properties dialog, select Add... and then Hard Disk in the center column.
Click Next

  - Select Create a new virtual Disk and click Next

  - Specify a disk size of 5 GB and choose Thin Provisioning and leave the default location.

  - Under Advanced Option accept the default SCSI (0:1) disk, but choose a Mode of
Independent and Persistent.

  - Click Finish and OK

You now have a second storage device that will be recognized by your OS as /dev/sdb.
Part Two
You will now boot up your VM and run the fdisk -l command
to view your new device.
fdisk -l /dev/sdb

Since archives can be made on raw devices or as files on a filesystem, you will be creating three primary partitions as raw tape devices, and a fourth partition that will be formatted with an ext2 filesystem.

  - make sdb1 ~300MB

  - make sdb2 ~700MB

  - make sdb3 ~2GB

  - make sdb4 ~2GB

Part Three: DUMPIt's time to make your level 0 backup of each of your filesystems.

Back up the root filesystem using the dump -0 command with the output going to /dev/sdb4:
dump -0uf /dev/sdb4 /dev/sda1

Do an incremental backup of the root filesystem using the dump -3 command with the output going to /dev/sdb2:
dump -3uf /dev/sdb2 /dev/sda1
Do an incremental backup of the root filesystem using the dump command with the output going to a file backup-mm-dd-yy.dmp:
 dump -5uf incremental-mm-dd-yy.dmp /dev/sda1

Back up the contents of the /etc directory using the tar command with the output going to a file called etc-mm-dd-yy.tar:
 tar -cvf etc-mm-dd-yy.tar /etc

Back up the contents of the /boot directory using the cpio command with the output going to a file called boot-mm-dd-yy.cpio:
find /boot | cpio -vocB > boot-mm-dd-yy.cpio

Turn In Now verify that each backup was successful by collecting the first 20 lines of the listing of each archive.  Label these listings and put them in one file called lab7.

Copy the lab7file to your account on
opus.cabrillo.edu using the following command: 

Hint: To get the first 20 lines of a command's output and append it to the file lab8:
<command> | head -n 20 >> lab7
Please triple check that your file is correct, your grade is riding on it!!! Turn in the assignment like this:
scp lab8 <you_user_name>@opus.cis.cabrillo.edu:lab8

Grading
You will receive 5 points for each correct listing for a total of 25 points.

