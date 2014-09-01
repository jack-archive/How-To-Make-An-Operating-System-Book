
#Real Mode

Real Mode is where the mode where the CPU first starts out, for compatability reasons. Real Mode uses 16-bit addresses which can access less than 1 Mb of RAM. In order to access more RAM there was the invention of segment registers. These are registers that can be used to access more memory using the notation `Segment-Register:Offset`. Then the value in the segment register gets multiplied by `16` and added to `Offset`. Increasing the amount of RAM able to be accessed.

Real Mode is also the only place where BIOS calls can be made.

What we want our bootloader to do in Real Mode is to load the kernel from disk, then setup a Protected Mode enviroment, and jump into Protected Mode.
