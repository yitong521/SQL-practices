# **SQL 复习**

1. **if判断语句：类似Execl 判断语句**

SELECT IF(age<25 OR age IS NULL,'25岁以下','25岁以及上') as age_cut,

COUNT(device_id) Number

2. **case用法**

MySQL 的 case when 的语法有两种：简单函数 ：CASE [col_name] WHEN [value1] THEN [result1]…ELSE [default] END搜索函数 ：CASE WHEN [expr] THEN [result1]…ELSE [default] END

3. **LEAD()**

SQL Server LEAD()提供对当前行之后的指定物理偏移量的行的访问:例如，通过使用LEAD()函数，可以从当前行访问下一行的数据或下一行之后的行，依此类推。eg:LEAD(return_value ,offset [,default])  OVER (     [PARTITION BY partition_expression, ... ]     ORDER BY sort_expression [ASC | DESC], ...)

4. **Substring()**

substr(str,pos,len): 从pos开始的位置，截取len个字符substring_index（“待截取有用部分的字符串”，“截取数据依据的字符”，截取字符的位置N）例如substring_index(tag, ',', 1)（这个是截取第一个逗号之前的字符段）

5. **窗口函数**

https://zhuanlan.zhihu.com/p/92654574
<窗口函数> over (partition by <用于分组的列名>
                order by <用于排序的列名>)<窗口函数>的位置，可以放以下两种函数：1） 专用窗口函数，包括后面要讲到的rank, dense_rank, row_number等专用窗口函数。2） 聚合函数，如sum. avg, count, max, min等因为窗口函数是对where或者group by子句处理后的结果进行操作，所以窗口函数原则上只能写在select子句中。聚合函数:聚合函数sum在窗口函数中，是对自身记录、及位于自身记录以上的数据进行求和的结果。比如0004号，在使用sum窗口函数后的结果，是对0001，0002，0003，0004号的成绩求和，若是0005号，则结果是0001号~0005号成绩的求和，以此类推。不仅是sum求和，平均、计数、最大最小值，也是同理，都是针对自身记录、以及自身记录之上的所有数据进行计算，现在再结合刚才得到的结果举例：

a. select * from

b.           (select *,

c.                     row_number() over (partition by 要分组的列 order by 要排序的列 desc))

d.                     as ranking

e.             from 表名) as a

f.             where ranking <= N;


6. **cross join**


在MySQL中，当CROSS JOIN不使用WHERE子句时，CROSS JOIN产生了一个结果集，该结果集是两个关联表的行的乘积。通常，如果每个表分别具有n和m行，则结果集将具有n*m行  https://cloud.tencent.com/developer/article/1532193

7. **一些关于表基础架构的名词解释**

https://blog.51cto.com/wushank/1641308 索引/ 主键/ 外键

SUBQUERIES：A subquery is a query that is nested inside another query, or inside another subquery. There are different types of subqueries.

SINGLE VALUE：The simplest subquery returns exactly one column and exactly one row. It can be used with comparison operators =, <, <=, >, or >=.

This query finds cities with the same rating as Paris:

SELECT name

FROM city

WHERE rating = (

SELECT rating

FROM city

WHERE name = 'Paris'

);

MULTIPLE VALUES

A subquery can also return multiple columns or multiple rows. Such subqueries can be used with operators IN, EXISTS, ALL, or ANY.

This query finds cities in countries that have a population above 20M:

SELECT name

FROM city

WHERE country_id IN (

SELECT country_id

FROM country

WHERE population > 20000000);

CORRELATED

A correlated subquery refers to the tables introduced in the outer query. A correlated subquery depends on the outer query. It cannot be run independently from the outer query.

This query finds cities with a population greater than the average population in the country:

SELECT *

FROM city main_city

WHERE population > (

SELECT AVG(population)

FROM city average_city

WHERE average_city.country_id = main_city.country_id);

This query finds countries that have at least one city:

SELECT name

FROM country

WHERE **EXISTS** (

SELECT *

FROM city

WHERE country_id = country.id);

8. **LIKE：**

Used to search for a specified pattern in a column.

SELECT * FROM Students WHERE Name LIKE ‘a%’;

9. **row_number() OVER (PARTITION BY COL1 ORDER BY COL2)** 

表示根据COL1分组，在分组内部根据 COL2排序，而此函数计算的值就表示每组内部排序后的顺序编号（组内连续的唯一的)

10. **HAVING**	

It performs the same as the WHERE clause but can also be used with aggregate functions.	SELECT COUNT(ID), AGE FROM Students GROUP BY AGE HAVING COUNT(ID) > 5;

11. **SQL Server Numeric Functions**

The table below lists some of the Numeric functions in SQL with their description:

Name	Description

ABS	Returns the absolute value of a number.

ASIN	Returns arc sine value of a number.

AVG	Returns average value of an expression.

COUNT	Counts the number of records returned by a SELECT query.

EXP	Returns e raised to the power of a number.

FLOOR	Returns the greatest integer <= the number.

RAND	Returns a random number.

SIGN	Returns the sign of a number.

SQRT	Returns the square root of a number.

SUM	Returns the sum of a set of values.

12. **SQL Server Date Functions**

The table below lists some of the Date functions in SQL with their description:

Name	Description

CURRENT_TIMESTAMP	Returns current date and time.

DATEADD	Adds a date/time interval to date and returns the new date.

DATENAME	Returns a specified part of a date(as a string).

DATEPART	Returns a specified part of a date(as an integer).

DAY	Returns the day of the month for a specified date.

GETDATE	Returns the current date and time from the database.

13. **modify 更改属性**

https://blog.csdn.net/weixin_43127295/article/details/110375392

14. DATE_TRUNC 函数根据您指定的日期部分（如小时、天或月）截断时间戳表达式或文字

https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/r_DATE_TRUNC.html

15. case when 用法：https://www.cnblogs.com/chenduzizhong/p/9590741.html

CASE 测试表达式
WHEN 简单表达式1 THEN 结果表达式1
WHEN 简单表达式2 THEN 结果表达式2 …
WHEN 简单表达式n THEN 结果表达式n
[ ELSE 结果表达式n+1 ]
END

SELECT 学号,课程号,
CASE
WHEN 成绩 >= 90 THEN '优'
WHEN 成绩 BETWEEN 80 AND 89 THEN '良'
WHEN 成绩 BETWEEN 70 AND 79 THEN '中'
WHEN 成绩 BETWEEN 60 AND 69 THEN '及格'
WHEN 成绩 <60 THEN '不及格'
END 成绩
FROM 成绩表
WHERE 课程号 = 'M01F011'

SELECT CASE WHEN age < 25 OR age IS NULL THEN '25岁以下'
WHEN age >= 25 THEN '25岁及以上'
END age_cut,COUNT(*)number
FROM user_profile
GROUP BY age_cut

3. count/sum/avg 某一种情况的数据

a. 方法1：SUM(CASE WHEN result = 'right' THEN 1 ELSE 0 END) AS right_question_cnt

b. COUNT(CASE WHEN result = 'right' THEN 1 ELSE null END) AS right_question_cnt

c. sum(if(qpd.result='right', 1, 0))

d. avg(if(qpd.result='right', 1, 0))

16. 表创建

a. 普通插入（全字段）：INSERT INTO table_name VALUES (value1, value2, ...)

b. 普通插入（限定字段）：INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...)

c. 多条一次性插入：INSERT INTO table_name (column1, column2, ...) VALUES (value1_1, value1_2, ...), (value2_1, value2_2, ...), ...

d. 从另一个表导入：INSERT INTO table_name SELECT * FROM table_name2 [WHERE key=value]

e. 带更新的插入：REPLACE INTO table_name VALUES (value1, value2, ...) （注意这种原理是检测到主键或唯一性索引键重复就删除原记录后重新插入）

5. 修改记录的方式汇总：

    a. 设置为新值：UPDATE table_name SET column_name=new_value [, column_name2=new_value2] [WHERE column_name3=value3]
    
    b. 根据已有值替换：UPDATE table_name SET key1=replace(key1, '查找内容', '替换成内容') [WHERE column_name3=value3]

16. 计算时间差

a. **TIMESTAMPDIFF(interval, time_start, time_end)**可计算time_start-time_end的时间差，单位以指定的interval为准，常用可选：SECOND 秒/ MINUTE 分钟（返回秒数差除以60的整数部分）/HOUR 小时（返回秒数差除以3600的整数部分）/ DAY 天数（返回秒数差除以3600*24的整数部分）/MONTH 月数/YEAR 年数

b. 限定条件：2021年8月，写法有很多种，比如用year/month函数的`year(date)=2021 and month(date)=8`，比如用date_format函数的`date_format(date, "%Y-%m")="202108"`

17. 删除数据

a. delete from (table_name)

18. 删除整张表的数据，并且增设自主键

a. 根据条件删除：

i. DELETE FROM tb_name [WHERE options] [ [ ORDER BY fields ] LIMIT n ]

ii. ALTER TABLE tb_name auto_increment=1

b. 全部删除（表清空，包含自增计数器重置）：TRUNCATE tb_name

9. 表的创建、修改与删除：

19. 1.1 直接创建表：

CREATE TABLE

[IF NOT EXISTS] tb_name -- 不存在才创建，存在就跳过

(column_name1 data_type1 -- 列名和类型必选

[ PRIMARY KEY -- 可选的约束，主键

| FOREIGN KEY -- 外键，引用其他表的键值

| AUTO_INCREMENT -- 自增ID

| COMMENT comment -- 列注释（评论）

| DEFAULT default_value -- 默认值

| UNIQUE -- 唯一性约束，不允许两条记录该列值相同

| NOT NULL -- 该列非空

], ...

) [CHARACTER SET charset] -- 字符集编码

[COLLATE collate_value] -- 列排序和比较时的规则（是否区分大小写等）

1.2 从另一张表复制表结构创建表： CREATE TABLE tb_name LIKE tb_name_old

1.3 从另一张表的查询结果创建表： CREATE TABLE tb_name AS SELECT * FROM tb_name_old WHERE options

20. 修改表：

alter add;添加列 alter change;改列名 alter modify;修改列数据类型

**建议：修改列属性时使用modify；修改列名使用change**

ALTER TABLE 表名 修改选项 。选项集合：

{ ADD COLUMN <列名> <类型>  -- 增加列

| CHANGE COLUMN <旧列名> <新列名> <新列类型> -- 修改列名或类型

| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT } -- 修改/删除 列的默认值

| MODIFY COLUMN <列名> <类型> -- 修改列类型

| DROP COLUMN <列名> -- 删除列

| RENAME TO <新表名> -- 修改表名

| CHARACTER SET <字符集名> -- 修改字符集

| COLLATE <校对规则名> } -- 修改校对规则（比较和排序时用到）

3.1 删除表：DROP TABLE [IF EXISTS] 表名1 [ ,表名2]。

22. 窗口函数和偏移：https://blog.nowcoder.net/n/46434af391984afe98109c30a7127efc?f=comment

23. with (定义一个database名) as （select from where）->将数据集放在前面，增强数据的可读性

24. having count(score) = count(uid) -> 一种写法，这里的写法表示取所有score都不为零的uid（按组划分）

25. 转换数字格式：Use the CAST() function to convert an integer to a DECIMAL data type.

a. CAST( substring_index(tag, ',', -1) AS DECIMAL

26. 字符数长度 :a. CHAR_LENGTH(nick_name)

27. 大小写转换： lower(); upper()

28. 百分比保留小数，不可以直接用round, 百分比格式化：CONCAT(x, '%')

1. 匹配
    1. 
    
    匹配串中可包含如下四种通配符：
    
    _：匹配任意一个字符；
    
    %：匹配0个或多个字符；
    
    - [ ]  [ ]：匹配[ ]中的任意一个字符(若要比较的字符是连续的，则可以用连字符“-”表 达 )；
    
    [^ ]：不匹配[ ]中的任意一个字符。
    
    例23．查询学生表中姓‘张’的学生的详细信息。
    
    | 1 | SELECT * FROM 学生表 WHERE 姓名 LIKE ‘张%’ |
    | --- | --- |
    
    例24．查询姓“张”且名字是3个字的学生姓名。
    
    | 1 | SELECT * FROM 学生表 WHERE 姓名 LIKE '张__’ |
    | --- | --- |
    
    如果把姓名列的类型改为nchar(20)，在SQL Server 2012中执行没有结果。原因是姓名列的类型是char(20)，当姓名少于20个汉字时，系统在存储这些数据时自动在后边补空格，空格作为一个字符，也参加LIKE的比较。可以用rtrim()去掉右空格。
    
    | 1 | SELECT * FROM 学生表 WHERE rtrim(姓名) LIKE '张__' |
    | --- | --- |
    
    例25.查询学生表中姓‘张’、姓‘李’和姓‘刘’的学生的情况。
    
    | 1 | SELECT * FROM 学生表 WHERE 姓名 LIKE '[张李刘]%’ |
    | --- | --- |
    
    例26.查询学生表表中名字的第2个字为“小”或“大”的学生的姓名和学号。
    
    | 1 | SELECT 姓名,学号 FROM 学生表 WHERE 姓名 LIKE '_[小大]%' |
    | --- | --- |
    
    例27.查询学生表中所有不姓“刘”的学生。
    
    | 1 | SELECT 姓名 FROM 学生 WHERE 姓名 NOT LIKE '刘%’ |
    | --- | --- |
    
    例28.从学生表表中查询学号的最后一位不是2、3、5的学生信息。
    
    | 1 | SELECT * FROM 学生表 WHERE 学号 LIKE '%[^235] |
    | --- | --- |

29. LIKE的写法【mark一下，原来有IF(profile LIKE '%female','female','male') 这样的方式】

SELECT IF(profile LIKE '%female','female','male') gender,COUNT(*) number
FROM user_submit
GROUP BY gender;

30. limit 可以跳行
    LIMIT m,n : 表示从第m+1条开始，取n条数据；
    LIMIT n ： 表示从第0条开始，取n条数据，是limit(0,n)的缩写
