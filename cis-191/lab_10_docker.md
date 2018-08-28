# Introduction  

In the last lab you'll follow steps to create a Dockerized application in your VM and run a kernel in virtualization. You can use a personal VM at home or the one that I've assigned you. 

# Qemu  

Use the command in cis_191:Using QEMU to launch a kernel. You can run the command on a VM at home or the one that I've assigned you. The easiest kernel to boot is the one you're using. The kernel and initrd that you're using is in:

  /boot/vmlinuz-$(uname -r)
  /boot/initrd.img-$(uname -r)

Supply those to ''qemu-system-x86_64''. Take a screenshot of your emulated kernel. 

# Docker  

Follow the instructions on the page cis_192:Dockerfile and Container Management. Since the steps match the VM configuration for CIS-192 you'll have to watch out for some important changes: 

  - Skip the steps in the "Network Setup" section. You don't need to change any firewall settings or make DNS entries.  

If you've completed the steps properly you should be able to load a web page on your VM's IP address. If you're using a school assigned VM you will have to use IPv6 or be on campus to see the web page. Take a screenshot of the page.

# Turn In  

  * A screenshot of your emulated kernel 
  * A screenshot of the Hello World webpage that the app shows you. 

# Grading  

  * 10 points for each screenshot 

