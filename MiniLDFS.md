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

