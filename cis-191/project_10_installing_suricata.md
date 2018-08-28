In this project you'll install the Suricata Network Intrusion Detection System (NIDS) three different ways.

## Introduction 

Installing and removing software on Linux has become easier with the advent of package management. In this lab you'll use the apt package manager to install Suricata from the default repositories. Then you'll install a newer version using a PPA. Finally you'll compile the most recent copy for your self. This is how we used to do it1

## Install Suricata 

Before you install Suricata use apt-cache to determine what packages are needed by Suricata. Store the list in a file called dependencies.txt and submit them with the project. Now installin Suricata the easy way:

```
$ sudo apt-get install suricata
```

Check to see if Suricata is running:

```
$ service suricata status
```

Is the suricata program running? If not, why not?

## Get the Leading Edge Suricata 

Let's replace the stable suricata with a more leading-edge version. To do that you should first remove the copy you just installed:

```
$ sudo apt-get remove suricata
$ sudo apt-get autoremove
```

Then follow the instructions here to add the PPA:
 [https://redmine.openinfosecfoundation.org/projects/suricata/wiki/Ubuntu_Installation_-_Personal_Package_Archives_%28PPA%29#Beta-releases](https://redmine.openinfosecfoundation.org/projects/suricata/wiki/Ubuntu_Installation___Personal_Package_Archives_(PPA)#Beta_releases)
What version of suricata did you install?

## Compile Your Own Copy 

For extra credit you should install Suricata the hard way. Start by using the git program to check out the latest source files:

```
$ cd ~
$ git clone https://github.com/inliniac/suricata.git
```

That will create a ~/suricata directory. Before you can suricata you should install some packages that contain essential elements of Ubunut's C/C++ compiler and others:

```
$ sudo apt-get install pkg-config autoconfautogenbuild-essential
```

There are more packages needed to build suricata (you will need to figure out which ones). But with the above installed you can get started. Here's the build procedure:
```

$ cd ~/suricata
$
git checkoutsuricata-3.1.3
$ ./autogen.sh
$ ./configure
$ ./make
```

Configure will fail telling you that things aren't found. Use apt-cache to search for a development package that provides what's missing and try again. You will need to rerun configure many times, each time installing a new "dev" dependency. Stick with it. If you get through the make step successfully you will have a built copy of suricata in your home directory. You can check the executable with the command:

```
$ ldd ~/suricata/src/.libs/suricata
linux-vdso.so.1 => (0x00007ffe53855000)
libhtp-0.5.23.so.1 => not found
libmagic.so.1 => /usr/lib/x86_64-linux-gnu/libmagic.so.1 (0x00007f66e8b36000)
libcap-ng.so.0 => /usr/lib/x86_64-linux-gnu/libcap-ng.so.0 (0x00007f66e8930000)
libpcap.so.0.8 => /usr/lib/x86_64-linux-gnu/libpcap.so.0.8 (0x00007f66e86ee000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f66e84d1000)
libyaml-0.so.2 => /usr/lib/x86_64-linux-gnu/libyaml-0.so.2 (0x00007f66e82b1000)
libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f66e8041000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f66e7c78000)
libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007f66e7a5d000)
/lib64/ld-linux-x86-64.so.2 (0x000055e2c50d3000)
```

Submit the executable with the assignment.

## Turn In 

  - dependencies.txt from the first part
  - Answers to the part 1 and part 2 questions.
  - /var/log/apt/history.log from your VM
  - ~/suricata/src/.libs/suricata

Submit your homework on canvas.

## Grading 

  * 10 points for parts 1 and 2
  * 10 points for part 3

