# The Bootloader

This is the most important part. It is what gets read and executed when the computer starts up. And, the most important part of the most important part is the number `0xaa55`, which must appear as the `511th` and `512th` bytes in the boot sector.

The goal for our bootloader is to setup a 64-bit enviroment, load the kernel from disk, and jump to the kernel, however not necessarily in that order.

## The Boot Sector

A Hard Drive is laid out in sectors, each one made up of 512 bytes. The first one is called the boot sector because it gets executed to boot the computer, and must end in `0xaa55`, in order to seperate the disk its on from disks that are not bootable.

# A Basic Boot Sector

```
jmp $ ; Just Hang

times 510 - ($ - $$) db 0

dw 0xaa55
```

Compile it with `nasm -f bin ./bootsector.s -o bootsector`. This will create a binary that can be run with qemu, like this `qemu-system-x86_64 ./bootsector`.

Not very exciting, right? Now try removing the last line, `dw 0xaa55`. Recompile and run. Now it won't even boot. That's because you need `0xaa55` at the end.
