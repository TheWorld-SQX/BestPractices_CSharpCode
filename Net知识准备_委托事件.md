## 预定义委托
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

 ## 关于 public delegate TResult Func<in T, out TResult>(T arg);
 
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








