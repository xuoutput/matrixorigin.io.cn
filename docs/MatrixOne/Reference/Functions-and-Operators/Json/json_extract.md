# **JSON_EXTRACT()**

## **函数说明**

`JSON EXTRACT` 是一个 JSON 查询函数，可用于查询 JSON 文档。

## **语法结构**

```sql
select json_extract(jsonDoc, pathExpression);
```

## **参数释义**

`pathExpression` 是 JSON 路径表达式，即在 JSON 文档中选择一个值。

路径表达式对于提取部分 JSON 文档或修改 JSON 文档的函数很有用，指定在该文档中的哪个位置进行操作。例如，以下查询从 JSON 文档中提取具有 name 键的成员的值：

```sql
mysql> SELECT JSON_EXTRACT('{"id": 14, "name": "Aztalan"}', '$.name');
+-----------------------------------------------------+
| json_extract({"id": 14, "name": "Aztalan"}, $.name) |
+-----------------------------------------------------+
| "Aztalan"                                           |
+-----------------------------------------------------+
1 row in set (0.00 sec)
```

路径表达式必须以 `$` 字符开头：

- `,` 后跟键名，使用给定键命名对象中的成员。键名需要使用双引号包含。

- `[N]`：选择数组的 *path* 后，将数组中位置 `N` 处的值命名。数组位置是从零开始的整数。如果数组是负数，则产生报错。

- 路径可以包含 `*` 或 `**` 通配符：

   + `.[*]` 计算 JSON 对象中所有成员的值。

   + `[*]` 计算 JSON 数组中所有元素的值。

   + `prefix**suffix`：计算以命名前缀开头并以命名后缀结尾的所有路径。

- 文档中不存在的路径（或不存在的数据）评估为 `NULL`。

## **示例**

如下一组 JSON 数组：

```
[3, {"a": [5, 6], "b": 10}, [99, 100]]
```

- `$[0]` 表示 3。

- `$[1]` 表示 {"a": [5, 6], "b": 10}。

- `$[2]` 表示 [99, 100]。

- `$[3]` 为 NULL (数组路径从 `$[0]` 开始，而 `$[3]` 表示第四组数据，这组数据不存在)。

由于 `$[1]` 与 `$[2]` 计算为非标量值，那么表达式可以嵌套。例如：

- `$[1].a` 表示 [5, 6]。

- `$[1].a[1]` 表示 6。

- `$[1].b` 表示 10。

- `$[2][0]` 表示 99。

键名在路径表达式中需要使用双引号。`$` 引用这个键值，也需要加双引号：

```
{"a fish": "shark", "a bird": "sparrow"}
```

这两个键都包含一个空格，必须用引号引起来：

- `$."a fish"` 表示 `shark`。

- `$."a bird"` 表示 `sparrow`。

使用通配符 `$` 的路径可以为包含多个值的数组：

```sql
mysql> select JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.*');
+---------------------------------------------------------+
| JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.*') |
+---------------------------------------------------------+
| [1, 2, [3, 4, 5]]                                       |
+---------------------------------------------------------+

mysql> SELECT JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.c[*]');
+------------------------------------------------------------+
| JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.c[*]') |
+------------------------------------------------------------+
| [3, 4, 5]                                                  |
+------------------------------------------------------------+
```

在下述示例中，路径 `$**.b` 计算为多个路径 (`$.a.b` 和 `$.c.b`) 并生成匹配路径值的数组：

```sql
mysql> select JSON_EXTRACT('{"a": {"b": 1}, "c": {"b": 2}}', '$**.b');
+---------------------------------------------------------+
| JSON_EXTRACT('{"a": {"b": 1}, "c": {"b": 2}}', '$**.b') |
+---------------------------------------------------------+
| [null, 1, 2]                                                  |
+---------------------------------------------------------+
```

在下述示例中，将展示从列中查询 JSON 值：

```sql
create table t1 (a json,b int);
insert into t1(a,b) values ('{"a":1,"b":2,"c":3}',1);

mysql> select json_extract(t1.a,'$.a') from t1 where t1.b=1;
+-------------------------+
| json_extract(t1.a, $.a) |
+-------------------------+
| 1                       |
+-------------------------+
1 row in set (0.00 sec)

insert into t1(a,b) values ('{"a":4,"b":5,"c":6}',2);

mysql> select json_extract(t1.a,'$.b') from t1 where t1.b=2;
+-------------------------+
| json_extract(t1.a, $.b) |
+-------------------------+
| 5                       |
+-------------------------+
1 row in set (0.00 sec)

mysql> select json_extract(t1.a,'$.a') from t1;
+-------------------------+
| json_extract(t1.a, $.a) |
+-------------------------+
| 1                       |
| 4                       |
+-------------------------+
2 rows in set (0.00 sec)

insert into t1(a,b) values ('{"a":{"q":[1,2,3]}}',3);

mysql> select json_extract(t1.a,'$.a.q[1]') from t1 where t1.b=3;
+------------------------------+
| json_extract(t1.a, $.a.q[1]) |
+------------------------------+
| 2                            |
+------------------------------+
1 row in set (0.01 sec)

insert into t1(a,b) values ('[{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6}]',4);

mysql> select json_extract(t1.a,'$[1].a') from t1 where t1.b=4;
+----------------------------+
| json_extract(t1.a, $[1].a) |
+----------------------------+
| 4                          |
+----------------------------+
1 row in set (0.00 sec)
```