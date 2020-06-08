# capcomDriverEOP

EOP proof-of-concept exploit for the Capcom.sys "rootkit" driver, where it has an IOCTL that takes a usermode buffer + disables SMEP for you

#### Things to keep in mind:

1. For a 32bit system, the input and output buffers for DeviceIoControl both need to be 4
2. For a 64bit system, the input buffer for DeviceIoControl needs to be 8 and the output buffer needs to be 4
3. The function that provides the IOCTL, 0AA012044h is for 32bit, 0AA013044h for 64bit
4. The device name is `Htsysm72FB`, you can get this from devicetree 

The function that gives both vulnerable IOCTLs

```
mov     r11d, 0AA012044h
mov     eax, ecx
mov     esi, ecx
cmp     edx, r11d
mov     r10d, 0AA013044h
jz      short loc_105EC
```

The function that disables SMEP with `mov cr4, rax`

```
sub_107A0 proc near
mov     rax, [rcx]
mov     cr4, rax
sti
retn
sub_107A0 endp
```

