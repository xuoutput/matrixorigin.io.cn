# **LOG()**

## **函数说明**

LOG(X)函数返回X的自然对数。

## **函数语法**

```
> LOG(X)
```

## **参数释义**

|  参数   | 说明  |
|  ----  | ----  |
| X | 必要参数，任何当前支持的数值数据 |

## **示例**

```sql
drop table if exists t1;
create table t1(a float, b float);
insert into t1 values(2,8);

mysql> select log(a), log(b) from t1;
+--------------------+--------------------+
| log(a)             | log(b)             |
+--------------------+--------------------+
| 0.6931471805599453 | 2.0794415416798357 |
+--------------------+--------------------+
1 row in set (0.00 sec)
```

## **限制**

LOG(X)目前仅支持单参数输入。
