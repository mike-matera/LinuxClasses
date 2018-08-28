The purpose of this lab is to explore the various ways to monitor the processes on a UNIX system. It includes knowing what utilities are available and how to use them.

Setup:You will perform this lab on each of the following systems:
  * Ubuntu Server
  * Ubuntu Desktop
  * Android

Part 1: Get a baseline for each UNIX systemBoot each system fresh and login the normal way (i.e. Use X-Windows on Ubuntu Desktop, adb on Android). On each of the systems determine (and record) the following:
  - The number of processes running (ps -e | wc -l)

  - Android note: The wc command doesn't exist (copy the output of ps to somewhere else and count lines)
  - More Android: ps -e doesn't work. Just run ps

  - The amount of memory used and free
  - The amount of swap space used and free
  - The top 10 processes by memory size(ps -e -o pid,s,uid,sz,args | sort -nk 4 | tail -10)

  - Android note: Sort and tail are also missing. Again, you will have to copy and paste

Turn InPlease combine your answers into a single document and post that document on blackboard.
ScoringYou will receive 5 points for correct answers on each system for a total of 15 points.