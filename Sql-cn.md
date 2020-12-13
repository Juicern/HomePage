Sql

-----

## Join

下图分别展示了left join, right join, inner join, outer join相关的7种用法

![sql-join](Sql-cn.assets/sql-join.png)

> 注意：left join与left outer join, right join 和 right outer join，没有任何差别 

## top

在sql server中似乎没有limit这个用法，只能用top来取前n个数，如：

```MSSQL
select top 1 queston_id from survey_log
```

## union

合并两个或多个select语句的结果集，结果集中的列名总是等于union中第一个select语句的列名

* union: 选取不同的值
* union all: 选取所有的值（可重复）

## case 

下述为示例

```MSSQL
case sex
when '1' then '男'
when '2' then '女'
else ‘其他’ end
```

## where

约束声明，使用Where来约束来自数据库的数据，where是在结果返回之前起作用的，且where中不能使用聚合函数

## having

过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在having中可以使用聚合函数

### 