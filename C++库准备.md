x86通常指的是32位的x86架构。x86是一种基于Intel的微处理器架构，最初用于16位处理器，但随着时间的推移，它发展为支持32位和64位处理器。然而，在许多上下文中，x86通常指代32位版本，而x64（或x86-64）指代64位版本。

在计算机中，32位和64位指的是处理器的数据位数。32位处理器支持每次处理32位数据，而64位处理器支持每次处理64位数据。64位处理器可以更有效地处理更大的内存，同时支持更大的数据和更复杂的计算。

需要注意的是，随着技术的发展，现代计算机中64位处理器逐渐成为主流，而32位处理器逐渐被淘汰。因此，如果您在现代计算机上使用x86，通常会是64位版本的x86-64。但在一些特定的老旧系统或特定需求下，仍然可能会使用32位的x86。

## 串口通信项目
VS C++桌面开发组件；第三方库 boost，sqlite；

## C++常用库 boost
Boost C++ 库（Boost Library）是一个由 C++ 社区开发和维护的开源项目，提供了大量高质量、通用性强的 C++ 库，涵盖了从低级别的操作，如容器、算法，到高级领域，如并发编程、数学、图像处理等各种功能。

Boost 的名称源自其目标：为 C++ 提供一个“推动”（boost）语言和标准库的工具。它的目的是填补标准 C++ 库的一些缺失和不足，同时提供一些现代化和高级的编程工具。Boost 强调通用性、可移植性和高质量，因此在 C++ 开发社区中广受欢迎。

Boost 库之所以受欢迎，是因为它在 C++ 标准库之外提供了许多非常有用的功能。它的库提供了各种各样的实现，从而使开发者能够更加高效地完成任务，而无需从头开始编写复杂的代码。

需要注意的是，虽然 Boost 是一个非常受欢迎的 C++ 库，但不是 C++ 标准的一部分。然而，一些 Boost 中的库最终被纳入了 C++ 标准库，例如 C++11 引入了一些 Boost 中的特性。因此，使用 Boost 可以帮助开发者在标准库中缺少功能的情况下，更加便捷地进行开发。

## boost库
b2.exe install --prefix="D:\boost\boost_1_76_0\lib32-msvc-14.2" --build-type=complete --toolset=msvc-14.2 threading=multi --build-type=complete address-model=64

b2.exe install --prefix="D:\boost\boost_1_76_0\lib64-msvc-14.2" --build-type=complete --toolset=msvc-14.2 threading=multi --build-type=complete address-model=32

b2.exe install --prefix="D:\Boost\x64" --build-type=complete --toolset=msvc-14.2 threading=multi --build-type=complete address-model=64

b2.exe install --prefix="D:\Boost\x86" --build-type=complete --toolset=msvc-14.2 threading=multi --build-type=complete address-model=32

## 使用Boost库之前，您需要将源代码编译成适合您的操作系统和编译器的库文件。

通常情况下，Boost库的源代码发行版仅包含头文件和一些源代码文件，它并不包含预编译的库文件。因此，在使用Boost库之前，您需要将源代码编译成适合您的操作系统和编译器的库文件。

"lib64-msvc-14.2" 目录是一个典型的示例，可能是由某人或某个组织自行构建的Boost库的二进制文件（预编译库）目录。它的命名可能是为了反映使用的编译器和操作系统信息，例如MSVC 14.2表示使用Microsoft Visual C++编译器的版本号是14.2，而"lib64"可能表示这些是64位版本的库。

生成Boost库的二进制文件涉及使用适当的编译器和构建工具，在适用的平台上进行编译和链接。因此，这些预编译的库文件可能会因不同的编译选项、操作系统和编译器版本而有所不同。


## sqlite3
下载的文件包sqlite-dll-win32-x86-3300100.zip里面有两个文件，分别为sqlite3.dll和sqlite3.def。拿到dll文件以后，

直接使用LoadLibrary动态加载；  
或者  
生成导入库文件.lib配合.dll文件静态加载。  

通常推荐采用第二种，因此需要得到lib文件。把这两个文件放到一个文件夹下，例如E:\VSProjects\Sqlite3Lib,打开Visual Studio的开发人员命令提示，切换到该目录下，执行命令：LIB /def:sqlite3.def

得到lib文件以后，我们就可以在自己的程序中使用了，不过在使用之前，我们还需要头文件支持，这个可以在最新的源代码中找到，这里用到的是第一个文件包sqlite-snapshot-201912260110.tar.gz里面的sqlite3.h文件，把它也放到.dll文件一起。

##  C++项目结构及命名空间
可以将类的声明放在源文件中，但通常的做法是将类的声明放在头文件中。这是因为在 C++ 中，头文件的主要目的是提供接口和声明，而源文件则包含具体的实现。以下是一些原因解释为什么通常将类的声明放在头文件中：

1. **分离接口和实现：** 将类的声明放在头文件中，可以将类的公共接口暴露给其他代码，同时将实现细节留在源文件中。这种分离可以提高代码的可维护性，因为其他代码可以使用接口，而不必了解实现的细节。

2. **模块化编程：** 头文件和源文件的分离允许将代码分割成模块，每个模块都有明确定义的接口。这有助于团队合作，不同成员可以并行地开发和修改不同模块的代码。

3. **编译效率：** 如果类的声明在源文件中，每次修改实现细节都可能导致需要重新编译引用了该类的所有源文件。将声明放在头文件中，只有在接口发生变化时才需要重新编译，从而提高了编译效率。

4. **可读性：** 头文件作为接口文档，可以让其他开发人员更轻松地了解类的公共接口和功能，而不必查看具体的实现细节。

5. **预编译和重用：** 头文件的内容可以被预编译，从而减少编译时间。另外，头文件中的声明可以被多个源文件重用，避免了代码的重复编写。

尽管可以将类的声明放在源文件中，但遵循将声明放在头文件中的惯例可以提高代码的可维护性、可读性和重用性。


## 析构函数
析构函数是在对象被销毁时自动调用的特殊成员函数，其主要目的是在对象生命周期结束时执行必要的清理工作，释放由对象持有的资源和执行对象的销毁操作。析构函数通常与构造函数（用于初始化对象）一起，共同管理对象的完整生命周期。

有以下几个重要的理由，说明为什么需要析构函数：

1. **资源释放：** 在对象的生命周期内，可能分配了各种资源，如内存、文件句柄、网络连接等。这些资源需要在对象销毁时进行释放，以避免内存泄漏或资源浪费。析构函数提供了一个适当的时机来执行这些释放操作。

2. **清理操作：** 有些对象在其生命周期内执行一些初始化操作，如打开文件、建立网络连接等。这些操作需要在对象销毁时进行逆操作，如关闭文件、断开网络连接等。析构函数提供了一个合适的地方来执行这些清理操作。

3. **避免资源泄漏：** 如果对象的析构函数没有适当地释放资源，那么这些资源可能会一直保持占用状态，从而导致资源泄漏。通过在析构函数中释放资源，可以避免这种情况。

4. **可预测的行为：** 通过使用析构函数，可以确保在对象生命周期结束时进行一致的清理操作，而不依赖于程序员显式调用某个方法来释放资源。

5. **方便的对象管理：** 析构函数的自动调用确保了对象的资源管理和清理操作不会被遗漏，从而减轻了程序员的负担，提高了代码的可维护性。

综上所述，析构函数是为了确保对象的资源和状态能够在对象生命周期结束时得到妥善处理而存在的。它是良好的软件工程实践的一部分，有助于编写更安全、稳定和可靠的代码。

##  vs C++ 类库项目引用文件目录
项目属性里   配置属性里  vc++ 目录 和 库目录  

## C++析构函数和c#析构函数
C++ 和 C# 中的析构函数在概念和目的上是相似的，都用于在对象生命周期结束时执行资源释放和清理操作。然而，它们之间存在一些重要的区别：

1. **调用方式**：
   - **C++**：析构函数是由编译器自动调用的，在对象的作用域结束时（如局部变量离开作用域）或通过 `delete` 运算符销毁动态分配的对象时。
   - **C#**：析构函数（Finalizer）是由垃圾回收器（Garbage Collector）在对象不再被引用时自动调用的。开发者无法直接控制析构函数的调用时机，而是依赖于垃圾回收器。

2. **执行时机**：
   - **C++**：对象的析构函数在对象的生命周期结束时执行，可以确保资源在合适的时机被释放和清理。
   - **C#**：对象的析构函数（Finalizer）的执行时机是由垃圾回收器决定的，可能会导致资源释放和清理的延迟。

3. **资源管理**：
   - **C++**：开发者可以在析构函数中手动释放资源，因为它是在对象销毁时执行的。C++ 中的析构函数在资源管理方面更加灵活，但也需要开发者自己确保正确释放资源，以避免资源泄漏。
   - **C#**：由于析构函数的执行时机不确定，开发者无法确保资源在预期的时间内被释放。为了更精细地管理资源，C# 引入了 `IDisposable` 接口，通过 `Dispose()` 方法手动释放资源，这种方式更加可控。

总的来说，虽然 C++ 和 C# 中的析构函数有类似的目的，但由于垃圾回收器的存在，C# 中的析构函数相对来说更具约束性。为了确保资源释放的可控性，C# 推荐使用 `IDisposable` 接口和 `Dispose()` 方法。

## C++ win标准函数调用约定
`APIENTRY` 是一个宏，用于指定函数的调用约定（calling convention）。在 Windows 操作系统中，函数调用约定是一种规则，用于指定函数参数传递、堆栈管理和寄存器使用等细节，以确保函数能够正确地被调用和返回。

在 Windows 平台上，常见的函数调用约定包括 `__cdecl`、`__stdcall` 和 `__fastcall`。`APIENTRY` 实际上是一个宏，被定义为 `__stdcall`，因此在使用 `APIENTRY` 时，会将函数声明为使用标准调用约定。

标准调用约定（`__stdcall`）的特点包括：

- 参数传递：参数从右向左依次压入堆栈，由调用者负责清理堆栈。
- 返回值传递：返回值通常通过 EAX 寄存器传递给调用者。
- 函数名称修饰：函数名后面会添加一个下划线（_）前缀，用于标识函数的调用约定。

在 Windows API 中，通常会使用 `APIENTRY` 或者直接使用 `__stdcall` 来声明 WinAPI 函数，以确保正确的调用约定。例如：

```cpp
APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
{
    // WinAPI function implementation
}
```

请注意，调用约定的选择会影响函数的调用方式和参数传递方式，因此在编写 Windows 程序时，务必要选择正确的调用约定来声明和定义函数，以确保函数能够正确地被调用和返回。

## C++ 自定义 宏 DLL_API
`DLL_API` 是一个宏，通常用于在编写动态链接库（DLL）时，标记函数或类的导出和导入。在 Windows 环境中，DLL 文件通常包含可以被其他程序调用的函数和数据。使用 `DLL_API` 宏可以帮助定义哪些函数和类是可以被外部程序访问的。

在不同的编译环境中，`DLL_API` 的具体定义可能会有所不同。通常，它会被定义为适当的关键字，以指示编译器将函数或类标记为可供外部访问的。例如，在 Windows 环境中，`DLL_API` 可能被定义为 `__declspec(dllexport)` 或 `__declspec(dllimport)`，具体取决于是在定义 DLL 时还是在使用 DLL 时。

在定义 DLL 中，`DLL_API` 标记了导出的函数和类：

```cpp
// 在定义 DLL 时
#define DLL_API __declspec(dllexport)

DLL_API void MyExportedFunction();
```

在使用 DLL 的代码中，`DLL_API` 标记了导入的函数和类：

```cpp
// 在使用 DLL 时
#define DLL_API __declspec(dllimport)

DLL_API void MyExportedFunction();
```

通过使用 `DLL_API` 宏，可以在定义和使用 DLL 时保持一致的接口标记，以确保正确的导入和导出。这有助于在编写可移植的代码时更容易地切换和使用不同的编译器和开发环境。


## C++ 在编写 DLL 时，定义了一些宏和函数，以便在其他程序中正确地导入和使用这些函数。
``` cpp
#pragma once

#ifdef IOTMODELLIB_EXPORTS
#define DLL_CLASS extern "C" class __declspec(dllexport)
#define DLL_API __declspec(dllexport)
#else
#define DLL_CLASS extern "C" class __declspec(dllimport)
#define DLL_API __declspec(dllimport)
#endif


#include <common\interface\ISerialPort.h>

#ifdef __cplusplus
extern "C" {
#endif // __cplusplus

    /// <summary>
    /// 获取串口模块实例指针
    /// </summary>
    /// <returns></returns>
    DLL_API common::ISerialPort* ISerialPort_GetInstance(const char* port, unsigned int baudRate);

#ifdef __cplusplus
}
#endif // __cplusplus

```
这段代码看起来是一个用于动态链接库（DLL）导出的头文件，其中包含了一些宏定义和函数声明。以下是对代码的详细解释：

1. `#pragma once`：这是预处理指令，用于确保头文件只被包含一次。

2. `#ifdef IOTMODELLIB_EXPORTS`：这是一个条件编译指令，检查是否定义了 `IOTMODELLIB_EXPORTS` 宏。这通常用于在定义 DLL 时设置导出的宏。

3. `#define DLL_CLASS ...`：这是一个宏定义，根据是否定义了 `IOTMODELLIB_EXPORTS` 宏，来分别定义导出和导入的类和函数宏。

4. `#else`：这是条件编译指令的分支，当没有定义 `IOTMODELLIB_EXPORTS` 宏时执行。

5. `#include <common\interface\ISerialPort.h>`：这是包含外部头文件的指令，用于引入 `ISerialPort` 接口的声明。

6. `#ifdef __cplusplus` 和 `#endif // __cplusplus`：这是条件编译指令，用于在 C++ 环境下将后续代码用 `extern "C"` 包裹，以保证 C++ 和 C 的函数名链接正确。

7. `DLL_API common::ISerialPort* ISerialPort_GetInstance(const char* port, unsigned int baudRate);`：这是一个函数声明，用于在动态链接库中导出获取串口实例的函数。

总体而言，这段代码用于在编写 DLL 时，定义了一些宏和函数，以便在其他程序中正确地导入和使用这些函数。根据不同的编译环境，这些宏会根据需要来调整函数的导出和导入行为，以确保正确的编译和链接。


## 
当分析这行代码的语法时，让我们从内到外逐步解释：

1. `std::function<void(const uint8_t*, size_t)>`：
   - `std::function`：这是 C++ 标准库中的模板类，用于封装可调用对象，类似于函数指针。
   - `<void(const uint8_t*, size_t)>`：这是一个模板参数，指定了可调用对象的类型。在这里，它表示可调用对象的签名，即参数类型和返回类型。

2. `void`：
   - 这是可调用对象的返回类型，表示这个可调用对象没有返回值。

3. `(const uint8_t*, size_t)`：
   - 这是可调用对象的参数列表，指定了它可以接受两个参数：
     - `const uint8_t*`：表示一个指向 `uint8_t` 类型的常量指针，即指向字节数组的指针。
     - `size_t`：表示一个无符号整数类型，用于表示数据的大小或长度。

所以，`std::function<void(const uint8_t*, size_t)>` 表示一个可以存储不返回任何值的可调用对象，这个可调用对象接受一个指向字节数组的常量指针以及一个无符号整数参数。

使用示例：

假设我们有以下的类定义，其中包含了 `ReadCallback` 成员变量：

```cpp
#include <iostream>
#include <functional>

class SerialPort {
public:
    std::function<void(const uint8_t*, size_t)> ReadCallback;
};
```

我们可以在使用 `SerialPort` 类的时候，设置和使用 `ReadCallback`，比如：

```cpp
void HandleReceivedData(const uint8_t* data, size_t size) {
    std::cout << "Received data: ";
    for (size_t i = 0; i < size; ++i) {
        std::cout << static_cast<int>(data[i]) << " ";
    }
    std::cout << std::endl;
}

int main() {
    SerialPort serialPort;
    serialPort.ReadCallback = HandleReceivedData; // 设置处理函数

    uint8_t receivedData[] = { 10, 20, 30, 40 };
    size_t dataSize = sizeof(receivedData) / sizeof(receivedData[0]);

    if (serialPort.ReadCallback) {
        serialPort.ReadCallback(receivedData, dataSize); // 调用处理函数
    }

    return 0;
}
```

在这个示例中，我们创建了一个 `SerialPort` 对象，并将 `ReadCallback` 设置为 `HandleReceivedData` 函数。当我们调用 `serialPort.ReadCallback(receivedData, dataSize);` 时，实际上就是在调用我们之前定义的 `HandleReceivedData` 函数，从而处理接收到的数据。







