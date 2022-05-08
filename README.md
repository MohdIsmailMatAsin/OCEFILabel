# Hackintosh: An Alternative Linux EFI Labeling 

> **Warning:**<div align="justify">_This is an alternative method which recommends **Linux Distro** is already installed. Exact **EFI** partition will be renamed. Use this method with precaution. Manual config.plist editing via **[ProperTree](https://github.com/corpnewt/ProperTree)** is recommended. This guide are not responsible if any issues occur. Storage is case sensitive, use the **[official](https://dortania-github-io.thrrip.space/OpenCore-Install-Guide/)** support method if you are not confident trough this process_</div>
> > **Reminder:**<div align="justify"> _This method don't require and depends on **[OpenLinuxBoot](https://github.com/dortania/OpenCore-Multiboot/blob/master/oc/Linux.md)**. Please read apropriately_</div>

</br>

## Method 1: Linux - Using [Gparted](https://gparted.org) or [KDE Partition Manager](https://github.com/KDE/partitionmanager)

**Step 1**<div align="justify">Boot to Linux using possible BIOS key without boot to OpenCore. Use list below as references. If not listed, please **[Google](https://www.google.com)**. Below is common key used for certain motherboard manufacturer:</div>

| **MANUFACTURER** | **BIOS KEY** | **MANUFACTURER** | **BIOS KEY** |
| ---------------- | ------------ | ---------------- | ------------ |
| ASUS             | F8           | Intel            | F10          |
| Gigabyte         | F12          | Asrock           | F11          |
| MSI              | F11          | EVGA             | F7           |
> > **Remark:**<div align="justify">BIOS key in the table above is not entirely correct. The key is depending on how the manufacturer designs the board. Please pay attention. This is basic knowledge to understand how your motherboard works. It is better to refer to any source, especially your motherboard manufacturer, for a better understanding</div>

**Step 2**<div align="justify">Manually edit config.plist with **[Xplist](https://github.com/ic005k/Xplist)** or **[ProperTree](https://github.com/corpnewt/ProperTree)**.</div>

**Step 3**<div align="justify">In config.plist, look for **Misc** > **Security** > **ScanPolicy**</div>

**Step 4**<div align="justify">Change the value of **ScanPolicy** to **2690819** or enable **OC_SCAN_ALLOW_FS_ESP**. Save the config.plist and reboot</div>

![via Linux EFI Rename](https://user-images.githubusercontent.com/72515939/153618855-3c59d86a-8c92-450b-bd15-33c8ef2a3566.png)

**Step 5**<div align="justify">Use Linux's built-in partition manager such as **GParted** or **KDE Partition Manager** to find Linux EFI. `/dev/nvme1np1`will be used as an example. Rename the Linux EFI partition, which is labelled as **NO NAME**, to whatever name you want, for example: **Arch**</div>

![153619493c30aa29b4acf4994ae441a96400ebb80](https://user-images.githubusercontent.com/72515939/153631618-711a7791-ac0e-46af-8bf7-52aeb198498f.png)

**Step 6**<div align="justify">Reboot PC</div>

</br>

## Method 2: Linux Terminal - Using [fdisk](https://github.com/FDOS/fdisk)

**Step 1**<div align="justify">As in **Method 1**, use the terminal command `sudo fdisk-l` to locate the Linux EFI partition. For example, `/dev/nvme1np1`</div>

> Output:

```zsh
Disk /dev/nvme1n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: KINGSTON SA2000M8500G                   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 945B20C6-EB46-234C-A4DC-A70A2985D995

Device             Start       End   Sectors   Size Type
/dev/nvme1n1p1      4096    618495    614400   300M EFI System       
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
/dev/nvme0n1p1     40    409639    409600   200M EFI System          
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

**Step 2**<div align="justify">Type `sudo` to get root access. Use the `fatlabel` command to label the Linux EFI partition. For example, `sudo fatlabel/dev/nvme1np1 Arch`</div>

**Step 3**<div align="justify">Press Enter or Return. Then, reboot and boot back to OpenCore. Arch EFI Partition is now visible in the OpenCore boot menu.</div>

</br>

## Method 3: Using [DiskGenius](https://www.diskgenius.com/) - Windows Only

**Step 1**<div align="justify">Boot to **Windows**</div>

**Step 2**<div align="justify">Download **[DiskGenius](https://www.diskgenius.com/)**</div>

**Step 3**<div align="justify">Launch **[DiskGenius](https://www.diskgenius.com/)**, select Linux EFI partition, extract, and delete the current OpenCore **EFI** bootloader on the Desktop within the EFI partition</div>

> 3.1 **Copy** to Desktop

![AUX10](https://user-images.githubusercontent.com/72515939/167094342-1f1d8391-1a11-4120-95af-040e481eb297.gif)

> 3.2 Delete the existing OpenCore EFI bootloader from the partition

![AUX102](https://user-images.githubusercontent.com/72515939/167094347-c4edf9c6-1e1b-4e1f-9132-c200e8675adf.gif)


**Step 4**<div align="justify">Edit **config.plist**, change the value of **ScanPolicy** to **2690819** by using **[Xplist](https://github.com/ic005k/Xplist)** or **[ProperTree](https://github.com/corpnewt/ProperTree)**</div>

**Step 5**<div align="justify">After editing, save **config.plist**. **Move/Drag** back edited copy from Desktop to EFI partition</div>

> 5.1 **Move/Drag** the EFI**

![AUX103](https://user-images.githubusercontent.com/72515939/167094354-c37ed351-2ff0-4f90-928d-44e9d6a1fb56.gif)


**Step 6**<div align="justify">Return to **[DiskGenius](https://www.diskgenius.com/)**, choose Linux EFI, then right-click and choose **Select Volume Name**. Rename Normal Label : **NO NAME** to **Arch**</div>

> 6.1 Before

<p align="center"><img width="275" alt="Screenshot 2022-04-12 231022" src="https://user-images.githubusercontent.com/72515939/162994786-b12b599f-0d68-42bb-9c90-2766687c7eb1.png"></p>

> 6.2 After

<p align="center"><img width="276" alt="Screenshot 2022-04-12 231907" src="https://user-images.githubusercontent.com/72515939/162996386-5ef4d51d-af0b-4d33-844c-c8538c55e2a7.png"></p>

**Step 7**<div align="justify">Close the application, and reboot. **Arch** EFI partition now visible on OpenCore boot menu.</div>

</br>

## ACKNOWLEGEMENT

I would like to **thanks** all folks in Hackintosh Community especially:

- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) 😍 a great guide
- [corpNewt](https://github.com/corpnewt) 😁 developing [ProperTree](https://github.com/corpnewt/ProperTree) 
- [Hackintosh Malaysia](https://www.facebook.com/groups/HackintoshMalaysia/about/) 😉 an official [Facebook](https://www.facebook.com) community for Hackintosh
- [r/Hackintosh](https://www.reddit.com/r/hackintosh/) 😘 my favourite [reddit](https://www.reddit.com) Hackintosh discussion platform
- [ic005k](https://github.com/ic005k) 😗 develop [Xplist](https://github.com/ic005k/Xplist)

</br>

## FOLLOW ME

- [reddit](https://www.reddit.com) ⭐ [u/mohdismailmatasin](https://www.reddit.com/user/mohdismailmatasin)

