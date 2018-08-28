
One of your Linux boxes has died! Luckily you are a brilliant administrator and have a timely backup. For the final you will restore your machine to proper working order. 
Setup:
Make space byDELETING ALL OF YOUR OLD VMs!Just like the midterm, I have created a custom template VM. You must clone it before you begin.

Step 1: Reinstall Ubuntu Linux

Your backup doesnât include any system directories, only userâs data. Therefore you will have to reinstall Ubuntu and the software packages that your users need. The CIS 191 Midterm VM template is already configured with disks. Once you have a cloned copy you can simply start it to initiate the Ubuntu installer. There are some very important things you should know before you start installing:

  - 
You will need to create a user âgandalfâ for this final. It will be easiest to do that upon installation
  - 
There are two hard disks. Install Linux on the 6G drive. DO NOT touch the second one, it contains the backup that you will use in a future step
  - 
You MUST setup LVM in the installation stage. 
  - 
You will need to create a 1 GB snapshot volume in a future step. You can make this much easier on yourself if you reserve space for it during the installation. Otherwise you will need to do step 1 of the midterm over again!

Once you have Ubuntu installed reboot and begin the rebuild process.

Step 2: Create a Snapshot Volume

In order to make backups easier you use snapshots. After installation you should have one volume group with two logical volumes in it (your root filesystem and a swap partition). There should also be remaining space. In the remaining space create a snapshot volume. Do not mount it. 

Step 3: Create Users and Groups

Do the following to restore your users and groups:

  - 
Create the âwizardsâ group with GID 3000 
  - 
Create the âhobbitsâ group with GID 4000 
  - 
Add the user âpippinâ with UID 4001 
  - 
Add pippin into the hobbits group
  - 
Add gandalf to the wizards group

Step 4: Reinstall non-Default Applications

In order to do their work your users must have the following packages installed:

  - 
dump
  - 
emacs
  - 
logwatch

Use the apt-get utility to install those programs. 

Step 5: Restore the Usersâ Data

The device /dev/sdb contains a backup of the /home directory that was made using the following command:

dump -0f /dev/sdb /home 

You must use the restore command to return the contents of /home/gandalf and /home/pippin to your rebuilt disk. Each home directory contained files beyond the default set. To see what files exist in the restore volume you can use this command:

 restore -tf /dev/sdb 

Be sure that the home directories match the backup once they are restored. You will not receive credit if the files are misplaced.

Step 6: Setup a Custom CRON Job

Your office systems send a ping every hour to a central host so that the central host knows they are alive and the network is working. You have implemented this on every host using CRON. In this part you must restore that functionality to your rebuilt system. The command that does the ping is:

 ping -q -c 1 opus > /dev/null
Setup a cron job to run this command every hour.You must use global system configuration to implement this ping. DO NOT use a userâs personal crontab to do this.

Turn In:

Do the following steps to save your work (as root):

 cd /tmp
 mkdir final
 cd final
 tar -cvf cron.tar /etc/cron*
 (pvdisplay; vgdisplay; lvdisplay) > lvm.out
 tar -cvf home.tar /home
 tar -cvf users.tar /etc/passwd /etc/group 
 dpkg -l > package.list
 cd ../
 tar -zcvf final.tar.gz final/
 scp final.tar.gz <your-name-here>@opus.cis.cabrillo.edu:

Grading:

  * 
Step 1: 25 points

  * 
25 points for installing Ubuntu with LVM

  * 
Step 2: 15 points

  * 
10 points for having a snapshot volume
  * 
5 points for wasting no space in your volume group

  * 
Step 3: 20 points

  * 
5 points for each correct group (10 possible)
  * 
5 points for each correct user (10 possible)

  * 
Step 4: 15 points

  * 
5 points for each installed package (15 possible)

  * 
Step 5: 20 points

  * 
10 points for restoring /home/gandalf
  * 
10 points for restoring /home/pippin

  * 
Step 6: 15 points

  * 
15 points for a configuration that is correct

Total: 110 points
