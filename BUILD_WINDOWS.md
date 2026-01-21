# Building kptools for Windows

本文档说明如何在 Windows 上编译 kptools。

## 方法1: 使用 GitHub Actions（推荐）

1. Fork 这个仓库到你的 GitHub
2. 进入 Actions 标签页
3. 选择 "Build kptools for Windows" workflow
4. 点击 "Run workflow"
5. 等待编译完成，下载 Artifacts

## 方法2: 本地编译（使用 MSYS2）

### 安装 MSYS2

1. 下载 MSYS2: https://www.msys2.org/
2. 安装到 `C:\msys64`
3. 打开 MSYS2 MINGW64 终端

### 安装依赖

```bash
pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-make mingw-w64-x86_64-zlib make
```

### 编译

```bash
# 复制 preset.h
cp ../kernel/include/preset.h .

# 编译
cd tools
make clean
make CC=gcc
```

### 输出

编译成功后会生成 `kptools.exe` 或 `kptools`。

## 方法3: 使用 WSL

```bash
# 在 WSL 中
cd tools
make clean
make
```

这会生成 Linux 版本的 kptools，但可以在 WSL 中使用。

## 验证

```bash
./kptools --version
./kptools --help
```

## 依赖

- GCC (MinGW64)
- zlib
- make

## 注意事项

1. **kptools 是 x86_64 架构**，用于在 PC 上修补 ARM64 的 boot.img
2. **kpimg 是 ARM64 架构**，是要注入到 Android 内核的代码
3. 编译 kptools 不需要 Android NDK

## 常见问题

### Q: 编译失败，提示找不到 zlib

A: 安装 zlib 开发包：
```bash
pacman -S mingw-w64-x86_64-zlib
```

### Q: 生成的是 kptools 而不是 kptools.exe

A: 这是正常的，MSYS2 环境下可能不会自动添加 .exe 后缀。可以手动重命名：
```bash
mv kptools kptools.exe
```

### Q: 如何获取 kpimg？

A: kpimg 需要从 APatch APK 中提取：
```bash
unzip APatch.apk -d temp
cp temp/assets/kpimg resources/common/binary/
```

或者从 KernelPatch 仓库编译（需要 Android NDK）。
