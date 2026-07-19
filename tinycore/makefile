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
