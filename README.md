How to build Linux image based on qemu_x86_defconfig but with utility locale.
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
qemu-system-i386 -M pc -kernel output/images/bzImage -drive file=output/images/rootfs.ext2,if=virtio,format=raw -append "rootwait root=/dev/vda console=tty1 console=ttyS0" -serial stdio -net nic,model=virtio -net user # qemu_x86_defconfig
```
Optionally add -smp N to emulate a SMP system with N CPUs. The login prompt will appear in the graphical window.
