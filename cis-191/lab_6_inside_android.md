For this lab we will get to the command line on an Android device. You will not need an Android device for this lab because we will be using the emulator in the Android Source Development Kit (SDK). If you have an Android phone or tablet the tools in this lab will enable you to reach the Linux prompt using a USB cable. I have installed components of the Android SDK in your VMs.
Part 1: Starting the Android Emulator on your VMYour VM should have the Android SDK installed in the home directory of the student account. The SDK contains an emulator that will boot Android Linux in a virtual phone window. This is used by developers to test their software. You can start the emulator with the following command:
/home/student/android-sdk-linux/tools/emulator64-arm -avd cis-191-superphone
A window will popup with graphical phone controls and Android will boot. Using an emulator is much slower than using real hardware (with some exceptions) so it will take your virtual phone a few minutes to boot into the Android desktop. Once the Desktop is shown you should be able to see the emulated device in the list of things you can connect to with the Android debugger (ADB):
/home/student/android-sdk-linux/platform-tools/adb devices
That command should show you that emulator-5554 is online. If not, wait for the bootup to complete then run:
/home/student/android-sdk-linux/platform-tools/adb shell
You are now connected to a shell on the Android device. You can proceed to Part 2.
Part 2: Determine System InformationIn this part you will use the commands from previous labs to answer questions about your Android device.
Question 1: Answer each of the following questions about all mounted EXT filesystems:
  - What device is the filesystem on?
  - Where is the filesystem mounted?
  - How much space is available on the filesystem?

Question 2: Answer the following questions about the system:
  - What is the type and model of the processor?
  - How many BogoMIPS does the processor have?
  - How much memory does the system have?
  - How much memory is free?
  - How long has the system been up?

Question 3: Questions about a process.Android doesn't use XWindows, it uses SurfaceFlinger. SurfaceFlinger is tightly integrated with the GPU and is lighter than X. Answer the following questions about SurfaceFlinger:
  - What is SurfaceFlinger's PID?
  - What are SurfaceFlinger's STDIN, STDOUT and STDERR pointing to?
  - How much memory is SurfaceFlinger using (what's it's VSIZE)?

Question 4: Questions about the system logs.Android's logs are not stored in /var/log. They're handled by an app. However, you can still see the contents of the kernel's log buffer. Inside the log buffer you will find the answer to the following questions:
  - What is the exact version of the kernel?
  - What's the email address of the person who compiled it?
  - When was it compiled?

Extra Credit - Part 1For extra credit you can perform the homework on your own copy of the Android SDK. For this version you should follow the instructions here:
[http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)
You can download it for Windows, Linux or Mac. The download will be about 500MB of data so be prepared to wait (or do it at school where the internet is fast).Once you have the SDK downloaded follow the instructions here:
[http://developer.android.com/sdk/installing/bundle.html](http://developer.android.com/sdk/installing/bundle.html)
When complete you should have open the Android programming environment. Now you can create your first Android app.When the IDE starts the first time it will ask you if you want to create a new Android app. You do! You will need to pick a name for your app but you should leave everything else in its default state. Google provides documentation for how to start your first app here:
[http://developer.android.com/training/basics/firstapp/creating-project.html](http://developer.android.com/training/basics/firstapp/creating_project.html)
You can leave your Android app empty. You don't need to write any code, the starter code is enough. The link below has instructions for how to run your new app in the emulator:
[http://developer.android.com/training/basics/firstapp/running-app.html#Emulator](http://developer.android.com/training/basics/firstapp/running_app.html#Emulator)
You will need to download an additional component that's the hardware description of the emulated hardware and a complete Linux implementation for it. Luckily that's smaller than the SDK. Once your app is running in the emulator do the following:
  - Take a screenshot of your app
  - Do Part 2 on your emulated device

NOTE:I experienced a bug in Windows where if I set the memory on my virtual phone too high you can't run. I changed my virtual device's memory to 512MB and that worked. There's a thread about it on [Stack Overflow](http://stackoverflow.com/questions/5969067/android-failed-to-allocate-memory/14420779#14420779). It just worked on Linux.
Extra Credit - Part 2For extra credit you can perform the homework on real hardware. If you've done the first part of the extra credit you have the tools necessary to plug your phone or tablet into your computer and get to the command line. Your device must be in developer mode. Here's how to enable developer mode:
[http://developer.android.com/tools/device.html](http://developer.android.com/tools/device.html)
Note on Linux: The above page also contains udev rules that you will need to add in order for a normal user to be able to access your phone. Without those you will need to run adb as root (don't do that). Once you have access to your device do the following:
  - Submit the output of the 'adb devices' command while your device is attached
  - Do Part 2 on your physical device.
  - 

Turn In:Turn in answers to the above questions on Blackboard.
Grading:
  * 20 points for Part 2 (5 points per question)
  * 15 points (Extra Credit) for EC - Part 1
  * 15 points (Extra Credit) for EC - Part 2

