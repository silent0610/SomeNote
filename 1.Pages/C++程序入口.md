---
Type:
  - Page
aliases: 
tags: 
modifiedDate: 2025/06/17, 15:18:06
---

# C++程序入口

- C++ 默认从 `main` 函数开始执行代码
    - C++ 标准规定. 一个独立的执行程序必须有一个名为 `main` 的函数作为其起始执行点。
    - **操作系统约定**：操作系统本身不一定关心这个入口点叫什么（它通常是一个内存地址），但 C/C++ 运行时环境会负责将操作系统的调用导向到 `main` 函数。
    - 初始化
        - 初始化全局和静态对象。
        - 设置标准 I/O 流（`std::cin`, `std::cout` 等）。
        - 运行其他必要的**启动代码**。 `main` 函数的执行标志着这些初始化工作已经完成，程序逻辑可以安全地开始运行。

## 自定义程序入口

通过一些**非标准或特定于平台/编译器的机制**来改变程序的启动点，或者在 `main` 之前执行你自己的代码。

### ⭐使用特定于平台或库的启动点 (如 WinMain, DllMain)

- ⭐**WinMain (Windows)**：在 Windows GUI 应用程序中，`WinMain` 是程序的主要入口点，而不是 `main` ==我好像用过来着, 是软渲染器吗? [软光栅实践](软光栅实践.md)== #TODO 
- **DllMain (Windows DLLs)**：对于 Windows 动态链接库 (DLL)，`DllMain` 是它们的入口点，用于在 DLL 加载/卸载时执行初始化/清理。

### 修改连接器入口点

- 通过**链接器的选项**来修改这个入口点，使其指向你自己的任意函数。
    - **GCC/Clang**: 你可以使用链接器选项 `-e <function_name>` 或 `-Wl,--entry=<function_name>`
    - **MSVC**: 你可以使用链接器选项 `/ENTRY:<function_name>`。

#### 缺点

- **破坏 C++ 运行时环境**：你的自定义入口函数将不会被 C++ 运行时环境预处理。这意味着全局对象的构造、静态变量的初始化、标准库（如 `std::cout`）的设置等都将不会发生。你需要手动完成这些工作，或者你的程序将无法正确使用 C++ 标准库的特性。
- **高度不便携**：依赖特定的编译器和操作系统。
- **仅用于底层开发**：这种方式通常只在编写操作系统内核、嵌入式固件或某些极低级的启动代码时才会使用。

### 使用构造函数和析构函数属性 (GCC/Clang 扩展)

GCC 和 Clang 编译器提供的一种扩展，允许你指定在 `main` 函数之前或之后运行的函数。

- **`__attribute__((constructor))`**:
    - **原理**：这个属性修饰的函数会在 `main` 函数**之前**被调用。所有带有这个属性的函数，会在程序的全局和静态对象的构造完成之后，但在 `main` 函数开始执行之前自动运行。
    - **用途**：用于在程序启动时执行一些全局初始化，例如注册一些回调、设置环境等。
