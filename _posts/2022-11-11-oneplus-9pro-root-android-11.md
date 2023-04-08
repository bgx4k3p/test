---
date: 2022-11-11 12:26:40
layout: post
title: OnePlus 9 Pro - Root Android
subtitle: How to Root OnePlus 9 Pro device.
description: Step by step instructions for rooting OnePlus 9 Pro device for penetration testing or other purposes.
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559820489/js-code_n83m7a.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559820489/js-code_n83m7a.jpg
category: android
tags:
  - android
  - root
  - oneplus
  - 9pro
author: mranderson
---

## Step 1. Install Android 11 Global LE15AA

The latest Android 11 Global ROM should be LE15AA_11.2.10.10

* The same process will work for any other Android version, just need to match the Magisked boot image.

## Step 2. Unlock bootloader if you haven't

1. This step will erase the phone entirely
2. Enable USB Debugging again and make sure your PC is authorize
3. OEM Unlock should be grayed out (already unlocked)

## Step 3. Download Magisked Boot.img

OOS 11.2.10.10 boot : Untouched/Magisked (AA)

[https://forum.xda-developers.com/t/guide-magisk-unlock-root-keep-root-oos-13-f-16.4252373/](https://forum.xda-developers.com/t/guide-magisk-unlock-root-keep-root-oos-13-f-16.4252373/)

## Step 4. Boot with temporary Root access

```bash
# Reboot in Fastboot mode
>adb reboot bootloader

# Confirm the device is showing
>fastboot devices
71xxxxb9        fastboot

# Temporary root (safer than flash directly)
>fastboot boot magisk_boot.img
Sending 'boot.img' (98304 KB)                      OKAY [  2.188s]
Booting                                            OKAY [  0.344s]
Finished. Total time: 2.609s
```

## Step 5. Complete Magisk install and Permanent Root

1. After booting with temporary magisk_boot.img, there will be a new app Magisk installed.
2. Open Magisk app and select **"Upgrade to full Magisk"**. It might ask to confirm installing from this source.
3. **Requires Additional Setup** - click **CANCEL**. If you clicked OK by accident, just boot in temporary root again from previous step.
4. Click Install and select **Direct Install (Recommended)**
5. Reboot

### References

[https://forum.xda-developers.com/t/guide-magisk-unlock-root-keep-root-oos-13-f-16.4252373/](https://forum.xda-developers.com/t/guide-magisk-unlock-root-keep-root-oos-13-f-16.4252373/)

[https://www.droidwin.com/temporarily-root-android-via-magisk](https://www.droidwin.com/temporarily-root-android-via-magisk)
