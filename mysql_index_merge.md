#INDEX MERGE

#什么是INDEX MERGE
`若优化器发现可以通过多个索引查找的数据的交集/并集定位数据, 优化器就会使用index merge的方式, index merge分为： 多个索引交集访问和多个索引并集访问, 或先交集再并集`;
1. 索引合并是将通过多个索引查找的数据的交集/并集定位数据;
2. 索引合并时会对索引进行交集, 并集, 或先并集再交集的操作, 以便合成一个索引;
3. 需要合并的索引只能是一个表的, 不能对多表进行索引合并;
`可以减少从数据表中取数据的次数, 提高查询效率`;
`若使用了index merge, explain的type列会显示index merge, key列会显示所有用到的索引`;
extra字段的值可能为:
using union; // 索引取并集; (OR时可能出现)
using sort_union; // 索引排序后取并集; 
using intersection; // 取交集; (AND时可能出现)

#何时使用INDEX MERGE
where查询的字段中可能有多个条件(或JOIN)涉及多个字段, 这些字段之间有AND, OR, 则就可能使用index merge, index merge其实就是：`对多个索引分别进行条件扫描, 然后将他们各自的结果进行合并(intersect/union)`;
例：
1. WHERE col1 = 1 OR col2 = 2, 最典型的场景;
2. WHERE (col1 =1 AND col2 = 2) OR col3 = 3,;
3. WHERE (col1 = 1 OR col1 = 2) OR col2 = 3;
4. WHERE (col1 = 1 OR col2 = 2) OR col2 = 3, 无法使用索引(非最左前缀);

mysql 5.1之后采用了index merge, 可以对多个索引分别进行条件扫描; 

合并的方式：
同一个表的多个范围扫描可以对结果进行合并, 合并的方式可以为：UNION, INTERSECT, 以及他们的组合(先进行内部intersect, 然后外面进行union);

#UNION
union用于合并多个SELECT的结果集, `union的语句必须有相同的列, 列必须有相似的数据类型, SELECT的字段顺序必须相同`; 
SELECT col1(s) FROM table1;
UNION
SELECT col1(s) FROM table2;
`默认union选择不重复的值, 若需要重复的值使用UNION ALL`;

#INTERSECTION
与UNION类似, UNION类似于OR, 只要一个值存在于第一个表或第二个表, 就会被选出; INTERSECTION类似于AND, 只有存在于第一个表和第二个表才会选出;
UNION是并集, INTERSECTION是交集;
[SQL1]
INTERSECION
[SQL2]

#INDEX INTERSECTION MERGE
出现了index intersection merge一般意味着索引建的不够合理, 可以通过建立符合索引进一步优化;


