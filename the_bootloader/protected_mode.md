# Protected Mode

Protected Mode is a more advanced CPU mode, allowing for 32-bit addresses, and more complex segmenting. This enchanced segmenting allows for memory protection, hence the name *Protected Mode*.

## The GDT (*G*lobal *D*escriptor *T*able)

The Global Descriptor Table is what allows for the more advanced segmenting and memory protection. The GDT is a table of segment descriptors. Each segment descriptor is an 8 byte structure, that describes a segment's base (begenning), limit (size), and premissions. This is the layout of bytes in a segment descriptor. The Flags and Limit bits 16-19 share one byte, each being four bits in size.

```
Limit 0-7
Limit 8-15
Base 0-7
Base 7-15
Base 16-23
Access Byte
Flags & Limit 16-19
Base 24-31
```

We will make a very basic GDT in our bootloader to be able to enter protected mode, then replace it with a more advanced one, once our kernel is booted.
