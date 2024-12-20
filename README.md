
如何将.NET 应用程序发布到鸿蒙上，肯定是很多人感兴趣的话题，**目前.NET完全具备可以在OpenHarmony系统上运行的能力，**.NET 现在有很多选项CoreCLR、Mono和NativeAOT。由于OpenHarmony的沙箱环境的限制，NativeAOT是最佳选择。孙策同学经过几个月的探索，他2024年12月14日在上海举办的.NET Conf China 2024 大会上和大家分享他的探索经验和成果，OpenHarmony作为一个开源的操作系统，本身就具有强大的兼容性和扩展性。而Avalonia则是一个跨平台的UI框架，能够在不同的操作系统上提供一致的用户体验。将这两者结合起来，简直就是强强联手！9月份写的这篇文章《.[NET 的 Native AOT 现在是什么样的？》](https://github.com)里已经有跨平台交叉编译NativeAOT的答案：使用 Zig 作为链接器和 sysroot，允许从 Windows 机器交叉编译到 Linux\-x64、Linux\-arm64、Linux\-musl\-x64 和 Linux\-musl\-arm64。

NativeAOT（Native Ahead\-Of\-Time Compilation）是一种将 .NET 程序编译成本地机器代码的技术，以提高应用程序的性能和启动速度。交叉编译是指在一个平台上为另一个平台生成代码的过程。例如，在 Windows 上为 Linux 生成可执行文件。

为了交叉编译，你需要为目标平台安装相应的工具链。例如，如果你想为 Linux 交叉编译，你需要在 Windows 上安装 Linux 的工具链（如 GCC、Make 等）。这通常可以通过安装 Windows Subsystem for Linux (WSL) 或使用其他工具如 MinGW 来实现。我们有了更好方法：这个项目的地址：[https://github.com/CeSun/PublishAotCross](https://github.com/CeSun/PublishAotCross "https://github.com/CeSun/PublishAotCross")。

使用步骤：

1、从zig官网：[https://ziglang.org/download/](https://ziglang.org/download/ "https://ziglang.org/download/")下载并配置 Zig：将 zig\-windows\-x86\_64\-0\.14\.0\-dev.2540\+f857bf72e.zip 解压并添加到 PATH。
这里要注意的一点是整个压缩包的内容要完整，复制二进制文件，还要复制 lib 目录，不然就可能发生找不到zig.exe 的错误，具体参考[https://christophvoigt.com/notes/unable\-to\-find\-zig\-installation\-directory\-filenotfound/](https://christophvoigt.com/notes/unable-to-find-zig-installation-directory-filenotfound/ "https://christophvoigt.com/notes/unable-to-find-zig-installation-directory-filenotfound/")。

[![image](https://img2023.cnblogs.com/blog/510/202412/510-20241219223828112-657734458.png "image")](https://github.com)

2、从[https://releases.llvm.org/](https://releases.llvm.org/ "https://releases.llvm.org/"):[slower加速器官网](https://chundaotian.com) 下载 LLVM 并将 `llvm-objcopy` 添加到 PATH，最简单的就是把llvm\-objcopy.exe 文件放到zig.exe 相同目录下。

3、在项目中添加 `PublishAotCross` 的引用，具体可参考：[https://github.com/CeSun/OpenHarmony.Avalonia](https://github.com/CeSun/OpenHarmony.Avalonia "https://github.com/CeSun/OpenHarmony.Avalonia") 。

[![image](https://img2023.cnblogs.com/blog/510/202412/510-20241219224440628-378792475.png "image")](https://github.com)

做好了上面的准备，就可以使用VS 的发布功能，下面的配置是使用新的 RID 发布项目，例如发布linux\-musl\-arm64 ：


```
dotnet publish -r linux-musl-arm64
```
[![image](https://img2023.cnblogs.com/blog/510/202412/510-20241219224442313-1958426109.png "image")](https://github.com)


