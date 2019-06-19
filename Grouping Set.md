# Hive 中的Grouping Set

Enhanced Aggregation, Cube, Grouping and Rollup

## Grouping Sets
grouping sets是一种将多个group by语法写在一条sql语句中的便利写法。

```
select
    A,
    B,
    C,
    group_id, 
    count(A)
from
    tableName
group by  --declare columns
    A,
    B,
    C
grouping sets
(
   (A,C),
   (A,B),
   (B,C),
   (C)
)
```
group_id：自动生成的字段，表示该行查询记录来自哪一个grouping sets，比如：（A，C）在这里的二进制编码为101等于5，那么group_id等于5。换句话说，查询结果中所有group_id为5的记录标识的是按照grouping_sets为（A，C）聚合查询的。
还是（A，C）这个例子，此时相当于group by A，C，但是select中出现了B，这时解释器会从tableName中复制一份，并将B置为null。
