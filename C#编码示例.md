## 以下是使用C#连接Oracle数据库的示例代码：

```csharp
using System;
using Oracle.ManagedDataAccess.Client;

class Program
{
    static void Main()
    {
        string connectionString = "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=10.1.1.6)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=dbm)));User Id=OT;Password=OT;";

        using (OracleConnection connection = new OracleConnection(connectionString))
        {
            try
            {
                connection.Open();
                Console.WriteLine("成功连接到Oracle数据库！");
                
                // 在这里执行你的数据库操作
                
                connection.Close();
            }
            catch (Exception ex)
            {
                Console.WriteLine("连接到Oracle数据库时出现错误： " + ex.Message);
            }
        }
    }
}
```

请注意，上述示例中使用了 `Oracle.ManagedDataAccess.Client` 命名空间，因此请确保已将 Oracle 数据提供程序（Oracle Data Provider for .NET）正确安装和添加到项目的引用中。此外，你需要根据你的实际情况修改 `connectionString` 变量的值，确保其中包含正确的主机地址、服务名、端口号、用户名和密码。

请注意，使用明文密码在代码中并不安全，这只是一个简单示例。在实际应用中，建议使用更安全的方式来存储和管理密码，例如使用配置文件、密钥管理服务或其他加密机制。

另外，此示例仅演示了连接到数据库的过程，你可以在成功连接后执行你的数据库操作，例如执行查询、插入、更新等操作。根据你的具体需求，你可以在连接成功的代码块中编写相关数据库操作的逻辑。


## 二进制数据转图片
```csharp
        /// <summary>
        /// 二进制数据转换为JPEG
        /// </summary>
        /// <param name="imageBytes">二进制图像数据</param>
        /// <param name="doctorName">医生名字</param>
        /// <returns>返回签名图路径</returns>
        private string GetSign(byte[] imageBytes, string doctorName)
        {
            string signPath = $@"D:\Files\Signature\{doctorName}.jpeg";
            if (!File.Exists(signPath))
            {
                try
                {
                    // 将二进制数据转换为Image
                    using (var stream = new MemoryStream(imageBytes))
                    {
                        using (Image image = Image.FromStream(stream))
                        {
                            // 保存为JPEG图像
                            image.Save(signPath, ImageFormat.Jpeg);
                        }
                    }
                }
                catch (Exception ex)
                {
                    signPath = "";
                    LogManager.Logger.GetInstance().Debug("转换并保存为JPEG图像时发生错误：" + ex.Message);
                }
            }
            return signPath;
        }
```
