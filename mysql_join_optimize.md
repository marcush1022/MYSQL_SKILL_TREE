#NESTED LOOP JOIN

#SNLJ
`Simple nested loop join(SNLJ)`
伪代码:
foreach row r in R
  foreach row s in S 
    if r and s satify join condition 
      then output the tuple <r, s>

join比较次数: S * R(即笛卡儿积);
外表扫描次数O: 1
内表的扫描次数I: R  

#INLJ
`Index nested loop join(INLJ)`
伪代码:
foreach row r in R
  lookup s in index
  if found s == r
    output the tuple <r, s>

join比较次数: R * indexHeight, index lookup的成本即树高;
外表扫描次数O: 1
内表的扫描次数I: 0 

#BNJL
`Block nested loop join(BNLJ)`
伪代码;
foreach row r in R
  store used columns as p from R in join buffer
    foreach row s in S
      if p and s satify join condition
        then output the tuple <r, s>
即多了一步join buffer, 与SNLJ相比改进的是内表的扫描次数，在最好的情况下仅需扫描内表一次;
join buffer缓存链接需要的列，然后以批量的方式与内表中的数据进行链接比较, 若join buffer可以存储所有的外表列，链接仅需扫描内外表一次;

join buffer的分配: join buffer 在进行join之前就进行分配，并且每次进行join就进行一次join buffer分配，若有N张表参与join, 则需要的join buffer 就是N-1个;

BNJL极大地避免了内表的扫描次数，若join buffer足以缓存外表的数据，则内表的扫描仅需一次，但是join的比较次数没变;

join比较次数: S * R(即笛卡儿积);
外表扫描次数O: 1
内表的扫描次数I: R * used_column / join_buffer + 1

#BKAJ
Batched Key Access Join
index nested loop join通过辅助索引进行链接后需要回表，这里需要大量的随机IO操作, 若能优化随机IO则能极大提升join性能, BKA通过用空间换时间，将随机IO转化为顺序IO;







