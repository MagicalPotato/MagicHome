### sql使用指南
```select DISTINCT clustername, cellid FROM volte.t_volte_predictresult;
-- DISTINCT会作用于后面所有出现的列,如果后面只跟着一列则只对一列起作用,如果跟着两列则是两列一起起作用.也就是这两列的组合当做一个整体并且唯一.

select DISTINCT clustername, cellid FROM volte.t_volte_predictresult limit 2 OFFSET 1;
-- limit 2 offset 1  从第1行开始输出2行  包括第1行 .数据库中是从0开始计数的.最开始的一行是第0行. 
如果从第1行开始只有5行但是你要检索10行,则只会显示有的5行.不会报错.

select clustername, cellid FROM volte.t_volte_predictresult ORDER BY clustername, cellid;
select clustername, cellid FROM volte.t_volte_predictresult ORDER BY clustername DESC,cellid DESC;
--ORDER BY 必须是一个sql语句的最后一个子句,否则会报错.排序是按列出的列名书序来拍的,先按照clustername排,
只有好几个clustername相同才会接着按照后面一列排.没写在select后的列也可以用来排序.
--ORDER BY 默认是升序排序,a-z.当然也可指定降序 DESC,z-a,价格从高到底等.  
如果要对每一列进行降序,必须要明确指定.而且注意到没,ORDER BY 是先排左面,左面相同才再根据右面排.
--一般来说order by 排序时A和a是等效的,一般的数据库都是这样,如果想要明确区分则需要联系管理员进行设置,但是很麻烦.

select clustername, cellid FROM volte.t_volte_predictresult WHERE cellid BETWEEN '11' and '22' ORDER BY clustername, cellid;
--这个之所以显示的值有误差是因为你这里的between是对一个字符串操作,而不是数字.字符串有自己的规则所以会看起来有误.

SELECT name,id from tab1 where a=1 or a=2 and c=3 ;
SELECT name,id from tab1 where (a=1 or a=2) and c=3 ;
SELECT name,id from tab1 where a IN(1,2) and c=3 ;
--数据库中and优先级更高,本意想筛选 a=1或者2 并且c=3 的值,但是实际执行的时候会先选出a=2 并且 c=3的值之后再进行或操作.
想要优先执行的话需要加括号. 用in关键字更便捷性能更高且可扩展性更好.

SELECT name,id from tab1 where name LIKE 'Fisk%' ;
-- like表示其后的语句是模糊匹配.%表示任意字符出出现任意次数,哪怕是有空格之类的都行.'Fish my bean bag'这种也是可以的. 
'%Fisk bean%'也可以通配  通配的大小写是根据数据库来的.有的区分有的不区分.
--%可以匹配0个1个或多个字符.  如果一个字符串'fisk      '后面跟了很多空格,则用'f%k'来匹配它肯定不行,
后面的空格就没了,更好的是'f%k%'  下划线_只匹配一个字符
```

```
select clustername || '(' || cellid || ')' as aaa FROM volte.t_volte_predictresult;
-- sql中的连字符,跟代码中的字符串连接符 + 效果相同 ,实际上有的数据库用的就是+  当用别名时,写as是很好的习惯.ALTER

select now() as data   --查询当前时间.  直接用select实际上可以进行很多运算. +-*/都可以.

select AVG(price) as aaa from tab1 where name='666';   --对price列取平均.可选where 条件
select count(*) from tab1; select COUNT(name) from tab1;   --count(*) 会对所有行计数,无论行有没有null值.
count(列)只会对该列不为null的值进行计数.
select MAX(abc) as a,MIN(abc) as b from tab1;  --sql允许对字符串取最大和最小,
按字符串默认排序也就是从小到大排完后取最后一个和最前一个 ,忽略null值.
select SUM(a*b) as a from tab1;  --sum对某一列的值求和 ,当然也会忽略null值.  

SELECT clustername, max(cellid),COUNT(*) from volte.t_volte_predictresult GROUP BY clustername; 
--GROUP BY 的本质实际上是先分组后运算.  先对clustername进行分组然后再执行其他运算.cluster1的全拿出来搞成一组,
cluster2,cluster3也拿出来搞成组,然后对每组进行后续的计算.

SELECT clustername, MAX(cellid) as aaa ,count(*) as bbb from volte.t_volte_predictresult where cellid >= '0' GROUP BY clustername HAVING MAX(cellid) is not null  ORDER BY aaa,bbb;
--有了group by之后,sql知道要对clustername分组.然后先根据条件过滤每个组的数据,然后才再执行前面的count函数. 
这种相互纠缠的逻辑感觉有点怪,但就是这样用.如果从group处截开,这就是个错误的sql.
--having 和 where有着相同的功能,但是where对行进行过滤,having对分好的组里的行进行过滤.  
由于having不支持别名,所以用的是原始的计算.用别名会报错.原始是啥数据库就认啥,count(*)就是count(*)
--数据库会把它当成一个整体的值来处理,我们不需要把它理解成一个特定的东西或者一个函数.  但是order by是支持别名的.  

select clustername , riskid from volte.t_volte_predictresult as a , volte.t_volte_summaryrisk as b where a.taskid=b.taskid
SELECT clustername, riskid FROM volte.t_volte_predictresult as  a  INNER JOIN volte.t_volte_summaryrisk as b on  a.taskid=b.taskid
SELECT a.clustername, b.riskid FROM volte.t_volte_predictresult as  a  full  JOIN volte.t_volte_summaryrisk as b on  a.taskid=b.taskid
--内联的本质实际上是where条件.大家推崇联结只是为了让sql显得逼格高,实际上where就能解决,当然,
联结的性能可能更好一点,不过好不到那里去  .如果上面那条没有where,则会返回两个表的乘积
--内联结相当于where,内联结只显示你写了要查询的列,而外联结则是有一个限定,
左联返回左表和右表符合条件的数据和左表中不符合条件的数据,比如a.a1=b.b2 用left join,会返回满足条件的左右表数据,和左表
--剩下的a1数据,但是对应的b1的值是null.   右联正好相反.  而全联是全部,也就是表中有一部分是a1,b1都有值,
有一部分a1有值b1是null,剩下的是a1是null,b1有值.

select * INTO tab1 from tab2  --把tab2复制一遍到tab1,如果tab1存在则会被覆盖
insert into tab1 select * from tab2 --把tab2的数据查出来插入到tab1. 查出来的数据只会按照顺序依次填充,
如果表不存在会报错,如果列数不一致也会报错.不管列是否对应反正就是按顺序填进去.

update tab1 SET a=1,b=2 where c= 000;  --在更新表的时候一定要有where条件否则整个表的列都会被修改.
set只需要写一次,要更新的接着写在后面就行. 如果设置a=null则相当于是变相删除了a的值.
--某种意义上,update相当于删除整列,而delete相当于删除整行.  定义外键有时候可以避免误删和误插数据.
强制执行引用完整性的数据库是无法删除有外键的表中的数据的,这是一种保护措施.

TRUNCATE from tab1;   --delete table 是删除表中的所有行,但是不删除表本身.truncate 有相同的功能但是效率更快.
update和delete都可以直接跟表名就行,但是强烈建议带上from,保持语法完整性和可移植性.

DROP TABLE tab1;   --删除表结构和表中的所有数据.
```

```
CREATE table tab1 
(
   nename  CHAR(100) NULL,
   neid    INTEGER   NOTNULL DEFAULT 001,     --建表语句可以指定默认值.  同列内不用标点,两列之间才用标点分隔.
   cellid  INTEGER   NOTNULL CHECK (cellid > 0),  --灵活的使用检查约束可以避免往数据库中出插入一些无效的数据.这个数据可能符合数据类型但是并不一定有效.比如场景要求id一定要大于10,你输入5肯定不行.
)

alter table TABLE1 ADD COLUMN nename char(100) DEFAULT wangze; 
ALTER TABLE TABLE1 ADD CONSTRAINT (nename LIKE 'wang%')  
--update 是更新表中的数据,而alter是更改表结构.更改表结构是一项很复杂的操作,最好在最开始就设计好表,
后续尽量不要对其结构做修改.这样可以避免一些不可预知的小问题. 
-- 有的数据库表不支持在更改表结构的同时更改表的主键和外键的约束关系.   可以对一个表增加检查约束条件,避免往其中插入无效数据.

CREATE VIEW view1 AS SELECT neid,nename,cellid,cellname FROM tab1 where riskid = 001;  
SELECT * FROM view1;  --使用视图可以简化和重用sql语句,而且能够给底层的数据加一层防护用以保护基础数据,
还能将不需要展示的数据隐藏.但是使用多重视图要注意性能问题.

BEGIN TRANSACTION
sql1 ;
sql2 ;
SAVEPOINT point1;     --事物处理实际上是执行一组sql语句.事物不会直接写入数据库,sql全部执行成功才写入,执行失败则不写入,这也是一种安全有效的保护措施.  savepoint为保留点,如果在实行sql3时出了错
sql3 ;             --可以使用rollback语句会退到某个保留点,通常建议处理复杂事物时多使用保留点,这样可以使得操作过程更灵活.
COMMIT TRANSACTION;
```
