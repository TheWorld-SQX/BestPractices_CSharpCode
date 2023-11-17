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

## 另一个示例
当你希望查询数据并将结果保存到 `List<string>` 和 `string` 变量中时，你需要使用 `OracleDataReader` 来执行查询并处理查询结果。以下是修改后的示例代码：

```csharp
using System;
using System.Collections.Generic;
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

                string queryString = "SELECT COLUMN_NAME FROM YOUR_TABLE_NAME"; // 修改为你的查询语句
                using (OracleCommand command = new OracleCommand(queryString, connection))
                {
                    using (OracleDataReader reader = command.ExecuteReader())
                    {
                        List<string> columnNames = new List<string>();
                        while (reader.Read())
                        {
                            string columnName = reader.GetString(0); // 修改为适当的索引，以匹配查询结果的列
                            columnNames.Add(columnName);
                        }

                        // 在这里可以处理 columnNames，例如将其保存到 list<string> 和 string 变量中
                    }
                }

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

在这个示例中，你需要修改 `YOUR_TABLE_NAME` 为你要查询的表的名称，并确保适当地修改索引以匹配你的查询结果的列。在 `OracleDataReader` 的循环内部，你可以读取每一行的数据并将其添加到 `List<string>` 中。请根据你的实际需求进行适当的修改。


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
## post第三方接口
```csharp
        /// <summary>
        /// 回传打印次数
        /// </summary>
        /// <param name="reqProcId">检查号</param>
        /// <param name="sourceId">数据源ID</param>
        /// <param name="hospitalCode">医疗机构代码</param>
        /// <param name="times">打印次数</param>
        /// <returns></returns>
        static void PostBackPrintedCount(string reqProcId, string sourceId, string hospitalCode, string times)
        {
            // 构建请求 URL
            string baseUrl = "http://172.23.8.209:5656";
            string apiUrl = "/api/PatientSearch/SavePrintTimes";
            string url = $"{baseUrl}{apiUrl}";
            // 构建请求参数
            var requestData = new
            {
                reqProcId,
                sourceId,
                hospitalCode,
                times
            };

            try
            {
                using (var client = new HttpClient())
                {
                    // 使用 JsonConvert 将请求参数转换成 JSON 字符串
                    string jsonContent = JsonConvert.SerializeObject(requestData);
                    var content = new StringContent(jsonContent, Encoding.UTF8, "application/json");

                    // 发送 POST 请求
                    var response = client.PostAsync(url, content).Result;
                    response.EnsureSuccessStatusCode();

                    // 处理响应
                    var responseBody = response.Content.ReadAsStringAsync().Result;
                    logger.Debug($"PostBackPrintedCount({jsonContent})=>Response: {responseBody}");
                }
            }
            catch (HttpRequestException ex)
            {
                logger.Error($"PostBackPrintedCount()=>请求发生错误: {ex.Message}");
            }
        }

```
## 查询数据
```csharp
        public override List<T> Query<T>(string sql, List<SqlParameter> cmdParms)
        {
            List<T> result = new List<T>() { };
            using (SqlConnection connection = new SqlConnection(connectStr))
            using (SqlCommand cmd = new SqlCommand())
            {
                PrepareCommand(cmd, connection, sql, cmdParms);
                using (SqlDataAdapter da = new SqlDataAdapter(cmd))
                using (DataSet ds = new DataSet())
                {
                    da.Fill(ds, "ds");
                    if (ds.Tables[0]?.Rows?.Count == 0)
                    {
                        return result;
                    }
                    result = DataTable2Object<T>(ds.Tables[0]);
                }
            }
            return result;
        }
```
这个代码片段是一个泛型方法，用于执行 SQL 查询并将结果映射为类型 `T` 的对象列表。我们来逐步解释代码的功能：

1. `public override List<T> Query<T>(string sql, List<SqlParameter> cmdParms)`: 这是一个公共方法，它重写了某个基类（或接口）中的方法，并且是一个泛型方法。它接受一个 SQL 查询字符串 `sql` 和一个包含 `SqlParameter` 的列表 `cmdParms` 作为输入参数，并返回一个类型为 `T` 的对象列表 `List<T>`。

2. `List<T> result = new List<T>() { };`: 在方法开始时，创建一个空的 `List<T>` 对象 `result`，用于保存查询结果。这里的花括号表示集合的初始化器，但因为没有添加任何元素，所以这个列表是空的。

3. 使用 `using` 语句创建 `SqlConnection` 和 `SqlCommand` 对象，并连接到数据库。

4. `PrepareCommand(cmd, connection, sql, cmdParms);`: 调用 `PrepareCommand` 方法，该方法可能是一个自定义的方法，用于设置 `SqlCommand` 对象的属性，包括 SQL 查询字符串和相关的参数。

5. 使用 `using` 语句创建 `SqlDataAdapter` 和 `DataSet` 对象。

6. `da.Fill(ds, "ds");`: 使用 `SqlDataAdapter` 执行 SQL 查询并将结果填充到 `DataSet` 中，其中 "ds" 是数据集的名称。

7. `if (ds.Tables[0]?.Rows?.Count == 0)`: 这里使用了条件语句来检查 `DataSet` 的第一个表是否为空（没有行）。如果为空，则说明查询没有返回任何结果，直接返回空的 `result` 列表。

8. 如果查询结果不为空（即有行数据），则通过调用 `DataTable2Object<T>(ds.Tables[0])` 方法，将 `DataSet` 中的第一个表转换为类型 `T` 的对象列表，并将结果保存在 `result` 变量中。

9. 返回 `result` 列表，其中包含从数据库中查询到的类型为 `T` 的对象列表。

需要注意的是，`DataTable2Object<T>(ds.Tables[0])` 方法没有在这个代码片段中提供，因此无法了解它是如何将 `DataTable` 转换为 `List<T>` 的。这可能是一个自定义方法，根据 `T` 的类型进行相应的映射处理。

## PrepareCommand
```csharp
        protected override void PrepareCommand(object command, object connection, string cmdText, List<SqlParameter> cmdParms)
        {
            SqlConnection conn = connection as SqlConnection;
            SqlCommand cmd = command as SqlCommand;
            if (conn.State != ConnectionState.Open)
            {
                conn.Open();
            }
            cmd.Connection = conn;
            cmd.CommandText = cmdText;
            cmd.CommandType = CommandType.Text;//cmdType;
            if (cmdParms != null)
            {
                foreach (System.Data.SqlClient.SqlParameter parameter in SqlParameterConverter(cmdParms))
                {
                    if ((parameter.Direction == ParameterDirection.InputOutput || parameter.Direction == ParameterDirection.Input) &&
                        (parameter.Value == null))
                    {
                        parameter.Value = DBNull.Value;
                    }
                    cmd.Parameters.Add(parameter);
                }
            }
        }

```
这段代码是一个重写的方法，用于准备一个 SQL 命令（`SqlCommand`）并设置相关的属性，以便执行数据库查询。让我们逐步解释这个方法的功能：

1. `protected override void PrepareCommand(object command, object connection, string cmdText, List<SqlParameter> cmdParms)`: 这是一个受保护的方法，重写了某个基类（或接口）中的方法。它接受四个参数：`command` 是一个 `SqlCommand` 对象，`connection` 是一个 `SqlConnection` 对象，`cmdText` 是要执行的 SQL 命令文本，`cmdParms` 是一个包含 `SqlParameter` 的列表，表示要使用的参数。

2. `SqlConnection conn = connection as SqlConnection;`: 将 `connection` 参数转换为 `SqlConnection` 对象，因为它可能是一个通用的 `object` 类型。

3. `SqlCommand cmd = command as SqlCommand;`: 将 `command` 参数转换为 `SqlCommand` 对象，同样因为它也是一个通用的 `object` 类型。

4. `if (conn.State != ConnectionState.Open) { conn.Open(); }`: 检查连接状态是否不是 `Open`（已打开），如果不是，则打开数据库连接。

5. `cmd.Connection = conn;`: 将 `SqlCommand` 对象的 `Connection` 属性设置为刚刚打开的数据库连接 `conn`。

6. `cmd.CommandText = cmdText;`: 设置 `SqlCommand` 对象的 `CommandText` 属性，将要执行的 SQL 命令文本赋值给它。

7. `cmd.CommandType = CommandType.Text;`: 设置 `SqlCommand` 对象的 `CommandType` 属性为 `CommandType.Text`，表示要执行的命令是一个文本类型的 SQL 命令。

8. `if (cmdParms != null) { ... }`: 检查传入的参数列表 `cmdParms` 是否为非空，如果不为空，则继续处理其中的参数。

9. `foreach (System.Data.SqlClient.SqlParameter parameter in SqlParameterConverter(cmdParms)) { ... }`: 这里使用 `SqlParameterConverter` 方法来将传入的 `cmdParms` 参数列表转换为 `System.Data.SqlClient.SqlParameter` 参数列表。在 `foreach` 循环中，对于每个参数，进行如下处理：

   - 检查参数的 `Direction` 属性，如果是输入输出或输入方向，并且参数的值为 `null`，则将参数的值设置为 `DBNull.Value`，表示数据库中的 NULL 值。

   - 将处理后的参数添加到 `SqlCommand` 对象的 `Parameters` 集合中。

总体来说，这个方法是用来准备 `SqlCommand` 对象的，并将查询需要的参数添加到命令中。它还负责打开数据库连接，并将连接和文本命令与参数关联起来，以便后续执行数据库查询。


## DataTable2Object
```csharp
        protected List<T> DataTable2Object<T>(DataTable table) where T : new()
        {
            List<T> result = new List<T>();
            Type itemType = typeof(T);
            string propertyName = "";
            try
            {
                foreach (DataRow row in table.Rows)
                {
                    T item = new T();
                    foreach (DataColumn column in table.Columns)
                    {
                        PropertyInfo p = itemType.GetProperty(column.ColumnName);
                        if (p == null || !p.CanWrite)
                        {
                            continue;
                        }
                        propertyName = p.Name;
                        object value = row[column.ColumnName];
                        value = value is DBNull ? null : value;
                        p.SetValue(item, value);
                    }
                    result.Add(item);
                }
            }
            catch (Exception e)
            {
                result.Clear();
                throw new Exception($"{e.Message}, property name is {propertyName}");
            }
            
            return result;
        }

```
这段代码是一个泛型方法 `DataTable2Object<T>`，用于将传入的 `DataTable` 对象转换为类型 `T` 的对象列表。让我们逐步解释这个方法的功能：

1. `protected List<T> DataTable2Object<T>(DataTable table) where T : new()`: 这是一个受保护的泛型方法。它接受一个 `DataTable` 对象 `table` 作为输入参数，并返回一个类型为 `T` 的对象列表 `List<T>`。泛型约束 `where T : new()` 表示类型 `T` 必须有一个无参的公共构造函数。

2. `List<T> result = new List<T>();`: 创建一个空的 `List<T>` 对象 `result`，用于保存转换后的对象列表。

3. `Type itemType = typeof(T);`: 获取类型 `T` 的 `Type` 对象，以便后续使用反射操作。

4. `string propertyName = "";`: 声明一个字符串变量 `propertyName`，用于记录在转换过程中可能发生异常的属性名。

5. 在 `try` 块中，对 `DataTable` 的每一行进行遍历。

6. `T item = new T();`: 在每一行数据开始时，创建一个类型 `T` 的新对象 `item`，该对象将用于存储该行数据转换后的结果。

7. 在内部循环中，对 `DataTable` 的每一列进行遍历。

8. `PropertyInfo p = itemType.GetProperty(column.ColumnName);`: 使用反射获取类型 `T` 中名为 `column.ColumnName` 的属性的 `PropertyInfo` 对象 `p`。

9. `if (p == null || !p.CanWrite) { continue; }`: 检查属性 `p` 是否为 `null` 或不可写入（即没有公共的 set 方法），如果是，则继续下一次循环。

10. `propertyName = p.Name;`: 将当前属性的名称记录到 `propertyName` 变量中，以便在后续可能的异常处理中进行提示。

11. `object value = row[column.ColumnName];`: 获取当前行中名为 `column.ColumnName` 的列的值。

12. `value = value is DBNull ? null : value;`: 检查值是否为 `DBNull`，如果是，则将其替换为 `null`。

13. `p.SetValue(item, value);`: 将获取的值 `value` 设置到对象 `item` 的属性 `p` 中。

14. 将转换后的对象 `item` 添加到 `result` 列表中。

15. 如果在转换过程中发生异常，将清空 `result` 列表，并抛出带有错误信息的异常，其中包含异常发生时的属性名称。

16. 返回包含转换后对象的 `result` 列表。

这个方法使用了反射来动态地将 `DataTable` 中的数据映射到类型 `T` 的对象中，前提是类型 `T` 必须具有一个无参的公共构造函数。它遍历每一行数据，根据列名获取相应的属性，并将数据赋值给属性，最终返回一个由转换后的对象组成的列表。

## 比较文件修改时间
```charp
(File.GetLastWriteTime(path.Replace(".jpg", string.Format("_{0}.jpg", quality))) < File.GetLastWriteTime(path))
```


## 时间转换

```csharp
if (!pacsData.UPDATE_DATE.HasValue)
                {
                    if (DateTime.TryParseExact(pacsData.ADUITTIME, "yyyy-MM-dd HH:mm:ss", CultureInfo.InvariantCulture, DateTimeStyles.None, out DateTime result))
                    {
                        pacsData.UPDATE_DATE = result ;
                    }
                    else
                    {
                        pacsData.UPDATE_DATE = DateTime.MinValue;
                    }
                }
```


## webservice超时
在处理Web服务调用的重试机制时，以下是一些最佳实践和建议：

1. **限制重试次数**：确定在哪些情况下应该触发重试，并设置最大的重试次数。通常，3到5次重试是一个合理的范围，但具体取决于你的应用程序需求和服务稳定性。避免无限重试，以防止无限循环。

2. **设置重试间隔**：在重试之间设置适当的时间间隔，以避免连续快速的重试请求。重试间隔可以在每次重试之间逐渐增加，以减轻服务负载。

3. **指数退避策略**：采用指数退避策略，即每次重试之间的时间间隔呈指数增加。这有助于减轻服务的压力，防止同时发起大量请求。例如，第一次重试等待1秒，第二次等待2秒，第三次等待4秒，以此类推。

4. **记录重试操作**：在记录日志中包含重试操作的信息，以便后续分析和故障排除。记录哪些请求触发了重试以及重试的结果。

5. **设置最大超时时间**：在进行重试时，考虑设置最大超时时间。如果总共的重试时间超过了最大超时时间，可以中止重试并处理超时情况。

下面是一个示例，演示如何在C#中实现一个简单的重试机制：

```csharp
async Task<string> CallWebServiceWithRetryAsync()
{
    int maxRetries = 3;
    TimeSpan retryInterval = TimeSpan.FromSeconds(1);
    int currentRetry = 0;

    while (currentRetry < maxRetries)
    {
        try
        {
            string responseXML = await pUB0008.QueryRisSelfPrintReportAsync(QueryRisSelfPrintReportInputTime(start.ToString("yyyy-MM-dd HH:mm:ss"), end.ToString("yyyy-MM-dd HH:mm:ss")));
            // 成功获取响应
            return responseXML;
        }
        catch (TimeoutException)
        {
            // 超时异常，等待一段时间后重试
            await Task.Delay(retryInterval);
            currentRetry++;
        }
        catch (WebException)
        {
            // Web异常，等待一段时间后重试
            await Task.Delay(retryInterval);
            currentRetry++;
        }
        catch (Exception)
        {
            // 其他异常，处理或记录日志
            throw;
        }
    }

    // 达到最大重试次数，处理重试失败情况
    throw new Exception("重试失败");
}
```

在上述示例中，我们定义了最大重试次数、重试间隔和一个重试循环来尝试调用Web服务。如果在最大重试次数内未成功获取响应，将抛出异常以处理重试失败情况。

请根据你的具体需求和服务稳定性，调整重试次数和间隔以达到最佳的重试效果。此外，还可以根据服务的错误响应进行更精细的重试策略。


## 
如果你是调用服务的一方，无法修改WebService并且需要实现超时重连，以下是一种推荐的方法：

1. **使用异步方法**：确保你的WebService调用是异步的，这样可以更好地处理超时情况，避免阻塞主线程。

2. **设置超时时间**：在调用WebService的地方，使用`Task.Delay`来实现自定义的超时机制。这样，你可以在指定的时间内等待服务响应，然后重试或处理超时情况。

```csharp
async Task<string> CallWebServiceWithTimeoutAsync()
{
    int maxRetries = 3; // 最大重试次数
    TimeSpan timeout = TimeSpan.FromSeconds(30); // 超时时间为30秒

    for (int retry = 0; retry < maxRetries; retry++)
    {
        var cts = new CancellationTokenSource();
        var delayTask = Task.Delay(timeout, cts.Token);
        
        var webServiceTask = pUB0008.QueryRisSelfPrintReportAsync(QueryRisSelfPrintReportInputTime(start.ToString("yyyy-MM-dd HH:mm:ss"), end.ToString("yyyy-MM-dd HH:mm:ss")));
        
        // 等待第一个任务完成，要么是服务响应，要么是超时
        var completedTask = await Task.WhenAny(webServiceTask, delayTask);
        
        if (completedTask == webServiceTask)
        {
            // 成功获取响应
            return await webServiceTask;
        }
        else
        {
            // 超时处理
            Log.Error("Web service call timed out.");
            cts.Cancel(); // 取消等待任务
        }
    }

    // 达到最大重试次数，处理重试失败情况
    throw new Exception("重试失败");
}
```

在上述代码中，我们使用`Task.Delay`来设置超时时间，并使用`Task.WhenAny`来等待第一个任务完成，要么是WebService响应，要么是超时。如果超时，我们取消等待任务并进行重试，最多重试指定次数。

这种方法可以让你在无法修改WebService的情况下实现超时和重连，同时避免在异常处理中进行业务逻辑。你可以根据需要调整超时时间和重试次数。


## pdf转图片
pdfiumviewer 开源；依赖pdfium不开源，需要x86和x32的pdfium.dll  
依赖项配置示例图  
![wpf示例](https://github.com/TheWorld-SQX/BestPractices_CSharpCode/blob/master/Image/pdfiumviewer%E4%BE%9D%E8%B5%96%E9%85%8D%E5%A5%97.jpg)
