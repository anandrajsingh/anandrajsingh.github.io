---
layout: post
title: Booting Process!
---
It is procedure of initializing the system.
It consists of everything that happens from when the computer power is switched on till the UI is completely operational.

**PROCESS**
- **Power On**
When the computer is powered on, the Basic Input/Output System (BIOS) initializes the hardware, including the screen and hardware, and tests the main memory. This process is called POST(Power On Self Test).
- **BIOS** (Basic Input/Output System)
The BIOS software is stored on a read-only memory (ROM) chip on the motherboard. After this, the remainder of the boot process is controlled by the Operating System(OS).


- **Mster Boot Record**
Once the POST is completed, system control passes from the BIOS to the **boot loader** 

- **Boot Loader**
The boot loader is usually stored on one of the system's storage devices, such as hard disk or SSD drive, either in the boot sector (for traditional BIOS/MBR systems) or the EFI partition (for more recent (Unified) Extensible Firmware Interface or EFI/UEFI systems). Up to this stage, the machine does not access any mass storage media.
For systems using the **BIOS/MBR** method, the boot loader resides at the first sector of the hard disk, also known as the Master Boot Record(MBR). The size of MBR is just 512 bytes. In this stage the boot loader examines the partition table and finds a bootable partition. Once it finds a bootable partition, it then searches for the second stage boot loader, for example GRUB, and loads it into RAM(Random Access Memory).
For systems using the **EFI/UEFI** method, UEFI firmware reads its Boot Manager data to determin which UEFI application is to be launched and from where. The firmware then launches the UEFI application, for example GRUB, as defined in the boot entry in the firmware's boot manager. This procedure is more complicated but more versatile than the older MBR methods.
The second stage of boot loader reside under **/boot**. A splash screen is displayed, which allows us to which Operating System and/or Kernel we want to boot. After OS and kernel are selected, the boot loader loads the Kernel of the Operating system into RAM and passes control to it. Kernels are almost always compressed, so the first job they have is to uncompress themselves. 

- **Kernel**
After this, it will check and analyze the system hardware and initialize any hardware device drivers built into the kernel.

- **initramfs**
The initramfs filesystem image contains programs and binary file that perform all actions needed to mount the proper root filesystem, including providing the kernel functionality required for the specific filesystem that willbe used, and loading the device drivers for mass storage controllers, by taking advantage of the **udev** system (for user device), which is responsible for figuring out which devices are present, locating the device drivers they need to operate properly, and loading them. After the root filesystem has been found, it is checked for errors and mounted.
- **/sbin/init**
The **mount** program instructs the operating system that a filesystem is ready for use and associates it with a particular point in the overall heirarchy of the filesystem (the **mount point**). If this is successful, the **initramfs** is cleared from RAM, and the **init** program on the root filesystem (**/sbin/init**) is executed.
init handles mounting and pivoting over to the final real root filesystem. If special hardware drivers are needed before the mass storage can be accessed, they must be in the initramfs image.
- **Command Shell**
Near the end of the boot process, **init** starts a number of text-mode login prompts. These enable you to type your username, followed by your password, and to eventually get a command shell. However, if you are running a system with graphical login interface, you will not see these at first.

- **Graphical User Interface**
