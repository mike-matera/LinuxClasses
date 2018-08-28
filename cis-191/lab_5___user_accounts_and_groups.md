In this lab, you will create and manage user accounts, and practice common services required to administer users on a UNIX/Linux computer.These activities include:
  - Creating Groups
  - Creating User Accounts
  - Resetting a forgotten password
  - Modifying a user's uid or gid
  - Locking an account
  - Customizing a login environment
  - Deleting a user Account

Creating GroupsCreate four new groups for your computer with the following GIDs:
  * hobbits 1600
  * elves 1700
  * dwarves 1800
  * wizards 1900

Creating User AccountsCreate five new user accounts using the following names and values:

  * cis191 UID=1201 (with their own private group GID=1201) Full name: CIS191 (Add this account to the wizards group)
  * frodo   UID=1601 GID=1600   Full name: Frodo Baggins
  * gollum   UID=1602 GID=1600   Full name: Smeagol
  * legolas   UID=1701 GID=1700   Full name: Legolas of Mirkwood
  * gimli   UID=1801 GID=1800   Full name: Gimli son of Gloin

The password for all of the above accounts should be Cabri11o.Do not create private groups, but do take in the following considerations:

  - Gollum wants his home directory to be named smeagol
  - All home directories should be created in the /home directory.
  - Legolas wants his shell to be tcsh
  - Gimli doesn't want to have a password. (If you can grant his wish, do so.)

Be sure you can login to each account.
Resetting a forgotten passwordFrodo forgot his password. You must reset it to Baggins and force him to change it the next time he logs in.
Modifying a user's name/identity

  - Change Frodo's GID to be the users group, but make sure he retains his membership in the hobbit group.
  - Legolas wants his username changed to glorfindel
  - Gimli needs his UID changed to 1800. (Make certain that Gimli can still log in and access and create files after this change has been made.

Locking an AccountGlorfindel (a.k.a. Legolas) has been engaged in suspicious activity. You must lock his account.
Customizing a login environment

  - Edit the /etc/issue file so that the first line says "Middle Earth Linux 1.0.".
  - Edit the /etc/motd file to include an announcement that this class is CIS 191B and that all activity on this computer is closely monitored.
  - Gimli is confused by all the messages that come to the screen when he logs on. Configure his account so that no messages are displayed on the screen when he logs in.Hint: search for hush login using Google.

Deleting a user AccountGollum has passed away, and his account must be removed. However, do not remove his home directory until you archive it to the /var/preserve directory and name it gollum.tar.
Turn InRun the program, glab6 until you are satisfied with the results. Save the results by redirecting to a file called lab6 and submit the file on Blackboard.You must fetch the glab6 script from opus like this:
wget http://opus.cis.cabrillo.edu/cis191/glab6
Be sure to make it executable before you run it and it must be run as root.