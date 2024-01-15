# ftrace
`ftrace(function trace)`通过`gcc -pg`插桩技术在所有函数的入口处添加一段代码，内核通过重载这段代码来实现具体的trace功能，以实现对内核信息的追踪。

## 原理解析
### gcc -pg
gcc -pg选项会为所有的汇编代码函数入口处添加一个名字类似`mcount`的函数，不同的工具链会使用不同的函数名。大致可以采取以下方法获得：
```bash
# x86_64
echo 'main(){}' | gcc -x c -S -o - - -pg | grep mcount
```
```S
<stdin>:1:1: warning: return type defaults to ‘int’ [-Wimplicit-int]
1:      call    *mcount@GOTPCREL(%rip)
```

gcc对于`mcount`是有具体实现的，但是linux内核不会链接libc库，因此`ftrace`需要自己去实现`mcount`占位(stub)函数。内核对于`mcount`这个占位函数的替换时间点的选择，产生了两种方式：

[静态插桩](#静态插桩)和[动态插桩](#动态插桩)。相较动态而言，静态插桩内核编译产物更大。

### 内核插桩流程


### 静态插桩
所谓静态插桩，是指在编译而非内核运行时，将`mcount`替换为`ftrace`的实现。

### 动态插桩

## 参考
* https://blog.linuxplumbersconf.org/2014/ocw/system/presentations/1773/original/ftrace-kernel-hooks-2014.pdf
* https://richardweiyang-2.gitbook.io/kernel-exploring/00-index-304-ftrace_internal
* https://www.cnblogs.com/pwl999/p/15535032.html
* https://cloud.tencent.com/developer/article/1878789