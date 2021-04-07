---
title: mysql和pymysql
date: 2020-08-13 11:41:06
tags:
  - DB-mysql
categories:
  - Database
---
## mysql的使用

### sql的基本语法
#### 1.说明
  1. 行：记录
  2. 列：字段
  3. 唯一标识每一个行/记录的字段叫做主键：
  4. RDBMS关系型数据库：
  4. redis适合做缓存数据库、mysql适合做网站、magoose适合做爬虫：
  5. rdbms客户端与服务器端通过sql语句进行通讯，sql是一种结构化查询语言：
    sql语句可以应用在很多的数据库软件上：
  6. DQL是数据查询语言，DML是数据操控语言：sql数据库语言是不区分大小写的：
  7. ubuntu系统中使用如下命令安装数据库服务端和客户端
    $ sudo apt-get install  mysql-client-core-5.6
    $ sudo apt-get install mariadb-client-core-10.0
    然后利用mysql -uroot -p进入数据库
    eixt/quit/ctrl+d退出数据库
  8. 为表定义字段的类型和约束可以规范表的完整性

#### 2.类型
  ![数据类型图](http://fuxiangyou1_admin.gitee.io/image/image/datatype.png)
  字段中常用的数据类型如下：
  +  整形：int bit
  +  小数：decimal   ：表示浮点数，如decimal（4，2）表示共存四位数，小数占2为
  +  字符串：varchar、char  ：varchar表示可变字符串长度度，多了会丢弃、char表示固定字符串长度多了会丢弃少了会补空格
  +   字符串text表示存储大文本，当字符大于4000时使用
  +  日期时间：data、time、datetime
  + 枚举类型：enum
  对于图片音视频文件不存储在数据库中，而是上传到某个服务器然后在表中存储这些文件的保存路径：

#### 3.约束
  1. 主键primary key：物理上存储的顺序
  2. 非空not null：此字段不允许为空
  3. 唯一unique：此字段的值不允许重复
  4. 默认default：不填写此值时会使用默认值，如果填写时以填写为准
  5. 外键foreign key： 对关系字段进行约束当为关系字段填写值时会关联到表中查询此值是否存6. 在，如果存在贼填写成功
  7. 如果不存在就填写不成功：（通常来说外键就是其他表的主键，存储其他表的主键的字段就是外键）

#### 4.常用操作
  0. 常用的数据库操作（注意语句要以分号的形式写，如果不写分号其会认为语句未完，sql是不区分大小写的：）
  1. show databases；显示连接的所有数据库
  2. select now();显示当前时间
  3. select version();
  4. create database  数据库名  charset='utf8';(注意指定编码字符未utf-8)
  5. show create database 数据库名；（用于显示数据库的创建细节）
  6. drop database 数据库名；删除数据库
  7. 数据库名不支持-中横线，要想认为是一个整齐必须使用类似模板字符串的标识符``即反引号
  8.  select database();显示当前使用或者选择的数据库
  9.  use 数据库名；表示使用或者切换该数据库；
  10. show tables 选择当前选择的数据库里面的所有表
  11. create table yyyy(id int primary key not null auto_increment,name varchar(25))用于创建一个表；方法中
  的每一个字段用逗号隔开，每个字段名后面空隔加上修饰符进行字段修饰；
  12. desc 表名；用于表格形式显示所有的表的字段；
  13. 创建多个字段的表的方法：
  create table  stuednts(
      id int unsigned primary key not null auto_increment,
      name varchar(25),
      age tinyint(3) unsigned,
      high decimal(5,2),
      gender enum("男","女","保密") default "保密",
      cls_id int unsigned
  )
  14. 向表中插入值得方法（一定表得字段得顺序去插入）
  insert into stuednts values(0,'老王',19,190.00,'男',1606);
  15. select * from stuednts （向指定表中查询所有得信息）
  16. sql得命令行中得注释为--；
  17.  增加删除修改表字段得方法为：alter table stuednts add birthday datetime;
  alter table 表名 add  字段名 字段类型描述（向表中添加字段）
  18. alter table 表明 modify 字段名 字段类型 字段描述；（修改表中字段不重命名版）
  19. alter table 表名 change 原字段名 新字段名 字段类型 字段描述；（修改表中字段重命名版）
  20. alter table 表名 drop 字段名； （字段删除后字段所在得数据也删除了）
  21. drop table 表名；用于删除数据表
  22. show create table 表名
  23. 一般在sql语句中带有名字得比如表名、数据库名、字段名通常带反引号以放出错；

#### 5.表的简单增删改查
  1. 插入数据(xxx是表名，插入了一个记录有两个字段值)
    insert into xxx value(1,'121')  注意：此时插入数据得字段值可以为null或者default，只要约束允许；
    注意：枚举类型值可以设定得时候用指定得值，也可以用枚举得下标表示枚举得值；
  2. 部分插入：insert into xxx(字段名) value(字段值)插入单个字段值；
  3. 多行插入：value后面可以接多个记录使得可以一行插入多个；
    insert into xxx value(21,'safsafds'),(31,'asdfsafdsa'),(41,'sdafsdafdsa')
  4. update 表名 set 字段=值 where  字段 = 值  注意：根据唯一字段作为条件去修改字段值，否则将修改整个字段列得值；
  5. 条件查询通过where：select * from xxx where name = '222';select * from  xxx where id>7;
  6. 条件查询指定列（记得用逗号分隔）：select id,name from where name = '222'
  7. 条件查询指定列并起别名(通过)：select id as ID,name as 姓名 from where name = '222'
  8. 物理删除在数据库中真删除和逻辑删除在表中标记删除,即通过特定的字段标记有没有删除：
    物理删除：delete from xxx  where name = '222'
    逻辑删除：update xxx set is_delete = 1 where id=11

### 数据库增删改查专题
#### 1.查询相关

  0. 模糊查询的查询条件要使用引号，可以是单引号也可以是双引号，但是不能是反引号；
  1. 去重查询(从students表中去重复查询gender)；
    select distinct gender from stuednts;
  2. 条件查询里面有的运算符有：
    比较运算符> < >= <=  =  !=  <> 
    条件运算符and or not
    注意：select * from xxx where id >= 8 and  id<=10;and运算符左边和右边都是一个整体而不是值；
    注意：select * from xxx where   not id >= 8 and gender = 2;not的用法，用在整体的条件的前面；
  3. 模糊查询为查询条件：
    可以使用like，用%替换0个或者多个字符，用_表示一个字符，
    select * from xxx where name like "_明%"
    可以使用rlike后接正则作为查询条件，
  4. 使用范围查询作为条件 字段 in (范围)  ：注意in前面还可以加not
    select * from xxx  where age  in (11,14)
    可以使用between  and 作为范围查询条件：注意between前面可以加not取反，且between和and是一个整体
    select * from xxx  where age  between 16 and 19
  5. 是否非空作为条件查询（is null / is not null）：
    select * from xxx where name is not null
  6. 在使用了数据库的情况下 通过source 数据库文件名导入数据库
  7. 注意：有时查询条件混淆的时候建议使用小括号明晰一下

#### 2.排序相关
  1.  按照指定条件之后从小到大排序（asc表示从小到大、desc表示从大到小排）：
  此时如果我们排序的那个字段比如age相同的时候系统会按照主键再去排；但是我们可以在by后面接多个字段
  2. 表示当前面字段相同时再按后面的字段排；而不是系统的字段排（多个字段之间使用逗号，）
  elect * from xxx where (age between 15 and  25) and gender = 1 order by age asc
  select * from xxx where (age between 15 and  25) and gender = 1 order by age desc,cls_id desc;


#### 3.聚合、分组（select查询的位置可以放置任何函数或者表达式子）
  聚合函数的用法：
  > count(*)  :查询条件查询后的记录总数：
  select  count(*) as 人数    from xxx where id = 8;
  max(age)：max函数用于查询指定字段的列的最大值：
  select max(age) from xxx;
  sum():函数的用法用于查询指定列的总数；
  select sum(age) from xxx;
  agv（）：函数用于返回指定列的平均值：
  select    avg(age) from xxx;
  round（值，小数点位数）函数用于四舍五入保留指定小数点位数；
  select round(avg(age),1)  from xxx;
  注意：聚合函数不能直接和其他字段列一起查找：可能会报错；但是不一定报错，注意场景
  分组====》group by 字段名
  实际上按照某一字段进行分组就是将每一组折叠起来只显示每一组中的第一个记录；
  分组的意义在于以和聚合函数一起配合使用；
  表示：只查询gender字段，并且按其将其分组；
  select gender,count(*) from xxx group by gender;
  次数的聚合函数count是对分组的每个组进行求值
  +--------+----------+
  | gender | count(*) |
  +--------+----------+
  | 女     |        7 |
  | 男     |        6 |
  +--------+----------+
    2 rows in set (0.00 sec)
  注意：和分组配合使用的聚合函数group_concat(name)的作用显示分组的每一组该字段的值
  select  gender,group_concat(name，age,cls_id)  from xxx  group by gender
  +--------+-----------------------------------------------------------------------------------+
  | gender | group_concat(name,':', age,':')                                                   |
  +--------+-----------------------------------------------------------------------------------+
  | 男     | 小明4:16:,小s:19:,小明dsafsda:11:,小明sadf:64:,小明sdafs:68:,小明sss:78:          |
  | 女     | 小明:11:,小明1:14:,小明2:17:,小明3:19:,小明5:13:,小明sdaf:10:,小明sdafdsafdsa:28: |
  +--------+-----------------------------------------------------------------------------------+
    2 rows in set (0.00 sec)
  3.having的用法，having作为分组的一个条件使用：having是对查出来的结果进行条件判断，而
  where是对原表进行判断；

#### 4.分页limit  start,count：注意
  1. select * from xxx limit 3;  注意只有一个参数表示查询的个数
  2. select * from xxx  limit 2,2; 注意如果有两个参数表示查询的开始个数和查询的总个数，注意不能省略逗号；
  3. 注意：limit 语句只能放在一个sql语句的最后

#### 5.链接查询、多表查询
  链接查询：（链接多个表，取多个表的共有值）
  1. 内连接：取多个表的交集：其实际上的处理过程为，从当前表中的每个记录的指定字段去找其他表对应的记录
    合并成为一行记录的过程；如果两个表都有该字段则合并显示，这就是内连接
    select * from xxx inner join xxxc;  这是一个简单的内连接查询；但是该连接没有去重，xxx表的每一个数据
    都对应了xxxc表中的多个记录；可以使用on作为条件
    select * from  xxx inner join xxxc on xxx.cls_id = xxxc.name;
    但是此时还有个问题，就是两个表的相同键都存在，因为我们显示了两个表的所有；正确的方法应该为
    select xxx.*,xxxc.name  from  xxx inner join xxxc on xxx.cls_id = xxxc.name;
    为了方便，我们可以将表通过as起别名；

  2. 外连接，外连接分为左连接和又连接：
    左连接：以join左边的表为基准去其他表去找记录，如果左边的表的指定字段在右边的表没有字段则显示null，
    通常内连接只有两个表都有该字段才会显示一条合并后的记录；
    右连接原理类似、
    连接的返回值通常是一个新表，是一个新的结果，可以通过having 加条件进行筛选；
    （where 通常用于对原表进行条件查询，having通常用于对一个结果或者新表进行条件查找；）
  3. 什么是自关联使用场景：省市三级联动、
    select *  from areas as privince inner join areas as ticy  on   cith.pid = provice.aid having provice.atitle = 'shandong'
    这就是自关联的使用场景、就是自己内连接自己的表；

#### 6.子查询
  1. 查询语句先执行子查询（子查询稍微有点消耗性能）：
  select * from xxx where height = ( select max(height)  from xxx ) ;
  这是一个包含子查询的拆查询语句；将来会先执行子查询语句；

#### 7.数据库的设计：

1. 范式（NF）：数据库的范式遵循的几种规范，分为八种，一般前三种就行了
第一范式(1NF)：表中每一个字段的信息不可再拆分：
第二范式(2NF)：一个表中必须有一个主键，且其他非主键必须依赖于我们的主键：
第三范式(3NF)：非主键必须直接依赖于主键、不能存在传递依赖，即不能存在非主键列a依赖于非主键b，
非主键b依赖于主键的情况

2. E-R模型：
    + 一对多
    + 多对一
    + 多对多

### sql的高级语法

#### 数据库的视图
  1. 视图试图是从一个或几个基本表(或视图)导出来的表。它与基本表不同，是一个虚表。
  数据库中只存放视图的定义，而不存放视图对应的数据，这些数据仍存放在原来的基本表中。
  所以一旦基本表中的数据发生变化，从视图中查询出来的数据也就随之改变了。
  从这个意义上说，视图就像是一个窗口，透过它可以看到数据库中自己感兴趣的数据及其变化。
  2. 视图的创建方法
  create view vvv_p as  select * from good where cate_name = '台式机';
  3.  数据表视图的好处：
  提高了重用性就像一个函数、对数据重构却不影响程序的运行、
  提高了安全性能，对不同的用户开放、让数据更加清晰；

#### 数据库中的事务
  事务的好处：可以防止出现不可避免的意外情况下数据可以恢复到最开始的状态；即要么都成功要么都失败
  所谓事务就是一个操作序列，这些操作要么都执行、要么都不执行，它是一个不可分割的工作单位；
  事务的四大特性：ACID即
  A：原子性 C一致性 I隔久性： D持久性；
  开启事务的方法：start transaction/begin
  提交事务：commit
  回滚事务rollback；回滚后回到事务开始状态；不发生修改；

  注意：mysql客户端是默认开启事务的；

#### 数据库中的索引
索引 ==》索引是一种特殊的文件比如（innodb数据表上的索引是表空间的一个组成部分）
他们包含着对数据表里的所有记录的引用指针；
索引能加快数据库的查询速度；索引的目的在于提高效率；
create  index 索引 on 表明里的字段（字段的最大长度）;
create  index tt on xxxss(num(50));
| Query_ID | Duration   | Query                                                              |
+----------+------------+--------------------------------------------------------------------+
|        1 | 0.00055875 | desc xxxss                                                         |
|        2 | 0.00679775 | select *  from xxxss where num = 'hahahahahhahahhahahha-----49999' |
|        3 | 0.03975900 | create  index tt on xxxss(num(50))                                 |
|        4 | 0.00052425 | select *  from xxxss where num = 'hahahahahhahahhahahha-----49999' |
+----------+------------+--------------------------------------------------------------------+
通过第二次和第四次的比较，通过索引查询效率大大提高；

show index from xxxss显示索引的方法；

注意：创建太多的索引将会影响更新和插入的速度，因为它需要更新文件的索引；所有只有进程查询的表才建议创建索引；

数据库中的索引
####  了解sql账户管理：（保证将来数据的安全）

  1. 通过desc查看数据库用户列表（改表在数据库mysql下）
  select user,host  from user;
  2. 创建账户和授权
  grant  权限列表 on 数据库 to 用户名@访问主机 indetified by 密码
  grant select on xxxss.* to 'zhansan'@'localhost' identified by '123456789' ;
  注意select此时是权限；
  3. 建议不要使用远程登录、非常危险：
  4. 删除数据库用户的方法：
  drop user zhansan@localhost;
  MYSQL的主从：从一个数据库服务器到其他服务器上、在复制数据时、一个服务器充当主服务器、
  mysql服务器之间的主从同步是义域二进制日志机制的，主服务器使用二进制日志来记录数据库的变动情况；
  从服务器通过读取和执行改日志文件来保持和主服务器的一致；
  完成主从服务器之间的同步建议看csdn
  作用：用于数据库的备份；且随时备份；读写分离（写入数据让主服务器响应，读数据让从服务器响应）

### 数据库与python的交互
#### mysql知识补充
  1. 切记分组之后的查询字段的函数是对每一组进行操作（分组后的查询是对每一组的查询；）
  select  cate_name,avg(price) from good group by cate_name;
  2. 外键插入的方法：怎么给一张表添加外键（两种种方法）
    通过reference 加其他表的主键比如xxx(id)\CONSTRAINT是约束的意思
    外键一般尽量少使用：会降低数据库的性能；
    cs_id int(30) references classes(id),（包含references 和指定表的字段的字段就是外键；）
    FOREIGN KEY (cs_id) REFERENCES classes(id)
    CONSTRAINT `FK_ID_CS` FOREIGN KEY (cs_id) REFERENCES classes(id)
  3. 如何取消外键：alter table 表名 drop foreign key 外键名称：

#### pymysql使用
1.  window系统下pip3 install pymysql
2. 创建链接数据库connect_t =  connect(多值字典参数)
3. cursor  = connect_t.cursor()创建邮标cursor去操作数据库
4. cursor.execute('select * from xxx;')操作数据库
5. cursor.close()  connect_t.close() 依次关闭游标和链接
6. 注意：处理查询操作必须都要提交我们的链接，connect.commit()

{% codeblock %}
from pymysql import *

def main():
    connect_t =  connect(host='localhost',port=3306,
    user='root',password='961948438',database='python',charset='utf8')
    cursor  = connect_t.cursor()
    cursor.execute('select * from xxx;')
    print('======')
    print(cursor.fetchmany(10))
    cursor.close()
    connect_t.close()
    pass

if __name__ == '__main__':
    main()
{% endcodeblock %}
6. 注意：sql的查询语句不需要利用链接（不是游标cursor）去调用commit提交：
7. 但是其他的增删改操作需要利用链接对象去调用commit方法去确认语句，如果此时不想确认sql操作可以调用
8.  链接对象下的rollback（）方法，但此时递增的数据库表字段可能已经发生了递增；

#### 了解sql注入
  1. 什么是sql注意：
  在程序事先定义好的 查询语句中添加额外的SQL语句 ，在管理员不知情的情况下实现非法操作，以此来实现 欺骗数据库服务器执行非授权的任意查询 ，从而进一步得到相应的数据信息。
  SQL注入通俗说就是：
  2. 通过SQL语句找到破绽，进行非法的数据读取。
  python可以通过execute（sql语句，[参数]）方法让execute自行拼接，而不是通过
  sql = ’select * from xxx  %s‘ %  inputname 手动拼接；
