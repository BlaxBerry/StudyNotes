# SQL基础

![](https://johobase.com/jb/wp-content/uploads/sql-case-logo-01.png)

SQL（**S**tructured **Q**uery **L**anguage）机构化查询语言

是**专门用来访问、处理数据库**的编程语言



- 是一门**数据库编程语言**
- 写出的代码叫**SQL语句**

- 只能在**关系型数据库**中使用



SQL可以实现：

- 查询数据（SELECT）

- 插入数据（INSERT INTO）

- 更新修改数据（UPDATE）

- 删除数据（DELETE）

- 创建新的数据库、数据表、视图...









## 注释

**--** （两个减号加空格）

```sql
-- 注释
```







## SELECT语句

用于从表中**查询数据**

执行的结果被储存在一个结果表（结里集）中

- 查找**某数据行的某一指定列**

```sql
SELECT 字段 FROM 表名 WHERE 字段=值;
```

- 查找表中**所有数据行的某一指定列**

```sql
SELECT 字段 FROM 表名;
```

- 查找**所有列**

```sql
SELECT * FROM 表名;
```

- 查询**某一数据行的多个列**

```sql
SELECT 列名1,列名2 FROM 表名 WHERE 字段=值;
```

- 查询所**有数据行的多个列**

```sql
SELECT 列名1,列名2 FROM 表名;
```

---

### AS关键字

给查询的结果字段设定别名

```sql
SELECT 字段 AS 别名 FROM 表;

SELECT 字段 AS 别名, 字段 AS 别名 FROM 表;

SELECT 字段 AS 别名 FROM 表 WHERE 字段=值;

SELECT 字段 AS 别名, 字段 AS 别名 FROM 表 WHERE 字段=值;
```

比如：

```sql
SELECT name AS studentsName, gender AS studentsGender FROM students WHERE age>=20;
```







## INSERT INTO语句

用于向表中**插入一条数据行**

```sql
INSERT INTO 表名(列1, 列2) VALUES (列1的值, 列2的值);
```

---

如下：

一个名为students的表

| id  （AI自增长） | name  (varchar) | age(varchar) |
| :--------------: | :-------------: | :----------: |
|        1         |      andy       |      20      |

向该表中插入一条数据，name是tom，age是28

```sql
INSERT INTO students (name,age) VALUES('tom', '28');
SELECT * FROM students;
```

| id  （AI自增长） | name  (varchar) | age(varchar) |
| :--------------: | :-------------: | :----------: |
|        1         |      andy       |      20      |
|        2         |       tom       |      28      |





## UPDATE语句

用于**修改表中数据行的字段**

通过WHERE语法指定要修改的列

- 修改指定的某一行的**某一个字段**：

```sql
UPDATE 表名 SET 字段=新值 WHERE 字段=值;
```

- 修改指定的某一行的**多个字段**：

```sql
UPDATE 表名 SET 字段1=值,字段2=值 WHERE 字段=值;
```

---

如下：

一个名为students的表

| id  （AI自增长） | name  (varchar) | age(varchar) |
| :--------------: | :-------------: | :----------: |
|        1         |      andy       |      20      |
|        2         |       tom       |      28      |

将id=2的数据行的name名字更新为jerry：

```sql
UPDATE syudents SET name='Jerry' WHERE id=2
```

将id=2的数据行的name名字更新为jerry，age改为80：

```sql
UPDATE syudents SET name='Jerry',age='80' WHERE id=2
```





## DELETE语句

用于**删除某一数据行**

```sql
DELETE FROM 表名 WHERE 字段=值;
```

---

如下：

一个名为students的表

| id  （AI自增长） | name  (varchar) | age(varchar) |
| :--------------: | :-------------: | :----------: |
|        1         |      andy       |      20      |
|        2         |       tom       |      28      |

从该表中删除 id=2的数据：

```sql
DELETE FROM students WHERE id=2;
```

| id  （AI自增长） | name  (varchar) | age(varchar) |
| :--------------: | :-------------: | :----------: |
|        1         |      andy       |      20      |







## WHERE子句

用于**限选择标准**

可用在SELECT、UPDATE、DELETE语句中

---

### WHERE子句中的运算符

- **=**：等于
- **<>（ != ）：**不等于
- **\>：**大于
- **<：**小于
- **\>=**：大于等于
- **<=**：小于等于
- **BETWEEN**：在某个范围内
- **LIKE**：搜索某种模式

```sql
SELECT * FROM students WHERE id=1;
SELECT * FROM students WHERE name='andy';

SELECT * FROM students WHERE id<>1;
SELECT * FROM students WHERE id!=1;
SELECT * FROM students WHERE name!='andy';

SELECT * FROM students WHERE id>1;

SELECT * FROM students WHERE id<1;

SELECT * FROM students WHERE id>=1;

SELECT * FROM students WHERE id<=1;

```

---

### AND 和 OR运算符

- **AND**：表示同时满足所有条件，相当于JavaSCript的&&
- **OR**：表示只要满足任意一个条件，相当于JavaSCript的||

如下：

显示students表中，age>20且num>=60的所有数据

```sql
SELECT * FROM students WHERE age>20 AND num>=60;
```

显示students表中，所有age>0 或num>60的数据

```sql
SELECT * FROM students WHERE age>0 OR num>60;
```





## ORDER BY子句

用于根据某字段**排序结果集的某字段或全部字段**

```sql
SELECT 字段 FROM 表 ORDER BY 字段 关键字;

SELECT 字段,字段 FROM 表 ORDER BY 字段 关键字;

SELECT * FROM 表 ORDER BY 字段 关键字;
```

---

### 升序排序

默认升序

**ASC**关键字，可省略

```sql
SELECT 字段 FROM 表 ORDER BY 字段 ASC;

SELECT 字段 FROM 表 ORDER BY 字段;
```

如：

```sql
-- 按id升序排列表所有数据
SELECT * FROM students ORDER BY id;

-- 按id升序排表的name字段
SELECT name FROM students ORDER BY id;

-- 按id升序排表的name和age字段
SELECT name,age FROM students ORDER BY id;

```

---

### 降序排列

**DESC**关键字

```sql
SELECT 字段 FROM 表 ORDER BY 字段 DESC;
```

如：

```sql
-- 按id降序排列表所有数据
SELECT * FROM students ORDER BY id DESC;

-- 按id降序排表的name字段
SELECT name FROM students ORDER BY id DESC;

-- 按id降序序排表的name和age字段
SELECT name,age FROM students ORDER BY id DESC;

```

---

### 多重排序

先按照一个字段升序/降序排列后，再按照另一个字段升序/降序排列

```sql
SELECT 字段 FROM 表 ORDER BY 字段1 关键字, 字段2 关键字;

SELECT 字段,字段 FROM 表 ORDER BY 字段1 关键字, 字段2 关键字;

SELECT * FROM 表 ORDER BY 字段1 关键字, 字段2 关键字;
```

如下：

表名是students

|  id  | name  | gender |
| :--: | :---: | :----: |
|  1   | Lili  |   1    |
|  2   |  Tom  |   2    |
|  3   | Andy  |   1    |
|  4   | Jerry |   2    |

先按照gender降序排列后，

再按照name字母顺序升序排列

```sql
SELECT * FROM students ORDER BY gender DESC, name ASC;
```

|  id  | name  | gender |
| :--: | :---: | :----: |
|  3   | Andy  |   1    |
|  1   | Lili  |   1    |
|  4   | Jerry |   2    |
|  2   |  Tom  |   2    |







## COUNT(*)函数

用于返回查询结果的总数

```sql
SELECT count(*) FROM 表;

SELECT count(*) FROM 表 WHERE 字段=值;
```

如下：

```sql
SELECT count(*) FROM students WHERE age>=20;

-- 返回符合条件的数据统计总数
-- 返回结果名是 count(*)
```

---

### AS关键字

给COUNT(*)设定别名

```sql
SELECT count(*) AS 别名 FROM 表;

SELECT count(*) AS 别名 FROM 表 WHERE 字段=值;
```

比如：

```sql
SELECT count(*) AS ageTotal FROM students WHERE age>=20;

-- 返回符合条件的数据统计总数
-- 返回结果名是 ageTotal
```







## .sql文件