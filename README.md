# OpenCore: Linux EFI Labeling Guide (Multiboot)

------

### This method recommends any **"Linux Distro"** is already installed

###### **Warning:** 

###### <p align="justify">This is an alternative method. Linux EFI Partition will be RENAMED!. Use this method with pre-caution. [OpenLinuxBoot.efi](https://dortania.github.io/OpenCore-Multiboot/oc/linux.html#method-a-openlinuxboot) is not required and manual config.plist editing via [ProperTree](https://github.com/corpnewt/ProperTree) or any `.plist editor` is encourage. I will not responsible `100%` if any issues happen using this method. Do note, please use official method instead if you have no problem with [OpenCore Multiboot](https://dortania.github.io/OpenCore-Multiboot/oc/linux.html) support.</p>

------

# Editing Plist

1. Boot to macOS - Edit your `config.plist`
2. Find - Misc/Security/ScanPolicy
3. Change - `ScanPolicy` value to `2690819` or `Enable OC_SCAN_ALLOW_FS_ESP`. Use [OCAuxilliarytools](https://github.com/ic005k/OCAuxiliaryTools) for easy `config.plist` editing (Not Recommend). Save the plist and Reboot.


![via Linux EFI Rename](https://user-images.githubusercontent.com/72515939/153618855-3c59d86a-8c92-450b-bd15-33c8ef2a3566.png)

4. Boot to any installed Linux Distro using possible key manually without OpenCore. Example, `F11` for `MSI`.
5. Find Linux `EFI` Partition path using any tools possible. There are several applications that are suitable for obtaining storage volume path/name.

------

## Method 1: Linux - GUI Support

- [Gparted](https://gparted.org)

- [KDE Partition Manager](https://github.com/KDE/partitionmanager)

We choose `/dev/nvme1np1` as an example. Normally, Linux EFI Partition is labeled as `NO NAME` which is unknown to OpenCore.

![153619493c30aa29b4acf4994ae441a96400ebb80](https://user-images.githubusercontent.com/72515939/153631618-711a7791-ac0e-46af-8bf7-52aeb198498f.png)

------

## Method 2: Linux Terminal - Non-GUI Support

- [fdisk](https://github.com/FDOS/fdisk)

In this situation, `/dev/nvme1np1` is an example. Type in command:

```zsh
sudo fdisk -l
```

Output Example:

```zsh
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

```zsh
fatlabel /dev/device NEW_LABEL
```

6. Type in `sudo` (superuser do) to get root access. Use **`fatlabel`** command to label `NO_NAME` or `NO NAME` EFI Partition. In this case, we will choose `Arch` as new label. Example are as follows:

```zsh
sudo fatlabel /dev/nvme1np1 Arch
```

7. Press `Enter/Return`. `Reboot` and boot back to `OpenCore`. `Arch` EFI Partition now visible.

------

## Method 3: DiskGenius - GUI Support (Windows)

1. Boot to Windows - Edit your `config.plist`, follow `Editing Plist`
2. Download [DiskGenius](https://www.diskgenius.com/)

<img width="912" alt="Screenshot 2022-04-12 230853" src="https://user-images.githubusercontent.com/72515939/162994338-39864d07-9f19-4b74-9d27-a0bf8a8cfa18.png">

3. Select any Linux EFI, right click and `Select Volume Name` and rename Normal Label (NO NAME) to any remarkable name you choose. In This case, we will choose `Arch`.

**<p align="center">Before</p>**

<p align="center"><img width="275" alt="Screenshot 2022-04-12 231022" src="https://user-images.githubusercontent.com/72515939/162994786-b12b599f-0d68-42bb-9c90-2766687c7eb1.png"></p>

**<p align="center">After</p>**

<p align="center"><img width="276" alt="Screenshot 2022-04-12 231907" src="https://user-images.githubusercontent.com/72515939/162996386-5ef4d51d-af0b-4d33-844c-c8538c55e2a7.png"></p>

4. Close the apps, and Reboot. `Arch` EFI Partition now visible.

------

# Acknowledgements

I would like to thanks all folks in Hackintosh Community especially:

- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for a great guide
- [Acidanthera](https://github.com/acidanthera) for a great work. KUDOS for them
- [CorpNewt](https://github.com/corpnewt) for developing simple USB mapping tools
- [dhinakg](https://github.com/USBToolBox/tool) for developing easy Windows based USB mapping tools inspired by CorpNewt USBMap
- [Hackintosh Malaysia](https://www.facebook.com/groups/HackintoshMalaysia/about/) for knowledge sharing
- [r/Hackintosh](https://www.reddit.com/r/hackintosh/) for an easy undocumented references
- [daliansky](https://github.com/daliansky) for publishing his own OpenCore ACPI method (OC-Little) and implementation
- [5T33Z0](https://github.com/5T33Z0/OC-Little-Translated) for translating daliansky OC-Little
- [rusty-bits](https://github.com/rusty-bits) for an easy EFI update using windows "Cmd-Prompt", linux and MacOS Terminal

