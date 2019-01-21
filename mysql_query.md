#查询优化器提示(hint)

可通过hint来控制查询优化器的最终执行计划,;

HIGH_PRIORITY, LOW_PRIORITY: 效果为控制语句的优先级高低，并不会影响查询性能，只是简单控制mysql访问某个数据表的顺序;

STRAIGHT_JOIN: 用法1让查询中的所有表按照在语句中出现的顺序进行关联, 用法2是固定前后两个表的关联顺序;

SQL_SMALL_RESULT, SQL_BIG_RESULT: 只对SELECT有效，告诉优化器对GROUP BY和DISTINCT查询如何使用临时表及排序, SQL_SMALL_RESULT表示结果集较小，可以将结果集放在内存中的索引临时表，SQL_BIG_SMALL表示结果集较大，建议使用磁盘临时表进行排序操作;

USE INDEX, IGNORE INDEX, FORCE INDEX: 使用或不适用索引进行查询，Mysql 5.1之后可以使用FOR GROUP BY和FOR ORDER BY来指定是否对排序和分组有效; 当优化器使用了错误的索引，或者因为其他原因（比如不使用order by时希望结果有序）时可以使用FORCE INDEX;

`查询优化提示可能收效甚小，但是却提升了维护的工作量，而且在mysql版本升级后可能导致新版的优化策略失效;`