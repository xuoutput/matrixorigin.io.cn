# **数据类型**

MatrixOne 的数据类型与MySQL数据类型的定义一致，可参考：
<https://dev.mysql.com/doc/refman/8.0/en/data-types.html>

## **整数类型**

|  数据类型   | 存储空间  |  最小值  | 最大值  |
|  ----  | ----  |  ----  | ----  |
| TINYINT  | 1 byte | 	-128  | 127 |
| SMALLINT  | 2 byte | -32768  | 32767 |
| INT  | 4 byte | 	-2147483648	  | 2147483647 |
| BIGINT  | 8 byte | -9223372036854775808	  | 9223372036854775807 |
| TINYINT UNSIGNED | 1 byte | 0	  | 255 |
| SMALLINT UNSIGNED | 2 byte | 0	  | 65535 |
| INT UNSIGNED | 4 byte | 0	  | 4294967295 |
| BIGINT UNSIGNED | 8 byte | 0	  | 18446744073709551615 |

## **浮点类型**

|  数据类型   | 存储空间  |  精度  | 语法表示 |
|  ----  | ----  |  ----  | ----  |
| FLOAT32  | 4 byte | 	23 bits  | FLOAT |
| FLOAT64  | 8 byte |  53 bits  | DOUBLE |

## **字符串类型**

|  数据类型  | 存储空间  | 语法表示 | 描述|
|  ----  | ----  |   ----  |----  |
| char      | 24 byte  |CHAR| 定长字符串 |
| varchar   | 24 byte  |VARCHAR| 变长字符串|
| text      | 1073741824 bytes| TEXT |长文本数据|

## **二进制类型**

|  数据类型  | 存储空间  | 语法表示 | 描述|
|  ----  | ----  |   ----  |----  |
| tinyblob  | 255 bytes   |TINYBLOB|不超过 255 个字符的二进制字符串|
| blob      | 65535 bytes   |BLOB|二进制形式的长文本数据|
| mediumblob| 16777215 bytes  |MEDIUMBLOB|二进制形式的中等长度文本数据|
| longblob  | 4294967295 bytes  |LONGBLOB|二进制形式的极大文本数据|

## **JSON 数据类型**

|JSON 值数据类型| 语法表示 |
|---|---|
|数值| INT、FLOAT|
|字符串| CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT、TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB |
|Bool| TRUE、FALSE|
|数组| 一个由零或多个值组成的有序序列。每个值可以为任意类型。数组使用 `[]` 括起来，元素之间用逗号 `,` 分隔。 |
|时间与日期|Date、Datetime|
|对象|对象：一个由零或者多个键值对组成的无序集合。其中键必须是字符串，值可以为任意类型。对象使用 `{}` 括起来，键值对之间使用逗号 `,` 分隔，键与值之间用冒号 `:` 分隔。|
|空值| NULL|

## **时间与日期**

|  数据类型  | 存储空间  | 精度 |  最小值   | 最大值  | 语法表示 |
|  ----  | ----  |   ----  |  ----  | ----  |   ----  |
|  Time  | 8 byte  |   time  |  -2562047787:59:59.999999 | 2562047787:59:59.999999  |   hh:mm:ss.ssssss  |
| Date  | 4 byte | day | 0001-01-01  | 9999-12-31 | YYYY-MM-DD/YYYYMMDD |
| DateTime  | 8 byte | second | 0001-01-01 00:00:00.000000  | 9999-12-31 23:59:59.999999 | YYYY-MM-DD hh:mi:ssssss |
| TIMESTAMP|8 byte|second|0001-01-01 00:00:00.000000|9999-12-31 23:59:59.999999|YYYYMMDD hh:mi:ss.ssssss|

## **Bool**

|  数据类型  | 存储空间  |
|  ----  | ----  |
| True  | 1 byte |
|False|1 byte|

## **定点类型Decimal(Beta)**

|  数据类型   | 存储空间  |  精度   | 语法表示 |
|  ----  | ----  |  ----  | ----  |
| Decimal  | 8 byte | 	19位  | Decimal(N,S) <br> N 表示数字位数的总数，范围是(1 ~ 18)，小数点和 -（负数）符号不包括在 N 中<br>S表示是小数点（标度）后面的位数，范围是(0 ~ N)<br>如果 S 是 0，则值没有小数点或分数部分。如果 S 被省略，默认是 0。如果 N 被省略，默认是 1。 <br>例如Decimal(10,8)，即表示数字总长度为10，小数位为8。|
| Decimal  | 16 byte | 	38位  | Decimal(N,S) <br> N 表示数字位数的总数，范围是(18 ~ 38)，小数点和 -（负数）符号不包括在 N 中<br>S表示是小数点（标度）后面的位数，范围是(0 ~ N)<br>如果 S 是 0，则值没有小数点或分数部分。如果 S 被省略，默认是 0。如果 N 被省略，默认是 18。<br>例如Decimal(20,19)，即表示数字总长度为20，小数位为19。 |

## **示例**

```sql
//Create a table named "numtable" with 3 attributes of an "int", a "float" and a "double"
> create table numtable(id int,fl float, dl double);

//Insert a dataset of int, float and double into table "numtable"
> insert into numtable values(3,1.234567,1.2345678912345678912);

// Create a table named "numtable" with 2 attributes of an "int" and a "float" up to 5 digits in total, of which 3 digits may be after the decimal point.
> create table numtable(id int,fl float(5,3));

//Insert a dataset of int, float into table "numtable"
> insert into numtable values(3,99.123);

//Create a table named "numtable" with 2 attributes of an "int" and a "float" up to 23 digits in total.
> create table numtable(id int,fl float(23));

//Insert a dataset of int, float into table "numtable"
> insert into numtable values(1,1.2345678901234567890123456789);

//Create a table named "numtable" with 4 attributes of an "unsigned tinyint", an "unsigned smallint", an "unsigned int" and an "unsigned bigint"
> create table numtable(a tinyint unsigned, b smallint unsigned, c int unsigned, d bigint unsigned);

//Insert a dataset of unsigned (tinyint, smallint, int and bigint) into table "numtable"
> insert into numtable values(255,65535,4294967295,18446744073709551615);

//Create a table named "names" with 2 attributes of a "varchar" and a "char"
> create table names(name varchar(255),age char(255));

//Insert a data of "varchar" and "char" into table "names"
> insert into names(name, age) values('Abby', '24');

//Create a table named "calendar" with 2 attributes of a "date" and a "datetime"
> create table calendar(a date, b datetime);

//Insert a data of "date" and "datetime" into table "calendar"
> insert into calendar(a, b) values('20220202', '2022-02-02 00:10:30');
> insert into calendar(a, b) values('2022-02-02', '2022-02-02 00:10:30');


//Create a table named "decimalTest" with 2 attribute of a "decimal" and b "decimal"
> create table decimalTest(a decimal(6,3), b decimal(24,18));
> insert into decimalTest values(123.4567, 123456.1234567891411241355);
> select * from decimalTest;
+---------+---------------------------+
| a       | b                         |
+---------+---------------------------+
| 123.456 | 123456.123456789141124135 |
+---------+---------------------------+

//Create a table named "booltest" with 2 attribute of a "boolean" and b "bool"
> create table booltest (a boolean,b bool);
> insert into booltest values (0,1),(true,false),(true,1),(0,false),(NULL,NULL);
> select * from booltest;
+-------+-------+
| a     | b     |
+-------+-------+
| false | true  |
| true  | false |
| true  | true  |
| false | false |
| NULL  | NULL  |
+-------+-------+
5 rows in set (0.00 sec)

//Create a table named "timestamptest" with 1 attribute of a "timestamp"
> create table timestamptest (a timestamp(0) not null, primary key(a));
> insert into timestamptest values ('20200101000000'), ('2022-01-02'), ('2022-01-02 00:00:01'), ('2022-01-02 00:00:01.512345');
> select * from timestamptest;
+---------------------+
| a                   |
+---------------------+
| 2020-01-01 00:00:00 |
| 2022-01-02 00:00:00 |
| 2022-01-02 00:00:01 |
| 2022-01-02 00:00:02 |
+---------------------+
```
