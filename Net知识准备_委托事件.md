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
