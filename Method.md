# OpenCore: Linux EFI Labeling Guide (Multiboot)

### This step can be used once any **"Linux Distro"** is installed.

###### **(Warning: Linux EFI Partition will be "Renamed)**



### Step

1. Boot to macOS - Edit your `config.plist`

2. Find - Misc/Security/ScanPolicy

3. Change - `ScanPolicy` value to `2690819` or `Enable OC_SCAN_ALLOW_FS_ESP`. Use [OCAuxilliarytools](https://github.com/ic005k/OCAuxiliaryTools) for easy `config.plist` editing

![Linux EFI Rename](https://user-images.githubusercontent.com/72515939/153618855-3c59d86a-8c92-450b-bd15-33c8ef2a3566.png)

4. Boot to installed Linux Distro using any possible key manually without OpenCore. Example, `F11` for `MSI`.

5. Find Linux `EFI` Partition path using any tools possible. There are several applications that are suitable for obtaining storage volume path/name. 
   
   

###### GUI Support

- [Gparted](https://gparted.org)

- [KDE Partition Manager](https://github.com/KDE/partitionmanager)
  
  

We choose `/dev/nvme1np1` as an example. Normally, Linux EFI Partition is labeled as `NO_NAME` or `NO NAME` which is unknown to OpenCore. 

![153619493c30aa29b4acf4994ae441a96400ebb80](https://user-images.githubusercontent.com/72515939/153631618-711a7791-ac0e-46af-8bf7-52aeb198498f.png)



###### Non-GUI Support (Terminal Based)

- [fdisk]([GitHub - FDOS/fdisk: Fixed disk tool - create partitions.](https://github.com/FDOS/fdisk))



Also `/dev/nvme1np1` as an example. Type in command

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

6. As example, use **`fatlabel`** command to label `NO_NAME` or `NO NAME` to `Arch`. 

```
sudo fatlabel /dev/nvme1np1 Arch
```

7. Reboot and boot back to OpenCore. `Arch` Partition now visible.


