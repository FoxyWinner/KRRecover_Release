# KRRecover_Release
KRRecover is a tool for ransomware recovery. Here are its installation package, operating environment and operating instructions

##What is KRRecover

KRRecover maintains a list of suspicious applications (suspected ransomware) with the user. If KRRecover finds that an application in the list of suspicious applications is running, it will detect the behavior of the suspicious applications during operation and forcibly stop the suspicious applications if necessary.Then, if the ransoming has already taken place, KRRecover will help the user's phone unlock the screen, lock the password and unlock the files encrypted by the ransomware.

## Runtime environment

Operating system: Android 4.4 (API 19)

Basic software tools: 

* SuperSU v2.79 
* XposedInstaller for Android 4.0-4.4
* Sandbox(We developed an API hook tool based on the Xposed framework)

## How to start

### 1. Gets system root privileges

Rooting the phone using SuperSU V2.79. When your computer is connected to an Android phone or emulator, it is divided into the following steps:

```shell
# Install SuperSU V2.79.
adb install ../SuperSU-v2.79.apk
adb root
adb remount
# Find the Recovery Flashable.zip extracted the correct CPU architecture su file and push it into the system directory, take the x86 example.
adb -e push ../SuperSU2.79recovery/x86/su /system/bin/su
```

Then go into ADB and execute the following command：

```shell
su root
cd /system/bin
chmod 06755 su
su --install
su --daemon&
setenforce 0
```

Finally, open SuperSU on your Android phone or emulator, click "New User" and update the binary file to complete root:

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/root_1.png" alt="root_1" style="zoom:50%;" /></div>

(Click continue)

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/root_2.png" alt="root_2" style="zoom:50%;" /></div>

(Click normal)

### 2. Install the Xposed environment and assign root permissions

First install Xposed_installer:

```shell
adb install ../Xposed_installerforAndroid4.0-4.4.apk
```

Then open it, click *Framwork*, ignore all warnings, and then click *Install/Update* to grant root and ultimately install the Xposed framework.

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/xposed_1.png" alt="xposed_1" style="zoom:50%;" /></div>

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/xposed_2.png" alt="xposed_2" style="zoom:50%;" /></div>

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/xposed_3.png" alt="xposed_3" style="zoom:50%;" /></div>

### 3. Install the sandbox, put  the configuration file in the specified location, and activate the sandbox module

Install the sandbox:

```shell
adb install ../sandbox.apk
```

Put `hooks.json` and `plugin.json` under `/data/local/tmp`. If the sandbox doesn't read it, it crashes:

```shell
adb -e push ../hooks.json /data/local/tmp
adb -e push ../plugin.json /data/local/tmp
```

Activate the sandbox module in Xposed Installer - modules:

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/activate.png" alt="activate" style="zoom:50%;" /></div>

Reboot the device.

### 4. Place two test files on the device

Place two test files in the `/sdcard/` directory to test ransomware encryption and recovery:

```shell
adb -e push ../test.png /sdcard/
adb -e push ../test.txt /sdcard/
```

### 5. Install and run KRRecover

Install the KRRecover:

```shell
adb install ../KRRecover.apk
```

KRRecover will automatically install KRRecover-sub and restart the device after installation is completed and operation is granted KRRecover root permission. Wait after the restart and grant KRRecover-sub root permission, too.

## What is the effect

For example, you can install ransomware and KRRecover will pop up to ask the user if the app is suspicious. If the user marks the application as suspicious, KRRecover will periodically check if the application is running, and if it is running, force it to stop and attempt to unlock the device password. Users can open KRRecover, click decryption, restore ransomware encrypted files.

Before opening the ransomware:

![KRRecover_0](https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/KRRecover_0.png)

After opening the ransomware:

![KRRecover_2](https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/KRRecover_2.png)

Wait for the ransomware to be forced to stop by KRRecover, then open KRRecover and click the decryption button:

<div align=center><img src="https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/KRRecover_3.png" alt="KRRecover_3" width="375" /></div>

![KRRecover_4](https://github.com/FoxyWinner/KRRecover_Release/blob/master/readme_images/KRRecover_4.png)

