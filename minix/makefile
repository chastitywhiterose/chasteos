boot:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -drive file=minix.img,format=raw,index=0,media=disk
install:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -cdrom minix.iso -drive file=minix.img,format=raw,index=0,media=disk
disk:
	qemu-img create minix.img 8G
