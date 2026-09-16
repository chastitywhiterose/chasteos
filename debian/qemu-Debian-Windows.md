First, set the path to where qemu is installed on Windows.

```
set path=C:\Program Files\qemu
```

Next, create a virtual hard disk which will be used to install a Linux distribution. I recommend at least 4 gigabytes for modern Linuxes especially if they include a GUI.

```
qemu-img create harddisk.img 4G
```

Next, boot the Intel 64-bit version of of QEMU with a good distribution. Debian is a solid choice as a first distribution. For example, the following command boots Debian on QEMU while allocating 1 gigabyte for RAM.

```
qemu-system-x86_64 -m 1G -drive file=debian-live-13.7.0-amd64-standard.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
```

```
Core-harddisk:
	qemu-system-x86_64 -drive file=harddisk.img,format=raw,media=disk
Core-cdrom:
	qemu-system-x86_64 -drive file=Core-current.iso,media=cdrom
Core-cdrom-short:
	qemu-system-x86_64 -cdrom Core-current.iso
Core-cdrom_and_harddisk:
	qemu-system-x86_64 -drive file=Core-current.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
harddisk:
	qemu-img create harddisk.img 1G
```