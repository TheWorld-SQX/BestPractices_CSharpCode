## 策略+工厂

下面是使用策略模式和工厂模式的具体示例，分别演示了如何降低if-else语句的使用，并使代码更加灵活和可扩展。

#### 使用策略模式示例：

策略模式允许根据不同的条件选择不同的策略，每个策略对应一种操作。首先，定义一个策略接口和多个实现该接口的策略类，然后在运行时选择合适的策略进行操作。

```csharp
// 定义策略接口
public interface IOperationStrategy
{
    int Execute(int a, int b);
}

// 实现具体的策略类
public class AddOperation : IOperationStrategy
{
    public int Execute(int a, int b)
    {
        return a + b;
    }
}

public class SubtractOperation : IOperationStrategy
{
    public int Execute(int a, int b)
    {
        return a - b;
    }
}

// 使用策略模式
public class Calculator
{
    private IOperationStrategy strategy;

    public Calculator(IOperationStrategy strategy)
    {
        this.strategy = strategy;
    }

    public int Calculate(int a, int b)
    {
        return strategy.Execute(a, b);
    }
}

// 在运行时选择合适的策略
var addStrategy = new AddOperation();
var calculator = new Calculator(addStrategy);
int result = calculator.Calculate(5, 3); // 结果为 8
```

通过策略模式，我们可以根据不同的条件选择不同的策略类，而不需要使用if-else语句。

#### 使用工厂模式示例：

工厂模式允许根据条件创建不同的对象，而不需要在代码中直接实例化对象。首先，定义一个工厂接口和多个实现该接口的工厂类，然后使用工厂来创建对象。

```csharp
// 定义产品接口
public interface IProduct
{
    void ShowInfo();
}

// 实现具体产品类
public class ConcreteProductA : IProduct
{
    public void ShowInfo()
    {
        Console.WriteLine("Product A");
    }
}

public class ConcreteProductB : IProduct
{
    public void ShowInfo()
    {
        Console.WriteLine("Product B");
    }
}

// 定义工厂接口
public interface IFactory
{
    IProduct CreateProduct();
}

// 实现具体工厂类
public class ConcreteFactoryA : IFactory
{
    public IProduct CreateProduct()
    {
        return new ConcreteProductA();
    }
}

public class ConcreteFactoryB : IFactory
{
    public IProduct CreateProduct()
    {
        return new ConcreteProductB();
    }
}

// 使用工厂模式
IFactory factory = new ConcreteFactoryA(); // 根据条件选择工厂
IProduct product = factory.CreateProduct(); // 创建产品
product.ShowInfo(); // 输出 "Product A"
```

通过工厂模式，我们可以根据条件选择不同的工厂来创建对象，而不需要在代码中直接实例化对象。这提高了代码的可扩展性。
