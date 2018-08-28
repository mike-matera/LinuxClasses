The process of installing and removing software is one of the greatest strengths of Linux. The majority of software you will need as an administrator is able to be installed from a trusted repositoryusing a package manager. This process automates most of what can be challenging about installing and removing software and makes removal of troublesome software complete and easy.

### Commands 

  * dpkg
  * apt-get

### Configuration 

  * /etc/apt/sources.list
  * /etc/apt/trusted.gpg

## Introduction 

Installing and removing software on Linux means making yourself familiar with the package manager on your system. There are several package managers on Linux and people argue over which one is the best. They all have strengths and weaknesses. Your choice of Linux distribution comes with the distribution's preferred package manager. This lecture will focus on Debian's package manager called apt, but the concepts transfer directly to other distributions. Below is a table of Linux distributions and their package managers:

^ Package Manager ^ Distributions ^
| Advanced Package Manager (apt) | Debian and derivatives including Ubuntu, Raspbian (Raspberry Pi), Knoppix, Linux Mint, Security Onion and Kali Linux | 
| Redhat Package Manager (rpm) | Fedora, RedHat Enterprise Linux and CentOS, SuSE Linux | 
| Itsy Package Manager (ipkg) | OpenWRT (some versions), Openmoko, webOS, QNAP Linux NAS appliances | 
| Open Package Manager (opkg) | OpenWRT (some versions), Open Embedded, Yocto, Angstrom | 
| Package Manager (pacman) | Arch Linux | 

The list isn't complete and all package managers aren't as full-featured as apt is. What's common to them all are some basic operations:

  - Adding and removing package repositories
  - Installing and removing packages
  - Keeping packages updated
  - Finding what packages are installed
  - Resolving package dependencies
  - Finding installed files
  - Fixing broken packages

## Understanding Dependencies 

When a program in one package uses libraries or commands from another it's said to dependon the other package. For example, in order to use Firefox you must have a graphical desktop (Firefox doesn't have a command line interface). Therefore the Firefox package depends on Xwindows and you cannot install Firefox if Xwindows is not installed. Also, you cannot remove Xwindows without first removing Firefox. The package manager understands these dependencies and does two important things for you: It automatically installs dependencies when you ask for a package and it prevents you from breaking a package by uninstalling something it depends on.
To see a package's dependencies do this:

```
apt-cache depends <package>
```

For example:

```
$ apt-cache depends firefox
firefox
 Depends: lsb-release
 Depends: libasound2
 Depends: libatk1.0-0
 Depends: libc6
 Depends: libcairo2
 Depends: libdbus-1-3
 Depends: libdbus-glib-1-2
 Depends: libfontconfig1
 Depends: libfreetype6
 Depends: libgcc1
 Depends: libgdk-pixbuf2.0-0
 Depends: libglib2.0-0
 Depends: libgtk2.0-0
 Depends: libpango-1.0-0
 Depends: libpangocairo-1.0-0
 Depends: libstartup-notification0
 Depends: libstdc++6
 Depends: libx11-6
 Depends: libxcomposite1
 Depends: libxdamage1
 Depends: libxext6
 Depends: libxfixes3
 Depends: libxrender1
 Depends: libxt6
 Suggests: ttf-lyx
 Recommends: xul-ext-ubufox
 Recommends: libcanberra0
 Recommends: libdbusmenu-glib4
 Recommends: libdbusmenu-gtk4
 Replaces: <kubuntu-firefox-installer>
 Replaces: <kubuntu-firefox-installer:i386>
 Conflicts: firefox:i386
```

You can also see what packages list a package as a dependency (a reverse-dependency):

```
apt-cache rdepends <package>
```

Packages may also depend on a particular versionof another package. The software on Linux is always changing and being updated. Most of the time new versions of software are backwards compatible with older versions. When a version is not backwards compatible that creates a problem for packages that depend on it: Use the old version or use the new version. When this happens Ubuntu usually tries to have both versions installed. When that's impossible it's possible that your packages can get stuck. You can't install something without first removing something else.

## Managing Repositories 

A repository is an online source of packages. By default Ubuntu comes installed with repositories from Canonical Ltd (the company behind Ubuntu). The default repositories are ever changing with new packages and updated versions of existing packages. A key role of an administrator is keeping software up to date. The apt-cache command from the previous section (as its name implies) uses the cache stored on your local disk. The cache is built from a list of all packages available online. The following command updates the list of packages available and rebuilds the cache:

```
$ sudo apt-get update
```

The update command only updates the cache, not the packages. I'll talk about updating packages later. It's always safe to run the apt-get update command and it's a good thing to do before you install any packages or use the apt-cache command. Running apt-get update shows you what repositories you have enabled. Those repositories are listed in your /etc/apt/sources.list file. Here's the default file on Ubuntu 14.04:

```
# deb cdrom:[Ubuntu 14.04.1 LTS _Trusty Tahr_ - Release amd64 (20140722.2)]/ trusty main restricted
# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://us.archive.ubuntu.com/ubuntu/ trusty main restricted
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty main restricted
## Major bug fix updates produced after the final release of the
## distribution.
deb http://us.archive.ubuntu.com/ubuntu/ trusty-updates main restricted
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty-updates main restricted
## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://us.archive.ubuntu.com/ubuntu/ trusty universe
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty universe
deb http://us.archive.ubuntu.com/ubuntu/ trusty-updates universe
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty-updates universe
## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://us.archive.ubuntu.com/ubuntu/ trusty multiverse
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty multiverse
deb http://us.archive.ubuntu.com/ubuntu/ trusty-updates multiverse
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty-updates multiverse
## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://us.archive.ubuntu.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://us.archive.ubuntu.com/ubuntu/ trusty-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu trusty-security main restricted
deb-src http://security.ubuntu.com/ubuntu trusty-security main restricted
deb http://security.ubuntu.com/ubuntu trusty-security universe
deb-src http://security.ubuntu.com/ubuntu trusty-security universe
deb http://security.ubuntu.com/ubuntu trusty-security multiverse
deb-src http://security.ubuntu.com/ubuntu trusty-security multiverse
## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu trusty partner
# deb-src http://archive.canonical.com/ubuntu trusty partner
## This software is not part of Ubuntu, but is offered by third-party
## developers who want to ship their latest software.
deb http://extras.ubuntu.com/ubuntu trusty main
deb-src http://extras.ubuntu.com/ubuntu trusty main
```

Each 'deb' line tells apt where to find binary packages and 'deb-src' lines tells apt where to find the source code for those packages. Most open source software requires people who distribute binary versions of software also make the source code available. This is a key part of what it means to be open source. It's common to have third party repositories installed. Software vendors that support Linux are increasingly realizing that using the package manager is a great way to distribute your software and keep it up to date. Installing Google Chrome on Linux works by installing this file:

```
$ cat /etc/apt/sources.list.d/google-chrome.list
### THIS FILE IS AUTOMATICALLY CONFIGURED ###
# You may comment out this entry, but any other modifications may be lost.
deb http://dl.google.com/linux/chrome/deb/ stable main
```

After adding this deb line the next apt-get update will "see" Google Chrome and related packages as able to be installed. Also, updates will be done automatically with the rest of system.

### Repositories and Trust 

One of the great strengths of the repository system is that it's resistant to the malware problems that make installing software on Windows a nightmare. Software from repositories is signed to protect it from being altered by someone who can intercept your network traffic. Before you can use a repository you must import its public key into your list of trusted keys. Be careful when you do this, once you do you will automatically trust all software from the repository. To see what repositories you trust do this:

```
$ sudo apt-key list
/etc/apt/trusted.gpg
--------------------
pub  1024D/437D05B5 2004-09-12
uid         Ubuntu Archive Automatic Signing Key <ftpmaster@ubuntu.com>
sub  2048g/79164387 2004-09-12
pub  1024D/FBB75451 2004-12-30
uid         Ubuntu CD Image Automatic Signing Key <cdimage@ubuntu.com>
pub  4096R/C0B21F32 2012-05-11
uid         Ubuntu Archive Automatic Signing Key (2012) <ftpmaster@ubuntu.com>
pub  4096R/EFE21092 2012-05-11
uid         Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>
pub  1024D/3E5C1192 2010-09-20
uid         Ubuntu Extras Archive Automatic Signing Key <ftpmaster@ubuntu.com>
pub  1024D/7FAC5991 2007-03-08
uid         Google, Inc. Linux Package Signing Key <linux-packages-keymaster@google.com>
sub  2048g/C07CB649 2007-03-08
pub  1024D/B01FA116 2009-12-24
uid         ROS Builder <rosbuild@ros.org>
sub  2048g/8F3611A0 2009-12-24
/etc/apt/trusted.gpg.d/x2go-stable.gpg
--------------------------------------
pub  1024R/0A53F9FD 2011-04-18
uid         Launchpad PPA for x2go
```

The output is generated from my laptop. You can see that I have some third-party keys present. See the manual page for apt-key to see how to add and remove keys.

## Installing and Removing Software 

Installing and removing software is easy! To install a package simply run:

```
apt-get install <package-name>
```

Knowing what package you want is a bit more tricky. When you execute a command that doesn't exist but could if you installed a package Ubuntu very helpfully tells you what package you might need:

```
$ ntop
The program 'ntop' is currently not installed. You can install it by typing:
sudo apt-get install ntop
```

This is one of the things I love most about Ubuntu. This is not just a nicety for n00bs it's something that this seasoned admin finds useful all the time. Sometimes, however, the help isn't enough to locate the package you need. When that happens you can search the apt cache for a package that matches your input:

```
$ apt-cache search ntop
diveintopython - free Python book for experienced programmers
diveintopython-zh - free Python book for experienced programmers (zh translation)
darkstat - network traffic analyzer
diveintopython3 - Book for learning Python 3
libndpi-dev - extensible deep packet inspection library - development files
libndpi1 - extensible deep packet inspection library - shared library
libnet-dns-fingerprint-perl - library to determine DNS server vendor, product and version
ntop - display network usage in web browser
ntop-data - display network usage in a web browser (data files)
ntop-dbg - display network usage in web browser (debug symbols)
plasma-active-dev - development files for Plasma Active
radeontop - Utility to show Radeon GPU utilization
sntop - A curses-based utility that polls hosts to determine connectivity
```

Notice that related packages are also returned. These packages may have the word "ntop" somewhere in their description. The apt-cache command will also show you detailed information about a package:

```
$ apt-cache show ntop
Package: ntop
Priority: optional
Section: universe/net
Installed-Size: 1575
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Ludovico Cavedon <cavedon@debian.org>
Architecture: amd64
Version: 3:5.0.1+dfsg1-2.2ubuntu1
Depends: libc6 (>= 2.15), libgdbm3 (>= 1.8.3), libgeoip1, libpcap0.8 (>= 0.9.8), libpython2.7 (>= 2.7), librrd4 (>= 1.3.0), zlib1g (>= 1:1.1.4), debconf (>= 0.5) | debconf-2.0, ntop-data (= 3:5.0.1+dfsg1-2.2ubuntu1), python-mako, net-tools, adduser, passwd
Suggests: graphviz, gsfonts
Filename: pool/universe/n/ntop/ntop_5.0.1+dfsg1-2.2ubuntu1_amd64.deb
Size: 589928
MD5sum: 3d237ae1b98a26c51a39f47fe667befa
SHA1: c18f46b78e0ab75719c0781fd0cdb8b0c1ac7d86
SHA256: d1cacabb790f9f8963d4a4839d3a7c392667c402d67950b0d0b534bee2bff86b
Description-en: display network usage in web browser
ntop is a network traffic probe that shows the network usage, similar to what
the popular top Unix command does. ntop is based on libpcap and it has been
written in a portable way in order to virtually run on every Unix platform and
on Win32 as well.
.
ntop users can use a web browser to navigate through ntop (that acts as a
web server) traffic information and get a dump of the network status. In the
latter case, ntop can be seen as a simple RMON-like agent with an embedded web
interface.
The use of:
 * a web interface
 * limited configuration and administration via the web interface
 * reduced CPU and memory usage (they vary according to network size
 and traffic)
make ntop easy to use and suitable for monitoring various kind of networks.
.
This package contains the ntop daemon.
Description-md5: 8d7e6637c6b740b203a76421995fd582
Homepage: http://www.ntop.org/
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Origin: Ubuntu
```

Installing ntop installs it's dependencies automatically (though if it's a lot of packages apt-get will ask you to confirm that you still want to install):

```
$ sudo apt-get install ntop
Reading package lists... Done
Building dependency tree   
Reading state information... Done
The following extra packages will be installed:
 fonts-dejavu libdbi1 librrd4 ntop-data python-mako python-markupsafe
 ttf-dejavu ttf-dejavu-extra
Suggested packages:
 geoip-database-contrib libjs-jquery-ui python-beaker python-mako-doc
The following NEW packages will be installed:
 fonts-dejavu libdbi1 librrd4 ntop ntop-data python-mako python-markupsafe
 ttf-dejavu ttf-dejavu-extra
0 upgraded, 9 newly installed, 0 to remove and 38 not upgraded.
Need to get 1,971 kB of archives.
After this operation, 8,283 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```

So what if we get tired of ntop? We can remove it just as easily as we installed it. There are two ways to remove a package:

```
apt-get remove <package>
apt-get purge <package>
```

The remove command removes all the executables and libraries but leaves any configuration in place. That way if you install it again the program will work just as it did when you removed it (with any of your customizations). The purge command removes the package's configuration. If you reinstall the package will go back to its fresh state. Let's purge ntop:

```
$ sudo apt-get purge ntop
Reading package lists... Done
Building dependency tree   
Reading state information... Done
The following packages were automatically installed and are no longer required:
 libdbi1 librrd4 ntop-data python-mako python-markupsafe
Use 'apt-get autoremove' to remove them.
The following packages will be REMOVED:
 ntop*
0 upgraded, 0 newly installed, 1 to remove and 38 not upgraded.
After this operation, 1,637 kB disk space will be freed.
Do you want to continue? [Y/n]
```

What about the dependencies that were installed? They were not removed automatically. If you want to get rid of them run the command:

```
$ sudo apt-get autoremove
Reading package lists... Done
Building dependency tree   
Reading state information... Done
The following packages will be REMOVED:
 libdbi1 librrd4 ntop-data python-mako python-markupsafe
0 upgraded, 0 newly installed, 5 to remove and 38 not upgraded.
After this operation, 6,463 kB disk space will be freed.
Do you want to continue? [Y/n]
```

The autoremove command removes all packages that were installed to meet dependencies but are no longer necessary. It's a nice way to tidy up after removing something.

### Metapackages 

There is a special kind of package called a metapackage. A metapackage is a package that doesn't install any software but it depends on a bunch of other packages. Metapackages are useful if you want to install a bundle of software related to a particular task or function. If you want to convert your Ubuntu Server VM to an Ubuntu Desktop VM all you have to do is install the ubuntu-desktop metapackage.

```
$ sudo apt-get install ubuntu-desktop
```

This will install the almost 2G of packages needed to convert Ubuntu Server to Ubuntu Desktop. You can convert it back:

```
$ sudo apt-get remove ubuntu-desktop
$ sudo apt-get autoremove

```

## Updating to the Latest Versions 

Desktop Ubuntu uses a graphical updater to help you keep your software up to date. On Ubuntu Server you must do this process by hand. Luckily it's easy. The first step is to make sure you have the latest packages available:

```
$ sudo apt-get update
```

Next you ask apt to upgrade any packages that have newer versions in the repository:

```
$ sudo apt-get upgrade
```

The upgrade command will not install some packages that have had their dependencies change. New kernel versions are usually held back because of the peculiar way that kernels are packaged. To get all packages updated run:

```
$ sudo apt-get dist-upgrade
```

Be careful! The dist-upgrade command is used to upgrade from one version of Ubuntu to another. Running the command when a new version is available will attempt to update all packages and may take a very long time and break your system. If you see that the dist-upgrade process is about to download thousands of packages, stop and think. If you want to get the new kernel and not all of the new Ubuntu you can install the kernel packages individually.

## Pro Tips 

Here are a few commands that every Ubuntu admin should know. To list all packages, installed and available:

```
dpkg -l
```

If you want to filter just on installed packages use grep:

```
$ dpkg -l | grep -e '^ii'
```

Packages come in *.deb files. You can install a package that's not in a repository directly using dpkg:

```
dpkg -i <packagefile.deb>
```

This won't automatically install dependencies and will fail if dependencies are missing. If dpkg fails manually install dependencies using apt-get. To find out what package a file belongs to do this:

```
dpkg -S <path-to-file>
```

For example:

```
$ dpkg -S /bin/cat
coreutils: /bin/cat
```

To see what files a package installed:

```
dpkg-query -L <package-name>
```

For example:

```
$ dpkg-query -L bash
/.
/etc
/etc/bash.bashrc
/etc/skel
/etc/skel/.profile
/etc/skel/.bash_logout
/etc/skel/.bashrc
/bin
/bin/bash
/usr
[...output snipped...]
```

There are many other useful things to know about apt. If you use Ubuntu Desktop I suggest installing the synaptic program which gives you a GUI that you can use to explore and install packages. It's the first thing I install after a fresh install of Ubuntu.
