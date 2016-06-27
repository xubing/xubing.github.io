---
layout:  post
title: PostgreSQL
tags: pg doc
description: PostgreSQL学习,PostgreSQL is a powerful, open source object-relational database system.
comments: true
---

### 前言

PostgreSQL is a powerful, open source object-relational database system.
PostgreSQL是一个开源的关系数据库。现在更强大，支持Json，还支持其他更多的。主流系统都支持。这个说明不错[传送门](http://www.tutorialspoint.com/postgresql/postgresql_data_types.htm). 传送门的文章有些地方是错误的，已经要认真看。比如删除trigger部分。有疑问的地方一定按照PG官方的pdf文档作为标准。
PG支持SQL标准的绝大部分，并且提供了很多现在特性：

- Complex SQL queries
- SQL Sub-selects
- Foreign keys
- Trigger
- Views
- Transactions
- Multiversion concurrency control (MVCC)
- Streaming Replication (as of 9.0)
- Hot Standby (as of 9.0)

### 安装

- Installing PostgreSQL on Linux/Unix
	- 官网下载
	
	```
	
	chmod +x postgresql-9.2.4-1-linux-x64.run 
	./postgresql-9.2.4-1-linux-x64.run
	su - postgres
	createdb testdb
	service postgresql restart
	
	```
- Mac下更简单，下载dmg，安装。

### 基本类型

支持类型很多，而且用户可以通过`CREATE TYPE SQL command`来创建类型。

- Numberic Types
	- smallint 2 bytes
	- integer 4
	- bigint 8
	- decimal variable		
	- numeric variable
	- real 4 bytes
	- double precision 8
	- smallserial 2 bytes  1-32767
	- serial 4 bytes
	- bigserial  8 bytes
- Monetary Type 货币类型 money	8 bytes
- 字符类型 Character Types	
	- character varying(n), varchar(n). variable-length with limit
	- character(n), char(n) .fixed-length, blank padded 。固定长度，空格补全
	- text variable unlimited length
- Binary Data Types 二进制
	- bytea。1 or 4 bytes plus the actual binary string	variable-length binary string
- 日期时间类型 Date/Time Types
	- timestamp [without time zone ] 8 bytes。
	- timestamp [with time zone ] 8 bytes。
	- date 4 bytes。
	- time [ (p)] [ without time zone ] 8 bytes。
	- time [ (p)] with time zone 8 bytes。
	- interval [fields ] [(p) ] 12 bytes。
- Boolean . 1 byte	
- Enumerated Type。这个类型需要创建，例如： `CREATE TYPE week AS ENUM ('Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun');`
- 几何类型 Geometric Type
	- Point  16 bytes
	- line 32
	- lseg 32
	- box 32 ((x1,y1),(x2,y2))
	- path 16+16n Closed path ((x1,y1),...)
	- path 16+16n Open path [(x1,y1),...]
	- polygon 40+16n ((x1,y1),...)
	- circle 24  <(x,y),r> 
- Network Address Type	
	- cidr 7 or 19 bytes	IPv4 and IPv6 networks
	- inet 7 or 19 bytes	IPv4 and IPv6 hosts and networks
	- macaddr	6 bytes	MAC addresses
- Bit String.这个用来存储bit masks。都是0或1. 有 bit(n)  bit varying(n)
- Text 搜索类型 Text Search Type	
	- tsvector
	- tsquery
- UUID 。大小写敏感32位字符串。
- XML 类型
- Json 类型 。很好很强大，可以替代很多东西。
- 数组类型 Array。PG提供了定义一个table的列作一个可变长度维度的数组的方法。。数组可以内建或者定义。

{% highlight sql %}	
CREATE TABLE monthly_savings (
   name text,
   saving_per_quarter integer[],
   scheme text[][]
);
or

CREATE TABLE monthly_savings (
   name text,
   saving_per_quarter integer ARRAY[4],
   scheme text[][]
);


SELECT name FROM monhly_savings WHERE saving_per_quarter[2] > saving_per_quarter[4];

UPDATE monthly_savings SET saving_per_quarter = '{25000,25000,27000,27000}'
WHERE name = 'Manisha';

UPDATE monthly_savings SET saving_per_quarter = ARRAY[25000,25000,27000,27000]
WHERE name = 'Manisha';

{% endhighlight sql %}	
- 合成组合类型 Composite Types

创建方法

{% highlight sql %}	
CREATE TYPE inventory_item AS (
   name text,
   supplier_id integer,
   price numeric
);
{% endhighlight sql %}	

使用方法：

{% highlight sql %}	
CREATE TABLE on_hand (
   item inventory_item,
   count integer
);

INSERT INTO on_hand VALUES (ROW('fuzzy dice', 42, 1.99), 1000);
SELECT (item).name FROM on_hand WHERE (item).price > 9.99;
SELECT (on_hand.item).name FROM on_hand WHERE (on_hand.item).price > 9.99;

{% endhighlight sql %}	

- 范围类型 Range Types
	- int4range
	- int8range
	- numrange
	- tsrange
	- tstzrange
	- daterange
- 对象标识符类型 Object Identifier Types
	- oid
	- regproc
	- regprocedure
	- regoper
	- regoperator
	- regclass
	- regtype
	- regconfig
	- regdictionary
- Pseudo 类型  这一系列类型有特殊的目的。它不能用在数据列，但是可以作为函数参数或者结果类型使用。不列举了。很多。

### 创建DB

直接命令

```
CREATE DATABASE testdb;

check use \l

```
### 选择DB

用\l进行检查，用 \c dbname进行选择数据库

### 删除数据库
DROP DATABASE testdb
### 创建Table

CREATE TABLE COMPANY
确认是否创建成功 用 \d
使用\d tablename查看具体创建信息
### 删除Table
DROP TABLE table_name;

### Schema

### 约束Constraints

约束可以作用在列或者表的level。列等级的约束仅仅作用于一列，表等级约束作用于整表。下面是一般用在PG中的约束：

+ NOT NULL Constraint: Ensures that a column cannot have NULL value.
+ UNIQUE Constraint: Ensures that all values in a column are different.
+ PRIMARY Key: Uniquely identifies each row/record in a database table.
+ FOREIGN Key: Constrains data based on columns in other tables.
{% highlight sql %}		
	CREATE TABLE COMPANY6(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
{% endhighlight sql %}	
+ CHECK Constraint: The CHECK constraint ensures that all values in a column satisfy certain conditions.

{% highlight sql %}		
	CREATE TABLE COMPANY5(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL    CHECK(SALARY > 0)
);
{% endhighlight sql %}	

+ EXCLUSION Constraint: The EXCLUDE constraint ensures that if any two rows are compared on the specified column(s) or expression(s) using the specified operator(s), not all of these comparisons will return TRUE.

{% highlight sql %}		
	CREATE TABLE COMPANY7(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT ,
   AGE            INT   ,
   ADDRESS        CHAR(50),
   SALARY         REAL,
   EXCLUDE USING gist
   (NAME WITH =,
   AGE WITH <>)
);

{% endhighlight sql %}	


删除约束  ``` ALTER TABLE table_name DROP CONSTRAINT some_name; ```

### Schema 

创建

{% highlight sql %}	
create table myschema.company(                                                                                    ID   INT              NOT NULL,                                                                                       NAME VARCHAR (20)     NOT NULL,                                                                                       AGE  INT              NOT NULL,                                                                                       ADDRESS  CHAR (25) ,                                                                                                  SALARY   DECIMAL (18, 2),                                                                                             PRIMARY KEY (ID)                                                                                                      );
{% endhighlight sql %}	

查看 

{% highlight sql %}	
 select * from myschema.company; 
{% endhighlight sql %}	

### INSERT Query
### SELECT Query
### Operator in PostgreSQL
操作符就是保留字或者用在where句子中可以执行操作的字符。比如比较，算术运算符。
PG中主要有:
- 算术运算符
	比如 a = 2 , b =3 
	- ＋
	- -
	- *
	- /
	- %   a%b = 2
	- ^  指数 a^b =8
	- |/ 平方根  |/25 = 5
	- ||/ 立方根 ||/27 =3
	- !/ factorial   5!=120
	- !! factorial   !!5!=120
	
	
- 比较
	- =
	- !=
	- <> 
	- **>**
	- <
	- **>=**
	- <= 
	
- 逻辑运算符 AND NOT OR
- Bitwise运算符 & ,|, ~,<< >>.  比如 p＝0011  q ＝ 0011 p&q = 0011. A<<2 move left 2.  
- **#**是XOR

### LIKE
like 操作符主要根据模式去匹配一个文本。如果匹配成功，返回true，也就是1.Like的操作符 **%**,**_**。
- %代表 0 ，1，或者多个数字 字母
- _ 代表一个简单的额数字或者字符

{% highlight sql %}	
SELECT FROM table_name
WHERE column LIKE 'XXXX%'

or

SELECT FROM table_name
WHERE column LIKE '%XXXX%'

or

SELECT FROM table_name
WHERE column LIKE 'XXXX_'

or

SELECT FROM table_name
WHERE column LIKE '_XXXX'

or

SELECT FROM table_name
WHERE column LIKE '_XXXX_'

WHERE SALARY::text LIKE '%200%'

WHERE SALARY::text LIKE '2_%_%'
WHERE SALARY::text LIKE '2___3' // 2 3中间5个字母 
{% endhighlight sql %}	

### LIMIT
经常用限制select语句返回的数量。

{% highlight sql %}	
SELECT * FROM COMPANY LIMIT 4;
SELECT * FROM COMPANY LIMIT 3 OFFSET 2;
{% endhighlight sql %}	

### ORDER BY
经常用在基于某列的生序或者降序的数据排列。

{% highlight sql %}	
SELECT * FROM COMPANY ORDER BY AGE ASC;
SELECT * FROM COMPANY ORDER BY NAME, SALARY ASC;
SELECT * FROM COMPANY ORDER BY NAME DESC;
{% endhighlight sql %}	

### GROUP BY
经常用在集合中，对输出进行剔除冗余数据。

{% highlight sql %}	
 SELECT NAME, SUM(SALARY)
         FROM COMPANY GROUP BY NAME ORDER BY NAME DESC;
{% endhighlight sql %}	

### WITH Clause
在PG中，with查询在大量查询的时候提供了一种利的方法。它把复杂大的查询拆解成更简单的，更容易阅读的form。

{% highlight sql %}	
With CTE AS
(Select
 ID
, NAME
, AGE
, ADDRESS
, SALARY
FROM COMPANY )
Select * From CTE;


eg2:
WITH RECURSIVE t(n) AS (
    VALUES (0)
    UNION ALL
    SELECT SALARY FROM COMPANY WHERE SALARY < 20000
)
SELECT sum(n) FROM t;

{% endhighlight sql %}	

### HAVING Clause
HAVING Clause可以允许我们在满足条件的函数结果中拿出特殊的行。
语法格式：

{% highlight sql %}	
SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2

eg:
SELECT NAME FROM COMPANY GROUP BY name HAVING count(name) > 1;
{% endhighlight sql %}	

### DISTINCT Keyword
 DISTINCT keyword 主要剔除select语句的重复记录，获取唯一记录。

{% highlight sql %}	 
 SELECT DISTINCT name FROM COMPANY;		
{% endhighlight sql %}	


### Joins
Join是一种通过两个表的值进行组合fields的方法。
常用的:
- The CROSS JOIN
 如果输入table有x ，y列，结果是x+y列。	
	
- The INNER JOIN
   这个就是我们常用的满足条件的列才会进行组合一起。
- The LEFT OUTER JOIN
	
	这个是INNER JOIN的扩展。SQL 标准定义了3种Outer Joins类型：left right，full。PG都支持。
	In case of LEFT OUTER JOIN, an inner join is performed first. Then, for each row in table T1 that does not satisfy the join condition with any row in table T2, a joined row is added with null values in columns of T2. Thus, the joined table always has at least one row for each row in T1.
	
	{% highlight sql %}	
	SELECT EMP_ID, NAME, DEPT FROM COMPANY LEFT OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
	{% endhighlight sql %}	
	
- The RIGHT OUTER JOIN
	
	First, an inner join is performed. Then, for each row in table T2 that does not satisfy the join condition with any row in table T1, a joined row is added with null values in columns of T1. This is the converse of a left join; the result table will always have a row for each row in T2.

{% highlight sql %}	
SELECT EMP_ID, NAME, DEPT FROM COMPANY RIGHT OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
{% endhighlight sql %}	

- The FULL OUTER JOIN

	First, an inner join is performed. Then, for each row in table T1 that does not satisfy the join condition with any row in table T2, a joined row is added with null values in columns of T2. Also, for each row of T2 that does not satisfy the join condition with any row in T1, a joined row with null values in the columns of T1 is added.

{% highlight sql %}	
SELECT EMP_ID, NAME, DEPT FROM COMPANY FULL OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
{% endhighlight sql %}	

### UNION Clause

 UNION 是用来将2个或者多个的查询结果组合成没有任何重复行的结果。为了使用UNION，每一个select必须用选择相同数目的列，相同数目的列表大师，相同的数据类型，并且相同的顺序，但是不必要又相同的长度。

{% highlight sql %}	
  SELECT EMP_ID, NAME, DEPT FROM COMPANY INNER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID
   UNION
     SELECT EMP_ID, NAME, DEPT FROM COMPANY LEFT OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
{% endhighlight sql %}	 
UNION ALL 允许有重复行。

### NULL 

   NULL是一个代表缺失值的term。一个在table中的NULL值表现就是一个空。NULL是一个特殊值，不同于zero或者spcae。
   基本语法：
   {% highlight sql %}
   CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);

   {% endhighlight sql %}

###  ALIAS

ALIAS 别名。  

 {% highlight sql %}
 table 上的语法：
 SELECT column1, column2....
FROM table_name AS alias_name
WHERE [condition];

column：
SELECT column_name AS alias_name
FROM table_name
WHERE [condition];

eg:

SELECT C.ID AS COMPANY_ID, C.NAME AS COMPANY_NAME, C.AGE, D.DEPT
        FROM COMPANY AS C, DEPARTMENT AS D
        WHERE  C.ID = D.EMP_ID;
 {% endhighlight sql %}
       

### Triggers
PG Triggers 是数据库的回调函数，当特定数据库时间发生的时候自动调用执行。下面是几个关于PG Triggers的要点：

+ PostgreSQL trigger can be specified to fire before the operation is attempted on a row (before constraints are checked and the INSERT, UPDATE or DELETE is attempted); or after the operation has completed (after constraints are checked and the INSERT, UPDATE, or DELETE has completed); or instead of the operation (in the case of inserts, updates or deletes on a view).

+ A trigger that is marked FOR EACH ROW is called once for every row that the operation modifies. In contrast, a trigger that is marked FOR EACH STATEMENT only executes once for any given operation, regardless of how many rows it modifies.

+ Both the WHEN clause and the trigger actions may access elements of the row being inserted, deleted or updated using references of the form NEW.column-name and OLD.column-name, where column-name is the name of a column from the table that the trigger is associated with.

+ If a WHEN clause is supplied, the PostgreSQL statements specified are only executed for rows for which the WHEN clause is true. If no WHEN clause is supplied, the PostgreSQL statements are executed for all rows.

+ If multiple triggers of the same kind are defined for the same event, they will be fired in alphabetical order by name.

+ The BEFORE, AFTER or INSTEAD OF keyword determines when the trigger actions will be executed relative to the insertion, modification or removal of the associated row.

+ Triggers are automatically dropped when the table that they are associated with is dropped.

+ The table to be modified must exist in the same database as the table or view to which the trigger is attached and one must use just tablename, not database.tablename.

+ A CONSTRAINT option when specified creates a constraint trigger. This is the same as a regular trigger except that the timing of the trigger firing can be adjusted using SET CONSTRAINTS. Constraint triggers are expected to raise an exception when the constraints they implement are violated..
 
 
{% highlight sql %}
 CREATE  TRIGGER trigger_name [BEFORE|AFTER|INSTEAD OF] event_name
ON table_name
[
 -- Trigger logic goes here....
];

eg:

CREATE OR REPLACE FUNCTION auditlogfunc() RETURNS TRIGGER AS $example_table$
    BEGIN
        INSERT INTO AUDIT(EMP_ID, ENTRY_DATE) VALUES (new.ID, current_timestamp);
        RETURN NEW;
    END;
$example_table$ LANGUAGE plpgsql;

CREATE TRIGGER example_trigger AFTER INSERT ON COMPANY
FOR EACH ROW EXECUTE PROCEDURE auditlogfunc();


{% endhighlight sql %}

- 查看监听TRIGGERS
	
	```
	SELECT * FROM pg_trigger;
	```
- 删除 TRIGGERS .这个是pdf文档看到的正确写法。

	{% highlight sql %}
	DROP TRIGGER [ IF EXISTS ] name ON table_name [ CASCADE | RESTRICT ]
	{% endhighlight sql %}


### INDEXES

用来db搜索加快数据检索的特殊lookup 表。简单说，INDEXES存放就是一个指向数据的指针的表。数据库中的index类似book的目录。

index会加快select ，where的检索，但是会降低update insert的输入。INdexes也可以是唯一的，类似UNIQUE约束。
{% highlight sql %}
CREATE INDEX index_name ON table_name;

单列
CREATE INDEX index_name
ON table_name (column_name);

多列
CREATE INDEX index_name
ON table_name (column1_name, column2_name);


删除 
DROP INDEX index_name;
{% endhighlight sql %}

- 什么时候金庸indexes?
 	- Indexes should not be used on small tables.
	- Tables that have frequent, large batch update or insert operations.
	- Indexes should not be used on columns that contain a high number of NULL values.
	- Columns that are frequently manipulated should not be indexed.
	
### ALTER TABLE
### TRUNCATE TABLE 
### VIEWS

Views are pseudo-tables.That is, they are not real tables.

{% highlight sql %}
CREATE [TEMP | TEMPORARY] VIEW view_name AS
SELECT column1, column2.....
FROM table_name
WHERE [condition];

DROP VIEW view_name
{% endhighlight sql %}

### TRANSACTIONS

- Transaction Control:
	- BEGIN TRANSACTION: to start a transaction。使用BEGIN TRANSACTION 或 BEGIN 
	- COMMIT: to save the changes, alternatively you can use END TRANSACTION command.
	- ROLLBACK: to rollback the changes.
	
{% highlight sql %}
 BEGIN;
DELETE FROM COMPANY WHERE AGE = 24;
ROLLBACK;

{% endhighlight sql %}

### LOCKS
锁，执行锁 或写入锁都会阻止用户去修改一行或者整个表。

### AUTO INCREMENT

PG有smallserial, serial and bigserial类型。它们类似其他数据库中的Auto increment.

{% highlight sql %}
	CREATE TABLE COMPANY(
   ID  SERIAL PRIMARY KEY,
   NAME           TEXT      NOT NULL,
   AGE            INT       NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
{% endhighlight sql %}

### PRIVILEGES

To allow other roles or users to use it, privileges or permission must be granted.

{% highlight sql %}
	
	Syntax for GRANT
	
	GRANT privilege [, ...]
ON object [, ...]
TO { PUBLIC | GROUP group | username }

eg:
GRANT ALL ON COMPANY TO manisha;


Syntax for REVOKE:

REVOKE privilege [, ...]
ON object [, ...]
FROM { PUBLIC | GROUP groupname | username }

eg:
REVOKE ALL ON COMPANY FROM manisha
{% endhighlight sql %}


### DATE/TIME

几个重要相关函数AGE(), CURRENT DATE/TIME()  DATE_PART() EXTRACT() ISFINITE() JUSTIFY。

 - AGE()

{% highlight sql %}
 SELECT AGE(timestamp '2001-04-10', timestamp '1957-06-13');
 {% endhighlight sql %}
 	
 - CURRENT DATE/TIME()  
 	CURRENT_DATE  CURRENT_TIME CURRENT_TIMESTAMP LOCALTIME LOCALTIMESTAMP
 - DATE_PART() DATE_TRUNC
 	 {% highlight sql %}
 	 select date_part('day',CURRENT_TIMESTAMP);
 	  {% endhighlight sql %}
 - EXTRACT() 
 - ISFINITE() 
 -JUSTIFY

### Functions

PG的Functions 比较有名的存储过程。

**Syntax**:

{% highlight sql %}
CREATE [OR REPLACE] FUNCTION function_name (arguments) 
RETURNS return_datatype AS $variable_name$
  DECLARE
    declaration;
    [...]
  BEGIN
    < function_body >
    [...]
    RETURN { variable_name | value }
  END; LANGUAGE plpgsql;
 {% endhighlight sql %}
 
### Useful Functions

 - COUNT
 - MAX MIN
 - AVG
 - SUM
 - ARRAY
 - Numeric
 - String
  