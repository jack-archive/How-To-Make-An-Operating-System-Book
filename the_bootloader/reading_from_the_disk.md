# Reading From The Disk

At some point we will want to run our kernel, which means that we will want to load it from disk. Because BIOS calls can only be made in Real Mode, its better to do this now, rather than later, because we have access to the BIOS's handy routines for doing it.

The most complicated thing about doing this is the strange way that we tell the BIOS what to read. This notation is called *CHS*, which stands for **C**ylinder **H**ead **S**ector notation. However, for simplicity, and since we know that our kernel is just after the boot sector, we will use Cylinder 0, Head 0 and Sector 2 (One to pass the boot sector, and one because Sectors are not zero-based). The way we tell the BIOS what and where we want to read, is just like with printing strings, we set certain registers, then throw an interupt.

First we need to tell the BIOS that we want to read a specific number of sectors. So we set `ah` to `0x02`.

```
mov ah, 0x02
```

We need to specify the drive number that we want to read from. We do this by setting the register `dh`, However the BIOS loads the number of the drive it booted into `dh` at boot, so we don't need to touch anything.

To tell the BIOS that where we 2 sectors, we would load `2` into the `al` register.

```
mov al, 2
```

To read from cylinder `0` and head `0`, we set the `ch` and `dh` registers, respectivly.

```
mov ch, 0
mov dh, 0
```

To start reading at the second sector (sectors are 1 based). We set the `cl` register to `2`.

```
mov cl, 2
```

To have the BIOS load what it reads into memory starting at `0x1000`, we set the segment and address registers `es` and `bx`. Telling the BIOS to load our data into the memory at `es`:`bx`. However you cannot set set `es` directly, so we will use `bx` as a temporary loacation before setting it to its actual value.

```
mov bx, 0x0
mov es, bx
mov bx, 0x1000
```

Then we interupt the BIOS using the interupt `0x13`.

```
int 0x13
```

If all went well, there should be the sectors we specified starting at the address `es`:`bx`. We can check for errors by checking the carry flag in the flags register. The carry flag is used to mean General Fault. There is even a special jump instruction, `jc`, that jumps if the carry flag is set.

```
jc disk_error
```

All put together it should look like this:

```
read_disk_16:
    mov ah, 0x02

    mov al, 2
    mov ch, 0
    mov dh, 0
    mov cl, 2

    mov bx, 0x0
    mov es, bx
    mov bx, 0x1000

    int 0x13

    jc .error

.error:
    push bx

    mov bx, disk_error_msg
    call print_string_16

    pop bx
 .msg: db 'Error Reading Disk', 0

```
