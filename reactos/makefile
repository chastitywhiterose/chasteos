boot-harddisk:
	qemu-system-x86_64 -drive file=harddisk.img,format=raw,media=disk
react-live-cdrom:
	qemu-system-x86_64 -drive file=ReactOS-0.4.15-x86-live.iso,media=cdrom
react-cdrom_and_harddisk:
	qemu-system-x86_64 -drive file=ReactOS-0.4.15-release-x86.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
harddisk:
	qemu-img create harddisk.img 1G
