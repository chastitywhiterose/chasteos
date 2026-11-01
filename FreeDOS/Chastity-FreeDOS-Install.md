# Chastity's Guide to Installing FreeDOS.

This step by step guide assumes QEMU is already installed.

First create a 2 Gigabyte hard disk we will use to install FreeDOS on in the QEMU virtual machine.

```
qemu-img create harddisk.img 2G -f raw
```

Boot into QEMU with the installation CD ISO and the hard disk.

```
qemu-system-x86_64 -m 1G -drive file=FD14LIVE.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
```

Follow the easy instructions FreeDOS provides to do the installation of your choice.

After the installation, boot with the hard disk and the bonus CD.

```
qemu-system-x86_64 -m 1G -drive file=FD14BNS.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
```

Inside FreeDOS, enter the command:

```
fdimples
```

The bonus CD will load and you can install C compilers and the FASM and NASM Assemblers from the development section.

With these tools, it becomes possible to assemble or compile all command line tools from my book: Assembly Arithmetic Algorithms for DOS. In fact, FASM is all you technically need.