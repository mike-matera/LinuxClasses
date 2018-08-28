
## Introduction 

The purpose of this lab is to establish a connection to your router VM and to take control of it as the root user.

## Using CIS Computers 

The computers in the STEM center and in classrooms 828 and 829 are different from the other computers on campus. They are meant for students in CIS classes. Your regular username and password will not work. Use the following credentials on CIS computers:
Username:StudentPassword:Cabri11o(a.k.a. funny Cabrillo)
It is not necessary that you complete the classwork on CIS computers.

## VLab Access 

Cabrillo's VMware lab is accessed via the web using the URL below:
 [https://vcenter.cis.cabrillo.edu:9443/vsphere-client/](https://vcenter.cis.cabrillo.edu:9443/vsphere-client/)

Your username and password are derived from your name and your student ID. Here's how to figure them out:
  * lll = first 3 letters of your last name (lowercase)
  * fff = first 3 letters of your first name (lowercase)
  * Ff = first 2 letters of your first name (first letter uppercase)
  * Ll = first 2 letters of your last name (first letter uppercase)
  * nnnn = last 4 digits of your student ID
  * ccc = The course number (with no letters)

Your username is:
  * cislab\lllfffccc

Your password is:

  * FfLlnnnn

Here's an example:

```
     Michael Matera (student ID 12345678)
 Class: CIS-192
Username: cislab\matmic192
Password: MiMa5678
```

Once you have logged in, rename one of the "studentX" folders and give it your name. Please use your full name. There are four servers in the folder. They are networked together according to the following diagram:

### CIS 192 - Network Drawing 

CIS 192 - Network Drawing
The username and password on your VMs are:
Username: studentPassword: Cabri11o

## Procedure 

First you will update two files (you will need to use sudo to become root):
 /etc/issue /etc/issue.net
These files contain the banner that is issued when you login to your VM. You must personalize it with your name and any other text you like. Please note I will see it!! Once you've completed that you must take a screenshot as proof that you've passed this milestone.

## Submit 

  - A screenshot of your updated /etc/issue or /etc/issue.net

