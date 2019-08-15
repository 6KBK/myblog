**项目三 **超过5名学生的课
![图片](https://uploader.shimo.im/f/GAdGMjryEYY0KghU!thumbnail)

**项目四 **交换工资交换工资有个小坑，就是故意设置了一个重复的数据。所以需要进行去重处理再计数。
![图片](https://uploader.shimo.im/f/B1ZqzFlyL80Rj6nm!thumbnail)

**项目五 **有趣的电影![图片](https://uploader.shimo.im/f/KxYX67bvmjYZQhTB!thumbnail)

# **项目六 **组合两张表
![图片](https://uploader.shimo.im/f/QAQvsEgabTEMjdcz!thumbnail)
# 项目七：删除重复的邮箱
连接两张Person表，筛选出重复且Id较大的记录，然后删除。
| ** 12 **   | ** DELETE p1 FROM Person AS p1 JOIN Person AS p2 ON p1.Email = p2.Email AND p1.Id > p2.Id; **   | 
|----|----|
# 项目八：从不订购的客户
可以输出id没有出现在Orders表格CustomerId字段中的客户名字：
| ** 12 **   | ** SELECT Name AS Customers FROM Customers WHERE Id NOT IN(SELECT CustomerId FROM Orders); **   | 
|----|----|
也可以把Customers表左连接（left join）Orders表，然后输出对应订单id为nulll的客户名字：
| ** 123 **   | ** SELECT Name AS Customers FROM Customers AS cLEFT JOIN Orders AS o ON c.Id = o.CustomerIdWHERE o.Id IS NULL; **   | 
|----|----|
# 项目九：超过经理收入的员工
连接（join）两张表，一个作为员工，一个作为上级。然后用WHERE找出符合条件的员工名字。
| ** 123 **   | ** SELECT e.Name AS Employee FROM Employee AS eJOIN Employee AS m ON e.ManagerId = m.IdWHERE e.Salary > m.Salary; **   | 
|----|----|

