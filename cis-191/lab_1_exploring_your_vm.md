IntroductionThe purpose of this lab is to establish a connection to your VM and to take control of it as the root user.
Using CIS ComputersThe computers in the STEM center and in classrooms 828 and 829 are different from the other computers on campus. They are meant for students in CIS classes. Your regular username and password will not work. Use the following credentials on CIS computers:
Username:StudentPassword:Cabri11o(a.k.a. funny Cabrillo)
It is not necessary that you complete the classwork on CIS computers.
Using VCenter and VMWareMost students will find it easiest to use the central servers to complete their lab assignments. Though you can do the assignments on your own computers you are responsible for setting up the proper VMs. In most cases that will be more trouble than it is worth.Access to VMWare happens via the web. You access the lab via this web page:
 [https://vcenter.cis.cabrillo.edu:9443](https://vcenter.cis.cabrillo.edu:9443/)
Username and passwordIs an encoded version of your name where:

  * lll = first 3 letters of your last name (lowercase)
  * fff = first 3 letters of your first name (lowercase)
  * Ff = first 2 letters of your first name (first letter uppercase)
  * Ll = first 2 letters of your last name (first letter uppercase)
  * nnnn = last 4 digits of your student ID

username: cislab\lllfff191
password: FfLlnnnn

Example:
Michael Matera (student ID 1234-567890)
username: cislab\matmic191
password: MiMa7890
Your VMsIn class you will be assigned a VM to use. The VM will be created for you by me. When you are assigned your VM rename it to yourFULL NAMEif that has not been done already. Do not access other student's VMs. All activity on VMware is logged!Your VMs are new and require initialization.The username on your VM is:
Username: studentPassword: Cabri11o
ProcedureFirst you will update two files (you will need to use sudo to become root):
/etc/issue/etc/issue.net
These files contain the banner that is issued when you login to your VM. You must personalize it with your name and any other text you like. Please note I will see it!! Once you've completed that you must:
1. List all of the devices on the PCI bus.
To Turn In
  - A screenshot of your updated /etc/issue or /etc/issue.net
  - A screenshot of your PCI devices.

Grading
  * 5 points for turning it in on time
  * 15 points for your screenshots.

