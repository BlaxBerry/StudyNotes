# Windows Ubuntu

![](https://s3-ap-northeast-1.amazonaws.com/images.programming-beginner-zeroichi.jp/uploads/posdb.jpg)



# WSL上的环境搭建

## WSL下载PostgreSQL

```bash
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
$ sudo apt update
$ sudo apt -y install postgresql libpq-dev

#下载完毕后，检查PostgreSQL的版本
$ psql --version
psql (PostgreSQL) 12.5 (Ubuntu 12.5-0ubuntu0.20.04.1)  
```

## WSL启动PostgreSQL

1. 先启动PostgreSQL

第一次数据库前使用时要先开启。

用 **sudo /etc/init.d/postgresql start** 启动PostgreSQL

```bash
$ sudo /etc/init.d/postgresql start 
```

如果不启动PostgreSQL就输入PostgreSQL命令，会报错，如下：

> psql: could not connect to server: No such file or directory
>
> Is the server running locally and accepting
>
> connections on Unix domain socket “/tmp/.s.PGSQL.5432”?

---

2. 再连接到数据库的管理员账号

启动PostgreSQL时自动创建了一个role，叫 **psotgres**

即通过 **sudo su psotgres** 切换连接到该role

```bash
$ sudo su psotgres
```

如果不连接到role就输入PostgreSQL命令会报错，如下：

>  psql: error: FATAL:  role "chen" does not exist   

---

- **第一次使用PostgreSQL**

```bash
sudo /etc/init.d/postgresql start
[sudo] password for chen:   
* Starting PostgreSQL 12 database server                     [ OK ] 

sudo su postgres  
/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (3 rows) 
```

---

## 启动步骤

```bash
#1.切换到数据库管理员账号(必做)
sudo su postgres

#2.输入本机user密码
[sudo] password for 用户:

#进入了PostgreSQL
```

```bash
DESKTOP-46BRUEO% sudo su postgres
[sudo] password for chen:
postgres@DESKTOP-46BRUEO:/home/chen$ psql -l
                              List of databases
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges
-----------+----------+----------+---------+---------+-----------------------
 BlaxBerry | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
(4 rows)

postgres@DESKTOP-46BRUEO:/home/chen$
```





# 数据库

```bash
#1.切换到数据库管理员账号
sudo su postgres

#2.输入本机user密码
[sudo] password for chen:

进入了PostgreSQL

#4.进入数据库
psql 数据库名

#5.退出数据库
\q
```

## psql -l  显示所有数据库

```PostgreSQL
psql -l
```

```PostgreSQL
postgres@DESKTOP-46BRUEO:/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (3 rows) 
```

---

## createdb 创建数据库 

```postgreSQL
$ createdb 数据库名
```

```Buntu
$ createdb BlaxBerry
```

createdb是系统命令，不是PostgreSQL命令。

```postgreSQL
postgres@DESKTOP-46BRUEO:/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (3 rows) 
 
postgres@DESKTOP-46BRUEO:~$ createdb BlaxBerry
postgres@DESKTOP-46BRUEO:/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 BlaxBerry | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (4 rows) 
 
```

---

## dropdb 删除数据库

```postgreSQL
$ dropdb 数据库名
```

```Buntu
$ dropdb BlaxBerry
```

dropdb是系统命令，不是PostgreSQL命令。

```Ubuntu
postgres@DESKTOP-46BRUEO:/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 BlaxBerry | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (4 rows) 
 
 postgres@DESKTOP-46BRUEO:~$ dropdb BlaxBerry
 postgres@DESKTOP-46BRUEO:/home/chen$ psql -l 
                              List of databases  
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges    
-----------+----------+----------+---------+---------+-----------------------  
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |    
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres 
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +       
           |          |          |         |         | postgres=CTc/postgres 
 (3 rows) 
```

---

## psql 进入数据库

```postgreSQL
psql 数据库名
```

```postgreSQL
psql BlaxBerry
```

进入数据库后终端的表示会改变如下

```PostgreSQL
数据库名=#
```

```PostgreSQL
BlaxBerry=#
```

```postgreSQL
postgres@DESKTOP-46BRUEO:~$ psql -l
                              List of databases
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges
-----------+----------+----------+---------+---------+-----------------------
 BlaxBerry | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
(4 rows)

postgres@DESKTOP-46BRUEO:~$ psql BlaxBerry
psql (12.5 (Ubuntu 12.5-0ubuntu0.20.04.1))
Type "help" for help.

BlaxBerry=#
```

---

## \q 退出数据库

从数据库中退出。

```postgreSQL
数据库=# \q
```

```postgreSQL
BlaxBerry=# \q
postgres@DESKTOP-46BRUEO:~$
```





# 数据表

```Ubuntu
1.切换到数据库管理员账号
sudo su postgres

2.输入本机user密码
[sudo] password for chen:

进入了PostgreSQL

4.进入数据库
psql 数据库名

5.创建数据表
CREATE TABLE 数据表名
```

---

## CREATE TABLE 创建数据表

**CREATE TABLE** 在当前数据库创建一个新的空白表，

该表将由发出此命令的用户所拥有。

```postgreSQL
CREATE TABLE 表名（
	列1 数据类型，
	列2 数据类型，
	...
	PRIMARY KEY(一个列或多个)
）;
```

```postgreSQL
CREATE TABLE students (id varchar(5),name varchar(15),sex varchar(1),birth date);
```

```postgreSQL
postgres@DESKTOP-46BRUEO:/home/chen$ psql BlaxBerry
psql (12.5 (Ubuntu 12.5-0ubuntu0.20.04.1))
Type "help" for help.

BlaxBerry=# CREATE TABLE students (id varchar(5),name varchar(15),sex varchar(1),birth date);
CREATE TABLE
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)

```

---

## 字段类型（数据类型）

[菜鸟教程psql数据类型](https://www.runoob.com/postgresql/postgresql-data-type.html)

| 常用数值型数据 |                                         |             |
| -------------- | --------------------------------------- | ----------- |
| **integer**    | 整数型                                  | age integer |
| **real**       | 浮点型                                  | tax real    |
| serial         | 序列型，自增整数，每次新增记录会自动增1 | id serial   |

| 常用文字型数据    |                                       |                    |
| ----------------- | ------------------------------------- | ------------------ |
| **varchar**(长度) | 可变长字符串，长度是个字符            | title varchar(255) |
| **text**          | 大文本，无长度限制                    | content text       |
| char(长度)        | 和varchar类似，但不够的字符自动补空格 | sex char(1)        |

| 常用布尔型数据 |                                                    |      |
| -------------- | -------------------------------------------------- | ---- |
| boolean        | 可用于判断逻辑删除，**建议逻辑删除**而不是物理删除 |      |

```PostgreSQL
CREATE TABLE reports (
	id serial PRIMARY KEY,
	title varchar,
	content text ,
	is_draft boolean default TRUE, #默认是草稿
	is_del boolean default FALSE #默认不删除
);
```

| 常用日期型数据 |              |      |
| -------------- | ------------ | ---- |
| date           | 年月日       |      |
| time           | 时分秒       |      |
| timestamp      | 年月日时分秒 |      |

| 常用PostgreSQL特有的 |                            |      |
| -------------------- | -------------------------- | ---- |
| **Array**            | 数组型，可以存一个数组     |      |
| inet                 | IP地址型，可存储一个IP地址 |      |
| JSON                 |                            |      |
| XML                  |                            |      |

```postgreSQL
CREATE TABLE users (
  id integer PRIMARY KEY,
  first_name varchar,
  last_name varchar,
  birth_date date,
  sex integer,
  enrolled_at date,
  graduated_at date,
  created_at timestamp,
  updated_at timestamp
);
```

---

## DROP TABLE 删除数据表

工作中几乎不用，用时候务必要注意

```postgreSQL
DROP TABLE 表名;
```

```postgreSQL
DROP TABLE students;
```

```postgreSQL
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)

BlaxBerry=# DROP TABLE students;
DROP TABLE

BlaxBerry=# \dt
Did not find any relations.
```

---

## \dt 显示数据库中的所有数据表

显示数据库中右多少数据表，表名、数据表的类型、所有者

```PstgreSQl
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)
```

---

## \d 显示数据表结构详细信息

显示该表的详细信息，字段的数据类型等

```
\d 表名
```

```
BlaxBerry=# \d students
                     Table "public.students"
 Column |         Type          | Collation | Nullable | Default
--------+-----------------------+-----------+----------+---------
 id     | character varying(5)  |           |          |
 name   | character varying(15) |           |          |
 sex    | character varying(1)  |           |          |
 birth  | date                  |           |          |
```

---

## \i 导入sql文件

把代码写入一个 .sql文件，调用该文件后执行文件内的代码。

```
nano 自定义名.sql

\i 自定义名.sql
```

如下，写了一个创建数据表的 .sql文件，然后调用该文件，执行了创建数据表的命令

```PostgreSQL
nano students.sql

...
#进入后输入下列创建数据表命令
CREATE TABLE students (id varchar(5),name varchar(15),sex varchar(1),birth date);
...

CTRL + o  保存
CTRL + x  退出

\i  students.sql

#验证
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)
BlaxBerry=# \d students
                     Table "public.students"
 Column |         Type          | Collation | Nullable | Default
--------+-----------------------+-----------+----------+---------
 id     | character varying(5)  |           |          |
 name   | character varying(15) |           |          |
 sex    | character varying(1)  |           |          |
 birth  | date                  |           |          |
```



# 表约束

约束用于规定表中的数据规则，

即给表的字段添加些约束条件。

可在创建表 **CREATE TABLE** 时添加,

或创建完数据表后用 **ALTER TABLE** 规定

如下：

```
#没有约束，字段只有数据类型
CREATE TABLE students (
	id varchar,
	name varchar,
	sex varchar,
	birth date
);

#添加了简单的约束，比如字节长度限制,设定主键等
CREATE TABLE students (
	id seria PRIMARY KEY,
	name varchar(15),
	sex varchar(1),
	birth date
);
```

## 常用约束条件

| 代码        | 含义                                    |
| ----------- | --------------------------------------- |
| NOT NULL    | 该字段内不能存储NULL空值                |
| UNIQUE      | 该字段内的值是唯一值                    |
| PRIMARY KEY | s设定该字段为主键（NOT NULL 和 UNIQUE） |
| DEFAULT     | 该该字段设定默认值                      |
| CHECK       | 给该字段设定条件                        |

## 实例

```PostgreSQL
CREATE TABLE reports (
	id serial PRIMARY KEY,
	title varchar(45) NOT NULL,
	content text CHECK(length(content) > 10),
	upload_time timestamp DEFAULT 'now',
	copyright boolean DEFAULT TRUE，
	download——price real DEFAULT 1000 
);
```





# 数据记录

类似SQL语言，可以参照。

## INSERT INTO 插入记录

```PostgreSQL
INSERT INTO 表 (字段1，字段2,...) VALUES (字段1的值，字段2的值,...);
```

```PostgreSQL
INSERT INTO students (id,name) VALUES (2,'Red');
```

如果向表中所有的字段列都被插入值：

```PostgreSQL
INSERT TO 表 VALUES (值，值，值，...);
```

```PostgreSQLs
INSERT INTO students VALUES (1,'Andy',80);
```

---

插入记录的返回值

```PostgreSQL
INSSERT 0 n
# n表示插入的记录行数
```

```PostgreSQL
INSERT INTO students VALUES (1,'Andy',80);
INSERT 0 1
#说明插入了1行记录

INSERT TNTO students VALUES (3,'Mike'，75)，(4,'Davis'，60);
INSERT 0 2
#说明插入了2行记录
```

---

**实例**：现存有一空的数据表students

```PostgreSQL
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
(0 rows)
```

### 插入一条记录

- 插入一条记录，所有的字段都插入值

```PostgreSQL
BlaxBerry=# INSERT INTO students VALUES (1,'Andy',80);
INSERT 0 1

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
(1 row)
```

- 插入一条记录，仅指定字段插入值：

```PostgreSQL
BlaxBerry=# INSERT INTO students (id,name) VALUES (2,'Red');
INSERT 0 1

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Red  |
(2 rows)
```

---

### 插入多行记录

```PostgreSQL
INSERT TO 表 VALUES(值，值)，(值，值)，...
```

```PostgreSQL
INSERT TNTO students (id,name) VALUES (3,'Mike'，75)，(4,'Davis'，60);
```

```PostgreSQL
BlaxBerry=# INSERT INTO students VALUES (3,'Mike',75),(4,'Davis',60);
INSERT 0 2

BlaxBerry=# SELECT * FROM students;
 id | name  | score
----+-------+-------
  1 | Andy  |    80
  2 | Red   |
  3 | Mike  |    75
  4 | Davis |    60
(4 rows)
```

---

### 插入数据类型错误时

```
BlaxBerry=# \d students
                                 Table "public.students"
 Column |       Type        | Collation | Nullable |           Default
--------+-------------------+-----------+----------+------------------------------
 id     | integer           |           | not null | 
 name   | character varying |           |          |
 score  | real              |           |          |

BlaxBerry=# INSERT INTO students VALUES (TRUE,000,0);
ERROR:  column "id" is of type integer but expression is of type boolean
LINE 1: INSERT INTO students VALUES (TRUE,000,0);
                                     ^
HINT:  You will need to rewrite or cast the expression.
```



## UPDATE 更新修改记录

```PostgreSQL
UPDATE 表 SET 列 = 值，列 = 值,... WHERE 条件；
```

```PostgreSQL
UPDATE students SET name = 'Mike',score = 75 WHERE id = 2;
```

```sql
BlaxBerry=# SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 |
  2 | Red   |    80 |
  3 | Mike  |    75 |
  4 | James |    60 |
  5 | Davis |    90 |
  6 | Andy  |    65 |
(6 rows)

BlaxBerry=# UPDATE students SET country = 'USA' WHERE id IN (1,2,5);
UPDATE 3
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 |
  4 | James |    60 |
  5 | Davis |    90 | USA
  6 | Andy  |    65 |
(6 rows)

BlaxBerry=# UPDATE students SET country = 'UK' WHERE id NOT IN (1,2,5);
UPDATE 3

BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)
```

---

### 修改指定记录

利用WHERE条件指定要修改的记录。

如下：修改一条记录的内容

```PostgreSQL
BlaxBerry=#  SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Red  |
(2 rows)

BlaxBerry=# UPDATE students SET name = 'Mike',score = 75 WHERE id = 2;
UPDATE 1

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Mike |    75
(2 rows)
```

---

### 修改所有记录

若不写WHERE条件，则是修改所有记录。

如下：修改所有记录的内容

```PostgreSQL
BlaxBerry=#  SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Red  |
(2 rows)

BlaxBerry=# UPDATE students SET name = 'Mike',score = 75;
UPDATE 2

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Mike |    75
  2 | Mike |    75
(2 rows)
```





## DELETE FROM 删除记录

```PostgreSQL
DELETE FROM 表 WHERE 条件；
```

```PostgreSQL
DELETE FROM students WHERE id >=3;
```

---

### 删除指定记录

利用WHERE条件指定记录。

如下，删除指定的多条记录

```PostgreSQL
BlaxBerry=# SELECT * FROM students;
 id | name  | score
----+-------+-------
  1 | Andy  |    80
  2 | Red   |
  3 | Mike  |    75
  4 | Davis |    60
  5 | lee   |     0
(5 rows)

BlaxBerry=# DELETE FROM students WHERE id >=3;
DELETE 3

BlaxBerry=# SELECT * FROM students;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Red  |
(2 rows)
```

---

### 删除全部记录

若不写WHERE条件，则是删除所有记录。

只是删除该数据表中的所有记录，该数据表还存在于数据库中。

如下：修改所有记录的内容

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY id;
 id | name | score
----+------+-------
  1 | Andy |    80
  2 | Mike |    75
(2 rows)

BlaxBerry=# DELETE FROM students;
DELETE 2

BlaxBerry=# SELECT FROM students;
--
(0 rows)

BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)
```





## SELECT 抽出显示记录

和SQL语言基本一致。

```PostgreSQL
SELECT 要找的内容 FROM 表 WHERE 筛选条件;
```

```PostgreSQL
SELECT * FROM students;
SELECT id,name FROM students;
SELECT * FROM students WHERE score >= 60;
SELECT name FROM students WHERE score >= 60;
```

显示所有记录的所有字段：

```PostgreSQL
BlaxBerry=# SELECT * FROM students;
 id | name  | score
----+-------+-------
  1 | Andy  |    80
  2 | Red   |    88
  3 | Mike  |    75
  4 | James |    60
  5 | Davis |    90
(5 rows)
```

显示多项字段：

```PostgreSQL
BlaxBerry=# SELECT name,score FROM students;
 name  | score
-------+-------
 Andy  |    80
 Red   |    88
 Mike  |    75
 James |    60
 Davis |    90
(5 rows)
```

---

### WHERE筛选条件

显示指定字段：

```PostgreSQL
BlaxBerry=# SELECT name FROM students WHERE score >= 60;
 name
-------
 Andy
 Red
 Mike
 James
 Davis
(5 rows)
```

#### WHERE的常用逻辑运算符

和SQL语法基本一致，可参照。

```sql
SELECT * FROM students WHERE id = 6;

SELECT * FROM students WHERE id >= 6;

SELECT * FROM students WHERE id != 6;

SELECT * FROM students WHERE id IN (2,4,6);

SELECT * FROM students WHERE id NOT IN (1,3,5);

SELECT * FROM students WHERE id BETWEEN 2 AND 6;

SELECT * FROM students WHERE name = 'Andy';

SELECT * FROM students WHERE name LIKE '%And%';

SELECT * FROM students WHERE name LIKE 'An__';

SELECT * FROM students WHERE name LIKE '_ndy';

SELECT * FROM students WHERE name IN ('Andy'，'Andersion');

SELECT * FROM students WHERE name IS NOT NULL;

SELECT * FROM students WHERE name IS NULL;

```

```sql
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 |
  4 | James |    60 |
  5 | Davis |    90 | USA
  6 | Andy  |    65 |
(6 rows)

BlaxBerry=# SELECT * FROM students WHERE country IS NULL;
 id | name  | score | country
----+-------+-------+---------
  3 | Mike  |    75 |
  4 | James |    60 |
  6 | Andy  |    65 |
(3 rows)
```

---

#### 子查询

```sql
SELECT * FROM students WHERE score = (SELECT score FROM students WHERE score >= 60);

SELECT * FROM students WHERE score = (SELECT MAX(score) FROM students)
```

---

### ORDER BY 排序

#### 递减排序

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id | name  | score
----+-------+-------
  5 | Davis |    90
  2 | Red   |    88
  1 | Andy  |    80
  3 | Mike  |    75
  4 | James |    60
(5 rows)
```

#### 递增排序

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY score ASC;
 id | name  | score
----+-------+-------
  4 | James |    60
  3 | Mike  |    75
  1 | Andy  |    80
  2 | Red   |    88
  5 | Davis |    90
(5 rows)
```

---

#### 多字段排序

```
BlaxBerry=# SELECT * FROM students ORDER BY id;
 id |  name  | score
----+--------+-------
  1 | Andy   |    80
  2 | Red    |    88
  3 | Mike   |    75
  4 | James  |    60
  5 | Davis  |    90
  6 | Andyyy |    65
(6 rows)

BlaxBerry=# SELECT * FROM students ORDER BY name,score DESC;
 id |  name  | score
----+--------+-------
  1 | Andy   |    80
  6 | Andyyy |    65
  5 | Davis  |    90
  4 | James  |    60
  3 | Mike   |    75
  2 | Red    |    88
(6
```

---

### AND 和 OR关键字

```sql
BlaxBerry=# SELECT * FROM students WHERE score >= 60 AND score < 90 ORDER BY score DESC;
 id | name  | score
----+-------+-------
  2 | Red   |    88
  1 | Andy  |    80
  3 | Mike  |    75
  4 | James |    60
(4 rows)
```

```SQL
BlaxBerry=# SELECT * FROM students WHERE score <= 80 OR score >= 90 ORDER BY score DESC;
 id | name  | score
----+-------+-------
  5 | Davis |    90
  1 | Andy  |    80
  3 | Mike  |    75
  4 | James |    60
(4 rows)
```

---

### LIKE 关键字

```PostgreSQL
BlaxBerry=# SELECT * FROM students WHERE name LIKE '%a%';
 id | name  | score
----+-------+-------
  4 | James |    60
  5 | Davis |    90
(2 rows)
```

```PostgreSQL
BlaxBerry=# SELECT * FROM students WHERE name LIKE 'a_';
 id | name | score
----+------+-------
(0 rows)

BlaxBerry=# SELECT * FROM students WHERE name LIKE '_a___';
 id | name  | score
----+-------+-------
  4 | James |    60
  5 | Davis |    90
(2 rows)

BlaxBerry=# SELECT * FROM students WHERE name LIKE 'Mi_e';
 id | name | score
----+------+-------
  3 | Mike |    75
(1 row)
```

---

### LIMIT 关键字

```PostgreSQL
LIMIT n  
#截取到第n个
```

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id |  name  | score
----+--------+-------
  5 | Davis  |    90
  2 | Red    |    88
  1 | Andy   |    80
  3 | Mike   |    75
  6 | Andyyy |    65
  4 | James  |    60
(6 rows)

# 前4名
BlaxBerry=# SELECT * FROM students ORDER BY score DESC LIMIT 4;
 id | name  | score
----+-------+-------
  5 | Davis |    90
  2 | Red   |    88
  1 | Andy  |    80
  3 | Mike  |    75
(4 rows)
```

---

### OFFSET 关键字

```PostgreSQL
OFFSET n
# 从第几个后面开始取
```

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id |  name  | score
----+--------+-------
  5 | Davis  |    90
  2 | Red    |    88
  1 | Andy   |    80
  3 | Mike   |    75
  6 | Andyyy |    65
  4 | James  |    60
(6 rows)

# 从第4开始截取
BlaxBerry=# SELECT * FROM students ORDER BY score DESC OFFSET 3;
 id |  name  | score
----+--------+-------
  3 | Mike   |    75
  6 | Andyyy |    65
  4 | James  |    60
(3 rows)


# 取前4中的第3，4名
#（即，从第3开始截取，然后截取其中的前2个）
BlaxBerry=# SELECT * FROM students ORDER BY score DESC LIMIT 2 OFFSET 2;
 id | name | score
----+------+-------
  1 | Andy |    80
  3 | Mike |    75
(2 rows)
```





# 统计数据

## DISTINCT 去重

```PostgreSQL
# 去重并显示字段
SELECT DISTINCT 字段1，字段2，...
```

```PostgreSQL
SELECT DISTINCT name FROM students;
SELECT DISTINCT name,score FROM students;
```

```PostgreSQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id | name  | score
----+-------+-------
  5 | Davis |    90
  1 | Andy  |    80
  2 | Red   |    80
  3 | Mike  |    75
  6 | Andy  |    80
  4 | James |    60
(6 rows)

# 仅去重并显示一个字段
BlaxBerry=# SELECT DISTINCT name FROM students;
 name
-------
 Red
 Mike
 Andy
 James
 Davis
(5 rows)

# 去重并显示 所有字段
BlaxBerry=# SELECT DISTINCT * FROM students;
 id | name  | score
----+-------+-------
  5 | Davis |    90
  2 | Red   |    80
  4 | James |    60
  3 | Mike  |    75
  6 | Andy  |    65
  1 | Andy  |    80
(6 rows)
```

---

---

## SUM() 求和函数 

```PostgreSQL
# 求和并显示字段
SELECT SUM（字段）
```

```PostgreSQL
SELECT SUM（score）FROM students；
```

```SQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id |  name  | score
----+--------+-------
  5 | Davis  |    90
  2 | Red    |    88
  1 | Andy   |    80
  3 | Mike   |    75
  6 | Andyyy |    65
  4 | James  |    60
(6 rows)

# 求分数总和
BlaxBerry=# SELECT SUM(score) FROM students;
 sum
-----
 450
(1 row)
```

---

---

## MAX() / MIN() 求最值函数

```PostgreSQL
#取字段的最大值并显示该字段
SELECT MAX(字段)
```

```SQL
SELECT MAX(score) FROM students;
```

```PostgreSQL
#取字段的最小值并显示该字段
SELECT MIN(字段)
```

```SQL
SELECT MIN(score) FROM students;
```

```SQL
BlaxBerry=# SELECT * FROM students ORDER BY score DESC;
 id |  name  | score
----+--------+-------
  5 | Davis  |    90
  2 | Red    |    88
  1 | Andy   |    80
  3 | Mike   |    75
  6 | Andyyy |    65
  4 | James  |    60
(6 rows)

#表中的分数最大值
BlaxBerry=# SELECT MAX(score) FROM students;
 max
-----
  90
(1 row)

#表中的分数最小值
BlaxBerry=# SELECT MIN(score) FROM students;
 min
-----
  60
(1 row)
```

**利用WHERE的子查询**：

```SQL
# score字段是表中最大值的记录的全部字段
BlaxBerry=# SELECT * FROM students WHERE score = (SELECT MAX(score) FROM students);
 id | name  | score
----+-------+-------
  5 | Davis |    90
(1 row)

# score字段是表中最小值的记录的全部字段
BlaxBerry=# SELECT * FROM students WHERE score = (SELECT MIN(score) FROM students);
 id | name  | score
----+-------+-------
  4 | James |    60
(1 row)
```

---

---

## GROUP BY 分组

GROUP BY 语句和 SELECT 语句一起使用，用来对相同的数据进行分组。

```SQL
# 分组显示字段
SELECT 字段1,字段2,... GROUP BY 字段1,字段2，...;
```

---

SELECT显示的**字段的个数和顺序**，必须和GROUP BY分组的**字段的个数和顺序必须一致**

```SQL
# 按照国籍进行分组
SELECT country FROM students GROUP BY country；

# 按照国籍和姓名进行分组
SELECT country,name FROM students GROUP BY country,name；
```

```sql
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)

# 将表分组显示country字段和name字段
BlaxBerry=# SELECT country,name FROM students GROUP BY country,name;
 country | name
---------+-------
 USA     | Andy
 USA     | Red
 UK      | James
 UK      | Andy
 USA     | Davis
 UK      | Mike
(6 rows)
```

---

GROUP BY 语句**必须放在 WHERE 子句中的条件之后，必须放在 ORDER BY 子句之前**

```PostgreSQL
SELECT 字段x WHERE 条件 GROUP BY 字段x ORDER BY 字段
```

```sql
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)
 
# 将表分组显示country字段和name字段，并按country排序
BlaxBerry=# SELECT country,name FROM students GROUP BY country,name ORDER BY country;
 country | name
---------+-------
 UK      | James
 UK      | Andy
 UK      | Mike
 USA     | Andy
 USA     | Red
 USA     | Davis
(6 rows)

# 将表分组显示country、name字段、score字段，并按country和score排序
BlaxBerry=# SELECT country,name,score FROM students GROUP BY country,name,score ORDER BY country,score DESC;
 country | name  | score
---------+-------+-------
 UK      | Mike  |    75
 UK      | Andy  |    65
 UK      | James |    60
 USA     | Davis |    90
 USA     | Red   |    80
 USA     | Andy  |    80
(6 rows)

BlaxBerry=# SELECT country,name,score FROM students WHERE score >= 70 GROUP BY country,name,score ORDER BY country,score DESC;
 country | name  | score
---------+-------+-------
 UK      | Mike  |    75
 USA     | Davis |    90
 USA     | Red   |    80
 USA     | Andy  |    80
(4 rows)
```

虽然，SELECT显示的**字段的个数和顺序**，必须和GROUP BY分组的**字段的个数和顺序必须一致**

但是，要分组下显示字段和函数结果时，可以无视函数。

```sql
SELECT 字段1，函数结果，字段2，函数结果 FROM 表 GROUP BY 字段1，字段2...
```

```sql
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)

BlaxBerry=# SELECT country,MAX(score) FROM students GROUP BY country;
 country | max
---------+-----
 UK      |  75
 USA     |  90
(2 rows)
```

```sql
# 只显示导演名，和最大值
BlaxBerry=# SELECT director,MAX(length_minutes) FROM movies GROUP BY director;
     director     | max
------------------+-----
 Anderson Stanton |  95
 John Lasseter    | 117
 Peter Doctor     |  92
(3 rows)

# 注意下列，因为显示了不同的title，还是会把重名导演的显示出来
BlaxBerry=# SELECT director,title,MAX(length_minutes) FROM movies GROUP BY director,title;
     director     |    title     | max
------------------+--------------+-----
 Peter Doctor     | Monsters,Inc |  92
 John Lasseter    | Toy Story    |  81
 Anderson Stanton | WALL-E       |  95
 John Lasseter    | Cars         | 117
(4 rows)
```

---

---

## HAVING 分组的筛选条件

HAVING是针对GROUP BY的过滤条件。

WHERE是针对SELECT的过滤条件。

```PostgreSQL
GROUP BY 字段 HAVING 条件
```

HAVING 子句**必须放置于 GROUP BY 子句后面，ORDER BY 子句前面**

```SQL
SELECT 字段 FROM 表 WHERE 条件 GROUP BY 字段 HAVING 条件 ORDER BY 字段
```

```SQL
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)
BlaxBerry=# SELECT country,name,score FROM students GROUP BY country,name,score ORDER BY country,score DESC;
 country | name  | score
---------+-------+-------
 UK      | Mike  |    75
 UK      | Andy  |    65
 UK      | James |    60
 USA     | Davis |    90
 USA     | Red   |    80
 USA     | Andy  |    80
(6 rows)

BlaxBerry=# SELECT country,name,score FROM students GROUP BY country,name,score HAVING score >= 70  ORDER BY country,score DESC;
 country | name  | score
---------+-------+-------
 UK      | Mike  |    75
 USA     | Davis |    90
 USA     | Red   |    80
 USA     | Andy  |    80
(4 rows)
```

```SQL
BlaxBerry=# SELECT country,MAX(score) FROM students GROUP BY country;
 country | max
---------+-----
 UK      |  75
 USA     |  90
(2 rows)

BlaxBerry=# SELECT country,MAX(score) FROM students GROUP BY country HAVING MAX(score) >= 80;
 country | max
---------+-----
 USA     |  90
```

---

---

## 加减乘除计算

```postgreSQL
字段 + - * / 字段
字段 + - * / 数值
```

```SQL
SELECT salary,tax,salary - tax FROM company;
SELECT salary,tax,salary - 100 FROM company;

SELECT price/2 AS half_price FROM product_list WHERE price >= 1000;
```

```SQL
BlaxBerry=# select * from movies;
 id |    title     |   director    | length_minutes
----+--------------+---------------+----------------
  1 | Toy Story    | John Lassete  |             81
  2 | A Bug`s Life | John Lasseter |             95
  3 | Monsters,Inc | Peter Doctor  |             92
  4 | Cars         | John Lasseter |            117
(4 rows)

BlaxBerry=# select title,(length_minutes + 80)/60 FROM movies;
    title     | ?column?
--------------+----------
 Toy Story    |        2
 A Bug`s Life |        2
 Monsters,Inc |        2
 Cars         |        3
(4 rows)

BlaxBerry=# select id,title,length_minutes,length_minutes + id FROM movies;
 id |    title     | length_minutes | ?column?
----+--------------+----------------+----------
  1 | Toy Story    |             81 |       82
  2 | A Bug`s Life |             95 |       97
  3 | Monsters,Inc |             92 |       95
  4 | Cars         |            117 |      121
(4 rows)
```

---

---

## AVG()  求平均值函数

AVG() 函数返回数值列的平均值。

```SQL
#显示平均值
SELECT AVG(字段)
```

```SQL
SELECT AVG(score) FROM students；
```

```sql
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)

BlaxBerry=# SELECT AVG(score) AS average FROM students;
 average
---------
      75
(1 row)
```

---

---

## COUNT() 计算行数函数

### 返回匹配指定条件的记录的个数

```SQL
SELECT COUNT(字段) FROM 表 WHERE 条件
```

### 返回表中的所有记录的总个数

```SQL
SELECT COUNT(*) FROM 表;
```

```SQL
BlaxBerry=#  SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 | USA
  2 | Red   |    80 | USA
  3 | Mike  |    75 | UK
  4 | James |    60 | UK
  5 | Davis |    90 | USA
  6 | Andy  |    65 | UK
(6 rows)

BlaxBerry=# SELECT COUNT(*) FROM students;
 count
-------
     6
(1 row)

BlaxBerry=# SELECT COUNT(score) AS pass FROM students WHERE score >=70;
 pass
------
    4
(1 row)

BlaxBerry=# SELECT COUNT(country) AS students_from_usa FROM students WHERE country = 'USA';
 students_from_usa
-------------------
                 3
(1 row)
```

```SQL
BlaxBerry=# select * from movies order by id;
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  1 | Toy Story    | John Lasseter    |             81
  2 | WALL-E       | Anderson Stanton |             95
  3 | Monsters,Inc | Peter Doctor     |             92
  4 | Cars         | John Lasseter    |            117
(4 rows)

BlaxBerry=# SELECT director,COUNT(id) FROM movies GROUP BY director;
     director     | count
------------------+-------
 Anderson Stanton |     1
 John Lasseter    |     2
 Peter Doctor     |     1
(3 rows)
```



# ALTER TABLE命令 变更表结构

**ALTER TABLE** 命令用于添加，修改，删除一张已经存在表的列。

也可以用 **ALTER TABLE** 命令添加和删除约束

---

## RENAME TO 重命名

### 重命名表名

```SQL
# 把表名从名1换成名2。
ALTER TABLE 表名1 RENAME TO 表名2;
```

```SQL
ALTER TABLE students RENAME TO class202;
```

```SQL
BlaxBerry=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)

BlaxBerry=# ALTER TABLE students RENAME TO class202;
ALTER TABLE

BlaxBerry=# \d
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | class202 | table | postgres
(1 row)
```

---

### 重命名字段名

```SQL
# 把字段名从名1换成名2。
ALTER TABLE 表名  RENAME 字段名1 TO 字段名2;
```

```SQL
ALTER TABLE students RENAME age TO birth;
```

```sql
BlaxBerry=# select * from movies;
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  3 | Monsters,Inc | Peter Doctor     |             92
  4 | Cars         | John Lasseter    |            117
  1 | Toy Story    | John Lasseter    |             81
  2 | WALL-E       | Anderson Stanton |             95
(4 rows)

# 把字段length_minutes重命名为length
BlaxBerry=# alter table movies rename length_minutes to length;
ALTER TABLE

BlaxBerry=# select * from movies;
 id |    title     |     director     | length
----+--------------+------------------+--------
  3 | Monsters,Inc | Peter Doctor     |     92
  4 | Cars         | John Lasseter    |    117
  1 | Toy Story    | John Lasseter    |     81
  2 | WALL-E       | Anderson Stanton |     95
(4 rows)
```

---

## ADD 添加字段  

给现存的表中添加一列。

```SQL
ALTER TABLE 表 ADD 列 数据类型;
```

如下，给students表中添加一列country字段：

```SQL
ALTER TABLE students ADD country varchar;
```

```SQL
BlaxBerry=# SELECT * FROM students;
 id | name  | score
----+-------+-------
  1 | Andy  |    80
  3 | Mike  |    75
  4 | James |    60
  5 | Davis |    90
  6 | Andy  |    65
  2 | Red   |    80
(6 rows)

BlaxBerry=# ALTER TABLE students ADD country varchar;
ALTER TABLE

BlaxBerry=# SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 |
  2 | Red   |    80 |
  3 | Mike  |    75 |
  4 | James |    60 |
  5 | Davis |    90 |
  6 | Andy  |    65 |
(6 rows)
```

---

## DROP   删除字段

从现存的表中删除一列

```SQL
ALTER TABLE 表 DROP 列;
```

如下，从students表中删除country一列：

```SQL
ALTER TABLE students DROP country;
```

```SQL
BlaxBerry=# SELECT * FROM students ORDER BY id;
 id | name  | score | country
----+-------+-------+---------
  1 | Andy  |    80 |
  2 | Red   |    80 |
  3 | Mike  |    75 |
  4 | James |    60 |
  5 | Davis |    90 |
  6 | Andy  |    65 |
(6 rows)

BlaxBerry=# ALTER TABLE students DROP country;
ALTER TABLE

BlaxBerry=# SELECT * FROM students ORDER BY id;
 id | name  | score
----+-------+-------
  1 | Andy  |    80
  2 | Red   |    80
  3 | Mike  |    75
  4 | James |    60
  5 | Davis |    90
  6 | Andy  |    65
(6 rows)
```

---

## TYPE 修改数据类型

修改现存表中的列的数据属性。

```SQL
ALTER TABLE 表 ALTER 列 TYPE 数据类型
```

```SQL
ALTER TABLE students ALTER sex TYPE integer;
```

如下，把score字段的数据类型从real浮点型改为iinteger整数型：

```SQL
BlaxBerry=# \d movies;
                     Table "public.movies"
  Column  |       Type        | Collation | Nullable | Default
----------+-------------------+-----------+----------+---------
 id       | integer           |           |          |
 title    | character varying |           |          |
 director | character varying |           |          |
 year     | integer           |           |          |

# 把字段year的数据类型从integer改为real
BlaxBerry=# alter table movies alter year type real;
ALTER TABLE

BlaxBerry=# \d movies;
                     Table "public.movies"
  Column  |       Type        | Collation | Nullable | Default
----------+-------------------+-----------+----------+---------
 id       | integer           |           |          |
 title    | character varying |           |          |
 director | character varying |           |          |
 year     | real              |           |          |
```

```sql
BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(255) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |

# 把title的数据类型从varchar(255)换成varchar(100)，减少字符的长度
BlaxBerry=# alter table movies alter title type varchar(100);
ALTER TABLE

BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(100) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |

```

## ？添加约束

### 添加主键

```PostgreSQL

```

```PostgreSQL

```

```PostgreSQL

```



## 删除约束

```PostgreSQL

```

```PostgreSQL

```

```PostgreSQL

```





# 索引

## CREATE INDEX 添加索引

提高查找时候的效率

比如通过搜索电影名称字段查找该电影的全部信息，可以给电影名字段添加索引，实现快速查找

```SQL
CREATE INDEX 索引名 on 表(字段)；
```

```SQL
CREATE INDEX name_index on students(name)；
```

```SQL
BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(100) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |

# 给title字段和director字段添加索引
BlaxBerry=# create index title_index on movies(title);
CREATE INDEX
BlaxBerry=# create index director_index on movies(director);
CREATE INDEX

BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(100) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |
Indexes:
    "director_index" btree (director)
    "title_index" btree (title)
```

---

---

## DROP INDEX 删除索引

```SQL
DROP INDEX 索引名；
```

```SQL
DROP INDEX name_index;
```

```SQL
BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(100) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |
Indexes:
    "director_index" btree (director)
    "title_index" btree (title)

# 删除添加在title字段和director字段的索引
BlaxBerry=# drop index director_index;
DROP INDEX
BlaxBerry=# drop index title_index;
DROP INDEX

BlaxBerry=# \d movies
                       Table "public.movies"
  Column  |          Type          | Collation | Nullable | Default
----------+------------------------+-----------+----------+---------
 id       | integer                |           |          |
 title    | character varying(100) |           |          |
 director | character varying      |           |          |
 length   | integer                |           |          |
 year     | integer                |           |          |
```





# 操作多个表（表结合）

```SQL
# 表1：
# 学生姓名，学生分数。学生国籍
BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |    80 | USA     | 1
  2 | Red   |    80 | USA     | 3
  3 | Mike  |    75 | UK      | 1
  4 | James |    60 | UK      | 2
  5 | Davis |    90 | USA     | 3
  6 | Curry |    65 | UK      | 2
(6 rows)

# 表2：
# 学生的信息
BlaxBerry=# select * from information;
 id | student_id |  message
----+------------+-----------
  1 |          1 | 1-01 迟到
  2 |          2 | 1-03 早退
  3 |          3 | 1-15 打架
  4 |          1 | 1-20 旷课
  5 |          8 | 1-25 早恋
(5 rows)
```

## 外关联

连接表1和表2，要**通过共同的字段使数据指向同一记录** ，比如：表1的id  和 表2的student_id

```SQL
SELECT 字段 FROM 表1，表2，... WHERE 表1.某字段 = 表2.某字段;

SELECT * FROM 表1，表2，... WHERE 表1.某字段 = 表2.某字段;
```

```sql
#显示全部字段
BlaxBerry=# select * from students,information where students.id =information.student_id;
 id | name | score | country | class | id | student_id |  message
----+------+-------+---------+-------+----+------------+-----------
  1 | Andy |    80 | USA     | 1     |  1 |          1 | 1-01 迟到
  2 | Red  |    80 | USA     | 3     |  2 |          2 | 1-03 早退
  3 | Mike |    75 | UK      | 1     |  3 |          3 | 1-15 打架
  1 | Andy |    80 | USA     | 1     |  4 |          1 | 1-20 旷课
(4 rows)

# 显示指定的字段 并用as别名方便书写
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id;
 name | class |  message
------+-------+-----------
 Andy | 1     | 1-01 迟到
 Red  | 3     | 1-03 早退
 Mike | 1     | 1-15 打架
 Andy | 1     | 1-20 旷课
(4 rows)

# 再通过and补充筛选条件，指明id是1的人
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id and s.id = 1;
 name | class |  message
------+-------+-----------
 Andy | 1     | 1-01 迟到
 Andy | 1     | 1-20 旷课
(2 rows)

# 再通过and补充筛选条件，指明id是2的人
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id and s.id = 2;
 name | class |  message
------+-------+-----------
 Red  | 3     | 1-03 早退
(1 row)

# 再通过and补充筛选条件，指明id是3的人
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id and s.id = 3;
 name | class |  message
------+-------+-----------
 Mike | 1     | 1-15 打架
(1 row)
```





连接表1和表2，要通过共同的字段 ，表1的id  和 表2的student_id

---

---

## JOIN 连接

含义类似SQL语言，可参照。

### LEFT OUTER JOIN  左外连接

```SQL
BlaxBerry=# select * from students left outer join information on students.id = information.student_id or
der by students.id;
 id | name  | score | country | class | id | student_id |  message
----+-------+-------+---------+-------+----+------------+-----------
  1 | Andy  |    80 | USA     | 1     |  1 |          1 | 1-01 迟到
  1 | Andy  |    80 | USA     | 1     |  4 |          1 | 1-20 旷课
  2 | Red   |    80 | USA     | 3     |  2 |          2 | 1-03 早退
  3 | Mike  |    75 | UK      | 1     |  3 |          3 | 1-15 打架
  4 | James |    60 | UK      | 2     |    |            |
  5 | Davis |    90 | USA     | 3     |    |            |
  6 | Curry |    65 | UK      | 2     |    |            |
(7 rows)

BlaxBerry=# select students.id,students.name,information.message from students left outer join information on students.id = information.student_id order by students.id;
 id | name  |  message
----+-------+-----------
  1 | Andy  | 1-01 迟到
  1 | Andy  | 1-20 旷课
  2 | Red   | 1-03 早退
  3 | Mike  | 1-15 打架
  4 | James |
  5 | Davis |
  6 | Curry |
(7 rows)

```

---

### RIGHT OUTER  JOIN  右外连接

```SQL
BlaxBerry=# select * from students right outer join information on students.id = information.student_id order by students.id;
 id | name | score | country | class | id | student_id |  message
----+------+-------+---------+-------+----+------------+-----------
  1 | Andy |    80 | USA     | 1     |  1 |          1 | 1-01 迟到
  1 | Andy |    80 | USA     | 1     |  4 |          1 | 1-20 旷课
  2 | Red  |    80 | USA     | 3     |  2 |          2 | 1-03 早退
  3 | Mike |    75 | UK      | 1     |  3 |          3 | 1-15 打架
    |      |       |         |       |  5 |          8 | 1-25 早恋
(5 rows)

BlaxBerry=# select students.id,students.name,information.message from students right outer join information on students.id = information.student_id order by students.id;
 id | name |  message
----+------+-----------
  1 | Andy | 1-01 迟到
  1 | Andy | 1-20 旷课
  2 | Red  | 1-03 早退
  3 | Mike | 1-15 打架
    |      | 1-25 早恋
(5 rows)
```

---

### INNER JOIN  内连接

```SQL
BlaxBerry=# select * from students inner join information on students.id = information.student_id order b
y students.id;
 id | name | score | country | class | id | student_id |  message
----+------+-------+---------+-------+----+------------+-----------
  1 | Andy |    80 | USA     | 1     |  1 |          1 | 1-01 迟到
  1 | Andy |    80 | USA     | 1     |  4 |          1 | 1-20 旷课
  2 | Red  |    80 | USA     | 3     |  2 |          2 | 1-03 早退
  3 | Mike |    75 | UK      | 1     |  3 |          3 | 1-15 打架
(4 rows)

BlaxBerry=# select students.name,information.message from students inner join information on students.id
= information.student_id order by students.id;
 name |  message
------+-----------
 Andy | 1-01 迟到
 Andy | 1-20 旷课
 Red  | 1-03 早退
 Mike | 1-15 打架
(4 rows)
```





# 视图

**视图就是一个SELECT语句**，把常用的SELECT语句简化成一个表的对象，便于读取。

读取视图时，和从数据表中抽出记录一样，从创建的该视图中SELECT FROM 该视图

对复杂的查询代码做一个视图会简化代码书写。推荐。

## CREATE VIEW

```sql
CREATE VIEW 视图名 AS SELECT语句；
SELECT 字段 FROM 视图名；
```

比如，一个外关联，要SELECT显示需要输入特别多代码，每次要显示每次都要输入，麻烦。

```sql
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id and s.id = 1;
 name | class |  message
------+-------+-----------
 Andy | 1     | 1-01 迟到
 Andy | 1     | 1-20 旷课
(2 rows)

#直接创建一个视图对象，使其为显示外关联内容的SELECT语句
BlaxBerry=# create view Andy_view as select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id and s.id = 1;
CREATE VIEW

#显示视图和从数据表中SELECT抽出记录一样
BlaxBerry=# select * from Andy_view;
 name | class |  message
------+-------+-----------
 Andy | 1     | 1-01 迟到
 Andy | 1     | 1-20 旷课
(2 rows)
```

---

## \dv  视图信息

```sql
BlaxBerry=# \dv
          List of relations
 Schema |   Name    | Type |  Owner
--------+-----------+------+----------
 public | andy_view | view | postgres
(1 row)
```

---

## DROP VIEW

```sql
BlaxBerry=# \dv
          List of relations
 Schema |   Name    | Type |  Owner
--------+-----------+------+----------
 public | andy_view | view | postgres
(1 row)

BlaxBerry=# drop view Andy_view;
DROP VIEW

BlaxBerry=# \dv
Did not find any relations.
```







# 常用函数

[菜鸟教程 psql常用函数](https://www.runoob.com/postgresql/postgresql-functions.html)

## length() 字符串长度

```SQL
# 显示字符串的长度
SELECT length(字段)
```

```PostgreSQL
BlaxBerry=# SELECT * FROM movies;
 id |    title     |   director    | length_minutes
----+--------------+---------------+----------------
  1 | Toy Story    | John Lassete  |             81
  2 | A Bug`s Life | John Lasseter |             95
  3 | Monsters,Inc | Peter Doctor  |             92
  4 | Cars         | John Lasseter |            117

BlaxBerry=# SELECT length(title) FROM movies;
 length
--------
      9
     12
     12
      4
(4 rows)

BlaxBerry=# SELECT title,length(title) FROM movies;
    title     | length
--------------+--------
 Toy Story    |      9
 A Bug`s Life |     12
 Monsters,Inc |     12
 Cars         |      4
(4 rows)
```

---

---

## concat() 连接字符串

```SQL
# 显示连接后的字符串
SELECT concat('字符串1'，字段，'字符串2')
# 字符串1字段字符串2
```

```SQL
SELECT conact(age，'岁');
```

```SQL
BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |    80 | USA     | 1
  2 | Red   |    80 | USA     | 3
  3 | Mike  |    75 | UK      | 1
  4 | James |    60 | UK      | 2
  5 | Davis |    90 | USA     | 3
  6 | Andy  |    65 | UK      | 1
(6 rows)

BlaxBerry=# SELECT concat(class,'班 ') AS class_name FROM students;
 class_name
------------
 1班
 1班
 1班
 3班
 3班
 2班
(6 rows)

BlaxBerry=# SELECT concat(class,'班 ',name),score FROM students;
  concat   | score
-----------+-------
 1班 Andy  |    80
 1班 Mike  |    75
 1班 Andy  |    65
 3班 Davis |    90
 3班 Red   |    80
 2班 James |    60
(6 rows)
```

```sql
BlaxBerry=# select * from movies order by id;
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  1 | Toy Story    | John Lasseter    |             81
  2 | WALL-E       | Anderson Stanton |             95
  3 | Monsters,Inc | Peter Doctor     |             92
  4 | Cars         | John Lasseter    |            117
(4 rows)

BlaxBerry=# select concat((length_minutes/60),'小时') from movies;
 concat
--------
 1小时
 1小时
 1小时
 1小时
(4 rows)

BlaxBerry=# select title,concat((length_minutes/60),'小时') as length_hours from movies;
    title     | length_hours
--------------+--------------
 Monsters,Inc | 1小时
 Cars         | 1小时
 Toy Story    | 1小时
 WALL-E       | 1小时
(4 rows)
```

---

---

## substring() 切割字符串

```postgreSQL
substring(字段，从第几个字符开始，截取几个字符)
```

```SQL
substring('123456',1,2) => '12'
substring('123456',3,4) => '3456'
```

```SQL
BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |    80 | USA     | 1
  2 | Red   |    80 | USA     | 3
  3 | Mike  |    75 | UK      | 1
  4 | James |    60 | UK      | 2
  5 | Davis |    90 | USA     | 3
  6 | Andy  |    65 | UK      | 1
(6 rows)

BlaxBerry=# SELECT concat(class,'班',name) FROM students;
  concat
----------
 1班Andy
 1班Mike
 1班Andy
 3班Davis
 3班Red
 2班James
(6 rows)

BlaxBerry=# SELECT substring(concat(class,'班',name),2,5) FROM students;
 substring
-----------
 班Andy
 班Mike
 班Andy
 班Davi
 班Red
 班Jame
(6 rows)

BlaxBerry=# SELECT substring(concat(class,'班',name),3,6) FROM students;
 substring
-----------
 Andy
 Mike
 Andy
 Davis
 Red
 James
(6 rows)
```

---

---

## random() 随机数

```SQL
# 显示一个随机数
SELECT random();
```

```SQL
BlaxBerry=# select random();
       random
--------------------
 0.4303452484893491
(1 row)

BlaxBerry=# SELECT RANDOM();
       random
--------------------
 0.796426
```

---

### 随机排序

利用order by 和random()实现随机排序数据表

```SQL
BlaxBerry=# select * from movies order by random();
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  1 | Toy Story    | John Lasseter    |             81
  4 | Cars         | John Lasseter    |            117
  3 | Monsters,Inc | Peter Doctor     |             92
  2 | WALL-E       | Anderson Stanton |             95
(4 rows)

BlaxBerry=# select * from movies order by random();
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  3 | Monsters,Inc | Peter Doctor     |             92
  1 | Toy Story    | John Lasseter    |             81
  2 | WALL-E       | Anderson Stanton |             95
  4 | Cars         | John Lasseter    |            117
(4 rows)

BlaxBerry=# select * from movies order by random() limit 3;
 id |    title     |     director     | length_minutes
----+--------------+------------------+----------------
  1 | Toy Story    | John Lasseter    |             81
  3 | Monsters,Inc | Peter Doctor     |             92
  2 | WALL-E       | Anderson Stanton |             95
(3 rows)
```

---

### 乱序抽出

利用limit和random()实现乱序抽出，比如实现抽奖

```sql
BlaxBerry=# select * from movies order by random() limit 1;
 id | title |   director    | length_minutes
----+-------+---------------+----------------
  4 | Cars  | John Lasseter |            117
(1 row)

BlaxBerry=# select * from movies order by random() limit 1;
 id | title  |     director     | length_minutes
----+--------+------------------+----------------
  2 | WALL-E | Anderson Stanton |             95
(1 row)

BlaxBerry=# select * from movies order by random() limit 1;
 id |    title     |   director   | length_minutes
----+--------------+--------------+----------------
  3 | Monsters,Inc | Peter Doctor |             92
(1 row)
```

---

---

## AS 别名

处理后的字段名显示为 **?column?**

若想更换列名，需要用**as**

```sql
BlaxBerry=# select * from movies;
 id |    title     |   director    | length_minutes
----+--------------+---------------+----------------
  1 | Toy Story    | John Lassete  |             81
  2 | A Bug`s Life | John Lasseter |             95
  3 | Monsters,Inc | Peter Doctor  |             92
  4 | Cars         | John Lasseter |            117
(4 rows)

BlaxBerry=# select length_minutes/60 FROM movies;
 ?column?
----------
        1
        1
        1
        2
(4 rows)
```

```sql
SELECT 处理的字段 AS 自定义名
```

```sql
SELECT price/2 AS half_price FROM product_list WHERE price >= 1000;
```

如下，自定义名为result：

```sql
BlaxBerry=# SELECT length_minutes/60 AS result FROM movies;
 result
--------
      1
      1
      1
      1
(4 rows)

BlaxBerry=# select title,length_minutes/60 AS result FROM movies;
    title     | result
--------------+--------
 Toy Story    |      1
 A Bug`s Life |      1
 Monsters,Inc |      1
 Cars         |      1
(4 rows)
```

```sql
#显示全部字段
BlaxBerry=# select * from students,information where students.id =information.student_id;
 id | name | score | country | class | id | student_id |  message
----+------+-------+---------+-------+----+------------+-----------
  1 | Andy |    80 | USA     | 1     |  1 |          1 | 1-01 迟到
  2 | Red  |    80 | USA     | 3     |  2 |          2 | 1-03 早退
  3 | Mike |    75 | UK      | 1     |  3 |          3 | 1-15 打架
  1 | Andy |    80 | USA     | 1     |  4 |          1 | 1-20 旷课
(4 rows)

# 显示指定的字段 并用as别名方便书写
BlaxBerry=# select s.name,s.class,i.message from students as s,information as i where s.id = i.student_id;
 name | class |  message
------+-------+-----------
 Andy | 1     | 1-01 迟到
 Red  | 3     | 1-03 早退
 Mike | 1     | 1-15 打架
 Andy | 1     | 1-20 旷课
(4 rows)
```

---

---

## NOW  ()   当前时间

```SQL
#显示当前时间
SELECT NOW(); 
```

```postgreSQL
postgres@DESKTOP-46BRUEO:~$ psql BlaxBerry
psql (12.5 (Ubuntu 12.5-0ubuntu0.20.04.1))
Type "help" for help.

BlaxBerry=# SELECT NOW();
              now
-------------------------------
 2021-02-05 01:10:44.644695+09
(1 row)
```





# 事务

可以有效避免误删误该。

## begin 开始

## rollback 回滚

## commit 提交

```sql
begin；
执行1；
执行2；
执行2；
....
（感觉没有问题的场合，直接提交）
commit；
```

```sql
begin；
执行1；
执行2；
执行2；
....
（突然有了错误，执行的都不需要了场合，回滚会begin时状态）
rollback；
```

```sql
BlaxBerry=# select * from movies order by id;
 id |    title     |     director     | length | year
----+--------------+------------------+--------+------
  1 | Toy Story    | John Lasseter    |     81 |
  2 | WALL-E       | Anderson Stanton |     95 |
  3 | Monsters,Inc | Peter Doctor     |     92 |
  4 | Cars         | John Lasseter    |    117 |
(4 rows)

BlaxBerry=# begin;
BEGIN
BlaxBerry=# update students set score = 0,country = 'SP' where id = 1;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 2;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 3;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 4;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 5;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 6;
UPDATE 1

BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |     0 | SP      | 1
  2 | Red   |     0 | SP      | 3
  3 | Mike  |     0 | SP      | 1
  4 | James |     0 | SP      | 2
  5 | Davis |     0 | SP      | 3
  6 | Curry |     0 | SP      | 2
(6 rows)

BlaxBerry=# rollback;
ROLLBACK

BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |    80 | USA     | 1
  2 | Red   |    80 | USA     | 3
  3 | Mike  |    75 | UK      | 1
  4 | James |    60 | UK      | 2
  5 | Davis |    90 | USA     | 3
  6 | Curry |    65 | UK      | 2
(6 rows)

# 数据表内容并没有被更改
```

```sql
BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |    80 | USA     | 1
  2 | Red   |    80 | USA     | 3
  3 | Mike  |    75 | UK      | 1
  4 | James |    60 | UK      | 2
  5 | Davis |    90 | USA     | 3
  6 | Curry |    65 | UK      | 2
(6 rows)

BlaxBerry=# begin;
BEGIN
BlaxBerry=# update students set score = 0,country = 'SP' where id = 1;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 2;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 3;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 4;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 5;
UPDATE 1
BlaxBerry=# update students set score = 0,country = 'SP' where id = 6;
UPDATE 1

BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |     0 | SP      | 1
  2 | Red   |     0 | SP      | 3
  3 | Mike  |     0 | SP      | 1
  4 | James |     0 | SP      | 2
  5 | Davis |     0 | SP      | 3
  6 | Curry |     0 | SP      | 2
(6 rows)

BlaxBerry=# commit;
COMMIT

BlaxBerry=# select * from students order by id;
 id | name  | score | country | class
----+-------+-------+---------+-------
  1 | Andy  |     0 | SP      | 1
  2 | Red   |     0 | SP      | 3
  3 | Mike  |     0 | SP      | 1
  4 | James |     0 | SP      | 2
  5 | Davis |     0 | SP      | 3
  6 | Curry |     0 | SP      | 2
(6 rows)

# 提交后数据表内容才真正被修改
```

