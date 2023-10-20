##  圆角窗口示例
当在WPF中创建具有圆角窗口样式和控件模板时，需要进行以下详细步骤：

**1. 创建WPF应用程序：**

首先，创建一个新的WPF应用程序，如果您已经有一个应用程序，可以直接跳到下一步。

**2. 添加资源字典：**

在您的应用程序中，添加一个资源字典文件，以便在其中定义窗口样式和控件模板。右键单击项目，选择 "Add" -> "Resource Dictionary"，将其命名为"CustomStyles.xaml"。

**3. 定义窗口样式：**

在`CustomStyles.xaml`中，定义窗口样式。以下是一个示例，创建一个具有圆角的窗口：

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Style x:Key="RoundedWindowStyle" TargetType="Window">
        <Setter Property="WindowStyle" Value="None"/>
        <Setter Property="AllowsTransparency" Value="True"/>
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Window">
                    <Grid Background="Transparent">
                        <Border CornerRadius="20" BorderBrush="Black" BorderThickness="1" Background="White" Margin="10">
                            <AdornerDecorator>
                                <ContentPresenter />
                            </AdornerDecorator>
                        </Border>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

这个样式创建一个具有圆角的窗口，带有黑色边框和白色背景。

**4. 引入资源字典：**

在应用程序的`App.xaml`文件中，将资源字典引入应用程序。打开`App.xaml`，并确保它包含以下内容：

```xml
<Application x:Class="YourNamespace.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="CustomStyles.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

确保将`ResourceDictionary`引入到应用程序中，以便您可以在整个应用程序中使用样式和模板。

**5. 应用窗口样式：**

在您的窗口的XAML中，应用窗口样式。打开`MainWindow.xaml`（或您的窗口的XAML文件），并确保窗口的定义包含`Style`属性，如下所示：

```xml
<Window x:Class="YourNamespace.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Rounded Window" Height="300" Width="400" Style="{StaticResource RoundedWindowStyle}">
    <!-- 窗口内容 -->
</Window>
```

这会使您的窗口采用您刚刚定义的圆角窗口样式。

**6. 创建自定义控件模板（可选）：**

如果您需要创建自定义控件模板，可以在`CustomStyles.xaml`中定义它，并在相应的控件上应用模板。

这些步骤允许您创建一个具有圆角窗口样式和自定义控件模板的WPF应用程序。确保将`YourNamespace`替换为您的应用程序的命名空间。如果需要更多自定义，您可以进一步调整样式和模板。
