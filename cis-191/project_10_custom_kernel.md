In this project you will compile a custom kernel and boot into it.

## Introduction 

Making a custom kernel is easy, though you should not do it lightly on a production machine. On your VM, however, you are at very little risk.

## Get a Kernel and Compile It 

Follow the instructions in[Kernel Customization](kernel_customization)to download and configure your kernel. It's always best to start with a working configuration. Ubuntu stores the default configuration of the current kernel in the /boot directory. Start your customization process by taking the current configuration. Run this command in the kernel source directory that was made when you used apt-get:

```
$cp config-$(uname -r) .config
```

Use the menu based configuration system by invoking the command:

```
$ make menuconfig
```

Change the setting in:

```
General Setup ---> Local Version
```

to include your name in the version string. You must preface your name with a hyphen. For example:

```
-matera
```

Save your configuration then compile and install your kernel. Reboot your VM and you should see that you have your new kernel working:

```
$ uname -r
3.19.8-ckt7-matera
```

Rename a copy of your kernel configuration file. You will turn it in with this project. Rename the configuration file from your kernel source directory like this:

```
$ cp .config ~/kernel-configuration.txt
```

## Recompile the scull Module 

The module you compiled for [Lab 7: Build a Custom Driver](lab_11_build_a_custom_driver)was for your stock kernel. It can't be inserted into your new kernel because the versions don't match exactly. For this part recompile your module and install it into your new kernel's modules directory. Follow the instructions in the lab to build it. This command installs the module:

```
$ sudo cp scull.ko /lib/modules/$(uname -r)/kernel/drivers/misc

$ sudo depmod -a
```

If your module was copied into the right place depmod will find it and check it's module dependencies. Verify that it was found:

```
$ grep scull /lib/modules/$(uname -r)/modules.dep
kernel/drivers/misc/scull.ko:
```

The scull module has no dependencies but you can see that it's present in the modules.dep file. Now you can install the module with the preferred command:

```
$ sudo modprobe scull
student@cis191-20:~/ldd3/scull$ lsmod | grep scull
scull         20480 0
```

Notice you don't need the *.ko extension! Make a copy of the modules.dep to turn in:

```
$ cp
```
/lib/modules/$(uname -r)/modules.dep ~/modules.dep
```

## Turn In 

Turn in your kernel configuration file and your module dependencies.
  - kernel-configuration.txt
  - modules.dep

## Grading 

  * 10 points for running your custom kernel
  * 10 points for having your driver present in your kernel

