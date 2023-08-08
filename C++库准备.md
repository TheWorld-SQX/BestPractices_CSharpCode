x86通常指的是32位的x86架构。x86是一种基于Intel的微处理器架构，最初用于16位处理器，但随着时间的推移，它发展为支持32位和64位处理器。然而，在许多上下文中，x86通常指代32位版本，而x64（或x86-64）指代64位版本。

在计算机中，32位和64位指的是处理器的数据位数。32位处理器支持每次处理32位数据，而64位处理器支持每次处理64位数据。64位处理器可以更有效地处理更大的内存，同时支持更大的数据和更复杂的计算。

需要注意的是，随着技术的发展，现代计算机中64位处理器逐渐成为主流，而32位处理器逐渐被淘汰。因此，如果您在现代计算机上使用x86，通常会是64位版本的x86-64。但在一些特定的老旧系统或特定需求下，仍然可能会使用32位的x86。

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


