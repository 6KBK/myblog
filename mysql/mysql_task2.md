# MySQL学习笔记 task2

打开学习用的数据库：
```sql
use test;
```
---
---
## 项目三 超过5名学生的课（难度：简单）
建立表格并填入数据：
```sql
CREATE TABLE IF NOT EXISTS courses (
	student VARCHAR(1) NOT NULL,
	class VARCHAR(20) NOT NULL
);

INSERT INTO courses
VALUES ("A", "Math");

INSERT INTO courses
VALUES ("B", "English");

INSERT INTO courses
VALUES ("C", "Math");

INSERT INTO courses
VALUES ("D", "Biology");

INSERT INTO courses
VALUES ("E", "Math");

INSERT INTO courses
VALUES ("F", "Computer");

INSERT INTO courses
VALUES ("G", "Math");

INSERT INTO courses
VALUES ("H", "Math");

INSERT INTO courses
VALUES ("I", "Math");

INSERT INTO courses
VALUES ("A", "Math");
```
查询语句如下：
```sql
SELECT test3.class
FROM (
	SELECT DISTINCT student, class
	FROM courses
) test3
GROUP BY test3.class
HAVING COUNT(test3.class) >= 5;

```
</br>

*子查询生成的表必须有别名*





-- ----------------------------------------------------


## 项目四：交换工资（难度：简单）
建立表格：
```sql
CREATE TABLE IF NOT EXISTS salary (
	id int NOT NULL,
	name VARCHAR(20) NOT NULL,
	sex VARCHAR(1) NOT NULL,
	salary int NOT NULL
);

SELECT *
FROM salary;

INSERT INTO salary
VALUES ("1", "A", "m", "2500");

INSERT INTO salary
VALUES ("2", "B", "f", "1500");

INSERT INTO salary
VALUES ("3", "C", "m", "5500");

INSERT INTO salary
VALUES ("4", "D", "f", "500");
```
update 语句
```sql
UPDATE salary
SET sex = CASE sex
	WHEN 'm' THEN 'f'
	ELSE 'm'
END;
```
注意！！！
我这里使用MySQL workbench 执行该语句时报错，因为该update语句没有where 子句，而workbench处于安全模式下，无法执行该语句。

-- Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column.  To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.

解决方法：打开右上角齿轮按钮，弹出preferences dialog,找到sal editor 选项，取消勾选 safe mode 并重启软件，即可解决该问题。

---


## 项目五：有趣的电影 （难度：简单）

```sql
CREATE TABLE IF NOT EXISTS cinema (
	id int NOT NULL,
	movie VARCHAR(20) NOT NULL,
	description VARCHAR(40) NOT NULL,
	rating float NOT NULL
);

INSERT INTO cinema
VALUES ('1', 'War', 'great 3D', '8.9');

INSERT INTO cinema
VALUES ('2', 'Science', 'fiction', '8.5');

INSERT INTO cinema
VALUES ('3', 'irish', 'boring', '6.2');

INSERT INTO cinema
VALUES ('4', 'Ice song', 'Fantacy', '8.6');

INSERT INTO cinema
VALUES ('5', 'House card', 'Interesting', '9.1');
```

查询语句如下：
```sql
SELECT id, movie, description, rating
FROM cinema
WHERE description != 'boring'
	AND id % 2 != 0
ORDER BY rating;

```













-- -------------------------------------------------------

## 项目六：组合两张表 （难度：简单）


```sql
CREATE TABLE IF NOT EXISTS Person (
	Personid int NOT NULL,
	Firstname VARCHAR(20) NOT NULL,
	Lastname VARCHAR(20) NOT NULL,
	PRIMARY KEY (`Personid`)
);

CREATE TABLE IF NOT EXISTS Address (
	AddressId int NOT NULL,
	Personid int NOT NULL,
	City VARCHAR(20) NOT NULL,
	State VARCHAR(20) NOT NULL,
	PRIMARY KEY (`AddressId`)
);

INSERT INTO Person
VALUES ('1', 'smith', 'alex');

INSERT INTO Person
VALUES ('2', 'johnson', 'david');

INSERT INTO Person
VALUES ('3', 'miller', 'ryan');

INSERT INTO Address
VALUES ('11', '1', 'NY city', 'NY');

INSERT INTO Address
VALUES ('12', '2', 'Los Angeles', 'California');

INSERT INTO Address
VALUES ('13', '3', 'Chicago', 'Illinois');
```
查询语句如下：
```sql
SELECT FirstName, LastName, City, State
FROM Address, Person
WHERE person.Personid = address.Personid;
```


























---
## 项目七：删除重复的邮箱（难度：简单）

建立表格的代码task1中已经给出
这一个没能自己写出来
要细细研究



```sql

DELETE p1 FROM Person p1,
    Person p2
WHERE
    p1.Email = p2.Email AND p1.Id > p2.Id
```
作者：LeetCode
链接：https://leetcode-cn.com/problems/two-sum/solution/shan-chu-zhong-fu-de-dian-zi-you-xiang-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。















---

## 项目八：从不订购的客户 （难度：简单）



建立表格：
```sql
CREATE TABLE IF NOT EXISTS Customers (
	id int NOT NULL,
	name VARCHAR(20) NOT NULL,
	PRIMARY KEY (`id`)
);

CREATE TABLE IF NOT EXISTS Orders (
	id int NOT NULL,
	CustomerId int NOT NULL,
	PRIMARY KEY (`CustomerId`)
);

INSERT INTO Customers
VALUES ('1', ' Joe');

INSERT INTO Customers
VALUES ('2', ' Henry');

INSERT INTO Customers
VALUES ('3', ' Sam');

INSERT INTO Customers
VALUES ('4', ' Max ');

INSERT INTO Orders
VALUES ('1', '3');

INSERT INTO Orders
VALUES ('2', '1');
```


查询语句如下：
```sql
SELECT Customers.name AS Customers
FROM Customers
	LEFT JOIN Orders ON Customers.id = Orders.CustomerId
WHERE orders.CustomerId IS NULL;
```

注意：
>-- 参考资料：https://blog.csdn.net/jinzheng069/article/details/25075705
-- 在外连接中，on里的属性等于具体值的条件，当涉及到的属性是主表的时候，这个条件其实无法发挥筛选作用的。
--    或者用一种方式精确表达，对于外连接+on条件情况时，先用on里的条件筛选，再补on中条件中筛选掉的主表里的属性。

-- ------------------------------------------------------------------------



## 项目九：超过经理收入的员工（难度：简单）


很简单的自联结
```sql

CREATE TABLE IF NOT EXISTS Employee (
	id int NOT NULL,
	name VARCHAR(20) NOT NULL,
	salary int NOT NULL,
	managerId int NULL,
	PRIMARY KEY (`id`)
);

INSERT INTO Employee
VALUES ('1', 'Joe', '70000', '3');

INSERT INTO Employee
VALUES ('2', 'Henry', '80000', '4');

INSERT INTO Employee
VALUES ('3', 'Sam', '60000', NULL);

INSERT INTO Employee
VALUES ('4', 'Max', '90000', NULL);
```
sql查询语句：
```sql
SELECT e1.name AS Employee
FROM Employee e1
	LEFT JOIN Employee e2 ON e1.ManagerId = e2.id
WHERE e1.salary > e2.Salary;
```