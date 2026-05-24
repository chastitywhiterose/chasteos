It turns out that Tiny Core Linux is trivially easy to boot with the QEMU emulator. I adapted some commands I formerly used with Minix to set myself up a basic makefile. I can create a hard disk or booth into one of the 3 available ISO files.

```
Core:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom Core-current.iso -drive file=disk.img,format=raw,index=0,media=disk
TinyCore:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom TinyCore-current.iso -drive file=disk.img,format=raw,index=0,media=disk
Core:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom CorePlus-current.iso -drive file=disk.img,format=raw,index=0,media=disk

disk:
	qemu-img create disk.img 1G
```

The purpose of the Tiny Core Linux experiment is to train myself on a Linux with only the command line. I will learn more and turn it into the ultimate programming OS. This could be the foundation for the ChasteOS project I have been floating around in my head.
