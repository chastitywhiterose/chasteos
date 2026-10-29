dos-harddisk:
	qemu-system-x86_64 -m 1G -drive file=harddisk.img,format=raw,media=disk
Core-cdrom:
	qemu-system-x86_64 -m 1G -drive file=FD14LIVE.iso,media=cdrom
Core-cdrom_and_harddisk:
	qemu-system-x86_64 -m 1G -drive file=FD14LIVE.iso,media=cdrom -drive file=harddisk.img,format=raw,media=disk
dos-floppy:
	qemu-system-x86_64 -m 1G -drive file=floppy.img,format=raw,index=0,if=floppy
dos-floppy_and_harddisk:
	qemu-system-x86_64 -m 1G -drive file=floppy.img,format=raw,index=0,if=floppy -drive file=harddisk.img,format=raw,media=disk
dos-floppy-short:
	qemu-system-x86_64 -fda x86BOOT.img
Core-cdrom-short:
	qemu-system-x86_64 -cdrom Core-current.iso
harddisk:
	qemu-img create harddisk.img 1G -f raw

