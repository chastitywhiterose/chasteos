Chastity's Minimal Linux Distro From Scratch

The following steps are the slightly modified steps I got from a Kindle book for creating a small Linux distribution and getting it read to run in a virtual machine like QEMU.

I highly recommend reading the book yourself because the author explains the reasoning for all these steps.

<https://www.amazon.com/Minimal-Linux-Distro-Scratch-Creating-ebook/dp/B0F1TWZBCY/>

I found it helpful because I just want to learn about how a Linux system is assembled because I am already happy with Debian on my PC and don't plan to change it as my primary operating system. However, I would like to distribute a toy playground for the programs I write. If I can take my Linux C or Assembly programs and put them on a bootable disk, it means running them on any PC without having to have an operating system already installed. In short, I wish to create a distro like Puppy Linux, but even smaller!

# Installing the Prerequisites

sudo apt install bc cpio bison libssl-dev libncurses-dev libelf-dev bzip2 make

sudo apt install automake autoconf git syslinux dosfstools xz-utils build-essential gcc wget

# Creating Your Workspace

sudo mkdir /distro
cd /distro

# Downloading the Linux Kernel

I had already downloaded the specific version of the Linux kernel for version 13 of the Linux From Scratch book. I will reuse it for this project by first opening a terminal where it was and then copying it to the /distro folder.

sudo cp linux-6.18.10.tar.xz /distro

# Extracting and compiling the kernel

sudo tar -xvf linux-6.18.10.tar.xz

cd linux-6.18.10

sudo make menuconfig

sudo make -j$(nproc)

# Verifying the Output

*Once the compilation is complete, the kernel binary—commonly
known as the bzImage will be generated. You should see a success
message like this:`bzImage successfully generated.`*

Let’s copy this kernel image into your working directory for later use:

sudo cp arch/x86/boot/bzImage /distro/vmlinuz

By renaming the output to vmlinuz, you create a standardized
naming convention, which simplifies further steps in the project.

# Downloading BusyBox

Begin by cloning the BusyBox repository. This will provide you with
the latest version of BusyBox:

sudo git clone --depth 1 https://git.busybox.net/busybox

Once cloned, navigate into the BusyBox directory:

cd busybox

# Configuring BusyBox

sudo make menuconfig

Before proceeding, there's an important tweak to make. Edit the
`.config` file to disable an unnecessary component that might
interfere with your build:

sudo vim .config

Locate the line:

CONFIG_TC=y
Change it to:
CONFIG_TC=n
Save and close the file. This modification ensures smooth
compilation.

Finally, build Busybox.

sudo make -j$(nproc)

# Installing BusyBox into Initramfs

Create a directory structure for your initial RAM filesystem
(initramfs):

sudo mkdir /distro/initramfs
sudo make CONFIG_PREFIX=/distro/initramfs install

This command installs all the BusyBox components into the
`initramfs` folder, which will later be packaged into an initramfs
image.

# Configuring the Boot process

cd /distro/initramfs
sudo rm linuxrc

## Create the init Script

sudo vim init

Write the following into the file.

```
#!/bin/sh
/bin/sh
```

Save and then give execute permissions to the init script.

sudo chmod +x init

While still in the initramfs directory, run this to package everything into the file the kernel will load.

sudo find . | cpio -o -H newc > ../initramfs.cpio

Note: When I tried this, I got a permission error message. So I had to

sudo su

To be logged in as root and try it again. Strange but it worked.

# Creating an Empty Disk Image

Go back to the /distro directory.

First, we’ll create a blank disk image to hold the operating system:

sudo dd if=/dev/zero of=OS bs=1M count=50

This command generates a 50 MB file filled with zeros to act as your
operating system's virtual disk.

## Formatting the Disk Image

Next, format the image with a FAT filesystem to make it usable:

sudo mkfs -t fat OS

This step prepares the disk image to store your kernel and initramfs.

## Mounting the Disk Image

Mount the disk image to a temporary directory to allow file operations:

sudo mkdir /distro/tmp
sudo mount OS /distro/tmp

This will give you access to the image at `/distro/tmp`.

## Copying Files to the Disk Image

Now, copy the necessary files—`vmlinuz` (the kernel) and
`initramfs.cpio` (the initramfs image)—into the disk image:

sudo cp /distro/vmlinuz /distro/tmp/vmlinuz
sudo cp /distro/initramfs.cpio /distro/tmp/initramfs.cpio

These files are essential for booting your system.

# Creating a Syslinux Configuration File

To enable booting, you’ll need a configuration file for the Syslinux
bootloader. Create the file in the mounted directory:

sudo vim /distro/tmp/syslinux.cfg

Add the following content to the file:

```
PROMPT 0
TIMEOUT 50
DEFAULT MyOS
LABEL MyOS
MENU LABEL MyOS
KERNEL /vmlinuz
INITRD /initramfs.cpio
```

This configuration ensures that the bootloader automatically loads
the kernel and initramfs with no manual intervention.

# Installing the Bootloader

After setting up the configuration, unmount the disk image and
install the Syslinux bootloader:

sudo umount /distro/tmp
sudo syslinux OS

Syslinux writes the bootloader to the disk image, making it bootable.

# Testing the Bootable Image

Finally, test your bootable Linux distribution using QEMU, a virtual
machine emulator:

qemu-system-x86_64 OS

If everything is set up correctly, your Linux system will boot and
present you with the shell provided by BusyBox. Congratulations—
you now have a working, minimal Linux distribution!

