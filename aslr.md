# use /proc/sys/kernel/randomize_va_space
[-] 0 – No randomization. Everything is static.
[-] 1 – Conservative randomization. Shared libraries, stack, mmap(), VDSO and heap are randomized.
[-] 2 – Full randomization. In addition to elements listed in the previous point, memory managed through brk() is also randomized.

According to an article How Effective is ASLR on Linux Systems?, you can configure ASLR in Linux using the /proc/sys/kernel/randomize_va_space interface.

The following values are supported:

0 – No randomization. Everything is static.
1 – Conservative randomization. Shared libraries, stack, mmap(), VDSO and heap are randomized.
2 – Full randomization. In addition to elements listed in the previous point, memory managed through brk() is also randomized.
So, to disable it, run

echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
and to enable it again, run

echo 2 | sudo tee /proc/sys/kernel/randomize_va_space
This won't survive a reboot, so you'll have to configure this in sysctl. Add a file /etc/sysctl.d/01-disable-aslr.conf containing:

kernel.randomize_va_space = 0
should permanently disable this.
