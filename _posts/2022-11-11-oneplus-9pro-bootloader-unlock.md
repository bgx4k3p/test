---
date: 2022-11-11 12:25
layout: post
title: OnePlus 9 Pro - Bootloader Unlock
subtitle: How to unlock bootloader on OnePlus 9 Pro device
description: Step by step instructions for rooting OnePlus 9 Pro device for penetration testing or other purposes.
image: https://bgx4k3p.github.io/test/assets/img/code-large.jpg
category: android
tags: android root oneplus-9 unlock
author: bgx4k3p
---

## Step 1. Install Android tools

The same thing can be done on Mac, Windows or Linux with**android-platform-tools** installed.

```bash
# Install Android tools on Mac with Homebrew
brew install --cask android-platform-tools
```

## Step 2. Enable OEM Unlocking on OnePlus 9 Pro

1. Go to **Settings > About Phone** > Tap on Build number 5-6 times to enable **Developer options**.
2. Now go to **> System  > Developer options** > enable **OEM Unlocking** and **USB debugging**.
3. Make sure your PC is authorized when you plugin the USB cable.


## Step 3. Get your IMEI

Get your phone’s IMEI by dialing ***#06#** in the phone’s dialer. The IMEI code will show up. You will need this for T-Mobile’s form later on.


## Step 4. Get your unlock code

1. Reboot into fastboot mode
```bash
adb reboot bootloader
```

2. Generate Unlock code for T-Mobile’s online form.

```bash
fastboot oem get_unlock_code

# Example what it looks like
MacBook:~$ fastboot oem get_unlock_code
(bootloader) Serial Number:
(bootloader) ====================================
(bootloader) 3dxxxx26
(bootloader) ====================================
(bootloader) Unlock code:
(bootloader) ====================================
(bootloader) 8B9D3B46A02677526B7953A8A2XXXXXX
(bootloader) 00F5147972F8B93DCA6E7B8AC3XXXXXX
(bootloader) ====================================
```
3. Combine the 2 lines into one long string

Example Unlock code:  
```bash
8B9D3B46A02677526B7953A8A2F6356700F5147972F8B93DCA6E7B8AC3XXXXXX
```

## Step 5. Fill out the T-Mobile online form

This is where you will need to enter your IMEI code and your unlock code (2 lines as one long code). Once you submit it, you should receive a flashable unlock token within two weeks.

https://www.oneplus.com/unlock_token


## Step 6. OEM Bootloader Unlock

**Warning!** Unlocking the bootloader will reset/erase your device. Back up first if anything important.

1. Reboot in Fastboot mode
```bash
adb reboot bootloader
```
2. Flash the Unlock token bin
```bash
fastboot flash cust-unlock unlock_code.bin 

Sending 'cust-unlock' (0 KB)                       OKAY [  0.002s]
Writing 'cust-unlock'                              (bootloader) Device is unlocked.
OKAY [  0.003s]
Finished. Total time: 0.015s
```

3. Unlock the bootloader

```bash
fastboot oem unlock

OKAY [  0.034s]
Finished. Total time: 0.034s
```


## References

### Guides: 
[https://www.oneplus.com/support/answer/detail/op588](https://www.oneplus.com/support/answer/detail/op588)

###  Unbrick
[https://forum.xda-developers.com/t/op9pro-repository-of-msm-unbrick-tools-tmo-eu-glo-in.4272549/](https://forum.xda-developers.com/t/op9pro-repository-of-msm-unbrick-tools-tmo-eu-glo-in.4272549/)

[https://onepluscommunityserver.com/list/Unbrick_Tools/OnePlus_9_Pro](https://onepluscommunityserver.com/list/Unbrick_Tools/OnePlus_9_Pro)


### OEM Unlocking Grayed out
[https://www.droidwin.com/enable-greyed-out-oem-unlock-in-oneplus-carrier-locked-t-mobile/#How_to_Enable_Greyed_out_OEM_Unlock_in_OnePlus_Carrier_Locked_T-Mobile](https://www.droidwin.com/enable-greyed-out-oem-unlock-in-oneplus-carrier-locked-t-mobile/#How_to_Enable_Greyed_out_OEM_Unlock_in_OnePlus_Carrier_Locked_T-Mobile)

