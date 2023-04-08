---
title: OnePlus 9 Pro - Convert T-Mobile branded device to Global
categories: android
tags: android root oneplus 9pro le2127 le2125
---

How to convert T-Mobile OnePlus 9 Pro branded device to Global

**OnePlus 9 Pro Device models**

T-Mobile branded OnePlus 9 Pro (LE2127) devices have known issues with Modem and losing 5G if the conversion is not done in the right order. This guide covers the process to convert to NA/Global (LE2125) and prevent modem issues. Global version allows for keeping with the latest updates from OnePlus One and are easier to Root if needed.

- LE2123 - EU : BA
- LE2125 - NA/Global : AA
- LE2127 - T-Mobile : ACB

**Prerequisites:**
- You must be SIM unlocked. This conversion will not SIM unlock you.
- Verify "Enable OEM unlocking" is not greyed out in Developer Options.
- Download the required signed ROMs from the link the bottom
- Familiarity with MSMDownload tools.
- Back up all data, it will be wiped.


## Step 1. Update the stock device to the latest T-Mobile OTA

Install T-Mobile OTA updates all the way until you get to the latest Android 12. It should be version 11_C.20 LE2127_11.C.20 or newer and will take multiple reboots.

## Step 2. Restore to Factory LE2123-EU (BA) with TMO-EU-MSM

This step will convert the device to LE2123-EU: Build Oxygen OS 11.2.2.2.LE15BA

1. Enable USB Debugging and make sure your PC is authorized

```bash
> adb devices
List of devices attached
3d1xxxxx        device
```
2. Open TMO-EU-MSM, select **Others** for user and click **Start**.
3. Reboot the device in EDL mode.

```bash
> adb reboot edl
```

4. MSM will restore back to stock as soon as the device connects.
**This locks the bootloader in the process if it was unlocked previously.**

5. Enable **OEM Unlock** and **USB Debugging** again and make sure your PC is authorized


## Step 3. Local update to LE15AA-NA/Global (AA) on Android 11
This step will convert the device to LE2125-NA/Global: Build Oxygen OS 11.2.9.9.LE15AA

1. Enable USB File transfer.
2. Upload LE15AA_11.2.9.9.zip root folder of the device.
3. Install Local update from system settings menu.
4. Reboot.
5. Enable OEM Unlock and USB Debugging again and make sure your PC is authorized.

## Step 4. Local update to LE2125-NA/Global (AA) on Android 12
This step will convert the device to LE2125-NA/Global: Build Oxygen OS 11.2.10.10.LE15AA

1. Enable USB File transfer.
2. Upload LE2125_11_C.23_OB1.zip root folder of the device.
3. Install Local update from system settings menu.
4. Reboot.
5. Enable OEM Unlock and USB Debugging again and make sure your PC is authorized

## Step 5. Restore with TMO-EU-MSM to Lock-in Modem !!!
This is step sounds backwards, but is important to Lock-in the modem version and prevent 5G/data issues with upgrading to Global Android versions later.

After restoring, the device will be back to LE2123-EU: Build Oxygen OS 11.2.2.2.LE15BA, but the modem baseband will be different.

1. Enable USB Debugging and make sure your PC is authorized

```bash
> adb devices
List of devices attached
3d1xxxxx        device
```
2. Open TMO-EU-MSM, select **Others** for user and click **Start**.
3. Reboot the device in EDL mode.

```bash
> adb reboot edl
```

4. MSM will restore back to stock as soon as the device connects.
**This locks the bootloader in the process if it was unlocked previously.**

5. Enable **OEM Unlock** and **USB Debugging** again and make sure your PC is authorized


## Step 6. Local update to LE15AA-NA/Global (AA) LE15AA_11.2.10.10
This is the highest Android 11 version before going to Android 12. It needs to be installed before going to Android 12 or higher to keep the 5G modem working. This step will convert the device to LE2125-NA/Global: Build Oxygen OS 11.2.10.10.LE15AA

1. Enable USB File transfer.
2. Upload LE15AA_11.2.10.10 zip to the root folder of the device.
3. Install Local update from system settings menu.
4. Reboot.
5. Enable OEM Unlock and USB Debugging again and make sure your PC is authorized.

## Step 7. Install your preferred ROM

After this procedure, it is save to update to Android 12 and 13 ROMs. 

## References
### Guides
[https://forum.xda-developers.com/t/tmo-9-pro-modem-retention-data-fix.4292445/](https://forum.xda-developers.com/t/tmo-9-pro-modem-retention-data-fix.4292445/)

[https://forum.xda-developers.com/t/convert-your-t-mobile-le2127-to-eu-via-msm-no-unlock-bin-needed.4272837/](https://forum.xda-developers.com/t/convert-your-t-mobile-le2127-to-eu-via-msm-no-unlock-bin-needed.4272837/)

### Signed ROMs
[https://forum.xda-developers.com/t/oneplus-9-pro-rom-ota-oxygen-os-repo-of-oxygen-os-builds.4254587](https://forum.xda-developers.com/t/oneplus-9-pro-rom-ota-oxygen-os-repo-of-oxygen-os-builds.4254587)

### Bootloader Unlock
[https://bgx4k3p.github.io/blog/android/linux/2022/11/11/oneplus-9pro-bootloader-unlock.html](https://bgx4k3p.github.io/blog/android/linux/2022/11/11/oneplus-9pro-bootloader-unlock.html)

### Root
[https://bgx4k3p.github.io/blog/android/linux/2022/11/11/oneplus-9pro-root-android-11.html](https://bgx4k3p.github.io/blog/android/linux/2022/11/11/oneplus-9pro-root-android-11.html)

### TWRP
[https://twrp.me/oneplus/oneplus9.html](https://twrp.me/oneplus/oneplus9.html)

