The emulator I will be using for this example is called QEMU.

<https://www.qemu.org/>  
<https://www.qemu.org/docs/master/system/invocation.html>

This article tells how to make a fake floppy disk for transferring files between a Linux host and emulated FreeDOS.

<https://www.both.org/?p=14441>

# Create a virtual floppy

```
dd bs=1024 count=1440 if=/dev/zero of=floppy.img
```

# Boot QEMU with the harddisk and floppy

```
qemu-system-x86_64 -m 1G -drive file=floppy.img,format=raw,index=0,if=floppy -drive file=harddisk.img,format=raw,media=disk
```

# Format the floppy inside FreeDOS

```
format A:
```

# Get Linux user id

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
