# **INSERT AND REPLACE**

## **REPLACE**

若表中有一个旧记录与一个有 UNIQUE 或 PRIMARY KRY 限制的新数据相同, 新数据会替换旧数据, 没有数据则 insert 数据;  
replace 与 insert 类似, 又一点除外: 若表中的旧记录与有 UNIQUE 或 PRIMARY KEY 限制的新纪录重复时, replace 会首先删除旧记录, 然后 insert 新记录, 使用 replace 相当于先对记录执行 delete 再执行 insert;   

除非表有一个 PRIMARY KEY 或 UNIQUE 索引, 否则使用 replace 没有意义, 该语句的效果与 insert 相同;

replace 使用一般是只想将数据在数据库中保留一份避免出现重复数据;

## **INSERT IGNORE**

忽略 insert 过程中出现的错误, 不忽略语法错误, 忽略主键存在的情况;   

没有 ignore 执行 insert (在UNIQUE或PRIMARY有相同值) 时, 这时会出现错误, 语句执行失败, 加了 ignore 后不会出现错误, 但是数据不会插入数据库;  

## **相同**
在不想向数据库中插入重复的主键或 UNIQUE 索引时, 可以使用 replace 或 insert ignore;

## **区别**
replace 相当于先 delete 再 insert, insert ignore 会忽略已存在的 UNIQUE 或 PRIMARY, 不会引起数据的修改;