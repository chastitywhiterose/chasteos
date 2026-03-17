Chastity's Gentoo Linux USB Media Notes

Commands to see devices

dmesg

lsblk

dmesg and lsblk both identified the device as sdc

To write the image to the disk:

example only:
dd if=install-amd64-minimal-<release timestamp>.iso of=/dev/sdd bs=4096 status=progress && sync


Commands I used:

I installed the livegui to the white 128GB USB drive I bought from Walmart.

dd if=livegui-amd64-20260215T164556Z.iso of=/dev/sdc bs=4096 status=progress && sync

Then I unplugged the white disk and plugged the purple disk.
On the purple disk I copied the minimal install iso.

dd if=install-amd64-minimal-20260215T164556Z.iso of=/dev/sdc bs=4096 status=progress && sync

Then I labeled the disks with a sharpie marker.
