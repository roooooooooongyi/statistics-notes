# Hive 中的Grouping Set

Enhanced Aggregation, Cube, Grouping and Rollup

## Grouping Sets
grouping sets是一种将多个group by语法写在一条sql语句中的便利写法。

```
select
    A,
    B,
    C,
    D,
    E
    group_id, 
    count(A) a,
    count(B) b,
    count(c) c
from
    tableName
group by  --declare columns
    A,
    B,
    C，
    D，
    E
grouping sets
(
   (A,C),
   (A,B),
   (B,C),
   (C)
)
```

执行以上代码，会出现什么呢？
还是回到grouping sets的定义，集成多个group by为一条语句，也就是多个group by的排列组合，并且每种组合通过group_id这个序号来**一一标识**。             
那么group_id怎么确定呢？Hive里是根据二进制来计算的，注意看group by后面字段的顺序，以这个顺序为准来标记grouping sets中每个set的二进制编码。举例来说，      
（A，C）的编码：00101             
（A，B）的编码：00011              
因为group by后总共有5个字段，所以二进制码总共有5位，然后从右向左根据元组中字段出现的顺序来写编码。这个编码转化成十进制后的数字就是我们最终的group_id。
最后需要注意的是，group by语法中没有出现的字段无法select出来，这里同理，比如在数据按照（A，C）group by的时候，B、C、D列都显示null，其他依次类推。



参考：https://www.cnblogs.com/ToDoToTry/p/5474231.html
