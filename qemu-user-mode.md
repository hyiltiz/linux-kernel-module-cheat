# QEMU user mode

This has nothing to do with the Linux kernel, but it is cool:

    sudo apt-get install qemu-user
    ./build -a arm
    cd buildroot/output.arm~/target
    qemu-arm -L . bin/ls

This uses QEMU's user-mode emulation mode that allows us to run cross-compiled userland programs directly on the host.

The reason this is cool, is that `ls` is not statically compiled, but since we have the Buildroot image, we are still able to find the shared linker and the shared library at the given path.

In other words, much cooler than:

    arm-linux-gnueabi-gcc -o hello -static hello.c
    qemu-arm hello

It is also possible to compile QEMU user mode from source with `BR2_PACKAGE_HOST_QEMU_LINUX_USER_MODE=y`, but then your compilation will likely fail with:

    package/qemu/qemu.mk:110: *** "Refusing to build qemu-user: target Linux version newer than host's.".  Stop.

since we are using a bleeding edge kernel, which is a sanity check in the Buildroot QEMU package.

Anyways, this warns us that the userland emulation will likely not be reliable, which is good to know. TODO: where is it documented the host kernel must be as new as the target one?

GDB step debugging is also possible with:

    qemu-arm -g 1234 -L . bin/ls
    ../host/usr/bin/arm-buildroot-linux-uclibcgnueabi-gdb -ex 'target remote localhost:1234'

TODO: find source. Lazy now.
