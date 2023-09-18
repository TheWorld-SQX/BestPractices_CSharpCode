## SQL 优化
为常用字段创建索引，尽量命中索引，分页避免返回大量数据


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
