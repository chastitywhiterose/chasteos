When running the version check script, I got the following error.

"ERROR: sh   is NOT Bash"

The following commands revealed that sh actually points to dash instead of bash

command: "which sh"
output: /usr/bin/sh

command: "ls -l /usr/bin/sh"
output: lrwxrwxrwx 1 root root 4 Jan  5  2023 /usr/bin/sh -> dash

command: "ls /bin/sh -l"
output: "lrwxrwxrwx 1 root root 4 Jan  5  2023 /bin/sh -> dash"

Based on what I read online, the fix is to replace the link like this:

command: "sudo ln /usr/bin/bash /usr/bin/sh -f"

That command forces sh to point to bash.

fix_source: https://devopstribe.it/2025/08/18/my-journey-in-building-a-gnu-linux-aarch64-arm-system/

After I applied this fix, I was able to run the build script and get it working!

---

OK:    Coreutils 9.1    >= 8.1
OK:    Bash      5.2.15 >= 3.2
OK:    Binutils  2.40   >= 2.13.1
OK:    Bison     3.8.2  >= 2.7
OK:    Diffutils 3.8    >= 2.8.1
OK:    Findutils 4.9.0  >= 4.2.31
OK:    Gawk      5.2.1  >= 4.0.1
OK:    GCC       12.2.0 >= 5.4
OK:    GCC (C++) 12.2.0 >= 5.4
OK:    Grep      3.8    >= 2.5.1a
OK:    Gzip      1.12   >= 1.3.12
OK:    M4        1.4.19 >= 1.4.10
OK:    Make      4.3    >= 4.0
OK:    Patch     2.7.6  >= 2.5.4
OK:    Perl      5.36.0 >= 5.8.8
OK:    Python    3.11.2 >= 3.4
OK:    Sed       4.9    >= 4.1.5
OK:    Tar       1.34   >= 1.22
OK:    Texinfo   6.8    >= 5.0
OK:    Xz        5.4.1  >= 5.0.0
OK:    Linux Kernel 6.1.0 >= 5.4
OK:    Linux Kernel supports UNIX 98 PTY
Aliases:
OK:    awk  is GNU
OK:    yacc is Bison
OK:    sh   is Bash
Compiler check:
OK:    g++ works
OK: nproc reports 8 logical cores are available

---
