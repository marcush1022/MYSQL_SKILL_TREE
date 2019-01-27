# **EXPLAIN**

> **explain显示了SQL语句的查询执行计划(QEP), 即Mysql的优化器如何执行**

## **extra**
包含Mysql解决查询的详细信息;
- using filesort: order by的结果，可能是一个CPU密集型的过程，可以通过选择合适的索引来改善性能, 通过索引对查询结果进行排序;
- using temproary: mysql需要创建一个临时表来存储结果, 通常发生在使用order by的情况;
- using index: 只需要使用索引就可以满足查询的要求，不需要直接访问表中的数据;
- range: 使用索引来返回一个范围内的行, 比如使用>或<查找时;
- ALL: 进行完全扫描;
- using where: 使用了where来处理结果，如执行全表扫描;
- using join buffer: 表示在获取连接条件时没有使用索引, 需要使用缓冲区来存储中间结果, 根据查询的具体情况可能需要添加索引来改进性能;
- impossible where: 根据where没有符合条件的行;
- using index condition: 使用了index condition pushdown优化;


## **select_type**

- SIMPLE: 简单SELECT;
- PRIMARY: 最外层SELECT;
- UNION: UNION中的第二个或后面的SELECT语句;
- SUBQUERY: 子查询的第一个SELECT;
- DEPENDENT SUBQUERY: 子查询的第一个SELECT, 取决于外面的查询;


## **type**

type: join的类型 从好到差依次是: 
```
system > const > eq_req > ref > fulltext > ref_or_null > index_merge > unique_subquery >index_subquery > range > index > ALL
```

> 一般查询至少需要达到range, 最好达到ref;
1. const: 表最多一个匹配行(最大值/最小值可以匹配这个查询), 在查询开始时读取，因此是常数，查询速度较快, 因为只读取一次;
2. eq_ref: 除const外最好的连接类型，表示索引被连接使用并且索引是UNIQUE或PRIMARY;
3. ref: 与eq_ref的唯一区别是使用的只是使用了索引的最左前缀;
4. index_merge: 表示使用了index merge优化方法;
5. unique_subquery: 该类型替换了IN中的子查询, 是一个索引查找函数，比子查询更快;
6. index_subquery: 类似unique_subquery, 可替换IN子查询, 但只使用与以下形式: value IN (SELECT key_column FROM single_table WHERE some_expr);
7. range: 这个链接类型使用索引返回一个范围内的行，如>和<;
8. index: 该链接类型与ALL相同，不同的是是在索引树中查询，通常比ALL快;
9. ALL: 全表扫描, 最慢的连接类型;


## **filtered**

查询条件过滤了表中多少行的记录，是一个估算的百分比值，rows显示的是行数的估计值，rows*filtered/100表示与前面的表join的行数；


## **possible_keys**

possible_keys: 可能应用于这张表中的索引, 为空表示无可能的索引;
若列出多个索引（例如大于3个）表示备选索引太多了，同时也可能存在无效的单列索引;


## **其他字段**

- key: 实际使用的索引，为空表示无可用索引, 可在select中使用USE INDEX(indexname)强制使用索引或IGNORE INDEX(indexname)强制忽略索引;
- key_len: 显示使用的索引的长度，不损失精确度的情况下索引越短越好;
- ref: 显示索引的哪一列被使用，如果可能的话是一个常数;
- rows: mysql认为必须检查的用来返回数据的行数;
- table: 显示数据是关于哪个表



