# compile
归纳直接编译内核的方法

## 替换以下变量
| 变量               | 说明                                                         | 例子                                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `${CROSS_COMPILE}` | 交叉编译工具链                                               | [gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-](https://mirrors.tuna.tsinghua.edu.cn/armbian-releases/_toolchain/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz) |
| `${arch}`          | 目标架构                                                     | `arm64`                                                                                                                                                                                                      |
| `${out_dir}`       | 内核编译输出目录。如果省略，则编译输出目录等价于内核源码目录 | ../build/                                                                                                                                                                                                    |

## 编译环境
* ubuntu组件
```bash,editable
apt-get install -y make flex libncurses-dev libssl-dev bison
```
### clangd配置（可省略）
* ubuntu组件
```bash,editable
# clangd >= 12
apt-get install -y bear clangd-12
ln -s /usr/bin/clangd-12 /usr/bin/clangd
```
* 内核源码路径下创建`.clangd`
```
CompileFlags:
    Remove: -mabi=lp64
```
* `vscode`
1. 安装插件`clangd`
2. 创建`setting.json`
`.vscode/setting.json`
```json,editable
{
    "clangd.arguments": [
        "--all-scopes-completion",
        "--query-driver=${CROSS_COMPILE}*",
        "--compile-commands-dir="
    ]
}
```
3. [编译内核](#编译命令)
```
bear make ... all
```
4. vscode -> ctrl + shift + p -> clangd: restart

## 编译命令
```bash,editable
# 创建输出目录
mkdir -p ${out_dir}
cp arch/${arch}/configs/defconfig ${out_dir}/.config
# 配置内核
make ARCH=${arch} CROSS_COMPILE=${CROSS_COMPILE} O=${out_dir} menuconfig
# 编译内核
make ARCH=${arch} CROSS_COMPILE=${CROSS_COMPILE} O=${out_dir} all
```

