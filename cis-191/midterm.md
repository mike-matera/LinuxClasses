DOH! You just got finished setting up a new install of Ubuntu Server when you remember that you skipped a step. Your boss asked you to create a 1GB snapshot volume to help with backups. You forgot to reserve space during the installation process and now it's too late. Or is it? 
SetupThere is a new VM template in the templates folder called "cis191midterm" Start by cloning that template to your folder. Be aware that space may be an issue so if you may want to clean up any VMs you have lying around. The midterm VM will boot onto the installer CD by default. You can get to the hard disk by either pressing escape during the BIOS menu or by telling the installer CD to boot from the first hard disk. (you'll have to select your language first)
Step 1 (30 points)The new VM had LVM setup during install so you won't need to start from scratch. The general procedure for what you must do is:
  - Boot into rescue mode and DO NOT mount any of the disks. Rescue mode is required because though you can enlarge an ext4 filesystem while mounted you cannot shrink one.
  - Using resize2fs shrink the ext4 root filesystem (it should be about 6G)
  - Using lvreduce shrink the logical volume. IMPORTANT: Normally the lvreduce command can automatically resize the filesystem. In rescue mode it can't. Therefore you will have to be absolutely sure that you do not shrink the logical volume to below the size of the filesystem. Doing this will result in permanent damage and you will have to re-clone your VM. Hint: resize2fs without arguments will grow a filesystem to the size of its volume.
  - Reboot
  - Create a snapshot logical volume with the extents you now have free (use the man page or google for how to do this). The free extents should total about 1G.
  - Mount your snapshot volume on /mnt

NOTE:Tab completion is broken for lvm. Don't rely on it to find entries in /dev
Notice what's in your snapshot volume? Run the following command to save the state of your progress:
# (df; pvdisplay; vgdisplay; lvdisplay) > /tmp/midterm_step1.log
Step 2 (20 points)Now that you have the snapshot sorted out you have one more task to do. Your company is making cutting edge software and you need a newer version of the Make utility to get the job done. In your home directory(/home/test)you will notice that a tar file with the latest make has been downloaded. It's your job to compile the source (using the procedure we covered in class) and create a DEB package.
Turn InYou must submit your files using SCP. Here's an example of how to do it properly:
mkdir /tmp/midtermcp /tmp/midterm_step1.log /tmp/midtermcp <path_to_DEB_package_goes_here> /tmp/midtermcd /tmp/midtermtar -czvf midterm.tar.gz *scp midterm.tar.gz <your_name_here>@opus:
Be sure to finish the written part of the exam on Blackboard.