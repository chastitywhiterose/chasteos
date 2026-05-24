Core:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom Core-current.iso -drive file=disk.img,format=raw,index=0,media=disk
TinyCore:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom TinyCore-current.iso -drive file=disk.img,format=raw,index=0,media=disk
CorePlus:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 1G -cdrom CorePlus-current.iso -drive file=disk.img,format=raw,index=0,media=disk

disk:
	qemu-img create disk.img 1G
