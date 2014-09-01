# The Stack

The Stack is a [Stack](http://en.wikipedia.org/wiki/Stack_%28abstract_data_type%29), hence the name, which is a LIFO Data Structure (**L**ast **I**n **F**irst **O**ut). There are two instructions that deal with the stack `push` and `pop`. And they push registers, constants, and memory, and pop to registers and memory, respectively.

```
mov ax, 10
push ax
```

The stack now contains `10` at the top, and `ax` still contains `10`. We can pop that `10` off into a register like so.

```
pop bx
```

Now `bx` contains `10`, and `10` was removed from the stack. There are also 2 other instructions that are helpful for writing functions. `pusha` pushes all the registers on to the stack, and `popa` pops all the registers back off in the order that they were pushed on to the stack by `pusha`. These are useful for saving the state of the calling function. Remember that the registers retain all of their values so pushing the registers on to the stack retains argument values in the registers.

```
my_func:
    pusha
    ; do stuff
    popa
    ret
```
