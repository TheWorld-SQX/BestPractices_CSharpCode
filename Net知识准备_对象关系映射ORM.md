SqlSugar 是一个轻量级的ORM（对象关系映射）库，它可以用于数据库操作。虽然 SqlSugar 提供了动态建表和动态查询的功能，但它主要用于操作已经存在的数据库表，而不是动态创建类。SqlSugar 的主要功能是映射数据库表到.NET对象，而不是根据数据库的列名动态创建类。

如果您的需求是根据数据库的列名动态创建类，并且希望符合开闭原则，您可以考虑使用.NET的反射功能和动态类型来实现。下面是一个简单的示例，演示如何根据数据库的列名动态创建类：

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Reflection.Emit;

public class DynamicClassGenerator
{
    public static Type CreateDynamicClass(Dictionary<string, Type> columns)
    {
        AssemblyName assemblyName = new AssemblyName("DynamicAssembly");
        AssemblyBuilder assemblyBuilder = AppDomain.CurrentDomain.DefineDynamicAssembly(assemblyName, AssemblyBuilderAccess.Run);
        ModuleBuilder moduleBuilder = assemblyBuilder.DefineDynamicModule("DynamicModule");
        TypeBuilder typeBuilder = moduleBuilder.DefineType("DynamicClass", TypeAttributes.Public);

        foreach (var column in columns)
        {
            FieldBuilder fieldBuilder = typeBuilder.DefineField("_" + column.Key, column.Value, FieldAttributes.Private);
            PropertyBuilder propertyBuilder = typeBuilder.DefineProperty(column.Key, PropertyAttributes.HasDefault, column.Value, null);

            MethodBuilder getMethodBuilder = typeBuilder.DefineMethod("get_" + column.Key, MethodAttributes.Public | MethodAttributes.SpecialName | MethodAttributes.HideBySig, column.Value, Type.EmptyTypes);
            ILGenerator getIl = getMethodBuilder.GetILGenerator();
            getIl.Emit(OpCodes.Ldarg_0);
            getIl.Emit(OpCodes.Ldfld, fieldBuilder);
            getIl.Emit(OpCodes.Ret);

            MethodBuilder setMethodBuilder = typeBuilder.DefineMethod("set_" + column.Key, MethodAttributes.Public | MethodAttributes.SpecialName | MethodAttributes.HideBySig, null, new[] { column.Value });
            ILGenerator setIl = setMethodBuilder.GetILGenerator();
            setIl.Emit(OpCodes.Ldarg_0);
            setIl.Emit(OpCodes.Ldarg_1);
            setIl.Emit(OpCodes.Stfld, fieldBuilder);
            setIl.Emit(OpCodes.Ret);

            propertyBuilder.SetGetMethod(getMethodBuilder);
            propertyBuilder.SetSetMethod(setMethodBuilder);
        }

        Type dynamicType = typeBuilder.CreateType();
        return dynamicType;
    }
}

class Program
{
    static void Main()
    {
        // 根据数据库列名创建动态类
        Dictionary<string, Type> columns = new Dictionary<string, Type>
        {
            { "Id", typeof(int) },
            { "Name", typeof(string) },
            // 其他列...
        };

        Type dynamicType = DynamicClassGenerator.CreateDynamicClass(columns);

        // 创建动态类的实例
        object dynamicObject = Activator.CreateInstance(dynamicType);
        
        // 设置和获取属性值
        PropertyInfo propertyInfo = dynamicType.GetProperty("Name");
        propertyInfo.SetValue(dynamicObject, "John Doe");
        string name = (string)propertyInfo.GetValue(dynamicObject);

        Console.WriteLine(name); // 输出 "John Doe"
    }
}
```

这个示例演示了如何根据数据库的列名动态创建类，并使用反射和动态类型来设置和获取属性值。您可以根据数据库的列名和类型来创建类的结构，然后在运行时动态使用这些类。这种方法可以帮助您实现根据不同的数据库表结构创建类，同时遵循开闭原则。
