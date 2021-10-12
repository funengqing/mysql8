# mysql8

## sql语言分类

DDL(Data Definition Language):用来定义数据库的对象,如表,视图,索引等,操作库和库的对象;

DML(Data Manipulation Language):用来在数据表中更新,增加,删除记录,操作表中的数据;

TCL(Transaction Control Language):用来做数据表中的事务管理;

DQL(Data Query Language):用来查询数据库中的数据;

DCL(Data Control Language):用来控制数据库用户权限;



### DDL

数据定义语言,用来定义数据库对象,库,表,列等;创建,删除,修改库或表的结构,主要分为数据库DDL和表DDL;

#### 操作数据库的DDL

##### 1.创建数据库

```mysql
-- 直接创建数据库,[]内的内容表示可以省略
create databade [if not exists] name;

# 指定字符集方式创建数据库
create databade [if not exists] name character set 字符集;

# 指定字符集和排序规则方式创建数据库
create databade [if not exists] name character set 字符集 collate 排序规则;
```



##### 2.查看数据库

```mysql
-- 查看所有数据库
show databases;

-- 查看指定数据库,查看目标数据库当前的字符集,编码方式,建表语句(如果没有修改)
show create database 数据库名;
```



##### 3.修改数据库

```mysql
-- 只能修改数据库的字符集和排序规则,不能修改数据库名
alter database 数据库名 character set 字符集 [cllate 排序规则];
```



##### 4.删除数据库

```mysql
drop database name;
```



##### 5.使用数据库

```mysql
-- 使用指定数据库或切换到指定数据库
use database name;

-- 查看正在操作的数据库
select database();

```





#### 操作数据表的DDL

表是数据库对象,由若干字段(列)组成,是使用最为频繁的数据库对象.

##### 1.创建表

```mysql
-- 创建表前要指定使用一个数据库,也就是说要在库下创建表
create table 表名(字段１ 数据类型,字段２ 数据类型);
-- 建议写成
create table 表名 (
 字段１ 数据类型,
 字段２ 数据类型
 字段３ 数据类型
);

```



##### 2.数据类型

2.1 数值类型



2.1.1 整数类型

| 类型名称    | 字节   | 说明                                        |
| ----------- | ------ | ------------------------------------------- |
| tinyint     | 1(8位) | 无符号: 0 ~ 255,有符号: -128 ~ 127,以些类推 |
| smallint    | 2      |                                             |
| mediumint   | 3      |                                             |
| int,integer | 4      |                                             |
| bigint      | 8      |                                             |

**建表时默认都是有符号的,如果要使用符号的,在建表时要指定,如:id int unsigned**



2.1.2 近似数类型

浮点数:float(M,D),double(M,D)

M-表示所存储的值共有Ｍ位

D-表示共保留小数点后D位

定点数:decimal(M,D),相较于浮点数,定点数更加精确

M-表示所存储的值共有Ｍ位

D-表示共保留小数点后D位



2.2 字符串类型

| 字符串类型 | 取值范围和存储需要 |
| ---------- | ------------------ |
| char(M)    |                    |
| varchar(M) |                    |
|            |                    |
|            |                    |
|            |                    |
|            |                    |
|            |                    |
|            |                    |
|            |                    |

char(M):长度是固定的,没使用完也是这个长度(M); varchar(M) 中是按实际长度存储,数字M表示最大的长度



2.3 日期和时间类型

Mysql中有多种表示日期和时间类型的数据类型,如下表

| 类型      | 字节 | 最小值 | 最大值 |
| --------- | ---- | ------ | ------ |
| date      | 4    |        |        |
| datetime  | 8    |        |        |
| timestamp | 4    |        |        |
| time      | 3    |        |        |
| year      | 1    |        |        |



##### 3.查看表

```mysql
-- 查看在该数据库下的所有表
show tables;

-- 查看表结构
desc 表名;或
describe 表名;

-- 查看创建表是的sql语句
show create table 表名;

-- 复制表结构,不会复制表中的内容
create table 新表名 like 旧表名

```



##### 4.修改表

```mysql
-- 添加新字段(列)
alter table 表名 add 字段(列) 类型;

-- 修改字段(列)类型
alter table 表名 modify 字段(列)名 新类型;

-- 修改字段(列)名
alter table 表名 change 旧字段(列)名 新字段(列) 类型;

-- 删除字段(列)
alter table 表名 drop 字段(列)名;

-- 修改表名
rename table 表名 to 新表名

-- 修改字符集
alter table 表名 character set 字符集;

```



##### 5.删除表

```mysql
-- 删除表
drop table 表名
```



### DML

数据操纵语言.

#### 一.插入记录

##### 1.指定字段插入

```mysql
-- 插入一行,可以查看表结构(desc table name)或创建表时的语句(show create table name)来确定字段
insert into 表名 (字段１,字段２,字段３) value (值１,值２,值３);

/*

字段中如果有自增的,最好写上,不费事,但容易出错(事),可以指定要插入的字段和值,与上面的一样;
字段与值和类型要一一对应;
除了数值类型外,其他字段类型的值必须使用引号(建议单引号)包起来
如果要插入空值,可以不写字段,或者插入null;
*/
```



##### 2.不指定字段插入

```mysql
insert into 表名 value (值１,值２,值３);
/*
相当于指定了所有字段(列),顺序及类型要和表结构中的要一致
*/
```



##### 3.蠕虫复制

```mysql
-- 复制内容到表中,不管表中是否有重复的数据
insert into 表名１ select * from 表名２;

/*
-- 复制表结构,不会复制表中的内容
create table 新表名 like 旧表名;

-- 先查看结构是否一样
desc table 表名;
*/

```





#### 二.更新表记录

##### 1.不带条件的更新

```mysql
-- 相当于批量修改该字段(列)的值
update 表名 set 字段１ = 值;
```



##### 2.带条件的更新

```mysql
-- 特定更改某些行中某些个字段的值,满足条件的就会被更改
update 表名 set 字段１ = 值 where 条件;
```





#### 三.删除记录

##### 1.带条件删除

```mysql
-- 删除指定一行
delete from 表名 where 条件;
```



##### 2.不带条件删除

```mysql
-- 清空表内容
delete from 表名;
```



### DQL

#### 一.单表查询

DQL 语句的作用是用来查询数据库中的数据,不会对数据库的数据进行修改.

##### 1.简单查询

```mysql
--　查询单个表中的所有数据
select * from 表名;

-- 指定(字段)列数据
select 字段１,字段２ from 表名;

-- 别名查询,as 关键字可以省略
select 字段１ as 别名１,字段２ as 别名２ from 表名[ as 表名];

-- 去除重查值(要看查询多少个字段,维度不同,结果不同)
select distinct 字段１,字段２ from 表名;

-- 查询结果参与运算,只会影响展示结果,不会影响表中的数据
-- 字段(列)必须是数值型数据,可计算的;
-- null值和任何值做运算结果都是null,但可以用函数来解决;
select 列１+固定值 from 表名;
select 列１+列２ from 表名;

```



##### 2.条件查询

```mysql
-- 使用多种运算符表示查询条件
-- >,<,=,>=,<=,!=,<>
select * from 表名 where id>5;

-- 使用逻辑运算符
-- and(&&,与), or(||,或), not(!,非)
select * from 表名 where id>5 and name='neng';

-- 指定范围查询in
select * from 表名 where id in(1,3,5);
-- 相当于
select * from 表名 where id=１;
select * from 表名 where id=３;
select * from 表名 where id=５;

-- 指定一个宽范围between 值１ and 值２,(包括值１和值２),相当于类似age>=值１ and age<=值2
select * from 表名 where id between 1 and 5;

-- 模糊查询like
select * from 表名 where name like '%nen%';
/*
%:0个或多个字符
_:占位,表示占一个字符
%nen:以nen结束的字符
nen%:以nen开始的字符
%neg%:包含有nen的字符
nen__:nen开头且总共５个字符
*/

-- 为空查询　is null
-- 查询某个字段是不是为空时,不能用=null来判断,因为null和任何值都不相等(null表示不存在)
select * from 表名 where score is null;
```



#### 二.排序

##### 1.单列排序

```mysql
 select 字段名 from 表名 [where 条件] order by 字段名 [asc|desc];
 -- 默认是升序asc
```

##### 2.组合排序

```mysql
 select 字段名 from 表名 [where 条件] order by 字段名1 [asc|desc],字段名2 [asc|desc];
```



#### 三.单行函数

计算一行中的数据

##### 1.数值函数

```mysql
-- 返回x的绝对值
select abs(x);

-- 返回大于或等于x的最小整数(向上取整)
select ceil(x);

-- 返回小于或等于x的最大整数(向下取整)
select floor(x);

-- 返回0~1之间的随机数
select rand();

-- 返回离x最近的整数(四舍五入)
select roudn(x);
```



##### 2.字符串函数

```mysql
-- 拼接函数,合并成一个字符串
select concat(s1,s2,s3,s4);

-- 查询子字符串的开始位置,从s中获取sub的开始位置
-- 谁,在哪里查
select locate(sub,s);

-- 转成小写
select lower(s);

-- 转成大写
select upper(s);

-- 替换,在哪里换,换谁,用谁换
select replace(s,s1,s2);

-- 截取一段子字符串,从s中的star位置开始截取一段长为length的字符串,从1开始算起
select substr(s,star,lenth);

-- 去空格,去掉s头尾的空格
select trim(s);

-- 反转字符串
select reverse(s);
```



##### 3.时间日期函数

```mysql
-- 返回系统当前时间,年月日时分秒
select now();

-- 返回系统当前时间,年月日时分秒
select sysdate();
select now(),sysdate(),sleep(5),now(),sysdate();
/*
now():一次,以后每一次都一样
sysdate():每一次都不一样
*/

-- 返回当前日期(年月日)
select curdate();

-- 返回当前日期(时分秒)
select curtime();

-- 返回参数日期中的月分值
select month("2021-12-12");

-- 返回参数日期是一年中的第几周
select week("2021-12-12");

-- 返回参数日期中的日值
select day("2021-12-12");

-- 在参数日期上增加相应的时间--重要
select date_add(date,INTERVAL expr type);
select date_add('2021-4-22 14:15:16',INTERVAL 3 YEAR);
/*
date参数是合法的日期表达式,expr 参数是希望添加的时间间隔
type可以是:MICROSECOND,SECOND,MINUTE,HOUR,DAY,WEEK,MONTH,QUARTER,YEAR,SECOND_MICROSECOND,MINUTE_MICROSECOND,MINUTE_SECOND,HOUR_MICROSECOND,HOUR_SECOND,HOUR_MINUTE...
*/
```



##### 4.流程控制函数

```mysql
-- 三目函数,condition为true时返回expr1,否则返回expr2
select if (condition,expr1,expr2);

-- 判空函数,expr1不为空时返回expr1,否则返回expr2
select ifnull(expr1,expr2);
select math,english,math+ifnull(english,0) as '总成绩' from student;
```



##### 5.其他函数

```mysql
-- 查看当前数据库版本
select version();

-- 查看当前用户
select user();

-- md5加密
select md5();
```





#### 四.聚合函数

计算一列当中的数据,并返回一个结果值,聚合函数会忽略空值

##### 1.count();

```mysql
-- 统计指定字段(列)记录数,记录为Null的不统计
-- 统计表中有多少条记录,也就是有多少行
select count('name') from student;-- 如果有NUll,则不准确
select count(*) from student;
```



##### 2.sum();

```mysql
-- 统计指定字段下的数值和,如果不是数值类型,结果为0
select sum(math) from student; 
```



##### 3.avg();

```mysql
-- 统计指定字段的平均数,如果不是数值类型,结果为0
select avg(math) from student; 
```



##### 4.max();

```mysql
-- 计算指定字段下的最大值
select max(math) from student; 
```



##### 5.min();

```mysql
-- 计算指定字段下的最小值
select min(math) from student; 
```



#### 五.分组

使用group by 关键字对查询信息进行分组,符合条件的相同的数据作为一组

怎么分组?字段中内容一样的作为同一组

##### 1.分组查询

```mysql
-- 字段内容一样的作为同一组
-- 使用group by 进行分组时,mysql8 会报错,需要相应处理
select 字段一,字段二 from 表名 group by 分组字段[having 条件];
```



##### 2.分组的条件过滤

```mysql
select * from 表名 group by 分组字段 [having 条件];
```



##### 3.where 与 having 的对比

```mysql
-- where 后面不能跟聚合函数,having 可以跟
select math,count(*) from student where age>18 group by math having count(*)>2; 
```



#### 六.limit

应用场景,网页中每页显示的条数(商品数量)

```mysql
-- 限制.限制每次查询记录的条数
select 字段 from 表名 [where 条件] [group by 字段] [having 条件] [order by asc|desc] [limit offest,length];
-- limit offset,length 或者 limit length;

select * from student limit 2,6;
-- 省略前面两条,从第3条开始显示,共显示6条.第一个参数省略时,表示从第一条开始显示.
```

#### 七.select 语句总结

##### 1.书写顺序

select 字段 from 表名 where 条件 group by 字段 having 条件 order by 字段 limit 参数

其中,select 字段,from表名为必写的,其余均可视实际情况而定.



##### 2.执行顺序

1. from 表名
2. where 条件
3. group by 字段
4. having 条件
5. select 字段
6. order by 字段
7. limit 参数



### 约束和策略

#### 一.主键约束

非空不重复,建议为非业务字段,不一定有意义,一张表应该有一个,最多一个

```mysql
-- 建表时声明主键字段
create table student(
	id int primary key,
    name varchar(20)
);

-- 建表时另起一行进行额外声明主键字段
create table student(
	id int,
    name varchar(20),
    primary key(id)
);

-- 建表后增加,很少使用,因为在建表前应该要想好再建表
alter table student add constraint [stu] primary key(id);

-- 删除主键
alter table student drop primary key;

-- 主键自增,建表时指定
create table student(
	id int primary key auto_increment,
    name varchar(20)
);
-- 建表后增加自增字段
alter table 表名 modify 主键列名 数据类型 auto_increment;

-- 其他
-- 默认的自增起始值为１,如果要修改起始值,使用
alter table 表名 auto_increment=123;

-- truncate截断表,重置所胡数据和结构
truncate table 表名;
/*
类似　delete from 表名;
truncate和delete 的区别
truncate 会相当于删除了该表的所有数据和结构,然后仅重新按照原来的字段建一张新表,指定的自增起始值不起作用,变成默认的从１开开始
delete 只清空表中的数据,原来的结构还在,如主键和自增,指定的自增值依旧能起作用
使用场景:能否修复原自增字段的数据
*/
```



#### 二.非空约束

指定字段不能为Null

```mysql
-- 建表时声明
create table student(
	id int primary key,
    name varchar(20) not null
);

-- 建表后修改
alter table student modify name varchar(20) not null;
```



#### 三.唯一约束

添加后该字段的值不能重复,但可以为Null,Null代表不存在,无法比较,也就无所谓重复不重复了,也就是说该字段可以全部为Null

```mysql
-- 建表时声明
create table student(
	id int primary key,
    name varchar(20) not null,
    email varchar(20) unique
);

-- 建表后修改
alter table student add constraint [约束名] unique(字段); 

-- 删除唯一约束
alter table student drop index 约束名
```



#### 四.默认约束

#### 五.外键约束



## 注释方式有三种

```mysql
-- content(-- 和content间要有空格)

# content(mysql独有的)

/* content
	content
	content
*/
```

