# OpenCore: Easy Linux EFI Labeling Guide

This step can be used once any **"Linux Distro"** is installed.

**Warning: Linux EFI Partition will be "Renamed"**

1. Boot to macOS - Edit your config.plist

2. Find - Misc/Security/ScanPolicy

3. Change - ScanPolicy value to 2690819
   
   or
   
   Enable OC_SCAN_ALLOW_FS_ESP
   
   Interger Value = 1024
   
   Hex Value=0x400
   
  <img width="826" alt="Linux EFI Rename" src="https://user-images.githubusercontent.com/72515939/153618855-3c59d86a-8c92-450b-bd15-33c8ef2a3566.png">

4. Boot to installed Linux Distro using any possible key manually without OpenCore. Example, F11 for MSI.

5. Find Linux EFI Partition path using any tools possible. I recommend GParted/KDE Partition Manager. We choose /dev/nvme1np1 as example. Sometime, Linux EFI Partition is named as "NO_NAME" or "EFI" which is unknown to OpenCore. Below is an example Linux EFI Partition renamed / labeled as "Arch". The patrition must ne in Fat32.

![Screenshot_20220211_232624](https://user-images.githubusercontent.com/72515939/153619493-c30aa29b-4acf-4994-ae44-1a96400ebb80.png)

**Code Sample**

```
fatlabel /dev/device NEW_LABEL
```

6. Linux EFI Rename. Type in Linux Terminal, 

```
sudo fatlabel /dev/nvme1np1 Arch
```

7. Reboot and boot back to OpenCore. Arch Partition now visible to choose.
