# **REPLACE**

若表中有一个旧记录与一个有UNIQUE或PRIMARY KRY限制的新数据相同，新数据会替换旧数据，没有数据则insert数据;
replace与insert类似，又一点除外：若表中的旧记录与有UNIQUE或PRIMARY KEY限制的新纪录重复时，replace会首先删除旧记录，然后insert新记录, 使用replace相当于现对记录执行delete再执行insert；

除非表有一个PRIMARY KEY或UNIQUE索引，否则使用replace没有意义，该语句的效果与insert相同;

replace使用一般是只想将数据在数据库中保留一份避免出现重复数据

#INSERT IGNORE

忽略insert过程中出现的错误，不忽略语法错误，忽略主键存在的情况；

没有ignore执行insert（在UNIQUE或PRIMARY有相同值）时，这时会出现错误，语句执行失败，加了ignore后不会出现错误，但是数据不会插入数据库

#相同
`在不想向数据库中插入重复的主键或UNIQUE索引时，可以使用replace或insert ignore`;

#区别
`replace相当于先delete在insert, insert ignore会忽略已存在的UNIQUE或PRIMARY，不会引起数据的修改`;