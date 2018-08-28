In this project you will examine the hardware on your Linux machine. You can perform this lab on your VM or your own computer. 

## Introduction 

Modern computers have  many devices, it can be difficult to know exactly what's inside the box. Even when you can look into the box, most of the devices that Linux uses aren't visible. The 
```
lshw
```

 command shows you what devices are connected to your computer.

## Part 1: Gather the Information 

```
lshw
```

 must be run as root in order to see the full set of hardware available. Be sure to execute it using the 
```
sudo
```

 command. The output of 
```
lshw
```

 can be very long, redirect it to a file and view the file with your favorite editor.

## Part 2: What's Inside? 

Now that you have the raw information you should be able to answer the following questions:
  - What SCSI devices are present in your system. (Don't think your computer has SCSI? You might be surprised if you look closely enough.)
  - What is the size of your L1, L2 and L3 caches?
  - How many memory DIMMs do you have? What sizes are they?

## Turn In 

  - Submit the output of lshw as a text file called 
```
lshw.txt
```

 (no screenshots!)
  - The answers to the above questions.

Submit your answers on Canvas.

## Grading 

  - 1 points for lshw.txt
  - 4 points for correct answers.

