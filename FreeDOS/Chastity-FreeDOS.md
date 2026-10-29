# Moving Linux Files to FreeDOS

It is possible to move any file on a Linux operating system into a mounted virtual image to be used as a floppy disk for FreeDOS.

The emulator I will be using for this example is called QEMU. It just works well for me after I learned the commands to make it work. Here are some links about QEMU.

<https://www.qemu.org/>  
<https://www.qemu.org/docs/master/system/invocation.html>

This article tells how to make a fake floppy disk for transferring files between a Linux host and emulated FreeDOS. It is where I learned this method from.

<https://www.both.org/?p=14441>

In case the original article goes missing, this tutorial I have written will remind me which commands I need to get my files on FreeDOS.

## Create a virtual floppy

```
dd bs=1024 count=1440 if=/dev/zero of=floppy.img
```

## Boot QEMU with the harddisk and floppy

```
qemu-system-x86_64 -m 1G -drive file=floppy.img,format=raw,index=0,if=floppy -drive file=harddisk.img,format=raw,media=disk
```

## Format the floppy inside FreeDOS

```
format A:
```

## Get Linux user id

```
id
```

Usually the uid result is 1000.

# Mount the floppy on Linux

```
mkdir floppy
sudo mount -o loop,uid=1000 floppy.img floppy
```

Unmount the floppy (BEFORE USING IT IN QEMU)

```
sudo umount floppy
```

The main reason I wanted to transfer files to FreeDOS was to get the DOS version of FASM running on FreeDOS.

Because FASM is a 32-bit DOS program that requires extended memory. I have also included cwsdpmi on the floppy because it has worked for me under DOSBox already so well.

The following command must be run after boot, preferably automatically if possible.

```
cwsdpmi -p
```

Then FASM will work fine. This means I can assembly programs inside FreeDOS running under QEMU. This could be used for future tutorials or even a video course on DOS programming.

The actual Assembly programs from the DOS edition of Assembly Arithmetic Algorithms don't require any DPMI program because they are 16 bit real mode .com files which use the 64 kilobyte limit.