# MySQL 优化

### 一、SQL语句优化

（1）使用limit对查询结果的记录进行限定
（2）避免select *，将需要查找的字段列出来
（3）使用连接（join）来代替子查询
（4）拆分大的delete或insert语句

### 二、选择合适的数据类型

（1）使用可存下数据的最小的数据类型，整型 < date,time < char,varchar < blob
（2）使用简单的数据类型，整型比字符处理开销更小，因为字符串的比较更复杂。如，int类型存储时间类型，bigint类型转ip函数
（3）使用合理的字段属性长度，固定长度的表会更快。使用enum、char而不是varchar
（4）尽可能使用not null定义字段
（5）尽量少用text，非用不可最好分表

### 三、选择合适的索引列

（1）查询频繁的列，在where，group by，order by，on从句中出现的列
（2）where条件中<，<=，=，>，>=，between，in，以及like 字符串+通配符（%）出现的列
（3）长度小的列，索引字段越小越好，因为数据库的存储单位是页，一页中能存下的数据越多越好
（4）离散度大（不同的值多）的列，放在联合索引前面。查看离散度，通过统计不同的列值来实现，count越大，离散程度越高：

```mysql
mysql> SELECT COUNT(DISTINCT column_name) FROM table_name;
```

### 四、使用命令分析

（1）SHOW查看状态
1.显示状态信息

```
mysql> SHOW [SESSION|GLOBAL] STATUS LIKE '%Status_name%';
```

session（默认）：取出当前窗口的执行
global：从mysql启动到现在
（a）查看查询次数（插入次数com_insert、修改次数com_insert、删除次数com_delete）

```
mysql> SHOW STATUS LIKE 'com_select';
```

（b）查看连接数(登录次数)

```
mysql> SHOW STATUS LIKE 'connections';
```

（c）数据库运行时间

```
mysql> SHOW STATUS LIKE 'uptime';
```

（d）查看慢查询次数

```
mysql> SHOW STATUS LIKE 'slow_queries';
```

（e）查看索引使用的情况：

```
mysql> SHOW STATUS LIKE 'handler_read%';
```

handler_read_key：这个值越高越好，越高表示使用索引查询到的次数。
handler_read_rnd_next：这个值越高，说明查询低效。
2.显示系统变量

```
mysql> SHOW VARIABLES LIKE '%Variables_name%';
```

3.显示InnoDB存储引擎的状态

```
mysql> SHOW ENGINE INNODB STATUS;
```

（2）EXPLAIN分析查询

```
mysql> EXPLAIN SELECT column_name FROM table_name;
```

explain查询sql执行计划，各列含义：
table：表名；
type：连接的类型
    -const：主键、索引；
    -eq_reg：主键、索引的范围查找；
    -ref：连接的查找（join）
    -range：索引的范围查找；
    -index：索引的扫描；
    -all：全表扫描；
possible_keys：可能用到的索引；
key：实际使用的索引；
key_len：索引的长度，越短越好；
ref：索引的哪一列被使用了，常数较好；
rows：mysql认为必须检查的用来返回请求数据的行数；
extra：using filesort、using temporary（常出现在使用order by时）时需要优化。
    -Using filesort  额外排序。看到这个的时候，查询就需要优化了
    -Using temporary 使用了临时表。看到这个的时候，也需要优化
（3）PROFILING分析SQL语句
1.开启profile。查看当前SQL执行时间

```mysql
mysql> SET PROFILING=ON; 
mysql> SHOW profiles;
```

2.查看所有用户的当前连接。包括执行状态、是否锁表等

```mysql
mysql> SHOW processlist;
```

（4）PROCEDURE ANALYSE()取得建议
通过分析select查询结果对现有的表的每一列给出优化的建议

```mysql
mysql> SELECT column_name FROM table_name PROCEDURE ANALYSE();
```

（5）OPTIMIZE TABLE回收闲置的数据库空间

```mysql
mysql> OPTIMIZE TABLE table_name;
```

对于MyISAM表，当表上的数据行被删除时，所占据的磁盘空间并没有立即被回收，使用命令后这些空间将被回收，并且对磁盘上的数据行进行重排（注意：是磁盘上，而非数据库）。
对于InnoDB表，OPTIMIZE TABLE被映射到ALTER TABLE上，这会重建表。重建操作能更新索引统计数据并释放成簇索引中的未使用的空间。
只需在批量删除数据行之后，或定期（每周一次或每月一次）进行一次数据表优化操作即可，只对那些特定的表运行。

（6）REPAIR TABLE修复被破坏的表

```mysql
mysql> REPAIR TABLE table_name;
```

（7）CHECK TABLE检查表是否有错误

```mysql
mysql> CHECK TABLE table_name;
```

### 五、定位慢查询

# SQL语句优化

1.’对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引。

2.应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。

3.应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如：

select id from t where num is null

可以在num上设置默认值0，确保表中num列没有null值，然后这样查询：

select id from t where num=0

4.应尽量避免在 where 子句中使用 or 来连接条件，否则将导致引擎放弃使用索引而进行全表扫描，如：

select id from t where num=10 or num=20

可以这样查询：

select id from t where num=10

union all

select id from t where num=20

5.下面的查询也将导致全表扫描：

select id from t where name like '%abc%'

若要提高效率，可以考虑全文检索。

6.in 和 not in 也要慎用，否则会导致全表扫描，如：

select id from t where num in(1,2,3)

对于连续的数值，能用 between 就不要用 in 了：

select id from t where num between 1 and 3

7.如果在 where 子句中使用参数，也会导致全表扫描。因为SQL只有在运行时才会解析局部变量，但优化程序不能将访问计划的选择推迟到运行时；它必须在编译时进行选择。然而，如果在编译时建立访问计划，变量的值还是未知的，因而无法作为索引选择的输入项。如下面语句将进行全表扫描：

select id from t where num=@num

可以改为强制查询使用索引：

select id from t with(index(索引名)) where num=@num

8.应尽量避免在 where 子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描。如：

select id from t where num/2=100

应改为:

select id from t where num=100*2

9.应尽量避免在where子句中对字段进行函数操作，这将导致引擎放弃使用索引而进行全表扫描。如：

select id from t where substring(name,1,3)='abc'--name以abc开头的id

select id from t where datediff(day,createdate,'2005-11-30')=0--'2005-11-30'生成的id

应改为:

select id from t where name like 'abc%'

select id from t where createdate>='2005-11-30' and createdate<'2005-12-1'

10.不要在 where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用索引。

11.在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。

12.不要写一些没有意义的查询，如需要生成一个空表结构：

select col1,col2 into #t from t where 1=0

这类代码不会返回任何结果集，但是会消耗系统资源的，应改成这样：

create table #t(...)

13.很多时候用 exists 代替 in 是一个好的选择：

select num from a where num in(select num from b)

用下面的语句替换：

select num from a where exists(select 1 from b where num=a.num)

14.并不是所有索引对查询都有效，SQL是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL查询可能不会去利用索引，如一表中有字段sex，male、female几乎各一半，那么即使在sex上建了索引也对查询效率起不了作用。

15.索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。

16.应尽可能的避免更新 clustered 索引数据列，因为 clustered 索引数据列的顺序就是表记录的物理存储顺序，一旦该列值改变将导致整个表记录的顺序的调整，会耗费相当大的资源。若应用系统需要频繁更新 clustered 索引数据列，那么需要考虑是否应将该索引建为 clustered 索引。

17.尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。

18.尽可能的使用 varchar/nvarchar 代替 char/nchar ，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些。

19.任何地方都不要使用 select * from t ，用具体的字段列表代替“*”，不要返回用不到的任何字段。

20.尽量使用表变量来代替临时表。如果表变量包含大量数据，请注意索引非常有限（只有主键索引）。

21.避免频繁创建和删除临时表，以减少系统表资源的消耗。

22.临时表并不是不可使用，适当地使用它们可以使某些例程更有效，例如，当需要重复引用大型表或常用表中的某个数据集时。但是，对于一次性事件，最好使用导出表。

23.在新建临时表时，如果一次性插入数据量很大，那么可以使用 select into 代替 create table，避免造成大量 log ，以提高速度；如果数据量不大，为了缓和系统表的资源，应先create table，然后insert。

24.如果使用到了临时表，在存储过程的最后务必将所有的临时表显式删除，先 truncate table ，然后 drop table ，这样可以避免系统表的较长时间锁定。

25.尽量避免使用游标，因为游标的效率较差，如果游标操作的数据超过1万行，那么就应该考虑改写。

26.使用基于游标的方法或临时表方法之前，应先寻找基于集的解决方案来解决问题，基于集的方法通常更有效。

27.与临时表一样，游标并不是不可使用。对小型数据集使用 FAST_FORWARD 游标通常要优于其他逐行处理方法，尤其是在必须引用几个表才能获得所需的数据时。在结果集中包括“合计”的例程通常要比使用游标执行的速度快。如果开发时间允许，基于游标的方法和基于集的方法都可以尝试一下，看哪一种方法的效果更好。

28.在所有的存储过程和触发器的开始处设置 SET NOCOUNT ON ，在结束时设置 SET NOCOUNT OFF 。无需在执行存储过程和触发器的每个语句后向客户端发送 DONE_IN_PROC 消息。

29.尽量避免向客户端返回大数据量，若数据量过大，应该考虑相应需求是否合理。

30.尽量避免大事务操作，提高系统并发能力。

---

# 使用索引的原则：

1.最左前缀匹配原则。

**mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配。所以要尽量把“=”条件放在前面，把这些条件放在最后。**

不会用到b的索引：

where a=1 and c>0 and b=2

会用到b的索引：

where a=1 and b=2 and c>0

2.尽量选择区分度高的列作为索引,区分度的公式是count(distinct col)/count(*)，表示字段不重复的比例，比例越大我们扫描的记录数越少。

3.**当取出的数据超过全表数据的20%时，不会使用索引。**

4.使用like时注意：

不使用索引：

like ‘%L%’

使用索引：

like ‘L%’

5.**尽量将or 转换为 union all**

不使用索引：

select * from user where name=’a’ **or** age=’20’

使用索引：

select * from user where name=’a’ **union all** select * from user where age=’20’

6.**字段加函数不会使用索引。**所以尽量把函数放在数值上

不使用索引：

where truncate(price) = 1

使用索引：

where price > 1 and price < 2

![Mysql语句优化的原则——让你写sql更加顺手](http://p1.pstatp.com/large/50970002f17b28c733fa)

7**.如果使用数字作为字符，则数字需要加引号，否则mysql会自动在列上加数据类型转换函数**

不使用索引

where mobile=18534874321

使用索引

where mobile=’18534874321’

8.字段加运算符不会使用索引。所以尽量把运算放在数值上

不使用索引:

SELECT ACCOUNT_NAME, AMOUNT FROM TRANSACTION WHERE AMOUNT + 3000 >5000;

使用索引:

SELECT ACCOUNT_NAME, AMOUNT FROM TRANSACTION WHERE AMOUNT > 2000 ;

9.使用组合索引时，必须要包括第一个列。

例如

alter table test add index(a,b,c)：

不使用索引：

where b=1, c=2

where b=1

where c=2

使用索引：

where a=1, b=1, c=2

where a=1, b=1

where a=1, c=2

10.**尽量避免使用is null或is not null**

不使用索引：

SELECT … FROM DEPARTMENT WHERE DEPT_CODE IS NOT NULL;

使用索引：

SELECT … FROM DEPARTMENT WHERE DEPT_CODE >0;

11.不等于（!=）不会使用索引

不使用索引:

SELECT ACCOUNT_NAME FROM TRANSACTION WHERE AMOUNT !=0;

使用索引:

SELECT ACCOUNT_NAME FROM TRANSACTION WHERE AMOUNT >0;

12.ORDER BY 子句只在以下的条件下使用索引：

**ORDER BY中所有的列必须包含在相同的索引中并保持在索引中的排列顺序.**

**ORDER BY中不能既有ASC也有DESC**

例如:

alter table t1 add index(a,b);

alter table t1 add index(c);

不使用索引：

select * from t1 order by a,c; 不在一个索引中

select * from t1 order by b; 没有出现组合索引的第一列

select * from t1 order by a asc, b desc; 混合ASC和DESC

**select \* from t1 where a=1 order by c; where和order by用的不是同一个索引，where使用索引，order by不使用。**

使用索引：

select * from t1 order by a,b;

select * from t1 order where a=1 order by b;

select * from t1 order where a=1 order by a,b;

select * from t1 order by a desc, b desc;

select * from t1 where c=1 order by c;

13.索引不是越多越好。mysql需要资源来维护索引，任何数据的变更（增删改）都会连带修改索引的值。所以，需要平衡考虑索引带来的查询加速和增删改减速。

其他注意事项

1.尽量避免使用select *

2.尽量使用表连接（join）代替子查询select * from t1 where a in (select b from t2)

3.性能方面，表连接 > (not) exists > (not) in

1）用exists代替in

低效:

SELECT *

FROM EMP

WHERE EMPNO > 0

AND DEPTNO IN (SELECT DEPTNO

FROM DEPT

WHERE LOC = ‘MELB’)

高效:

SELECT *

FROM EMP

WHERE EMPNO > 0

AND EXISTS (SELECT ‘X’

FROM DEPT

WHERE DEPT.DEPTNO = EMP.DEPTNO

AND LOC = ‘MELB’)

2）用not exists代替not in

低效：

SELECT …

FROM EMP

WHERE DEPT_NO NOT IN (SELECT DEPT_NO

FROM DEPT

WHERE DEPT_CAT=’A’);

高效：

SELECT ….

FROM EMP E

WHERE NOT EXISTS (SELECT ‘X’

FROM DEPT D

WHERE D.DEPT_NO = E.DEPT_NO

AND DEPT_CAT = ‘A’);

3）用表连接代替exists

exits：

SELECT ENAME

FROM EMP E

WHERE EXISTS (SELECT ‘X’

FROM DEPT

WHERE DEPT_NO = E.DEPT_NO

AND DEPT_CAT = ‘A’);

表连接：

SELECT ENAME

FROM DEPT D,EMP E

WHERE E.DEPT_NO = D.DEPT_NO

AND DEPT_CAT = ‘A’ ;

4.清除不必要的排序

低效：

select count(*) from (select * from user where id > 40 order by id);

高效：

select count(*) from (select * from user where id > 40);

5.having -> where

避免使用HAVING子句, HAVING 只会在检索出所有记录之后才对结果集进行过滤. 这个处理需要排序,总计等操作. 如果能通过WHERE子句限制记录的数目,那就能减少这方面的开销.

低效：

select * from user group by id having id > 40;

高效：

select * from user where id > 40 group by id;

6.除非确实需要去掉重复的行，否则尽量使用union all而不是union。**因为union会自带distinct操作，**代价很大

使用explain查看sql性能

1.explain用法：在select之前加上explain即可。

例如：explain select * from test;

注意：explain并不会真正运行语句，而是只返回执行计划。

怎么看执行计划？一个简单的优化原则：令sql读取尽可能少的行。

2.实战案例1：

问题语句运行超过5s：

SELECT `branch`.`id`, `branch`.`name`, `branch`.`registered_time`, `branch_region`.`region_id`, `user`.`username`, `user`.`mobile`, count(o.order_id) as order_num

FROM (`branch`)

LEFT JOIN `user` ON `user`.`branch_id` = `branch`.`id`

LEFT JOIN `branch_role` ON `branch_role`.`id` = `user`.`role_id`

LEFT JOIN `branch_region` ON `branch_region`.`branch_id` = `branch_role`.`branch_id`

LEFT JOIN `orders` o ON `branch`.`id` = `o`.`supplier_id`

WHERE branch.id NOT IN (select supplier_id from signing where seller_id=6683 and status < 6)

AND `branch`.`group` = 'SUPPLIER'

AND `branch_role`.`flag` = 'ADMINISTRATOR'

AND `branch`.`status` = 'NORMAL'

GROUP BY `branch`.`id`

ORDER BY `branch`.`registered_time` desc

LIMIT 20;

使用explain查看执行计划：

![Mysql语句优化的原则——让你写sql更加顺手](http://p3.pstatp.com/large/509800001677a71d797b)

根据“读取尽可能少的数据”的原则，发现读取行数最多的步骤读取了4792行。进而发现这个步骤没有用到索引（NULL）。而这个没有用索引的表是orders的supplier_id列。

加索引试试看：

alter table orders add index(supplier_id);

再次使用explain查看执行计划：

![Mysql语句优化的原则——让你写sql更加顺手](http://p3.pstatp.com/large/50970002f3096a9fd9be)

可以看到这个步骤使用了索引，读取的行数减少到了599行。

实际执行一下，秒出。

3.explain执行计划各个字段的意义：

1）id：语句的执行顺序，倒序执行

2）select_type：主要有以下几个类型：

**lsimple：表示简单的select，没有union和子查询**

**lprimary：最外层的select。在有子查询的语句中，最外面的select查询就是primary**

**lunion：union语句的第二个或者说是后面那一个**

**lunion result：union的结果**

**lsubquery: 子查询中的第一个 select**

3）table：涉及的表。

4）type：连接类型。主要有以下几个：（重点查看）

**lconst：说明只有一个匹配行，使用了主键或唯一性索引。通常是最优化的情况。**

**leq_ref，ref，ref_or_null：表示走了简单索引**

**lindex_merge：表示使用了多个索引的组合**

**lrange：表示通过索引取出了一个范围内的值。例如where a in (1,2)**

**lindex：表示对索引进行了全扫描**

**lALL：表示全表扫描**

**注意：以上类型从上到下性能越来越差。**

5）possible_keys：可供使用的索引

6）keys：实际使用的索引

7）key_gen：索引长度

8）ref：显示使用哪个列或常数与索引一起从表中选择行

**9）rows：读取的行数。（重点查看）**

10）Extra：备注