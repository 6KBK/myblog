# MySQL学习笔记 task4
---
---
## 项目十六


很简单，项目12的语句稍微改动即可
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
            GROUP BY score
			ORDER BY score DESC
		) temp, (
				SELECT @i := 0
			) t1
	) t2
	ON score.score = t2.score
ORDER BY t2.score DESC;
```


---

## 项目十七

_还是没搞清楚回答率是啥，以下查询语句是找回答次数最多的一个question\_id_


建立表格：

```sql
CREATE TABLE IF NOT EXISTS survey_log (
	uid int NOT NULL,
	action varchar(20) NOT NULL,
	question_id int NOT NULL,
	answer_id int NULL,
	q_num int NOT NULL,
	timestamp int NOT NULL,
	PRIMARY KEY (`timestamp`)
);

INSERT INTO survey_log
VALUES ('5', 'show', '285', NULL, '1'
	, '123');

INSERT INTO survey_log
VALUES ('5', 'answer', '285', '124124', '1'
	, '124');

INSERT INTO survey_log
VALUES ('5', 'show', '369', NULL, '2'
	, '125');

INSERT INTO survey_log
VALUES ('5', 'skip', '369', NULL, '2'
	, '126');

```
查询语句：

```sql
SELECT t1.question_id survey_log
FROM (
	SELECT question_id, COUNT(question_id) AS a
	FROM survey_log
	WHERE answer_id IS NOT NULL
	GROUP BY question_id
) t1, survey_log
WHERE t1.question_id = survey_log.question_id
	AND survey_log.answer_id IS NOT NULL
ORDER BY t1.a
LIMIT 0, 1;

```

## 项目十八

插入数据：

```sql
insert into employee values ('5','Janet','69000',null,'1');
insert into employee values ('6','Randy','85000',null,'1');

```
数据如下：
| id | name  | salary | managerId | DepartmentId |
----|-------|--------|-----------|--------------|
|  1 | Joe   |  70000 |         3 |            1 |
|  2 | Henry |  80000 |         4 |            2 |
|  3 | Sam   |  60000 |      NULL |            2 |
|  4 | Max   |  90000 |      NULL |            1 |
|  5 | Janet |  69000 |      NULL |            1 |
|  6 | Randy |  85000 |      NULL |            1 |

6 rows in set (0.00 sec)


查询语句：
```sql
SELECT t2.name AS Department, t1.name AS Employee, t1.Salary
FROM employee t1
	INNER JOIN department t2, (
		SELECT a1.name
		FROM employee a1
			INNER JOIN employee a2 ON a1.departmentid = a2.departmentid
		WHERE a1.salary <= a2.salary
		GROUP BY a1.name
		HAVING COUNT(a1.name) <= 3
	) t3
WHERE t1.DepartmentId = t2.id
	AND t1.name = t3.name
ORDER BY t1.DepartmentId, t1.salary DESC;
```


## 项目十九
又不会了，好复杂

建立表格：

```sql


```
查询语句：

```sql

```

## 项目二十

头疼
╯︿╰
建立表格

```sql


```
查询语句：

```sql

```




























