# Hive分析窗口函数(二) 

NTILE、ROW_NUMBER、RANK、DENSE_RANK、CUME_DIST、PERCENT_RANK

这一类主要用于排序编号，分组排序编号，取前topn，分组百分比和组内比例等用途。

***\*注意： 这类序列函数不支持WINDOW子句。\****

数据准备：

```sql
CREATE EXTERNAL TABLE lxw1234 (
cookieid string,
createtime string,   --day 
pv INT
) ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
stored as textfile location '/tmp/lxw11/';
DESC lxw1234;
cookieid                STRING 
createtime              STRING 
pv INT 
hive> select * from lxw1234;
OK
cookie1 2015-04-10      1
cookie1 2015-04-11      5
cookie1 2015-04-12      7
cookie1 2015-04-13      3
cookie1 2015-04-14      2
cookie1 2015-04-15      4
cookie1 2015-04-16      4
cookie2 2015-04-10      2
cookie2 2015-04-11      3
cookie2 2015-04-12      5
cookie2 2015-04-13      6
cookie2 2015-04-14      3
cookie2 2015-04-15      9
cookie2 2015-04-16      7
```

### NTILE

NTILE(n)，用于将分组数据按照顺序切分成n片，返回当前切片值
NTILE不支持ROWS BETWEEN，比如 NTILE(2) OVER(PARTITION BY cookieid ORDER BY createtime ROWS BETWEEN 3 PRECEDING AND CURRENT ROW)
如果切片不均匀，默认增加第一个切片的分布

```sql
    SELECT 
    cookieid,
    createtime,
    pv,
    NTILE(2) OVER(PARTITION BY cookieid ORDER BY createtime) AS rn1,	--分组内将数据分成2片
    NTILE(3) OVER(PARTITION BY cookieid ORDER BY createtime) AS rn2,  --分组内将数据分成3片
    NTILE(4) OVER(ORDER BY createtime) AS rn3        --将所有数据分成4片
    FROM lxw1234 
    ORDER BY cookieid,createtime;
    cookieid day           pv       rn1     rn2     rn3
    -------------------------------------------------
    cookie1 2015-04-10      1       1       1       1
    cookie1 2015-04-11      5       1       1       1
    cookie1 2015-04-12      7       1       1       2
    cookie1 2015-04-13      3       1       2       2
    cookie1 2015-04-14      2       2       2       3
    cookie1 2015-04-15      4       2       3       3
    cookie1 2015-04-16      4       2       3       4
    cookie2 2015-04-10      2       1       1       1
    cookie2 2015-04-11      3       1       1       1
    cookie2 2015-04-12      5       1       1       2
    cookie2 2015-04-13      6       1       2       2
    cookie2 2015-04-14      3       2       2       3
    cookie2 2015-04-15      9       2       3       4
    cookie2 2015-04-16      7       2       3       4
```



### –比如，统计一个cookie，pv数最多的前1/3的天

```sql
    SELECT 
    cookieid,
    createtime,
    pv,
    NTILE(3) OVER(PARTITION BY cookieid ORDER BY pv DESC) AS rn 
    FROM lxw1234;
    --rn = 1 的记录，就是我们想要的结果
    cookieid day           pv       rn
    ----------------------------------
    cookie1 2015-04-12      7       1
    cookie1 2015-04-11      5       1
    cookie1 2015-04-15      4       1
    cookie1 2015-04-16      4       2
    cookie1 2015-04-13      3       2
    cookie1 2015-04-14      2       3
    cookie1 2015-04-10      1       3
    cookie2 2015-04-15      9       1
    cookie2 2015-04-16      7       1
    cookie2 2015-04-13      6       1
    cookie2 2015-04-12      5       2
    cookie2 2015-04-14      3       2
    cookie2 2015-04-11      3       3
    cookie2 2015-04-10      2       3
```

### ROW_NUMBER

ROW_NUMBER() –从1开始，按照顺序，生成分组内记录的序列
–比如，按照pv降序排列，生成分组内每天的pv名次
ROW_NUMBER() 的应用场景非常多，再比如，获取分组内排序第一的记录;获取一个session中的第一条refer等。

```sql
    SELECT 
    cookieid,
    createtime,
    pv,
    ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY pv desc) AS rn 
    FROM lxw1234;
    cookieid day           pv       rn
    ------------------------------------------- 
    cookie1 2015-04-12      7       1
    cookie1 2015-04-11      5       2
    cookie1 2015-04-15      4       3
    cookie1 2015-04-16      4       4
    cookie1 2015-04-13      3       5
    cookie1 2015-04-14      2       6
    cookie1 2015-04-10      1       7
    cookie2 2015-04-15      9       1
    cookie2 2015-04-16      7       2
    cookie2 2015-04-13      6       3
    cookie2 2015-04-12      5       4
    cookie2 2015-04-14      3       5
    cookie2 2015-04-11      3       6
    cookie2 2015-04-10      2       7
```

### RANK 和 DENSE_RANK

—RANK() 生成数据项在分组中的排名，排名相等会在名次中留下空位
—DENSE_RANK() 生成数据项在分组中的排名，排名相等会在名次中不会留下空位

```sql
    SELECT 
    cookieid,
    createtime,
    pv,
    RANK() OVER(PARTITION BY cookieid ORDER BY pv desc) AS rn1,
    DENSE_RANK() OVER(PARTITION BY cookieid ORDER BY pv desc) AS rn2,
    ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY pv DESC) AS rn3 
    FROM lxw1234 
    WHERE cookieid = 'cookie1';
    cookieid day           pv       rn1     rn2     rn3 
    -------------------------------------------------- 
    cookie1 2015-04-12      7       1       1       1
    cookie1 2015-04-11      5       2       2       2
    cookie1 2015-04-15      4       3       3       3
    cookie1 2015-04-16      4       3       3       4
    cookie1 2015-04-13      3       5       4       5
    cookie1 2015-04-14      2       6       5       6
    cookie1 2015-04-10      1       7       6       7
    rn1: 15号和16号并列第3, 13号排第5
    rn2: 15号和16号并列第3, 13号排第4
    rn3: 如果相等，则按记录值排序，生成唯一的次序，如果所有记录值都相等，或许会随机排吧。
```

###  CUME_DIST,PERCENT_RANK

这两个序列分析函数不是很常用，这里也介绍一下。

```sql
CREATE EXTERNAL TABLE lxw1234 (
dept STRING,
userid string,
sal INT
) ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
stored as textfile location '/tmp/lxw11/';
hive> select * from lxw1234;
OK
d1      user1   1000
d1      user2   2000
d1      user3   3000
d2      user4   4000
d2      user5   5000
```

### CUME_DIST

–CUME_DIST 小于等于当前值的行数/分组内总行数
–比如，统计小于等于当前薪水的人数，所占总人数的比例

```sql
    SELECT 
    dept,
    userid,
    sal,
    CUME_DIST() OVER(ORDER BY sal) AS rn1,
    CUME_DIST() OVER(PARTITION BY dept ORDER BY sal) AS rn2 
    FROM lxw1234;
    dept    userid   sal   rn1       rn2 
    -------------------------------------------
    d1      user1   1000    0.2     0.3333333333333333
    d1      user2   2000    0.4     0.6666666666666666
    d1      user3   3000    0.6     1.0
    d2      user4   4000    0.8     0.5
    d2      user5   5000    1.0     1.0
    rn1: 没有partition,所有数据均为1组，总行数为5，
         第一行：小于等于1000的行数为1，因此，1/5=0.2
         第三行：小于等于3000的行数为3，因此，3/5=0.6
    rn2: 按照部门分组，dpet=d1的行数为3,
         第二行：小于等于2000的行数为2，因此，2/3=0.6666666666666666
```

### PERCENT_RANK

–PERCENT_RANK 分组内当前行的RANK值-1/分组内总行数-1
应用场景不了解，可能在一些特殊算法的实现中可以用到吧。

```sql
    SELECT 
    dept,
    userid,
    sal,
    PERCENT_RANK() OVER(ORDER BY sal) AS rn1,   --分组内
    RANK() OVER(ORDER BY sal) AS rn11,          --分组内RANK值
    SUM(1) OVER(PARTITION BY NULL) AS rn12,     --分组内总行数
    PERCENT_RANK() OVER(PARTITION BY dept ORDER BY sal) AS rn2 
    FROM lxw1234;
    dept    userid  sal     rn1    rn11     rn12    rn2
    ---------------------------------------------------
    d1      user1   1000    0.0     1       5       0.0
    d1      user2   2000    0.25    2       5       0.5
    d1      user3   3000    0.5     3       5       1.0
    d2      user4   4000    0.75    4       5       0.0
    d2      user5   5000    1.0     5       5       1.0
    rn1: rn1 = (rn11-1) / (rn12-1) 
    	 第一行,(1-1)/(5-1)=0/4=0
    	 第二行,(2-1)/(5-1)=1/4=0.25
    	 第四行,(4-1)/(5-1)=3/4=0.75
    rn2: 按照dept分组，
         dept=d1的总行数为3
         第一行，(1-1)/(3-1)=0
         第三行，(3-1)/(3-1)=1
```



