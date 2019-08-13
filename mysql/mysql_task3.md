# MySQL学习笔记 task3
---
---
## 项目十
项目十要求的表格在项目九中已经建立，只需增加一列`DepartmentId`即可
```sql
ALTER TABLE employee
	ADD COLUMN DepartmentId int NOT NULL;
```

在MySQL workbench中执行`select * from employee;`,再在弹出的窗口中直接手动添加上`DepartmentId`列的数据即可。
建立`Department`表：
```sql
CREATE TABLE IF NOT EXISTS Department (
	id int NOT NULL,
	name varchar(20) NOT NULL,
	PRIMARY KEY (id)
);

INSERT INTO Department
VALUES ('1', 'IT');

INSERT INTO Department
VALUES ('2', 'Sales');
```
查询语句：
```sql
SELECT Department.name, employee.name, employee.salary
FROM employee, (
		SELECT MAX(salary) AS max_salary
		FROM employee
		GROUP BY DepartmentId
	) temp, Department
WHERE employee.Salary = temp.max_salary
	AND Department.id = employee.Departmentid
ORDER BY Department.name;
```

---
## 项目十一
建立表格:
```sql
CREATE TABLE IF NOT EXISTS seat (
	id int NOT NULL,
	student varchar(20) NOT NULL,
	PRIMARY KEY (id)
);

INSERT INTO seat
VALUES ('1', 'Abbot');

INSERT INTO seat
VALUES ('2', 'Doris');

INSERT INTO seat
VALUES ('3', 'Emerson');

INSERT INTO seat
VALUES ('4', 'Green');

INSERT INTO seat
VALUES ('5', 'Jeames');
```

_[参考LeetCode626题](https://blog.csdn.net/qq_41822173/article/details/81013370)_

查询语句：
```sql
SELECT a.id, a.student
FROM (
	SELECT CASE 
			WHEN id % 2 = 0 THEN id - 1
			WHEN id % 2 != 0
			AND id != (
				SELECT COUNT(*)
				FROM seat
			) THEN id + 1
			ELSE id
		END AS id, student
	FROM seat
) a
ORDER BY a.id;
```
---

## 项目十二
建立表格：
```sql
CREATE TABLE IF NOT EXISTS score (
	id int NOT NULL,
	score float NOT NULL,
	PRIMARY KEY (id)
);

INSERT INTO score
VALUES ('1', '3.50');

INSERT INTO score
VALUES ('2', '3.65');

INSERT INTO score
VALUES ('3', '4.00');

INSERT INTO score
VALUES ('4', '3.85');

INSERT INTO score
VALUES ('5', '4.00');

INSERT INTO score
VALUES ('6', '3.65');

```


查询语句：
```sql
SELECT t2.score, t2.rank
FROM score
	INNER JOIN 
	(
		SELECT score, @i := @i + 1 AS `rank`
		FROM (
			SELECT DISTINCT score
			FROM score
			ORDER BY score DESC
		) temp, (
				SELECT @i := 0
			) t1
	) t2
	ON score.score = t2.score
ORDER BY t2.score DESC;
```


---
## 项目十三
建立表格：
```sql
CREATE TABLE IF NOT EXISTS num (
	id int NOT NULL,
	num int NOT NULL,
	PRIMARY KEY (id)
);

INSERT INTO num
VALUES ('1', '1');

INSERT INTO num
VALUES ('2', '1');

INSERT INTO num
VALUES ('3', '1');

INSERT INTO num
VALUES ('4', '2');

INSERT INTO num
VALUES ('5', '1');

INSERT INTO num
VALUES ('6', '2');

INSERT INTO num
VALUES ('7', '2');

```

查询语句：
```sql


--不会了，先空着吧,没有思路😭。。。


```

---

## 项目十四
建立表格：

```sql
CREATE TABLE IF NOT EXISTS tree (
	id int NOT NULL,
	p_id int NULL,
	PRIMARY KEY (id)
);

INSERT INTO tree
VALUES ('1', NULL);

INSERT INTO tree
VALUES ('2', '1');

INSERT INTO tree
VALUES ('3', '1');

INSERT INTO tree
VALUES ('4', '2');

INSERT INTO tree
VALUES ('5', '2');


```
查询语句：
```sql
SELECT DISTINCT t1.id，
	CASE 
		WHEN t1.p_id IS NULL THEN 'root'
		WHEN t2.id IS NULL THEN 'leaf'
		ELSE 'inner'
	END AS type
FROM tree t1
	LEFT JOIN tree t2 ON t1.id = t2.p_id
ORDER BY t1.id;
```


---


## 项目十五
建立表格：

```sql
CREATE TABLE IF NOT EXISTS employee2 (
	id int NOT NULL,
	name varchar(20) NOT NULL,
	Department varchar(20) NOT NULL,
	ManagerId int NULL,
	PRIMARY KEY (id)
);

INSERT INTO employee2
VALUES ('101', 'John', 'A', NULL);

INSERT INTO employee2
VALUES ('102', 'Dan', 'A', '101');

INSERT INTO employee2
VALUES ('103', 'James', 'A', '101');

INSERT INTO employee2
VALUES ('104', 'Amy', 'A', '101');

INSERT INTO employee2
VALUES ('105', 'Anne', 'A', '101');

INSERT INTO employee2
VALUES ('106', 'Ron', 'B', '101');


```

查询语句：
```sql

SELECT name
FROM employee2
WHERE id IN (
	SELECT ManagerId
	FROM (
		SELECT id, name, Department, ManagerId
		FROM employee2
		WHERE ManagerId IS NOT NULL
	) a
	GROUP BY ManagerId
	HAVING COUNT(ManagerId) >= 5
);

```
---
---