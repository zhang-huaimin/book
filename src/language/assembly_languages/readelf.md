# readelf
`readelf`用来显示`elf`文件的信息

## elf文件
ELF(Executable and Linking Format)作为缺省的二进制文件格式来使用。

三中类型：
1. 可重定位的对象文件(Relocatable file)
由汇编器汇编生成的 .o 文件
2. 可执行的对象文件(Executable file)
可执行应用程序
3. 可被共享的对象文件(Shared object file)
动态库文件，也即 .so 文件

* `.text` `section` 装载了可执行代码
* `.data` `section` 装载了被初始化的数据
* `.bss` `section` 装载了未被初始化的数据
* `.rec` 打头的 `sections` 装载了[重定位](#重定位)信息
* `.symtab` 或者 `.dynsym section` 装载了符号信息
* `.strtab` 或者 `.dynstr section` 装载了字符串信息

## 常用参数
下面是对[示例c文件](test.c)的实验。

| 参数                 | 说明                         |
| -------------------- | ---------------------------- |
| `-a`                 | `-h -l -S -s -r -d -V -A -I` |
| [-r](#重定位)        | 显示elf重定位信息            |
| [-h](#elf文件头信息) | 显示elf文件头信息            |
| [-S](#段头信息)      | 显示elf段头信息              |
| [-s](#符号表)        | 显示elf符号表                |

### 重定位
重定位section保存了重定位相关的信息，这些信息描述了如何在链接或者运行过程中，对ELF目标文件的某部分内容或进程镜像进行补充和修改

* Offset: 重定向信息在二进制文件中的偏移量
* Info: 重定向信息在二进制文件中的信息
* Type: 符号类型信息
* Sym. Value: 符号值

```bash
# gcc -c test.c
# readelf -r test.o
Relocation section '.rela.text' at offset 0x278 contains 1 entry:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
000000000040  000a0000011b R_AARCH64_CALL26  0000000000000000 sub + 0

Relocation section '.rela.eh_frame' at offset 0x290 contains 2 entries:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
00000000001c  000200000105 R_AARCH64_PREL32  0000000000000000 .text + 0
000000000034  000200000105 R_AARCH64_PREL32  0000000000000000 .text + 28

# 计算公式R_AARCH64_CALL26 = S + A - P
S: 要重定位的符号的运行时地址
A: 用于重定位的加数
P: 搬迁地点的地址
R_AARCH64_CALL26 = 0x40 + 0x0 - 0x40 = 0x0
```

### elf文件头信息
* Magic: `7f 45 4c 46`代表`elf`文件
* Class: `ELF64`代表`64`位`elf`文件
* Type: `REL`: 可重定位文件，`EXEC`: 可执行文件

```
# gcc -c test.c
# readelf -h test.o
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              REL (Relocatable file)
  Machine:                           AArch64
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          0 (bytes into file)
  Start of section headers:          800 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           0 (bytes)
  Number of program headers:         0
  Size of section headers:           64 (bytes)
  Number of section headers:         12
  Section header string table index: 11
```

### 段头信息

```
# gcc -c test.c
# readelf -r test.o
There are 12 section headers, starting at offset 0x320:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .text             PROGBITS         0000000000000000  00000040
       0000000000000054  0000000000000000  AX       0     0     4
  [ 2] .rela.text        RELA             0000000000000000  00000278
       0000000000000018  0000000000000018   I       9     1     8
  [ 3] .data             PROGBITS         0000000000000000  00000094
       0000000000000000  0000000000000000  WA       0     0     1
  [ 4] .bss              NOBITS           0000000000000000  00000094
       0000000000000000  0000000000000000  WA       0     0     1
  [ 5] .comment          PROGBITS         0000000000000000  00000094
       0000000000000058  0000000000000001  MS       0     0     1
  [ 6] .note.GNU-stack   PROGBITS         0000000000000000  000000ec
       0000000000000000  0000000000000000           0     0     1
  [ 7] .eh_frame         PROGBITS         0000000000000000  000000f0
       0000000000000050  0000000000000000   A       0     0     8
  [ 8] .rela.eh_frame    RELA             0000000000000000  00000290
       0000000000000030  0000000000000018   I       9     7     8
  [ 9] .symtab           SYMTAB           0000000000000000  00000140
       0000000000000120  0000000000000018          10    10     8
  [10] .strtab           STRTAB           0000000000000000  00000260
       0000000000000017  0000000000000000           0     0     1
  [11] .shstrtab         STRTAB           0000000000000000  000002c0
       0000000000000059  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  D (mbind), p (processor specific)
```

### 符号表

```bash
Symbol table '.symtab' contains 12 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS test.c
     2: 0000000000000000     0 SECTION LOCAL  DEFAULT    1 .text
     3: 0000000000000000     0 SECTION LOCAL  DEFAULT    3 .data
     4: 0000000000000000     0 SECTION LOCAL  DEFAULT    4 .bss
     5: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT    1 $x
     6: 0000000000000000     0 SECTION LOCAL  DEFAULT    6 .note.GNU-stack
     7: 0000000000000014     0 NOTYPE  LOCAL  DEFAULT    7 $d
     8: 0000000000000000     0 SECTION LOCAL  DEFAULT    7 .eh_frame
     9: 0000000000000000     0 SECTION LOCAL  DEFAULT    5 .comment
    10: 0000000000000000    40 FUNC    GLOBAL DEFAULT    1 sub
    11: 0000000000000028    44 FUNC    GLOBAL DEFAULT    1 main
```

## 附录
* [arm64 elf](https://docslib.org/doc/4448214/elf-for-the-arm-64-bit-architecture-aarch64#pfd)