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

5. Find Linux EFI Partition path using any tools possible. GParted (GUI Support), KDE Partition Manager (GUI Support) or fdisk (No GUI Support) is recommended. Other tools such as "fdisk" also We choose /dev/nvme1np1 as example. Sometime, Linux EFI Partition is labeled as "NO_NAME" or "EFI" which is unknown to OpenCore. Below is an success example of Linux EFI Partition labeled as "Arch". The patrition must ne in Fat32.

**KDE Partition Manager (GUI Support):**

![Screenshot_20220211_232624](https://user-images.githubusercontent.com/72515939/153619493-c30aa29b-4acf-4994-ae44-1a96400ebb80.png)

**fdisk (No GUI Support)"

Command:

```
sudo fdisk -l
```

Output:

```
Disk /dev/nvme1n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: KINGSTON SA2000M8500G                   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 945B20C6-EB46-234C-A4DC-A70A2985D995

Device             Start       End   Sectors   Size Type
/dev/nvme1n1p1      4096    618495    614400   300M EFI System       <----- Linux EFI Partition
/dev/nvme1n1p2    618496 905040661 904422166 431.3G Linux filesystem
/dev/nvme1n1p3 905040662 976768064  71727403  34.2G Linux swap


Disk /dev/nvme0n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: KINGSTON SA2000M8500G                   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 4C938ED0-0EBB-41B1-A33A-6645E6CAAD62

Device          Start       End   Sectors   Size Type
/dev/nvme0n1p1     40    409639    409600   200M EFI System          <----- MacOS EFI Partition
/dev/nvme0n1p2 409640 976773127 976363488 465.6G Apple APFS


Disk /dev/sdb: 447.14 GiB, 480113590272 bytes, 937721856 sectors
Disk model: SanDisk SSD PLUS
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0778dc02

Device     Boot Start       End   Sectors   Size Id Type
/dev/sdb1        2048 937721855 937719808 447.1G af HFS / HFS+


Disk /dev/sda: 223.57 GiB, 240057409536 bytes, 468862128 sectors
Disk model: SanDisk SSD PLUS
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0778dc00

Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1        2048 468860927 468858880 223.6G af HFS / HFS+


Disk /dev/sdc: 149.01 GiB, 159998918144 bytes, 312497887 sectors
Disk model: Hitachi HDT72101
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0778dc0c

Device     Boot Start       End   Sectors  Size Id Type
/dev/sdc1        2048 312496127 312494080  149G af HFS / HFS+
```


Code Sample

```
fatlabel /dev/device NEW_LABEL
```

6. Linux EFI Rename. Type in Linux Terminal, 

```
sudo fatlabel /dev/nvme1np1 Arch
```

7. Reboot and boot back to OpenCore. Linux Partition "Arch" now visible to choose.
