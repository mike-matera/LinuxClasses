In this lab you will must read and understand the procedure in[Logical Volume Management](logical_volume_management). Once you have tried and mastered that procedure will:
Create a physical volume on:
  - /dev/sdb
  - /dev/sdc

Place both of those volumes into a volume group. Once your volume group is created you will create a logical volume that mirrors its contents on both disks. The lvcreate command to create a mirror (a.k.a. RAID1) looks like this:

```
$ sudo
lvcreate --type=mirror -l <number-of-extents> -n <logical-volume-name> <volume-group-name>
```

You will have to determine the number of extents to use, it will depend on the size of your disks. When you're done save the output of vgdisplay into a file:

```
$ vgdisplay -v > volume-groups.txt
```

Submit your file Blackboard.