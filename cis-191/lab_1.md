The purpose of this lab is to install Red Hat 9.0 onto the Virtual Machine that you made in Lab 0 and compare it to UNIX installations of Solaris, SCO, and BSD.

In doing this lab, you will be demonstrating your ability to perform an interactive, default install of a Linux Operating System from a CD-ROM image. By a default install is meant that you will choose the default values for all the settings you will specify during the installation with the following exceptions:

  - You must manually specify the hostname to be shrike.localdomain.

  - Include SSH as an accepted protocol for the medium firewall setting.

  - Select Pacific Standard Time for the timezone setting.

  - Specify funny Cabrillo as the root password.

  - Choose Minimal for Package Selection

  - Select No for creating an Emergency boot floppy.

You will also demonstrate logging in to your new installation as superuser and running system commands to gather information about your installation.

### Procedure 

### Part I 

Installing Red Hat 9

  - Log in to cislab.cis.cabrillo.edu using your rdp client.

  - Create a new virtual machine and select the "Console" tab.

You should see the BIOS come up and the virtual machine should fail to boot trying to find a DHCP server on the network.

  - NOTE: In VMware select "Linux 2.4.x" as the operating system type of your new VM

  - Click the "CDROM" tool in the toolbar and use the submenus to connect the cdrom to the redhat9-shrike.iso image, which is on the disk4-2 under the ISO folder.

  - Now click in the console window and type <ctrl><alt>-Ins to reboot the virtual machine.

  - This time, the virtual machine should present a boot screen for Red Hat Linux.  Press the Enter key at the boot prompt.

  - When asked whether to test the installation media, use the TAB key to Skip the test.

  - Proceed through the installation using the default values except where noted
above.  The action of the mouse will be very slow and lazy like.  It is best to leave the mouse pointer in the lower right corner where the Next button is, and use the tab and arrow keys to navigate the screen.

  - If you properly choose the minimal installation during package selection, you will not be asked to insert additional media.  If you are asked, it means
you did not select a minimal install.  You will have to start the installation over.

  - When the installation is complete, it will reboot your system.  You should verify that the reboot disconnects the CDROM so that you don't come back up to the install screen.

  - When you do reboot the virtual machine, you should come up to a screen that
looks like the image below.  It will pause here for 10 seconds before autobooting.



![image](http:::www.cabrillo.edu:~jgriffin:cis191:files:resources:bootscreen.jpg)



  - As the system boots up, see if you can note a failure in one of the services
as they come up.

Due to the outdated RedHat 9 operating system with respect to the ESXi hypervisor, the NIC appears to the OS as not having a link connection.

  - When the system presents you with the redhat9 login prompt, log in as root.

  -  Change directory to the /sbin directory and issue the following two command:

mv ethtool Ethtool

service network restart

The network service will now come up properly.

### Part II 

Familiarizing yourself with your newly installed system

  - Change back to root's home directory and issue the ifconfig command.

Run the command again but redirect the output to the file, lab1.

  - Your task now is to visit each of the toplevel directories and run the following two commands:

find . | wc -l

du -sh .

collecting the results into the lab1 file, e.g.

(cd /bin; pwd; find . | wc -l ; du -sh .) >> lab1

  - Finally, issue the following three commands sending their output to the lab1 file.

fdisk -l >> lab1

mount >> lab1

df -h >> lab1

  - Complete the diagram on the hand out you received in class for this Red Hat9 Installation.

  - You are now ready to turn in your work and shutdown your virtual machine.
See the turn-in instructions below before you shut donw the virtual machine.

### Part III 

Comparing Linux to UNIX

  - Bring up each of the three UNIX VMs in the VM folder and compare the toplevel directory of each one to the Red Hat Linux toplevel directory.  What directories do they have in common?  What directories are unique to UNIX only?  Enter this information on the handout that I handed out in class.

  - When shutting down the UNIX VMs, you must power them off; they do not automatically power off as Linus systems do.

### Turn in 

Make sure you are in root's home directory (/root) where the following two files should be: anaconda-ks.cfg, and lab1.  Append the anaconda-ks.cfg file to the lab1 file and secure copy the lab1 file to you home directory on opus:

cat anaconda-ks.cfg >> lab1; scp lab1 logname@opus.cis.cabrillo.edu:

Do not forget the trailing colon (:) character!!

Can you think of a way to make sure this submission was successful?

### Grading Rubric 

Your lab will be graded for the following components:

1 point -

for the size and number of files in each toplevel directory (17 pts. total)

5 points -

for the anaconda-ks.cfg file with the correct specifications outlined above

3 points -

for the ifconfig data showing your network connection.

