# Printing Text

In order to print text the BIOS needs us to tell it what, and how, to print. We do this by setting a few registers.

```
mov ah, 0x0e
mov al, 'H'
```

Moving `0x0e` to `ah` tells the BIOS that we want it to print in scrolling teletype mode, and moving `'H'` into `al` tells the BIOS to print `'H'`.

Now we have to acturally tell the BIOS that we want to do a print. We do this by issuing an interupt, using the `int` instruction. The specific interupt we want is interupt `0x10`, so the final instruction is:

```
int 0x10
```

Put it all together, along with our existing code from the basic boot sector and it should look like this:

```
mov ah, 0x0e
mov al, 'H'
int 0x10

jmp $

times 510 - ($ - $$) db 0
dw 0xaa55

```

Compile and run it:

```
nasm -f bin ./bootsector.s -o bootsector && qemu-system-x86_64 ./bootsector
```

Now lets make it into a routing to print a string, refrenced `bx`

First, be nice and push all the registers onto the stack.

```
pusha
```

Then Set `ah` to `0x02` for teletype mode.

```
mov ah, 0x02
```

And define a `loop` sublabel

```
 .loop:
```

We need to load each char in sequence into al. The item stored in `bx` is a pointer to the first char in the string. So we can do pointer arithmitic and iterate through our string.

```
mov al, [bx]
add bx, 1
```

We assume null terminated strings, so we can check if the end of the string is here by comparing `al` to `0`. Then jumping to a `done` sublabel if `al` is `0`.

```
cmp al, 0
je .done
```

The `done` sublabel should pop the registers back off the stack, and return to the caller.

```
 .done:
    popa
    ret
```

If `al` was not `0`, we should throw interupt `0x10` and loop pack around.

```
int 0x10
jmp .loop
```

Putting it all together we come up with this code:

```
print_string_16:
    pusha
    mov ah, 0x02
 .loop:
    mov al, [bx]
    add bx, 1

    cmp al, 0
    je .done

    int 0x10

 .done:
    popa
    ret
```
