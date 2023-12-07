## SQL 优化
为常用字段创建索引，尽量命中索引，分页避免返回大量数据

## 分页避免大量数据 
分页  涉及的关键概念  页数PageNumber和页大小PageSize

是的，你可以使用 OFFSET FETCH 结合来实现每页显示 20 行数据。以下是一个示例 SQL 查询，以实现每页显示 20 行数据：

```sql
SELECT * FROM Orders
ORDER BY OrderDate
OFFSET 0 ROWS FETCH NEXT 20 ROWS ONLY;
```

这个查询会从 "Orders" 表中按照 "OrderDate" 列的升序排序中，检索前 20 行数据，从第 1 页开始。当用户请求下一页时，你可以通过递增 OFFSET 子句的值来获取下一页的数据。例如：

```sql
-- 第一页，显示前 20 行
SELECT * FROM Orders
ORDER BY OrderDate
OFFSET 0 ROWS FETCH NEXT 20 ROWS ONLY;

-- 第二页，显示接下来的 20 行
SELECT * FROM Orders
ORDER BY OrderDate
OFFSET 20 ROWS FETCH NEXT 20 ROWS ONLY;

-- 第三页，显示接下来的 20 行
SELECT * FROM Orders
ORDER BY OrderDate
OFFSET 40 ROWS FETCH NEXT 20 ROWS ONLY;
```

依此类推，通过调整 OFFSET 子句的值，你可以实现分页显示每页 20 行数据的功能。



## SQL Server 中，使用 `OFFSET` 和 `FETCH` 子句来实现分页查询
在 SQL Server 中，你可以使用 `OFFSET` 和 `FETCH` 子句来实现分页查询。以下是一个示例，假设有一个名为 `Patient` 的表，其中包含 `ID` 和 `Name` 列：

```sql
SELECT ID, Name
FROM Patient
ORDER BY ID  -- 你可以根据需要选择合适的排序列
OFFSET 0 ROWS  -- 跳过的行数（页数-1 * 每页行数）
FETCH NEXT 10 ROWS ONLY;  -- 每页的行数
```

上述查询表示从 `Patient` 表中按照 `ID` 列的升序顺序获取第一页（偏移量为0，表示第一页）的10行记录。你可以根据实际情况修改 `ORDER BY` 子句以及 `OFFSET` 和 `FETCH` 子句的参数。

如果你希望实现动态的分页，可以将偏移量和每页行数作为参数传递给查询。例如，使用存储过程：

```sql
CREATE PROCEDURE GetPatientsPaged
    @PageSize INT,
    @PageNumber INT
AS
BEGIN
    DECLARE @Offset INT = @PageSize * (@PageNumber - 1);

    SELECT ID, Name
    FROM Patient
    ORDER BY ID
    OFFSET @Offset ROWS
    FETCH NEXT @PageSize ROWS ONLY;
END;
```

然后，你可以调用存储过程并传递相应的参数：

```sql
EXEC GetPatientsPaged @PageSize = 10, @PageNumber = 1;
```

这将返回第一页的10行记录。



## 常见数据库管理系统对分页查询的支持
不是所有的数据库管理系统都支持 `OFFSET` 和 `FETCH` 子句，或者它们可能使用不同的语法。这种分页查询语法通常在支持 ANSI SQL 标准的主流数据库管理系统中可用，如：

- **SQL Server**: SQL Server 支持 `OFFSET` 和 `FETCH` 子句，因此你可以使用上面提到的分页查询语法。

- **MySQL**: MySQL 5.5+ 支持 `LIMIT` 和 `OFFSET` 子句，可以用于分页。

- **PostgreSQL**: PostgreSQL 支持 `OFFSET` 和 `FETCH` 子句，可以用于分页查询。

- **Oracle Database**: Oracle 12c+ 支持 `OFFSET` 和 `FETCH` 子句，用于分页查询。

但是，一些较旧的数据库或非关系型数据库可能不支持这种方式的分页查询。因此，如果你使用的是特定的数据库，最好查看该数据库的文档，了解它是否支持 `OFFSET` 和 `FETCH`，或者查看其它分页技术。在某些情况下，你可能需要使用不同的分页策略。

## 别的分页方案  使用数据库引擎的分页功能：大多数数据库引擎（如MySQL、SQL Server、PostgreSQL）都提供了内置的分页功能，例如LIMIT和OFFSET（对于MySQL）或ROW_NUMBER()窗口函数（对于SQL Server）。 
除了使用数据库引擎的内置分页功能，还可以考虑以下一些分页方案：

1. **游标分页**：这是一种在数据库中进行分页的方法，不同于常规的`LIMIT`和`OFFSET`或窗口函数。它基于游标的位置来获取下一页的数据。这种方法对于大数据集合分页效率更高，因为它不需要跳过前面的数据。

2. **Keyset Pagination（键集分页）**：这是一种基于数据行的唯一键（例如自增ID或时间戳）来执行分页的方法。每次查询时，你使用上一页结果的最后一个键作为查询条件来获取下一页的数据。这种方法不需要计算页数或跟踪偏移量，因此在性能上表现很好。

3. **分页库或工具**：许多编程语言和数据库都有专门的分页库或工具，可以简化分页操作。例如，对于C#，你可以使用PagedList或Pager库，它们提供了更简单的分页实现方法。

4. **物理分页和逻辑分页**：物理分页是指在数据库中实际限制返回的行数，而逻辑分页是获取所有结果，但只在应用程序中显示当前页。逻辑分页通常需要更多的数据库查询，但允许你在客户端进行更多的过滤和排序。

5. **缓存分页数据**：如果你的数据不经常变化，可以考虑将分页数据缓存在应用程序的内存中，以减少对数据库的查询。但要小心缓存的数据一致性问题。

6. **搜索引擎分页**：如果你的应用程序涉及到搜索，可以使用搜索引擎（如Elasticsearch、Solr等）来执行高效的分页和搜索操作。

7. **客户端分页**：在某些情况下，如果数据量不是很大，你可以在客户端进行分页，即在应用程序内存中对数据进行分页，而不是向数据库发出分页查询。这可以减轻数据库负担，但要小心性能问题。

每种分页方法都有其适用的场景和优缺点，选择合适的方法取决于你的应用程序需求、数据量和性能要求。重要的是在实际情况中评估各种选项，并选择最适合你的场景的分页方法。

## 分页
在 SQL Server 中，使用游标进行分页是一种实现分页查询的方法。游标允许你在结果集中按照一定的顺序逐行移动，从而实现对结果集的逐行处理。以下是一个使用游标进行分页的简单示例。

首先，假设有一个名为 `YourTable` 的表，包含一些数据，我们将使用游标来实现分页查询。

```sql
-- 创建存储过程
CREATE PROCEDURE GetPagedData
    @PageSize INT,
    @PageNumber INT
AS
BEGIN
    SET NOCOUNT ON;

    DECLARE @StartRow INT, @EndRow INT;

    -- 计算起始行和结束行
    SET @StartRow = (@PageNumber - 1) * @PageSize + 1;
    SET @EndRow = @PageNumber * @PageSize;

    -- 创建游标
    DECLARE pagingCursor CURSOR FOR
    SELECT Column1, Column2, ...
    FROM YourTable
    ORDER BY YourOrderByColumn;

    -- 声明变量来存储查询结果
    DECLARE @Column1 DataType1, @Column2 DataType2, ...;

    -- 打开游标
    OPEN pagingCursor;

    -- 将游标移到起始行
    FETCH ABSOLUTE @StartRow FROM pagingCursor INTO @Column1, @Column2, ...;

    -- 循环获取指定页的数据
    WHILE @@FETCH_STATUS = 0 AND @@ROWCOUNT <= @PageSize
    BEGIN
        -- 在这里处理获取到的数据，可以通过 SELECT 或其他操作

        -- 输出获取到的数据
        PRINT 'Column1: ' + CAST(@Column1 AS NVARCHAR(MAX)) + ', Column2: ' + CAST(@Column2 AS NVARCHAR(MAX));

        -- 将游标移到下一行
        FETCH NEXT FROM pagingCursor INTO @Column1, @Column2, ...;
    END

    -- 关闭游标
    CLOSE pagingCursor;
    DEALLOCATE pagingCursor;
END
```

在这个存储过程中，`@PageSize` 和 `@PageNumber` 是输入参数，表示每页的记录数和要查询的页数。游标按照指定的排序列（`YourOrderByColumn`）对结果集进行排序，并通过 `FETCH ABSOLUTE` 和 `FETCH NEXT` 命令逐行获取数据。

你可以根据你的实际表结构和需求修改上述示例中的表名、列名以及排序列。这只是一个基本的例子，实际使用时请注意性能和查询优化。在某些情况下，也可以使用 `OFFSET` 和 `FETCH` 子句来实现简单的分页，这是 SQL Server 2012 及更高版本引入的标准分页语法。


## 干了这么多年C#，后悔没早点用这种“分页”，简单/高效/易维护，比其它的分页方式强多了， 哪些方案优化 最佳实践 比较好的做法
C# 中的分页是一个常见的任务，而且确实有一些最佳实践和优化策略可以帮助你编写更简单、高效和易于维护的分页代码。以下是一些关于如何优化 C# 分页的最佳实践：

1. **使用 LINQ 分页**：如果你使用 Entity Framework 或 LINQ to SQL 等 ORM 工具，你可以使用 LINQ 来轻松地执行分页操作。这是一种非常简单和可读性高的方法，因为你可以直接在代码中指定分页逻辑，而不需要编写复杂的 SQL 查询。

   ```csharp
   var query = dbContext.SomeTable.Where(x => x.SomeCondition);
   var pagedData = query.Skip((pageNumber - 1) * pageSize).Take(pageSize).ToList();
   ```

2. **使用存储过程或视图**：如果你的应用程序需要频繁执行相同的分页查询，考虑使用存储过程或视图来封装这些查询。这可以提高性能，并减少代码的重复性。

3. **避免在数据库中拉取大量数据**：尽量在数据库中进行分页，而不是在应用程序中拉取所有数据再进行分页。这可以减轻数据库的负担，并提高性能。

4. **使用异步编程**：如果你的应用程序需要处理大量分页数据，考虑使用异步编程模型来提高性能和响应性。

5. **缓存分页结果**：如果分页数据不经常变化，可以考虑使用缓存来存储分页结果，从而减少数据库查询的次数。

6. **进行性能调优**：使用工具和技术来监测和调优查询性能，例如 Entity Framework 的性能分析工具或 SQL Server 的查询执行计划。

7. **错误处理和异常处理**：在分页操作中，务必进行错误处理和异常处理，以处理可能出现的问题，例如无效的页码或页大小。

8. **支持客户端排序和过滤**：允许客户端指定排序和过滤条件，以提供更灵活的查询选项。

9. **提供适当的界面反馈**：如果用户执行了分页操作，提供适当的界面反馈，以告知他们正在加载数据。

10. **单元测试**：编写单元测试来确保分页逻辑的正确性。这可以帮助你在代码发生更改时及早发现问题。

总之，C# 中的分页操作可以通过使用适当的工具、技术和最佳实践来优化，以确保代码的简单性、高效性和易维护性。根据你的具体应用场景，可以选择合适的方法来实现分页功能。

## 使用 LINQ 和 Entity Framework 进行分页
这段示例代码演示了如何使用 LINQ 和 Entity Framework 进行分页。让我解释一下代码的分页过程：

1. `dbContext.SomeTable`：首先，从数据库上下文 `dbContext` 中选择要分页的数据表（或实体集）。在示例中，假设数据表为 `SomeTable`。

2. `.Where(x => x.SomeCondition)`：在选定数据表后，可以使用 LINQ 的 `Where` 方法来过滤数据。这里，使用了一个条件 `x.SomeCondition`，该条件将根据你的需求来筛选数据。

3. `.Skip((pageNumber - 1) * pageSize)`：`Skip` 方法用于跳过前面的记录，以达到指定页码的起始位置。`pageNumber` 是当前页码，`pageSize` 是每页显示的记录数。计算出需要跳过的记录数 `(pageNumber - 1) * pageSize`。

4. `.Take(pageSize)`：`Take` 方法用于选择指定数量的记录，即当前页的记录数，由 `pageSize` 决定。

5. `.ToList()`：最后，将查询结果转换为列表，这将触发实际的数据库查询，并将结果返回为一个包含所需分页数据的列表。

这段代码通过 `Skip` 和 `Take` 方法来实现分页逻辑。通过计算跳过的记录数和选择的记录数，你可以在数据库中执行一个相对较小的查询，仅获取当前页的数据，从而实现了分页。

需要注意的是，此示例中使用的是 Entity Framework，但其他 ORM 工具也提供类似的功能。这种方法提供了简单和可读性高的代码，使分页操作变得更加容易。


## 视图
SQL Server中的视图（Views）是一个虚拟表，可以从一个或多个现有的表中检索数据，并将其呈现为一个单独的表。视图提供了一种方便的方式来查询和使用数据，可以隐藏底层表的复杂性，还可以加强安全性，只允许用户访问他们需要的数据。以下是SQL Server中常用的视图语句示例：

1. 创建视图：

```sql
CREATE VIEW [视图名]
AS
SELECT [列1], [列2], ...
FROM [表名]
WHERE [条件];
```

示例：

```sql
CREATE VIEW EmployeeView
AS
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE Department = 'HR';
```

2. 查看视图：

```sql
SELECT * FROM [视图名];
```

示例：

```sql
SELECT * FROM EmployeeView;
```

3. 更新视图：

请注意，视图通常用于查询数据，但在某些情况下，也可以更新数据。要更新视图，必须确保视图满足以下条件：

- 视图必须基于单个表。
- 视图中的每个列都必须直接映射到底层表的列。

示例：

```sql
-- 更新视图中的数据
UPDATE EmployeeView
SET FirstName = 'NewFirstName'
WHERE EmployeeID = 1;
```

4. 删除视图：

```sql
DROP VIEW [视图名];
```

示例：

```sql
DROP VIEW EmployeeView;
```

5. 带有联接的视图：

视图可以基于多个表的联接创建。例如：

```sql
CREATE VIEW SalesInfo
AS
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

这些是SQL Server中创建和使用视图的一些基本示例。视图是一种强大的数据库对象，可以简化数据访问和提高数据安全性，但在设计和使用时需要小心考虑性能和复杂性。

## 触发器
在SQL Server中，触发器（Triggers）是一种数据库对象，用于自动响应数据库中的特定事件，例如插入（INSERT）、更新（UPDATE）或删除（DELETE）操作。触发器可以在这些事件发生时自动执行特定的SQL代码或存储过程。SQL Server支持两种主要类型的触发器：AFTER触发器和INSTEAD OF触发器。

以下是SQL Server中创建和使用触发器的基本语法和示例：

1. **创建AFTER触发器**：

```sql
CREATE TRIGGER [触发器名称]
ON [表名]
AFTER [INSERT | UPDATE | DELETE]
AS
BEGIN
    -- 触发器的操作和逻辑
END;
```

2. **创建INSTEAD OF触发器**：

```sql
CREATE TRIGGER [触发器名称]
ON [表名]
INSTEAD OF [INSERT | UPDATE | DELETE]
AS
BEGIN
    -- 触发器的操作和逻辑
END;
```

3. **示例 - 创建AFTER触发器**：

```sql
-- 创建一个AFTER INSERT触发器，用于在向Employees表中插入新行后记录日志
CREATE TRIGGER AfterInsertEmployee
ON Employees
AFTER INSERT
AS
BEGIN
    INSERT INTO EmployeeLog (EmployeeID, Action, LogDate)
    SELECT EmployeeID, 'INSERT', GETDATE()
    FROM inserted;
END;
```

4. **示例 - 创建INSTEAD OF触发器**：

```sql
-- 创建一个INSTEAD OF DELETE触发器，用于在删除前检查权限
CREATE TRIGGER InsteadOfDeleteOrder
ON Orders
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM deleted WHERE Status = 'Shipped')
    BEGIN
        RAISEERROR('Cannot delete shipped orders.');
    END
    ELSE
    BEGIN
        DELETE o
        FROM Orders o
        INNER JOIN deleted d ON o.OrderID = d.OrderID;
    END
END;
```

5. **触发器中的特殊表**：

在触发器中，可以使用特殊的表 `inserted` 和 `deleted` 来访问与触发事件相关的数据。`inserted` 表包含了插入或更新操作后的新数据，而 `deleted` 表包含了删除或更新操作前的旧数据。您可以使用这些表来执行操作和验证更改。

需要注意的是，触发器是强大的数据库工具，但过度使用触发器可能会导致复杂性和性能问题。因此，在设计和使用触发器时需要仔细考虑，并确保其逻辑清晰和高效。同时，要了解触发器的执行顺序，因为数据库中可能有多个触发器与同一个表和事件相关联。
