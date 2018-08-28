# Lab 6 OverlayFS  

In this lab you'll experiment with OverlayFS.

## Make a Base Volume  

Start by partitioning the disk ''/dev/sdc''. Partition the disk this way: 

  * Use a GPT disk label. 
  * Create a single partition on the disk 
    * Name it "Freezer" 
    * Format it with the 'btrfs' filesystem

## Mount Your Base Volume  

Create a temporary area to work on OverlayFS. I suggest the following directories: 

  * ''/mnt/lower'' -- mount ''/dev/sdc1'' here.
  * ''/mnt/upper'' -- mount a ''tmpfs'' virtual filesystem here. 
  * ''/mnt/overlay'' -- mount your ''overlayfs'' virtual filesystem here. 

<alert>Remember the work directory and the upper directory must be in the same filesystem!</alert>

Copy the contents of ''/var/www'' to ''/mnt/lower'' before you attempt to mount your ''overlayfs''. 

Once you think you have successfully created your overlay use the following commands to view the contents:

<code bash>
$ tree /mnt 
$ mount 
```

## Turn In  

Submit the output of the two commands. 

## Grading  

  * 10 points for creating a btrfs filesystem on /dev/sdc1 
  * 10 points for your overlayfs that uses memory as the upper layer. 
