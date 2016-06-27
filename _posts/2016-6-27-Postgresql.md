---
layout:  post
title: PostgreSQL
tags: pg doc
description: PostgreSQL学习
comments: true
---

### 前言

PostgreSQL是一个开源的关系数据库。现在更强大，支持Json，还支持其他更多的。主流系统都支持。这个说明不错[传送门](http://www.tutorialspoint.com/postgresql/postgresql_data_types.htm).支持SQL标准的绝大部分，并且提供了很多现在特性：

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

	