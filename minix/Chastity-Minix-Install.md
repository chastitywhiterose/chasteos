Chastity's Minix 3 Notes

I finally managed to install an operating system inside the QEMU emulator. The OS I tried this time was Minix 3.

The website had a lot of helpful information for each PC emulator.

<https://wiki.minix3.org/doku.php?id=usersguide:runningonqemu>

But before I begin my process of explaining about how I installed Minix, let me explain the reasoning for this.

I did not want to mess up my Debian 12 System which has been used to publish several of my books and manage all my Linux programming repositories which are archived on Github and Gitlab.

Therefore, the safe way was to use an emulator. QEMU works well but the instructions in the install guide I linked to above did not exactly work because my version of QEMU is much newer.

However, with a combination of the guide and help I found on forums online, I was able to get 3 basic commands that work perfectly. The following makefile sums them up.


```
boot:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -drive file=minix.img,format=raw,index=0,media=disk
install:
	qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -cdrom minix.iso -drive file=minix.img,format=raw,index=0,media=disk
disk:
	qemu-img create minix.img 16G
```

Here are the steps that I did in the order I did them.

# Create Hard Disk Image

This command creates a 16 gigabyte file because I believed it would probably be large enough for what I intend to do with this system.

```
qemu-img create minix.img 16G
```

# Boot with Install CD and Disk Image

Next, I booted with both the disk image created with the last command and the CD ISO image I downloaded of the current version.

The exact file was "minix_R3.3.0-588a35b.iso.bz2" and was current when I downloaded it on 4-30-2026.


```
qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -cdrom minix.iso -drive file=minix.img,format=raw,index=0,media=disk
````

I followed the instructions given on the screen to install Minix to the hard disk from the ISO. When it asked me what size I wanted home to be, I chose 4096 megabytes because I planned to download my best git repositories for programming in Minix using C and other languages.

Whenever I am done installing or using Minix, either of the following commands

```
shutdown -h now
```

or

```
poweroff
```

Will turn the emulated PC off so I can exit QEMU safely.

Once Minix was installed, I just booted from the hard disk image. Notice that the command is mostly the same except the ISO image for the CD is not included this time because we don't need it.

# Boot From Hard Disk

```
qemu-system-x86_64 -rtc base=utc -net user -net nic -m 256 -drive file=minix.img,format=raw,index=0,media=disk
```

The next guide from the Minix 3 website has more information on installing more things onto Minix after it is installed.

<https://wiki.minix3.org/doku.php?id=usersguide:postinstallation>

Minix has a package manager with scripts to install tons of packages and it lets you choose which you want during the process. The first command updates the database and the second will start the script.

```
pkgin update
pkgin_sets
```

The sets include most things I wanted. The clang compiler can be installed as well as important things like the vim text editor.

However, I like to use nano. It can be installed like this:

```
pkgin install nano
```

As you see, installing software from the repository is just as easy in Minix as it is in Debian.

The final step I use when installing a now Minix system is to create a regular user account for myself.

This page contains the details for managing user accounts.

<https://wiki.minix3.org/doku.php?id=usersguide:managinguseraccounts>

```
user add -m -g users chastity
passwd chastity
```

With all these steps, I now have a working Minix system. There are obviously more steps that I can do to customize it and turn it into my most reliable OS but I will cover most of them in a separate document.

# Connect with SSH

The post installation instructions tell how to install openssh and set it to automatically start on boot. However, I added the mkdir command to create /etc/rc.d/
because it did not already exist on my system. The following series of commands worked for me.

```
pkgin update
pkgin install openssh
mkdir /etc/rc.d/
cp /usr/pkg/etc/rc.d/sshd /etc/rc.d/
printf 'sshd=YES\n' >> /etc/rc.conf
/etc/rc.d/sshd start
```

After ssh is set up, "poweroff" to shutdown the machine because it needs to be rebooted with a different qemu command. I created a simple makefile rule for this.

```
boot-ssh:
	qemu-system-x86_64 -rtc base=utc -net user,hostfwd=tcp::10022-:22 -net nic -m 1G -drive file=minix.img,format=raw,index=0,media=disk
```

Once Minix is booted this way, it is possible to connect to it from my Debian host OS.

```
ssh chastity@localhost -p10022
```
From this point, I am logged into my user account I created in Minix but I am typing commands into the terminal emulator instead of qemu. This means I can copy paste things which is extremely important for development of C and assembly programs.
