## `Action<in T>` 预定义委托(类型)_public delegate void Action<in T>(T obj);
```csharp

    // 摘要:
    //     Encapsulates a method that has a single parameter and does not return a value.
    //
    // 参数:
    //   obj:
    //     The parameter of the method that this delegate encapsulates.
    //
    // 类型参数:
    //   T:
    //     The type of the parameter of the method that this delegate encapsulates.
    public delegate void Action<in T>(T obj);

这段注释是关于 .NET 中的泛型委托 `Action<T>` 的解释。`Action<T>` 是一个预定义的委托类型，用于表示一个具有单个参数并且没有返回值的方法。在这个委托类型中，`T` 是一个类型参数，它指定了方法的参数类型。

具体来说：

- `Action<in T>`：这是一个泛型委托类型的声明。`in T` 表示泛型类型参数 `T` 是作为输入（方法参数）而被使用的。`in` 关键字表示参数是输入参数，也就是方法可以从参数中获取数据，但不能修改参数本身。

- `(T obj)`：这是委托的方法签名。它表示委托可以接受一个类型为 `T` 的参数，而方法本身没有返回值。

所以，当你声明一个 `Action<T>` 类型的委托，你可以将一个接受 `T` 类型参数的方法绑定到该委托上，然后可以通过调用委托来调用这个方法。委托实际上充当了方法的封装器，
使你能够像调用方法一样调用委托，以便在需要的地方执行该方法。

这个委托类型在许多场景下非常有用，例如在异步编程、事件处理等情况下，可以传递不同类型的方法作为参数，从而实现灵活的方法调用。

```
## 自定义委托和预定义委托不同：

在正常情况下的委托声明和使用以及预定义的委托有以下不同之处：

**正常情况下的委托声明和使用**：
在正常情况下，你可以自定义一个委托类型，该委托类型可以代表任何方法签名，可以有多个参数和一个返回值。你需要声明委托类型，并且可以将满足这个签名的方法绑定到委托实例上。以下是一个示例：

```csharp
delegate int CustomDelegate(int x, int y);

class Program
{
    static int Add(int a, int b)
    {
        return a + b;
    }

    static void Main(string[] args)
    {
        CustomDelegate customDelegate = Add; // 绑定 Add 方法到委托
        int result = customDelegate(3, 4);   // 调用委托来执行 Add 方法
        Console.WriteLine(result);           // 输出：7
    }
}
```

**预定义的委托**：

.NET Framework 提供了几种预定义的委托类型，这些委托类型已经在命名空间 `System` 中定义好，可以直接使用。其中之一就是 `Action<T>`，它代表一个没有返回值的方法，
并且只接受一个类型为 `T` 的参数。这些预定义的委托类型让开发者能够更方便地声明和使用委托，无需自定义委托类型。以下是一个示例：

```csharp
class Program
{
    static void PrintMessage(string message)
    {
        Console.WriteLine(message);
    }

    static void Main(string[] args)
    {
        Action<string> printDelegate = PrintMessage; // 使用预定义的 Action<T> 委托
        printDelegate("Hello, world!");              // 调用委托来执行 PrintMessage 方法
    }
}
```

所以，与自定义的委托相比，预定义的委托如 `Action<T>` 更加简洁，方便，且不需要额外的委托类型声明。它们在特定的场景下非常有用，尤其是在需要声明方法参数和返回类型的情况下。


 ## Func<in T, out TResult>(T arg)  关于 public delegate TResult Func<in T, out TResult>(T arg);
 
 预定义委托 Func<in T, out TResult>(T arg);
 
 这段注释是关于 .NET 中的泛型委托 `Func<T, TResult>` 的解释。`Func<T, TResult>` 是一个预定义的泛型委托类型，用于表示一个具有一个参数并返回指定类型 `TResult` 的方法。

具体来说：

- `Func<in T, out TResult>`：这是一个泛型委托类型的声明。`in T` 表示泛型类型参数 `T` 是作为输入（方法参数）而被使用的，`out TResult` 表示泛型类型参数 `TResult` 是作为输出（返回值）而被使用的。

- `(T arg)`：这是委托的方法签名。它表示委托可以接受一个类型为 `T` 的参数，并且方法会返回一个类型为 `TResult` 的值。

- `T`：这是方法的参数类型。
- `TResult`：这是方法的返回值类型。

所以，当你声明一个 `Func<T, TResult>` 类型的委托，你可以将一个接受 `T` 类型参数并返回 `TResult` 类型值的方法绑定到该委托上。然后，你可以通过调用委托来调用这个方法，委托会返回一个 `TResult` 类型的结果。

这个委托类型在很多情况下非常有用，特别是在需要传递函数作为参数的场景中，它可以灵活地表示各种函数签名。


## 订阅了事件的方法

在事件驱动编程中，"订阅了事件的方法" 指的是那些已经通过将自己的方法添加到事件的委托列表中，从而表示希望在事件触发时执行的方法。

当你定义一个事件时，实际上在类中创建了一个委托字段（delegate field），这个委托字段用于维护一个委托列表，其中包含了所有订阅了该事件的方法。当事件触发时，这个委托列表中的方法会被调用。

在你的代码中，例如：

```csharp
public event PropertyChangedEventHandler PropertyChanged;
```

这个 `PropertyChanged` 事件是一个委托，而具体的委托类型是 `PropertyChangedEventHandler`。在这个事件触发时，会调用所有订阅了 `PropertyChanged` 事件的方法。

订阅了事件的方法就是你在代码中显式使用事件 += 运算符，将自己的方法添加到事件的委托列表中的那些方法。例如，假设你有一个 `ViewModel` 类，其中有一个方法 `HandlePropertyChanged`：

```csharp
public class ViewModel
{
    public event PropertyChangedEventHandler PropertyChanged;

    public void HandlePropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        // 处理属性更改
    }

    public void SubscribeToPropertyChangedEvent()
    {
        PropertyChanged += HandlePropertyChanged; // 订阅事件
    }

    public void UnsubscribeFromPropertyChangedEvent()
    {
        PropertyChanged -= HandlePropertyChanged; // 取消订阅事件
    }
}
```

在上面的示例中，`HandlePropertyChanged` 方法订阅了 `PropertyChanged` 事件。当 `PropertyChanged` 事件触发时，`HandlePropertyChanged` 方法会被调用，以执行与属性更改相关的操作。

所以，"订阅了事件的方法" 就是已经通过事件 += 运算符将自己的方法添加到事件的委托列表中的方法。



## MVVMToolKit 里的委托
这段注释是关于 `RelayCommand<T>` 类的构造函数的解释。`RelayCommand<T>` 是 Community Toolkit 中的一个类，用于实现 `ICommand` 接口，以便在 MVVM 架构中进行命令绑定。以下是这个构造函数的解释：

- **初始化一个可以始终执行的 `RelayCommand<T>` 实例**：

  这是 `RelayCommand<T>` 类的构造函数，它用于创建一个新的命令实例，该命令可以始终执行，即不会考虑命令是否可以执行的状态。

- **参数 `execute`**：

  `execute` 参数是一个 `Action<T?>` 委托，它表示执行逻辑。这个委托代表一个具有单个参数类型为 `T` 的方法，该方法表示当命令被调用时要执行的逻辑。

- **异常 `ArgumentNullException`**：

  如果 `execute` 参数为 null，那么会抛出 `ArgumentNullException` 异常。这是一种参数验证，确保传递给构造函数的执行逻辑不为空。

- **言论**：

  这段注释还提供了一个关于为什么在 `execute` 参数中使用可空类型的建议。由于 `ICommand` 接口公开了接受可空 `System.Object` 参数的方法，所以如果类型 `T` 是引用类型（类），最好将其声明为可空类型，并且在 `execute` 方法内部始终进行检查。

总之，这个构造函数用于创建一个 `RelayCommand<T>` 实例，它始终可以执行，并且通过传递的 `Action<T?>` 委托指定了执行的逻辑。在使用这个命令时，你可以在 `execute` 方法中定义要执行的操作，并且如果需要，可以对参数进行验证和处理。

**补充**：
"RelayCommand" 这个命名在 MVVM 框架中是相对常见的，它传达了这个命令的基本工作原理和用途。下面是一些关于 "RelayCommand" 命名的解释：

1. **"Relay" 的含义**：
   在这个上下文中，"Relay" 是一个通信术语，表示将一个信号或消息从一个地方传递到另一个地方。在 MVVM 架构中，命令可以看作是一种在视图（UI）和视图模型（ViewModel）之间进行通信的机制，以便触发某些操作。

2. **"Command" 的含义**：
   "Command" 表示一个可执行的操作，可以在特定的条件下被调用。在 MVVM 中，命令通常用于将用户操作绑定到视图模型中的方法。

综合来看，"RelayCommand" 的命名传达了一个基本的思想：这是一种用于在不同的部分之间传递并执行操作的机制。具体来说，在 `RelayCommand` 中，"Relay" 指的是将操作从视图传递到视图模型，而 "Command" 表示可以执行的操作。

虽然 "RelayCommand" 的命名并不是唯一的，不同的 MVVM 框架可能会使用不同的名称，但它已经成为了一个相对常见的约定，因此在许多框架和代码库中使用这个名称可以帮助其他开发者更快地理解其作用和用途。

## 预定义委托作为可选参数
在C#中，可以通过为参数提供默认值来使参数成为可选参数。这意味着如果您调用方法时不提供该参数，将使用默认值。在您提供的代码中，`Func<object, bool> _canExecute = null` 中的 `= null` 部分指定了默认值为 `null`。

这是有用的，因为在某些情况下，您可能不需要传递 `canExecute` 参数。例如，如果您的 `Command` 构造函数是为了创建一个始终可执行的命令，那么在创建命令实例时，您可以只提供 `execute` 参数，而不必传递 `canExecute` 参数，因为默认情况下它被设置为 `null`。

如果需要判断命令是否可执行，可以在稍后的代码中通过检查 `canExecute` 参数来决定。如果 `canExecute` 为 `null`，则默认情况下该命令将始终可执行，因为在 `CanExecute` 方法中将返回 `true`。如果您提供了一个非空的 `canExecute` 方法，那么 `CanExecute` 方法将使用这个方法来确定命令是否可执行。

这种方式使代码更加灵活，因为它允许您选择是否提供一个方法来检查命令是否可执行，而不是强制要求提供。这符合C#中方法参数的可选性和默认值的特性。








