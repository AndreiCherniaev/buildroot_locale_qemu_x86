How to build Linux image based on qemu_x86_defconfig but with utility [locale](https://man7.org/linux/man-pages/man1/locale.1.html).
## Compare with qemu_x86_defconfig
Config [my_qemu_x86_defconfig](my_external_tree/configs/my_qemu_x86_defconfig) is the same as [qemu_x86_defconfig](buildroot/configs/qemu_x86_defconfig) except
```
BR2_GENERATE_LOCALE="en_US"
BR2_SYSTEM_ENABLE_NLS=y
BR2_PACKAGE_GLIBC_UTILS=y
```
BR2_ENABLE_LOCALE_WHITELIST has default value C, en_US

## Clone
```
git clone --remote-submodules --recurse-submodules -j8 https://github.com/AndreiCherniaev/buildroot_cpio_i386_bios.git
cd buildroot_cpio_i386_bios/
```
## Make image
```
make clean -C buildroot
make BR2_EXTERNAL=$PWD/my_external_tree -C $PWD/buildroot my_qemu_x86_defconfig
make -C buildroot
```
## Save non-default buildroot .config
To save non-default buildroot's buildroot/.config to $PWD/my_external_tree/configs/my_qemu_x86_defconfig
```
make -C $PWD/buildroot savedefconfig BR2_DEFCONFIG=$PWD/my_external_tree/configs/my_qemu_x86_defconfig
```
## Start in QEMU
Run the emulation with:
```
cd buildroot
qemu-system-i386 -M pc -kernel output/images/bzImage -drive file=output/images/rootfs.ext2,if=virtio,format=raw -append "rootwait root=/dev/vda console=tty1 console=ttyS0" -serial stdio -net nic,model=virtio -net user # qemu_x86_defconfig
```
Optionally add -smp N to emulate a SMP system with N CPUs. The login prompt will appear in the graphical window.
## locale
```
# locale
LANG=
LC_CTYPE="POSIX"
LC_NUMERIC="POSIX"
LC_TIME="POSIX"
LC_COLLATE="POSIX"
LC_MONETARY="POSIX"
LC_MESSAGES="POSIX"
LC_PAPER="POSIX"
LC_NAME="POSIX"
LC_ADDRESS="POSIX"
LC_TELEPHONE="POSIX"
LC_MEASUREMENT="POSIX"
LC_IDENTIFICATION="POSIX"
LC_ALL=
# locale -a
C
POSIX
en_US
en_US.utf8
```
