## UI界面
菜单栏，一级菜单，二级菜单
## MVVM Light 做了哪些工作
如果不使用MVVM Light框架，你需要手动处理一些额外的工作来实现MVVM模式和相关功能。以下是一些你可能需要额外做的工作：

1. **属性通知：** 在视图模型中，你需要自己实现属性通知机制，确保在属性值发生变化时，通知视图进行更新。这通常需要实现 `INotifyPropertyChanged` 接口，并在属性的 `set` 方法中触发属性变化事件。

2. **命令绑定：** 你需要实现自定义的命令类（比如 `RelayCommand`），以便在视图中绑定按钮等UI元素到视图模型中的方法。

3. **通信机制：** 如果需要在不同的视图模型之间进行通信，你需要实现自定义的事件或消息机制，以实现松耦合的通信。

4. **IoC容器：** 如果你希望实现依赖注入，你需要手动引入第三方IoC容器，如Unity、Autofac等，并配置依赖关系。

5. **资源共享：** 如果要共享资源，你需要手动管理资源字典和资源的分离，以确保资源在不同的视图和视图模型之间得到正确的共享。

6. **事件处理：** 在视图模型中，你需要手动处理一些UI事件，如按钮点击事件等。

7. **代码生成：** 你需要自行编写大量的样板代码，如属性、命令、通信和事件等。

8. **定位和绑定：** 在视图中，你需要手动定位并绑定视图模型，可能需要创建一个自定义的ViewModelLocator。

9. **处理资源释放和生命周期：** 手动管理资源释放和视图模型的生命周期，以避免内存泄漏和不必要的资源占用。

总之，不使用MVVM Light框架意味着你需要手动实现许多与MVVM模式相关的功能，包括属性通知、命令绑定、通信、依赖注入等。这可能会增加开发的工作量和复杂性，特别是在大型项目中。因此，如果你希望更快速地实现MVVM模式，并享受框架提供的便利功能，MVVM Light框架可能是一个更好的选择。
## vs 创建wpf 项目初始结构
当你通过Visual Studio 2019创建一个全新的WPF项目时，项目的初始结构通常如下所示：

```
- YourWpfProject
  - YourWpfProject
    - App.xaml
    - App.xaml.cs
    - MainWindow.xaml
    - MainWindow.xaml.cs
    - Properties
      - AssemblyInfo.cs
  - YourWpfProject.sln
```

在这个初始结构中，`YourWpfProject`文件夹包含了项目的主要内容，如应用程序文件、窗口文件和属性文件。下面是各个文件和文件夹的说明：

- `App.xaml` 和 `App.xaml.cs`：应用程序的入口点，用于全局资源定义和应用程序级别的设置。

- `MainWindow.xaml` 和 `MainWindow.xaml.cs`：主窗口的XAML文件和代码文件，通常是应用程序的主界面。

- `Properties` 文件夹：存放与项目属性相关的文件，如 `AssemblyInfo.cs`。

- `YourWpfProject.sln`：Visual Studio解决方案文件，用于管理项目、项目文件和其他资源。

这个初始结构提供了一个简单的起点，你可以在此基础上逐步添加其他的文件夹、类和资源，以满足项目的需求。

## `AssemblyInfo.cs`
`AssemblyInfo.cs` 文件通常用来存放关于程序集（assembly）的元数据和配置信息，这些信息可以包括版本号、公司信息、版权声明等。该文件对于标识和描述程序集的属性非常有用，它会被编译进程序集中。

以下是一些常见的在 `AssemblyInfo.cs` 文件中设置的属性和信息，以及相应的代码示例说明：

1. **程序集版本号（AssemblyVersion）：** 用于标识程序集的版本号。

```csharp
[assembly: AssemblyVersion("1.0.0.0")]
```

2. **文件版本号（AssemblyFileVersion）：** 用于标识程序集文件的版本号。

```csharp
[assembly: AssemblyFileVersion("1.0.0.0")]
```

3. **产品版本号（AssemblyInformationalVersion）：** 用于指定产品版本的更详细的信息。

```csharp
[assembly: AssemblyInformationalVersion("1.0.0")]
```

4. **程序集标题、说明和公司信息（AssemblyTitle、AssemblyDescription、AssemblyCompany）：** 用于提供关于程序集的文字描述。

```csharp
[assembly: AssemblyTitle("MyWpfApp")]
[assembly: AssemblyDescription("A sample WPF application")]
[assembly: AssemblyCompany("My Company")]
```

5. **版权信息（AssemblyCopyright）：** 用于指定版权信息。

```csharp
[assembly: AssemblyCopyright("Copyright © 2023 My Company")]
```

6. **程序集的唯一标识（GuidAttribute）：** 用于指定程序集的全局唯一标识符（GUID）。

```csharp
[assembly: Guid("01234567-89AB-CDEF-0123-456789ABCDEF")]
```

7. **兼容性设置（AssemblyCompatibility）：** 用于指定程序集与其他程序集的兼容性。

```csharp
[assembly: AssemblyCompatibility(CompatibilityVersion.Compatible)]
```

这些示例说明了 `AssemblyInfo.cs` 文件中的一些常见元数据设置。通过在 `AssemblyInfo.cs` 文件中配置这些信息，你可以提供有关程序集的重要信息，帮助其他开发人员了解程序集的用途、版本和所属公司等信息。


## 项目组织怎么安排

