In this lab you'll use LVM to implement advanced features on your filesystem.

## Part 1: Extend the Size of your Root Volume 

In this part you will extend the size of your root volume. Here's what your VM's first volume group looks like:

```
$ sudo vgdisplay ubuntu-vg
  Using volume group(s) on command line
  Finding volume group "ubuntu-vg"
 --- Volume group ---
 VG Name        ubuntu-vg
 System ID      
 Format        lvm2
 Metadata Areas    1
 Metadata Sequence No 7
 VG Access       read/write
 VG Status       resizable
 MAX LV        0
 Cur LV        2
 Open LV        2
 Max PV        0
 Cur PV        1
 Act PV        1
 VG Size        31.76 GiB
 PE Size        4.00 MiB
 Total PE       8130
 Alloc PE / Size    3813 / 14.89 GiB
 Free PE / Size    4317 / 16.86 GiB
 VG UUID        yPik5p-5Bc5-6S7Y-af9c-O4Q2-DHHR-bt8quH
```

Notice that there are extents on the ubuntu-vg volume group that can still be allocated (about 17G worth). Using the ''lvextend'' and the ''resize2fs''commands perform the following tasks:

  - Extend the /dev/ubuntu-vg/root logical volume by 3000 extents.
  - Extend the root filesystem to use the additional extents.

WARNING: Do NOT extend the root logical volume to take all remaining space. You will need that space for the next part. Check that you've still got free space after doing step 1. If not you still have time to take the space away. After you extend the filesystem reducing the space gets much harder.

If you have extended the root logical volume properly the output of ''lvdisplay'' command should be:

```
$ sudo lvdisplay /dev/ubuntu-vg/root
 --- Logical volume ---
 LV Path        /dev/ubuntu-vg/root
 LV Name        root
 VG Name        ubuntu-vg
 LV UUID        u54Jmh-7gXk-S2gF-Q4QQ-1RTP-s3Yq-Z8W6Pg
 LV Write Access    read/write
 LV Creation host, time ubuntu, 2015-09-03 11:36:20 -0700
 LV Status       available
 # open         1
 LV Size        24.62 GiB
 Current LE       6302
 Segments        2
 Allocation       inherit
 Read ahead sectors   auto
 - currently set to   256
 Block device      252:0
```

Notice that the size is now 24.62 GiB with 6302 logical extents.

## Part 2: Create a Snapshot 

In this part you'll create a snapshot volume and mount it. The snapshot will hold the state of your VM so that you can recover files if you delete them. You will need the ''lvcreate'' and ''mount'' commands to:

  - Create a snapshot volume based on /dev/ubuntu-vg/root using the remaining1317 physical extents
  - Make a /snapshot directory and mount the snapshot on it
  - Edit /etc/fstab so that your snapshot volume is mounted every time your VM starts

## Part 3: Encrypted Volumes (Extra Credit) 

For extra credit you will create an encrypted volume inside of a file using LUKS and a loopback device. In the real world you might do this to make an encrypted flash key instead of a loopback device. Before you begin you will need to install additional packages:

```
apt-get install cryptsetup-bin
```

The cryptsetup command creates special LVM volumes. Start by creating a 10MB file using dd (just like the one you created in project 4) and binding it to a loopback device. Now you can format the loopback device with the LUKS encrypted format:

```
cryptsetup luksFormat <loopback-device>
```

The cryptsetup command will ask you for a password for your encrypted volume. Use the password "lefty" so that I can read it once you've turned it in. The LUKS format is not a filesystem format, it's a format that tells Linux about the encryption algorithms used on your device. You can see what's in the LUKS format using the following command:

```
$ sudo cryptsetup luksDump /dev/loop0
LUKS header information for /dev/loop0
Version:    
1
Cipher name:  
aes
Cipher mode:  
xts-plain64
Hash spec:   
sha1
Payload offset:
4096
MK bits:    
256
MK digest:   
4f 3d 18 b9 63 e1 af 62 04 5f af 91 84 92 65 5b 37 80 02 72
MK salt:    
f9 60 68 19 7f 22 6a 4e 45 fa dc bf dd 0b cc a3
       
7d 11 8a cc 5e 23 06 e8 14 12 68 2b 00 d6 f5 60
MK iterations: 
33250
UUID:     
f42b3b10-08c8-468e-94a9-59d8dd85feb6
Key Slot 0: ENABLED
Iterations:     
133890
Salt:        
a5 d7 f9 45 09 a2 ca 2d 1b d8 a0 f5 cf df 4f e5
           
9b 8c ec 59 ad 0e ed 77 14 3b 2d 0a 42 20 96 08
Key material offset:
8
AF stripes:      
4000
Key Slot 1: DISABLED
Key Slot 2: DISABLED
Key Slot 3: DISABLED
Key Slot 4: DISABLED
Key Slot 5: DISABLED
Key Slot 6: DISABLED
Key Slot 7: DISABLED
```

Notice that there are eight key slots. Your volume can have up to eight different passwords! In order for your volume to hold files you must now open it. Opening it puts your key into the kernel and creates a special block device that does transparent encryption and decryption:

```
cryptsetup luksOpen <loop-device> <pick-a-volume-name>
```

The command will ask for the password. If it works you should see your volume name in the /dev/mapper directory:

```
ls -la /dev/mapper
```

This is your encrypted block device. Using the mkfs and mount commands do the following:

  - Format your encrypted volume with the fat32 filesystem
  - Mount your newly formatted device
  - Create a file in the new device called README.txt that contains your name.

Once you have your name in the file you should tear down the encrypted volume:

```
sudo umount <path-to-mount>
sudo cryptsetup luksClose <your-volume-name>
sudo losetup -d <loop-device>
gzip <image-file>
```

Submit your gzipped image file with the lab for extra credit.Turn InOnce you have completed the lab execute the following commands:

```
vgdisplay -v ubuntu-vg > /home/student/ubuntu-vg.txt
mount > /home/student/mounts.txt
cp /etc/fstab /home/student/fstab
df > /home/student/df.txt
```

Submit ubuntu-vg.txt, mounts.txt, fstab and df.txt on Canvas.

## Grading 

  * 5 points for each file (20 possible)
  * 15 points for the extra credit

