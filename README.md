# OpenCore: Linux EFI Labeling Guide (Multiboot)

### **Warning:** 

### <p align="justify">
This is an **alternative** method. Recommends any **"Linux Distro"** is already installed. The **Linux EFI Partition** will be renamed! Use this method with precaution. Manual config.plist editing via **[ProperTree](https://github.com/corpnewt/ProperTree)** or any **.plist editor** is encouraged. I will not be 💯 responsible if any issues happen using this method. Do note, please use the **[official](https://dortania-github-io.thrrip.space/OpenCore-Install-Guide/)** method if you have no issues with Multiboot. Refer to the official **[OpenCore Multiboot](https://dortania.github.io/OpenCore-Multiboot/oc/linux.html)** support for more info.
</p>

</br>

## Method 1: Linux - Using [Gparted](https://gparted.org) or [KDE Partition Manager](https://github.com/KDE/partitionmanager)

📌 This is **GUI support** guide. At first, **boot** to linux using possible BIOS key without boot to OpenCore. Use list below as references. If not listed, please 👓 [GOOGLE](https://www.google.com).

- ✏️ ASUS = F8
- ✏️ Gigabyte = F12
- ✏️ MSI = F11      
- ✏️ Intel = F10
- ✏️ Asrock = F11
- ✏️ EVGA = F7

📌 **Edit** config.plist by using [Xplist](https://github.com/ic005k/Xplist) or [ProperTree](https://github.com/corpnewt/ProperTree) for manual editing.

📌 **Find** Misc/Security/ScanPolicy.

📌 **Change** ScanPolicy value to `2690819` or `Enable OC_SCAN_ALLOW_FS_ESP`. **Save** the plist and **Reboot**.

![via Linux EFI Rename](https://user-images.githubusercontent.com/72515939/153618855-3c59d86a-8c92-450b-bd15-33c8ef2a3566.png)

📌 Use any linux built-in partition manager such as **GParted** or **KDE Partition Manager** to find linux **EFI**. As **example**, `/dev/nvme1np1` will be chosen. **Rename** linux **EFI** partition labelled as **NO NAME** to any name. In this case, **Arch** are choosen as new label.

📌 **Reboot** PC.

![153619493c30aa29b4acf4994ae441a96400ebb80](https://user-images.githubusercontent.com/72515939/153631618-711a7791-ac0e-46af-8bf7-52aeb198498f.png)

</br>

## Method 2: Linux Terminal - Using [fdisk](https://github.com/FDOS/fdisk)

📌 This is not **GUI support** method. Same as **Method 1**, linux **EFI partition** is labeled as `NO NAME` which is unknown to **OpenCore**.

📌 In this situation, `/dev/nvme1np1` is an example. Type in **Terminal**:

`sudo fdisk -l`

📌 Output Example:

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

📌 Type in **sudo** (superuser do) to get **root access**. Use `fatlabel` command to label **NO_NAME** / **NO NAME** EFI partition. In this case, **Arch** are choosen as new label. **Example** are as follows:</p>

💻 **Fatlabel basic command**

`fatlabel /dev/device NEW_LABEL`

💻 **Use command**

`sudo fatlabel /dev/nvme1np1 Arch`

📌 Press **Enter/Return**. `Reboot` and **reboot** back to **OpenCore**. **Arch** EFI Partition now **visible**.

</br>

## Method 3: Using [DiskGenius](https://www.diskgenius.com/) - GUI Support (Windows Only).

📌 This is **GUI support** guide. At first, **boot** to Windows.

📌 **Download** [DiskGenius](https://www.diskgenius.com/).

📌 **Edit** config.plist by using [Xplist](https://github.com/ic005k/Xplist) or [ProperTree](https://github.com/corpnewt/ProperTree) for manual editing.

📌 **Change** ScanPolicy value to `2690819` or `Enable OC_SCAN_ALLOW_FS_ESP`. **Save** the plist and **Reboot**.

📌 **Save** the plist and **Reboot**.

<img width="912" alt="Screenshot 2022-04-12 230853" src="https://user-images.githubusercontent.com/72515939/162994338-39864d07-9f19-4b74-9d27-a0bf8a8cfa18.png">

📌 Select any linux **EFI**, right click and **Select Volume Name** and rename **Normal Label (NO NAME)** to **any** remarkable name you choose. In this case, **Arch** are choosen as new label.

**<p align="center">Before</p>**

<p align="center"><img width="275" alt="Screenshot 2022-04-12 231022" src="https://user-images.githubusercontent.com/72515939/162994786-b12b599f-0d68-42bb-9c90-2766687c7eb1.png"></p>

**<p align="center">After</p>**

<p align="center"><img width="276" alt="Screenshot 2022-04-12 231907" src="https://user-images.githubusercontent.com/72515939/162996386-5ef4d51d-af0b-4d33-844c-c8538c55e2a7.png"></p>

📌 Close the apps, and Reboot. **Arch** EFI partition now visible instead of **NO NAME** using [OpenCore](https://github.com/acidanthera/OpenCorePkg).

</br>

## ACKNOWLEGEMENT

📌 I would like to thanks all folks in Hackintosh Community especially:

- 💠 [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) 😍 a great guide
- 💠 [corpNewt](https://github.com/corpnewt) 😁 developing [ProperTree](https://github.com/corpnewt/ProperTree) 
- 💠 [Hackintosh Malaysia](https://www.facebook.com/groups/HackintoshMalaysia/about/) 😉 an official [Facebook](https://www.facebook.com) community for Hackintosh
- 💠 [r/Hackintosh](https://www.reddit.com/r/hackintosh/) 😘 my favourite [reddit](https://www.reddit.com) Hackintosh discussion platform
- 💠 [ic005k](https://github.com/ic005k) 😗 develop [Xplist](https://github.com/ic005k/Xplist)

</br>

## FOLLOW ME

- ❤️ [reddit](https://www.reddit.com) - ⭐ [u/mohdismailmatasin](https://www.reddit.com/user/mohdismailmatasin)
