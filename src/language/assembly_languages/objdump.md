# objdump
`objdump`用来显示二进制文件（如`elf`）的信息

## 常用参数
下面是对[示例c文件](test.c)的实验。

| 参数      | 说明                                    |
| --------- | --------------------------------------- |
| `-x`      | `-a -f -h -r -t`同时                    |
| [-D](#-D) | 反汇编所有`section`                     |
| [-h](#-h) | 显示目标文件各个`section`的头部摘要信息 |


### -D

<details><summary>-D结果示例</summary>

搜索`<main>`和`<sub>`
```bash
# gcc -c test.c
# objdump -D test.o
test.o:     file format elf64-littleaarch64


Disassembly of section .text:

0000000000000000 <sub>:
   0:   d10083ff        sub     sp, sp, #0x20
   4:   b9000fe0        str     w0, [sp, #12]
   8:   b9000be1        str     w1, [sp, #8]
   c:   b9400fe1        ldr     w1, [sp, #12]
  10:   b9400be0        ldr     w0, [sp, #8]
  14:   4b000020        sub     w0, w1, w0
  18:   b9001fe0        str     w0, [sp, #28]
  1c:   b9401fe0        ldr     w0, [sp, #28]
  20:   910083ff        add     sp, sp, #0x20
  24:   d65f03c0        ret

0000000000000028 <main>:
  28:   a9be7bfd        stp     x29, x30, [sp, #-32]!
  2c:   910003fd        mov     x29, sp
  30:   52800020        mov     w0, #0x1                        // #1
  34:   b9001fe0        str     w0, [sp, #28]
  38:   b9401fe1        ldr     w1, [sp, #28]
  3c:   b9401be0        ldr     w0, [sp, #24]
  40:   94000000        bl      0 <sub>
  44:   b90017e0        str     w0, [sp, #20]
  48:   52800000        mov     w0, #0x0                        // #0
  4c:   a8c27bfd        ldp     x29, x30, [sp], #32
  50:   d65f03c0        ret

Disassembly of section .comment:

0000000000000000 <.comment>:
   0:   43434700        .inst   0x43434700 ; undefined
   4:   4728203a        .inst   0x4728203a ; undefined
   8:   5420554e        b.al    40ab0 <main+0x40a88>
   c:   636c6f6f        .inst   0x636c6f6f ; undefined
  10:   6e696168        rsubhn2 v8.8h, v11.4s, v9.4s
  14:   726f6620        .inst   0x726f6620 ; undefined
  18:   65687420        fnmls   z0.h, p5/m, z1.h, z8.h
  1c:   6d724120        ldp     d0, d16, [x9, #-224]
  20:   63724120        .inst   0x63724120 ; undefined
  24:   65746968        fnmls   z8.h, p2/m, z11.h, z20.h
  28:   72757463        .inst   0x72757463 ; undefined
  2c:   31312065        adds    w5, w3, #0xc48
  30:   322d322e        orr     w14, w17, #0xfff80000
  34:   2e323230        usubw   v16.8h, v17.8h, v18.8b
  38:   28203230        stnp    w16, w12, [x17, #-256]
  3c:   2d6d7261        ldp     s1, s28, [x19, #-152]
  40:   312e3131        adds    w17, w9, #0xb8c
  44:   20292934        .inst   0x20292934 ; undefined
  48:   322e3131        orr     w17, w9, #0x7ffc0000
  4c:   3220312e        orr     w14, w9, #0x1fff
  50:   30323230        adr     x16, 64695 <main+0x6466d>
  54:   00313131        .inst   0x00313131 ; NYI

Disassembly of section .eh_frame:

0000000000000000 <.eh_frame>:
   0:   00000010        udf     #16
   4:   00000000        udf     #0
   8:   00527a01        .inst   0x00527a01 ; undefined
   c:   011e7804        .inst   0x011e7804 ; undefined
  10:   001f0c1b        .inst   0x001f0c1b ; undefined
  14:   00000014        udf     #20
  18:   00000018        udf     #24
  1c:   00000000        udf     #0
  20:   00000028        udf     #40
  24:   200e4100        .inst   0x200e4100 ; undefined
  28:   00000e48        udf     #3656
  2c:   00000020        udf     #32
  30:   00000030        udf     #48
  34:   00000000        udf     #0
  38:   0000002c        udf     #44
  3c:   200e4100        .inst   0x200e4100 ; undefined
  40:   039e049d        .inst   0x039e049d ; undefined
  44:   0eddde49        .inst   0x0eddde49 ; undefined
```
</details>

### -h

<details><summary>-h结果示例</summary>

```bash
# gcc -c test.c
# objdump -h test.o
test.o:     file format elf64-littleaarch64

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         00000054  0000000000000000  0000000000000000  00000040  2**2
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .data         00000000  0000000000000000  0000000000000000  00000094  2**0
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000000  0000000000000000  0000000000000000  00000094  2**0
                  ALLOC
  3 .comment      00000058  0000000000000000  0000000000000000  00000094  2**0
                  CONTENTS, READONLY
  4 .note.GNU-stack 00000000  0000000000000000  0000000000000000  000000ec  2**0
                  CONTENTS, READONLY
  5 .eh_frame     00000050  0000000000000000  0000000000000000  000000f0  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
```
