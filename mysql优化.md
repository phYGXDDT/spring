mysql优化

数据库基础部分

第一章 数据库介绍篇

1-1 什么是数据库

​	数据库，保存数据库的仓库。它体现我们的电脑中，就是一个文件系统。然后把数据都保存在这些特殊的文件中，并且需要使用固定的语言（SQL语言）去操作文件中的数据。

技术定义：

​	数据库（Database）是按照数据结构来组织、存储和管理数据的建立在计算机存储设备上的仓库。

1-2 数据库介绍

​	我们开发应用程序的时候，程序中的所有数据，最后都需要保存到专业软件中。这些专业的保存数据的软件我们称为数据库。

​	我们学习数据库，并不是学习如何去开发一个数据库软件，我们学习的是如何使用数据库以及数据库中的数据记录的操作。而数据库软件是由第三方公司研发。

1-3 数据库的分类

​	**关系型、非关系型的数据库**。

常见的数据库软件：

- Oracle：它是Oracle公司的大型关系型数据库，它是收费的。
- DB2：IBM公司的数据库，它是收费的。
- SqlServer：微软数据库，收费。
- SyBase：SyBase公司的，工具PowerDesign数据库建模工具。
- MySql：早期瑞典一个公司发明，后期被sun公司收购，后期被Oracle。

Java开发应用程序主要使用的数据库：MySQL（5.5）、Oracle、DB2。

1-4 什么是关系型数据库

​	在开发软件的时候，软件中的数据之间必然会有一定的关系存在，需要把这些数据保存在数据库中，同时也要维护数据之间的关系，这时就可以直接使用上述的哪些数据库。而上述的所有数据库都属于关系型数据库。

​	描述数据之间的关系，并保存在数据库中，同时学习如果根据这些关系查询数据库中的数据，关系型数据，设计数据库的时候，需要使用E-R图来描述。实体关系E-R：实体关系图。

​	实体：可以理解为我们Java程序中的一个对象。在E-R图中使用矩形（长方形）表示。针对一个实体中的属性，我们称为这个实体的数据，在E-R图中使用椭圆表示。实体和实体之间的关系，在E-R图中使用菱形表示。



第二章 MySQL在Linux-安装篇

2-1 vmware中安装linux注意事项

1、记得关闭防火墙

```shell
service iptables stop
chkconfig iptables off(关闭开机自启：所谓的永久关闭防火墙)
```

2、创建统一的管理目录

```shell
mkdir -p /export/software
mkdir -p /export/servers
```

3、软件环境

```shell
VMware、crt、centos6.9
```

4、安装环境

```shell
1、VMware软件安装
2、构建虚拟机
3、需要配置Linux（ip,mac地址,hostname,防火墙），可以通过crt这个客户端连接进行操作
4、在Linux操作系统进行安装mysql-5.6
说明：因为在Linux操作系统中，安装软件的方式主要有3种：1、源码安装（redis）2、rpm安装 3、yum在线安装（安装MySQL为例）  ---  Linux联网
```

2-2 centos6.9安装MySQL

1、检查是否有自带的mysql

```shell
[root@hadoop-01 server]# rpm -qa | grep mysql
mysql-libs-5.1.73-8.el6_8.x86_64
```

2、卸载自带的mysql

```shell
[root@hadoop-01 server]# rpm -e --nodeps mysql-libs-5.1.73-8.el6_8.x86_64
[root@hadoop-01 server]#
```

3、下载mysql安装包

4、上传安装包到Linux服务器

```shell
rz 上传文件到指定目录(yum install -y lrzsz)
/export/software/mysql
```

5、安装

```shell
rpm -ivh *.rpm
```

6、查看初始化密码

```shell
A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !
You will find that password in '/root/.mysql_secret'.
```

```shell
[root@mysql ~]# cat /root/.mysql_secret
# The random password set for the root user at Wed Aug  8 22:19:00 2018 (local time):
xQkCU3KBASAS_c

[root@mysql ~]# 
```

7、启动mysql并登录

```shell
#启动mysql
service mysql start
```

```shell
#登录mysql
mysql -uroot -p
(粘贴密码：xQkCU3KBASAS_c)
```

8、修改密码

```shell
set PASSWORD=PASSWORD('123456');
```

9、退出客户端

```sql
mysql>quit
```

10、用新密码进行登录

```shell
mysql -uroot -p
123456 (新密码)
```

11、远程授权

```sql
grant all privileges on *.* to 'root' @'%' identified by '123456';
flush privileges;
```

12、验证远程授权是否成功

```shell
通过windows的mysql客户端工具连接，是否连接上就授权成功，没有连接上，说明没有授权成功！
```



第三章 MySQL-基础操作篇

3-1 登录MySQL

```sql
mysql -uroot -p
123456
```

3-2 退出mysql

```shell
mysql> quit
```

3-3 输入查询

- 查看当前mysql的版本号及当前时间

```sql
mysql> select version(),current_date;
```

```sql
mysql> select version(),current_date;
+-----------+--------------+
| version() | current_date |
+-----------+--------------+
| 5.7.32    | 2020-12-15   |
+-----------+--------------+
1 row in set (0.00 sec)
```

- mysql中sql语句不区分大小写
- 可以进行简单计算（如下所示）

```sql
mysql> select sin(pi()/4),(4+1)*5;

mysql> select sin(pi()/4),(4+1)*5;
+--------------------+---------+
| sin(pi()/4)        | (4+1)*5 |
+--------------------+---------+
| 0.7071067811865475 |       25 |
+--------------------+---------+
1 row in set (0.00 sec)
```

- 多条语句比较短，可以写一行

```sql
mysql> select version(); select now();

mysql> select version(); select now();
+-----------+
| version() |
+-----------+
| 5.7.32    |
+-----------+
1 row in set (0.00 sec)

+---------------------+
| now()               |
+---------------------+
| 2020-12-15 08:08:42 |
+---------------------+
1 row in set (0.00 sec)
```

- 多个字段之间可以用逗号分隔，多行组成一条语句结束以分号结束

```sql
mysql> select
    -> user()
    -> ,
    -> current_date;
+----------------+--------------+
| user()         | current_date |
+----------------+--------------+
| root@localhost | 2020-12-15   |
+----------------+--------------+
1 row in set (0.00 sec)
```

- sql语句写了一半，又不想执行可以在语句末尾加上‘\c’

```sql
mysql> select
    -> user()
    -> \c
mysql>
```

3-4 创建和使用数据库

- 查看当前有哪些数据库

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

- 创建数据库

```sql
mysql> create database menagerie;
Query OK, 1 row affected (0.00 sec)
```

- 使用及切换数据库

```sql
mysql> use menagerie
Database changed
```

3-5 创建表及使用

- 查看当前数据库有哪些表

```sql
mysql> show tables;
```

- 创建一个表

```sql
mysql> create table pet (name varchar(20),owner varchar(20),
    -> species varchar(20),sex char(1),birth date,death date);

```

- 校验创建表语句是否执行一致

```sql
mysql> show create table pet;

+-------+-------------
| Table | Create Table                                                                             +-------+-------------
| pet   | CREATE TABLE `pet` (
  `name` varchar(20) DEFAULT NULL,
  `owner` varchar(20) DEFAULT NULL,
  `species` varchar(20) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `birth` date DEFAULT NULL,
  `death` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+-------+--------------
```

- 查看表详情

```sql
mysql> desc pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

- 准备数据

```sql
Fluffy Harold cat f 1993-02-04
Clawa Gwen cat m 1994-03-17
Buffy Harold dog f 1989-05-13
Fang Benny dog m 1990-08-27
Bowser Diane dog m 1979-08-31 1995-07-29
Chirpy Gwen bird f 1998-09-11
Whistler Gwen bird 1997-12-09
Slim Benny snake m 1996-04-29
```

3-6 表中导入数据

​	在表中导入数据的方式有两种

- 第一种：将以上数据整理成SQL语句，insert into pet...

- 第二种：通过加载文件的方式将数据导入到表中

  1、创建一个pet.txt的文件（注：每个字段中用tab键隔开，字段没有值得记录用\N代替）

  ```sql
  Fluffy	Harold	cat	f	1993-02-04
  Clawa	Gwen	cat	m	1994-03-17
  Buffy	Harold	dog	f	1989-05-13
  Fang	Benny	dog	m	1990-08-27
  Bowser	Diane	dog	m	1979-08-31	1995-07-29
  Chirpy	Gwen	bird	f	1998-09-11
  Whistler	Gwen	bird	\N	1997-12-09	\N
  Slim	Benny	snake	m	1996-04-29
  ```

  2、加载数据

  ```sql
  mysql> load data local infile '/root/data/pet.txt' into table pet;
  ```

  3、校验是否加载进入

  ```sql
  mysql> select * from pet;
  +----------+--------+---------+------+------------+------------+
  | name     | owner  | species | sex  | birth      | death      |
  +----------+--------+---------+------+------------+------------+
  | Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
  | Clawa    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Buffy    | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
  | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +----------+--------+---------+------+------------+------------+
  8 rows in set (0.00 sec)
  ```

3-7 数据检索部分

1、检索全部数据

```sql
mysql> select * from pet;
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Clawa    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
| Buffy    | Harold | dog     | f    | 1989-05-13 | NULL       |
| Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
| Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
+----------+--------+---------+------+------------+------------+
8 rows in set (0.00 sec)
```

2、删除表中全部数据

```sql
mysql> delete from pet;
mysql> load data local infile '/root/data/pet.txt' into table pet;
```

3、更新表中特定记录的数据

- 更新表中名字为Bowser的生日

```sql
mysql> update pet set birth = '1989-08-31' where name = 'Bowser';
```

4、查询特定的行

- 查询名字为Bowser的记录

```sql
mysql> select * from pet where name = 'Bowser';
+--------+-------+---------+------+------------+------------+
| name   | owner | species | sex  | birth      | death      |
+--------+-------+---------+------+------------+------------+
| Bowser | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+-------+---------+------+------------+------------+
1 row in set (0.00 sec)
```

说明：字符串比较不区分大小写！如下所示：

```sql
mysql> select * from pet where name = 'Bowser';
+--------+-------+---------+------+------------+------------+
| name   | owner | species | sex  | birth      | death      |
+--------+-------+---------+------+------------+------------+
| Bowser | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+-------+---------+------+------------+------------+
1 row in set (0.00 sec)

mysql> select * from pet where name = 'BowsEr';
+--------+-------+---------+------+------------+------------+
| name   | owner | species | sex  | birth      | death      |
+--------+-------+---------+------+------------+------------+
| Bowser | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+-------+---------+------+------------+------------+
1 row in set (0.00 sec)

mysql> select * from pet where name = 'BOWSER';
+--------+-------+---------+------+------------+------------+
| name   | owner | species | sex  | birth      | death      |
+--------+-------+---------+------+------------+------------+
| Bowser | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+-------+---------+------+------------+------------+
1 row in set (0.00 sec)
```

（1）查找生日为1998年以后的特定查询

```sql
mysql> select * from pet where birth >= '1998-1-1';
+--------+-------+---------+------+------------+-------+
| name   | owner | species | sex  | birth      | death |
+--------+-------+---------+------+------------+-------+
| Chirpy | Gwen  | bird    | f    | 1998-09-11 | NULL  |
+--------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)
```

（2）多条件查询（and|or）

```sql
mysql> select * from pet where species = 'dog' and sex = 'f';
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
1 row in set (0.00 sec)

mysql> select * from pet where species = 'snake' or species = 'bird';
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
| Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL  |
| Slim     | Benny | snake   | m    | 1996-04-29 | NULL  |
+----------+-------+---------+------+------------+-------+
3 rows in set (0.00 sec)

```

- 优先执行括号中的逻辑

```sql
mysql> select * from pet where (species = 'cat' and sex = 'm')
    -> or (species = 'dog' and sex = 'f');
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Clawa | Gwen   | cat     | m    | 1994-03-17 | NULL  |
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
2 rows in set (0.00 sec)
```

5、检索特定的列

```sql
mysql> select name,birth from pet;
+----------+------------+
| name     | birth      |
+----------+------------+
| Fluffy   | 1993-02-04 |
| Clawa    | 1994-03-17 |
| Buffy    | 1989-05-13 |
| Fang     | 1990-08-27 |
| Bowser   | 1989-08-31 |
| Chirpy   | 1998-09-11 |
| Whistler | 1997-12-09 |
| Slim     | 1996-04-29 |
+----------+------------+
8 rows in set (0.00 sec)
```

- 查询不重复的字段要使用distinct

```sql
mysql> select distinct owner from pet;
+--------+
| owner  |
+--------+
| Harold |
| Gwen   |
| Benny  |
| Diane  |
+--------+
4 rows in set (0.00 sec)
```

- 可以使用组合条件查询特定的列

```sql
mysql> select name,species,birth from pet 
    -> where species = 'dog' or species = 'cat';
+--------+---------+------------+
| name   | species | birth      |
+--------+---------+------------+
| Fluffy | cat     | 1993-02-04 |
| Clawa  | cat     | 1994-03-17 |
| Buffy  | dog     | 1989-05-13 |
| Fang   | dog     | 1990-08-27 |
| Bowser | dog     | 1989-08-31 |
+--------+---------+------------+
5 rows in set (0.01 sec)
```

6、排序

- 根据某个字段进行排序（关键词：order by）

```sql
mysql> select name,birth from pet order by birth;
+----------+------------+
| name     | birth      |
+----------+------------+
| Buffy    | 1989-05-13 |
| Bowser   | 1989-08-31 |
| Fang     | 1990-08-27 |
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Slim     | 1996-04-29 |
| Whistler | 1997-12-09 |
| Chirpy   | 1998-09-11 |
+----------+------------+
8 rows in set (0.00 sec)
```

- 升降序排列（desc：降序；asc：升序）

```sql
mysql> select name,birth from pet order by birth desc;  //降序排列
mysql> select name,birth from pet order by birth asc;   //升序排列
```

- 多列排序

​	根据species字段升序排列，根据birth字段降序排列

​	**注：order by species中无asc，desc，默认为升序排列**

```sql
mysql> select name,species,birth from pet 
    -> order by species,birth desc;
+----------+---------+------------+
| name     | species | birth      |
+----------+---------+------------+
| Chirpy   | bird    | 1998-09-11 |
| Whistler | bird    | 1997-12-09 |
| Claws    | cat     | 1994-03-17 |
| Fluffy   | cat     | 1993-02-04 |
| Fang     | dog     | 1990-08-27 |
| Bowser   | dog     | 1989-08-31 |
| Buffy    | dog     | 1989-05-13 |
| Slim     | snake   | 1996-04-29 |
+----------+---------+------------+
8 rows in set (0.00 sec)
```

7、日期计算

查看宠物多少岁，就可以使用日期的函数timestampdiff()

```sql
# 查看当前的日期
mysql> select curdate() from pet;
+------------+
| curdate()  |
+------------+
| 2020-12-15 |
+------------+

# 获取当年的年
mysql> select year('2018-02-05') as years from pet;
+-------+
| years |
+-------+
|  2018 |
+-------+

# 获取当年的月
mysql> select month('2018-02-05') as months from pet;
+-------+
| months |
+-------+
|     2 |
+-------+

# 获取当年的日
mysql> select day('2018-02-05') as days from pet;
+------+
| days |
+------+
|    5 |
+------+
```

```sql
mysql> select name,birth,curdate(),
    -> timestampdiff(year,birth,curdate()) as age
    -> from pet;
+----------+------------+------------+------+
| name     | birth      | curdate()  | age  |
+----------+------------+------------+------+
| Fluffy   | 1993-02-04 | 2020-12-15 |   27 |
| Claws    | 1994-03-17 | 2020-12-15 |   26 |
| Buffy    | 1989-05-13 | 2020-12-15 |   31 |
| Fang     | 1990-08-27 | 2020-12-15 |   30 |
| Bowser   | 1989-08-31 | 2020-12-15 |   31 |
| Chirpy   | 1998-09-11 | 2020-12-15 |   22 |
| Whistler | 1997-12-09 | 2020-12-15 |   23 |
| Slim     | 1996-04-29 | 2020-12-15 |   24 |
+----------+------------+------------+------+
8 rows in set (0.00 sec)
```

8、null和not null值

对一些字段类型要进行检查。判断某些字段是否为NULL，或者NOT NULL

```sql
mysql> select name,birth,death, timestampdiff(year,birth,death) as age from pet where death is not null order by age;
+--------+------------+------------+------+
| name   | birth      | death      | age  |
+--------+------------+------------+------+
| Bowser | 1989-08-31 | 1995-07-29 |    5 |
+--------+------------+------------+------+
1 row in set (0.00 sec)
```



第四章 实例

​	以下是如何解决MySQL的一些常见问题的示例

​	首先创建一个表，并且导入数据

```sql
create table shop(
	article int(4) unsigned zerofill default '0000' not null,
  dealer char(20) default '' not null,
  price double(16,2) default '0.00' not null,
  primary key(article,dealer)
);

insert into shop values(1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),(3,"C",1.69),(3,"D",1.25),(4,"D",19.95);
```

1、检索表中的所有数据

```sql
mysql> select * from shop;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | A      |  3.45 |
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | B      |  1.45 |
|    0003 | C      |  1.69 |
|    0003 | D      |  1.25 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
7 rows in set (0.00 sec)
```

2、求某一列的最大值和最小值

```sql
mysql> select max(price) as maxPrice from shop;
+----------+
| maxPrice |
+----------+
|    19.95 |
+----------+
1 row in set (0.00 sec)

mysql> select min(price) as minPrice from shop;
+----------+
| minPrice |
+----------+
|     1.25 |
+----------+
1 row in set (0.00 sec)
```

3.1、过滤出某一个字段值最大的整条记录数据

```sql
mysql> select * from shop where price = (select max(price) from shop;
);
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
1 row in set (0.00 sec)

```

3.2、也可以通过关联查询进行检索

```sql
mysql> select s1.article,s1.dealer,s1.price from shop s1 left join shop s2 on s1..price < s2.price where s2.article is null;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
1 row in set (0.00 sec)

mysql> select article,dealer,price
    -> from shop
    -> order by price desc
    -> limit 1;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
1 row in set (0.00 sec)
```

4、求出每一列的最大值，并且根据某一个字段进行分组

```sql
mysql> select article,max(price) as price from shop group by article;
+---------+-------+
| article | price |
+---------+-------+
|    0001 |  3.99 |
|    0002 | 10.99 |
|    0003 |  1.69 |
|    0004 | 19.95 |
+---------+-------+
4 rows in set (0.00 sec)
```

5、

```sql
mysql> select article,dealer,price from shop s1 where price = (select max(s2.price) from shop s2 where s1.article = s2.article);
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | C      |  1.69 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
4 rows in set (0.00 sec)
```



第五章 SQL中的聚合函数

​	SQL语言中定义了部分函数，可以帮助我们完成对查询结果的计算操作。

- count统计个数（行数）
- sum函数：求和
- avg函数：求平均值
- max、min函数：求最大和最小值

5-1 count函数

语法：

```sql
select count(*)|count(列名) from 表名
```

**注意：count在根据指定的列统计的时候，如果这一列中有null不会被统计在其中。**

```
mysql> select count(*) from pet;
+----------+
| count(*) |
+----------+
|        8 |
+----------+
1 row in set (0.00 sec)

mysql> select count(sex) from pet;
+------------+
| count(sex) |
+------------+
|          7 |
+------------+
1 row in set (0.00 sec)

mysql> select count(death) from pet;
+--------------+
| count(death) |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
```

5-2 sum函数

语法：

```sql
select sum(列名) from 表名
```

注意事项：

​	1、如果使用sum多列进行求和的时候，如果某一列中有null，这一列所在的行中的其他数据不会被加到总和。

​	2、可以使用mysql数据库提供的函数`ifnull(列名，值)`。

​	3、在数据库中定义的double类型数据，是一个近似值，需要精确到准确的位数，这时可以把这一列设计成numeric类型。numeric（数据的总列数，小数位数）

```sql
mysql> select sum(price) from shop;
+------------+
| sum(price) |
+------------+
|      42.77 |
+------------+
1 row in set (0.01 sec)
```

5-3 avg函数

语法：

```sql
select avg(列名) from 表名
```

```sql
mysql> select avg(price) from shop;
+------------+
| avg(price) |
+------------+
|   6.110000 |
+------------+
1 row in set (0.00 sec)
```

5-4 max函数

语法：

```sql
select max(列名) from 表名
```

```sql
mysql> select max(price) from shop;
+------------+
| max(price) |
+------------+
|      19.95 |
+------------+
1 row in set (0.00 sec)
```

5-5 min函数

语法：

```
select min(列名) from 表名
```

```sql
mysql> select min(price) from shop;
+------------+
| min(price) |
+------------+
|       1.25 |
+------------+
1 row in set (0.00 sec)
```



第六章 SQL分类

6-1 DDL（数据定义问题）

数据定义语言：Data Definition Language

用来定义数据库对象，如数据表、视图、索引等。

```sql
创建数据库：create database test;
创建视图：create view test;
创建索引：create index test;
创建表：create table test1(.......);
```



6-2 DML（数据操纵问题）

数据处理语言：Data Manipulation Language

在数据库中更新，增加和删除记录

如update、insert、delete



6-3 DCL（数据控制问题）

数据控制语言：Data Control Language

指用于设置用户权限和控制事务语句

如grant、revoke、if...else、while、begintransaction



6-4 DQL（数据查询问题）

数据查询语言：Data Query Language

如select



第七章 数据库的备份与恢复

7-1 备份命令

​	在mysql的安装目录的bin目录下有mysqldump命令，可以完成对数据库的备份。

语法：

```sql
mysqldump -u用户名 -p密码 数据库名 > 磁盘SQL文件路径
```

由于mysqldump命令不是sql命令。需要在dos窗口下使用。

​	注意：在备份数据的时候，数据库不会被删除。可以手动删除数据库。同时在恢复数据的时候，不会自动的给我们创建数据库，仅仅只会恢复数据库中的表和表中的数据。

```shell
mysqldump -uroot -p123456 menagerie > /root/data/menagerie.sql

//备份文件
-rw-r--r--. 1 root root 3118 Oct 20 04:04 menagerie.sql
```

7-2 恢复命令

​	恢复数据库，需要手动的先创建数据库：create database heima2;

语法：

```
mysql -u用户名 -p密码 导入库名 < 磁盘SQL文件绝对路径
```

需求：

​	① 创建heima8数据库。

​	② 重新开启一个新的dos窗口。

​	③ 将mvdb2备份的数据表和表数据恢复到mvdb6中。

```sql
//恢复命令
mysql -uroot -p123456 itcast < /root/data/menagerie.sql
```



第八章 多表查询

8-1 笛卡尔积介绍

​	笛卡尔积是指在数学中，两个集合X和Y的笛卡尔积，又称直积，表示为X×Y，第一个对象是X的成员而第二个对象是Y的所有可能有序对的其中一个成员。

准备数据：

```sql
create table A(
	A_ID int primary key auto_increment,
  A_NAME varchar(20) not null
);
insert into A values(1,'apple');
insert into A values(2,'orange');
insert into A values(3,'banana');

create table B(
	A_ID int primary key auto_increment,
  B_PRICE double
);
insert into B values(1,2.30);
insert into B values(2,3.50);
insert into B values(4,null);
```

展示效果：

```sql
mysql> select * from A,B;
+------+--------+------+---------+
| A_ID | A_NAME | A_ID | B_PRICE |
+------+--------+------+---------+
|    1 | apple  |    1 |     2.3 |
|    2 | orange |    1 |     2.3 |
|    3 | banana |    1 |     2.3 |
|    1 | apple  |    2 |     3.5 |
|    2 | orange |    2 |     3.5 |
|    3 | banana |    2 |     3.5 |
|    1 | apple  |    4 |    NULL |
|    2 | orange |    4 |    NULL |
|    3 | banana |    4 |    NULL |
+------+--------+------+---------+
9 rows in set (0.01 sec)
```

作用：笛卡尔积的数据，对程序是没有意义的，我们需要对笛卡尔积中数据再次进行过滤

对于多表查询操作，需要过滤出满足条件的数据，需要把多个表进行连接，连接之后需要加上过滤的条件。

```sql
mysql> select * from A,B where A.A_ID=1;
+------+--------+------+---------+
| A_ID | A_NAME | A_ID | B_PRICE |
+------+--------+------+---------+
|    1 | apple  |    1 |     2.3 |
|    1 | apple  |    2 |     3.5 |
|    1 | apple  |    4 |    NULL |
+------+--------+------+---------+
3 rows in set (0.00 sec)
```

8-2 内连接

图示：

![1608095831199](D:\Java\image\1608095831199.png)

​	语法一：

```sql
select 列名,列名... from 表名1,表名2 where 表名1.列名=表名2.列名;
```

​	语法二：

```sql
select * from 表名1 inner join 表名2 on 条件
```

```sql
mysql> select * from A inner join B on A.A_ID=B.A_ID;
+------+--------+------+---------+
| A_ID | A_NAME | A_ID | B_PRICE |
+------+--------+------+---------+
|    1 | apple  |    1 |     2.3 |
|    2 | orange |    2 |     3.5 |
+------+--------+------+---------+
2 rows in set (0.00 sec)
```

8-3 左外连接

外连接：左外连接、右外连接、全连接、自连接。

图示：

![1608095941795](D:\Java\image\1608095941795.png)

左外连接：用左边表去右边表中查询对应记录，不管是否找到，都将显示左边表中全部记录。

即：虽然右表没有香蕉对应的价格，也要把他查询出来。

语法：

```sql
select * from 表名1 left outer join 表2 on 条件;
```

```sql
mysql> select * from A left join B on A.A_ID=B.A_ID;
+------+--------+------+---------+
| A_ID | A_NAME | A_ID | B_PRICE |
+------+--------+------+---------+
|    1 | apple  |    1 |     2.3 |
|    2 | orange |    2 |     3.5 |
|    3 | banana | NULL |    NULL |
+------+--------+------+---------+
3 rows in set (0.01 sec)
```

8-4 右外连接

图示：

![1608096131042](D:\Java\image\1608096131042.png)

右外连接：用右边表去左边表查询对应的记录，不管是否找到，右边表全部记录都将显示。

即：不管左方能够找到右方价格对应的水果，都要把左方的价格显示出来。

语法：

```sql
select * from 表名1 right outer join 表2 on 条件;
```

```sql
mysql> select * from A right join B on A.A_ID=B.A_ID;
+------+--------+------+---------+
| A_ID | A_NAME | A_ID | B_PRICE |
+------+--------+------+---------+
|    1 | apple  |    1 |     2.3 |
|    2 | orange |    2 |     3.5 |
| NULL | NULL   |    4 |    NULL |
+------+--------+------+---------+
3 rows in set (0.01 sec)
```

8-5 全外连接

全外连接：左外连接和右外连接的结果合并，单会去掉重复的记录。

语法：

```sql
select * from 表1 full outer join 表2 on 条件;
select * from a full outer join b on a.A_ID=b.A_ID; 但是mysql数据库不支持此语法。
```

8-6 关联子查询

子查询：把一个sql的查询结果作为另外一个查询的参数存在。

1、in和exist关键词的用法

关联子查询其他的关键词使用：

回忆：age =23 or age =24 等价于age in(23,24)

in 表示条件应该是在多个列值中。

in：使用在where后面，经常表示是一个列表的数据，只要被查询的数据在这个列表中存在即可。

```sql
mysql> select * from A where A.A_ID in(1,2,3,4);
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
|    3 | banana |
+------+--------+
3 rows in set (0.10 sec)

等同于
mysql> select * from A where A.A_ID = 1 or A.A_ID=2 or A.A_ID=3 or A.A_ID=4;
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
|    3 | banana |
+------+--------+
3 rows in set (0.00 sec)

not in的使用：

mysql> select * from A where A.A_ID not in(3,4);
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
+------+--------+
2 rows in set (0.00 sec)
```

exist：表示存在，当子查询的结果存在，就会显示主查询中的数据。

```sql
mysql> select * from A where exists(select A_ID from B);
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
|    3 | banana |
+------+--------+
3 rows in set (0.00 sec)
```

2、union和union all的使用

**UNION语句**：用于将不同表中相同列中查询的数据展示出来；（不包括重复数据）

**UNION ALL语句**：用于将不同表中相同列中查询的数据展示出来；（包括重复数据）

```sql
mysql> select * from A union select * from B;
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
|    3 | banana |
|    1 | 2.3    |
|    2 | 3.5    |
|    4 | NULL   |
+------+--------+
6 rows in set (0.00 sec)

mysql> select * from A union all select * from B;
+------+--------+
| A_ID | A_NAME |
+------+--------+
|    1 | apple  |
|    2 | orange |
|    3 | banana |
|    1 | 2.3    |
|    2 | 3.5    |
|    4 | NULL   |
+------+--------+
6 rows in set (0.00 sec)
```

3、case when语句

case when语句语法结构：

```sql
CASE sex
WHEN '1' THEN '男'
WHEN '2' THEN '女'
ELSE '其他' END
```

准备数据：

```sql
//创建表
create table employee(
	empid int,
  deptid int,
  sex varchar(20),
  salary double
);

//加载数据
insert into employee values(1,10,'female',5500.00);
insert into employee values(2,10,'male',4500.00);
insert into employee values(3,20,'female',1900.00);
insert into employee values(4,20,'male',4800.00);
insert into employee values(5,40,'female',6500.00);
insert into employee values(6,40,'female',14500.00);
insert into employee values(7,40,'male',44500.00);
insert into employee values(8,50,'male',6500.00);
insert into employee values(9,50,'male',7500.00);
```

```sql
select *,
case
when salary < 5000 then '低等收入'
when salary >= 5000 and salary < 10000 then '中等收入'
when salary > 10000 then '高等收入'
end as level,
case sex
when "female" then 1
when "male" then 0
end as flag
from employee;
```



第九章 MYSQL的数据类型

MySQL中定义数据字段的类型对你数据库的优化是非常重要的。

MySQL支持多种类型，大致分为三类：数值、日期/时间和字符串（字符）类型。

9-1 数值类型

MySQL支持所有标准SQL数值数据类型。

这些类型包括严格数值数据类型（integer、smallint、decimal和numeric）、以及近似数值数据类型（float、deal和double frecision）。

关键字INT是INTEGER的同义词，关键字dec是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持MyISAM、MEMORY、InnoDB和BDB表。

作为SQL标准的扩展，MySQL也支持整数类型tinyint、mediumint和bigint。下面的表显示了需要的每个整数类型的存储和范围。

| 类型         |                  大小                  | 范围（有符号）                                               | 范围（无符号）                                               | 用途           |
| ------------ | :------------------------------------: | :----------------------------------------------------------- | ------------------------------------------------------------ | -------------- |
| INTYINT      |                 1字节                  | (-128, 127)                                                  | (0, 255)                                                     | 小整数值       |
| SMALLINT     |                 2字节                  | (-32 768, 327 67)                                            | (0, 65 535)                                                  | 大整数值       |
| MEDIUMINT    |                 3字节                  | (-8 388 608, 8 388 607)                                      | (0, 16 777 215)                                              | 大整数值       |
| INT或INTEGER |                 4字节                  | (-2 147 483 648, 2 147 483 647)                              | (0, 4 294 967 295)                                           | 大整数值       |
| BIGINT       |                 8字节                  | (-9 233 372 036 854 775 808, 9 223 372 036 854 775 807)      | (0, 18 446 744 073 709 551 615)                              | 极大整数值     |
| FLOAT        |                 4字节                  | (-3.402 823 466 E+38, -1.175 484 651 E-38, 0, (1.175 494 351 E-38, 3.402 823 466 351 E+38)) | 0, (1.175 494 351 E-38, 3.402 823 466 E+38)                  | 单精度浮点数值 |
| DOUBLE       |                 8字节                  | (-1.797 693 134 862 315 7 E+308,-2.225 073 858 507 201 4 e-308), 0, (2.225 073 858 507 201 4 e-308, 1.797 396 134 862 315 7 e=308) | 0, (2.225 073 858 507 201 4 e-308, 1.797 693 134 862 315 7 e+308) | 双精度浮点数值 |
| DECIMAL      | 对DECIMAL(M,D).如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值         |

9-2 日期和时间类型

表示时间的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效范围和一个“零”值。当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。

| 类型      | 大小（字节） | 范围                                                         | 格式                | 用途                     |
| --------- | ------------ | ------------------------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3            | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3            | '-838:59;59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1            | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8            | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4            | 1970-01-01 00:00:00/2038的结束时间是**2147483647**秒，北京时间**2038-1-19 11:4:07**，格林尼治时间2038年1月19日凌晨03:14:07 | YYYYMMDDHHMMSS      | 混合日期和时间值，时间戳 |

9-3 字符串类型

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

| 类型       | 大小             | 用途                          |
| ---------- | ---------------- | ----------------------------- |
| CHAR       | 0-255字节        | 定长字符串                    |
| VARCHAR    | 0-65536字节      | 变长字符串                    |
| TINYBLOB   | 0-255字节        | 不超过255个字符的二进制字符串 |
| TINYTEXT   | 0-255字节        | 短文本字符串                  |
| BLOB       | 0-65535字节      | 二进制形式的长文本数据        |
| TEXT       | 0-65535字节      | 长文本数据                    |
| MEDIUMBLOB | 0-16777215字节   | 二进制形式的中等长度文本数据  |
| MEDIUMTEXT | 0-16777215字节   | 中等长度文本数据              |
| LONGBLOB   | 0-4294967295字节 | 二进制形式的极大文本数据      |
| LONGTEXT   | 0-4294967295字节 | 极大文本数据                  |

CHAR和VARCHAR类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部定格被保留等方面也不同。在存储或检索过程中不进行大小写转换。

BINARY和VARBINARY类似于CHAR和VARCHAR，同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。

BLOB是一个二进制大对象，可以容纳可变数量的数据。有四种BLOB类型：TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB。它们区别于可容纳存储的范围不同。

有四种TEXT类型：TINYTEXT、TEXT、MEDIUMTEXT和LONGTEXT。对应的这四种BLOB类型，可存储的最大长度不同，可根据实际情况选择。



补充：

MySQL5.0以上版本：

1、一个汉字栈多少长度与编码有关：

**UTF-8**：一个汉字=3个字节

**GBK**：一个汉字=2个字节

2、varchar(n)表示n个字符，无论汉字和英文，MySQL都能存入n个字符，仅是实际长度有所区别。

3、MySQL检查长度，可用SQL语言来查看：

```sql
select LENGTH(filedname) from tablename;
```



1、整型

| MySQL数据类型 | 含义（有符号）                       |
| ------------- | ------------------------------------ |
| tinyint(m)    | 1个字节 范围(-128~127)               |
| smallint(m)   | 2个字节 范围(-32768~32767)           |
| mediumint(m)  | 3个字节 范围(-8388608~8388607)       |
| int(m)        | 4个字节 范围(-2147483648~2417483647) |
| bigint(m)     | 8个字节 范围(+-9.22*10的18次方)      |

取值范围如果加了unsigned，则最大值翻倍，如tinyint unsigned的取值范围为（0~256）。

int(m)里面的m是表示SELECT查询结构集中的显示长度，并不影响实际的取值范围，没有影响到显示的宽度，不知道这个m有什么用。

2、浮点型(float和double)

| MySQL数据类型 | 含义                                          |
| ------------- | --------------------------------------------- |
| float(m,n)    | 单精度浮点型 8位精度(4字节) m总个数，n小位数  |
| double(m,n)   | 双精度浮点型 16位精度(8字节) m总个数，n小位数 |

设一个字段定义为float(5,3)，如果插入一个数123.45678，实际数据库里存的是123.457，但总个数还可以实际为准，即6位。

3、定点数

浮点型在数据库中存放的是近似值，而定点类型在数据库中存放的是精确值。

decimal(m,d)参数m<65是总个数，d<30且d<m是小位数。

4、字符串(char、varchar、_text)

| MySQL数据类型 | 含义                            |
| ------------- | ------------------------------- |
| char(n)       | 固定长度，最多255个字符         |
| varchar(n)    | 固定长度，最多655355个字符      |
| tinytext      | 可变长度，最多255个字符         |
| text          | 可变长度，最多65535个字符       |
| mediumtext    | 可变长度，最多2的24次方-1个字符 |
| longtext      | 可变长度，最多2的32次方-1个字符 |

char和varchar：

- **1.char(n)若存入字符数小于n，则以空格补于	其后，查询之时再将空格去掉。所以char类型存储的字符串末尾不能有空格，varchar不限于此。
- **2.char(n)固定长度，char(4)不管是存入几个字符，都将占用4个字节，varchar是存入的实际字符数 +1 个字节（n<=255）或2个字节（n>255）,所以varchar(4)存入3个字符将占用4个字节。
- **3.char类型的字符串检索速度要比varchar类型快。

varchar和text：

- **1.varchar可指定n，text不能指定，内部存储varchar是存入的时间字符数 +1 个字节（n<=255)或2个字节（n>255）,text是实字符数 +2 个字节。
- **2.text类型不能有默认值。
- **3.varchar可直接创建索引，text创建索引要指定前多少个字符。varchar查询速度快于text，在都创建索引的情况下，text的索引似乎不起作用。

5、二进制(Blob)

- **1.BOLB和text存储方式不同，text以文本方式存储，英文存储区分大小写，而blob是以二进制方式存储，不区分大小写。
- **2.bolb存储的数据只能整体读出。
- **3.text可以指定字符集，blob不能指定字符集。

6、日期时间类型

| MySQL数据类型 | 含义                          |
| ------------- | ----------------------------- |
| date          | 日期 '2008-12-2'              |
| time          | 时间 '12:25:36'               |
| datetime      | 日期时间 '2008-12-2 22:06:44' |
| timestamp     | 自动存储记录修改时间          |

若定义一个字段为timestamp，这个字段里的时间数据会随其他字段修改的时候自动刷新，所以这个数据类型的字段可以存放这条记录最后被修改的时间。

**数据类型的属性**

| MySQL关键字        | 含义                     |
| ------------------ | ------------------------ |
| NULL               | 数据列可包含NULL值       |
| NOT NULL           | 数据列不允许包含NULL值   |
| DEFAULT            | 默认值                   |
| PRIMARY KEY        | 主键                     |
| AUTO_INCREMENT     | 自动增长，适用于整数类型 |
| UNSIGNED           | 无符号                   |
| CHARACTER SET name | 指定一个字符集           |





第十章 MYSQL GROUP BY语句

GROUP BY语句根据一个或多个列结果集进行分组。

在分组的列上我们可以使用COUNT、SUM、AVG等函数。

语法结构：

```sql
SELECT cloumn_name,function(column_name)
FROM table_name
WHERE cloumn+name operator value
GROUP BY cloumn_name;
```

准备数据：

```sql
create table employee_tb1(
	id int(11) not null,
  name char(10) not null default '',
  date datetime not null,
  singin tinyint(4) not null default '0' comment '登录次数',
  primary key(id)
) engine=InnoDB default charset=utf8;

insert into employee_tb1 values('1','xiaoming','2016-04-22 15:25:33','1'),('2','xiaowang','2016-04-20 15:25:47','3'),('3','xiaoli','2016-04-19 15:26:02','2'),('4','xiaowang','2016-04-07 15:26:14','4'),('5','xiaoming','2016-04-11 15:26:40','4'),('6','xiaoming','2016-04-04 15:26:54','2');
```

```sql
mysql> select name,count(*) from employee_tb1 group by name;
+----------+----------+
| name     | count(*) |
+----------+----------+
| xiaoli   |        1 |
| xiaoming |        3 |
| xiaowang |        2 |
+----------+----------+
3 rows in set (0.00 sec)
```

注意：

​	1、group by可以实现一个最简单的去重查询，假设想看下有哪些员工，除了用distinct，还可以用：

```sql
SELECT name FROM employee_tb1 GROUP BY name;
```

​	返回的结果集就是所有员工的名字。

​	2、分组后的条件使用HAVING来限定，WHERE是对原始数据进行条件限制。几个关键字的使用顺序为：`where、group by、having、order by`。例如：

```sql
SELECT name,count(*) FROM employee_tb1 WHERE id <> 1 GROUP BY name HAVING count(*) > 5 ORDER BY count(*) DESC;
```





第十一章 MySQL LIKE子句

我们知道在MySQL中使用SQL SELECT命令来读取数据，同时我们可以在SELECT语句中使用WHERE子句来获取指定记录。

WHERE子句中可以使用等号 = 来设定获数据的条件，如"company='itcast'"。

但是有时候我们需要获取company字段还有‘it’字符的所有记录，这时我们就需要在WHERE子句中使用SQL LIKE子句。

SQL LIKE子句中使用百分号 % 字符来表示任意字符，类似于UNIX或正则表达式的星号 * 。

如果没有使用百分号 % ,LIKE子句与等号 = 的效果是一样的。

语法：

​	以下是SQL SELECT语句使用LIKE子句从数据库表中读取数据的通用语法。

```sql
SELECT filed1,filed2,...filedN
FROM table_name
WHERE	filed1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
```

- 你可以在WHERE子句中指定任何条件。
- 你可以在WHERE子句中使用LIKE子句。
- 你可以使用LIKE子句代替等号 =。
- LIKE通常与 % 一同使用，类似于一个元字符的搜索。
- 你可以使用AND或者OR指定一个或多个条件。
- 你可以在DELETE或UPDATE命令中使用WHERE...LIKE子句来指定条件。

```sql
mysql> select * from pet where name like '%b%';
+--------+--------+---------+------+------------+------------+
| name   | owner  | species | sex  | birth      | death      |
+--------+--------+---------+------+------------+------------+
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
| Bowser | Diane  | dog     | m    | 1989-08-31 | 1995-07-29 |
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
| Bowser | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
+--------+--------+---------+------+------------+------------+
4 rows in set (0.00 sec)
```



第十二章 MySQL NULL值处理

我们已经知道MySQL使用SQL SELECT命令以及WHERE子句来读取数据库表中的数据，但是当提供的查询条件字段为NULL时，该命令可能就无法正常工作。

为了处理这种情况，MySQL提供了三大运算符：

- **IS NULL**：当列的值是NULL，此运算符返回true。
- **IS NOT NULL**：当列的值不为NULL，此运算符返回true。
- **<=>**：比较操作符（不同于=运算符），当比较的两个值为NULL时返回true。

关于NULL的条件比较运算是比较特殊的，你不能使用 = NULL或!= NULL在列中查找NULL值。

在MySQL中，NULL值与任何其他值的比较（即使是NULL）永远返回false，即NULL = NULL返回false。

MySQL中处理NULL使用IS NULL和IS NOT NULL运算符。





第十三章 MySQL元数据

你可能想知道MySQL一下三种信息：

- **查询结果信息**：SELECT、UPDATE或DELETE语句影响的记录数。
- **数据库和数据表信息**：包含了数据库及数据库表的结构信息。
- **MySQL服务器信息**：包含了数据库服务器的当前状态，版本号等。

在MySQL的命令提示符中，我们可以很容易的获取以上服务器信息。

**获取服务器元数据**

​	以下命令语句可以在MySQL的命令提示符使用，也可以在脚本中使用，如PHP脚本。

| 命令              | 描述                       |
| ----------------- | -------------------------- |
| SELECT VERSION()  | 服务器版本信息             |
| SELECT DATABASE() | 当前数据库名（或者返回空） |
| SELECT USER()     | 当前用户名                 |
| SHOW STATUS       | 服务器状态                 |
| SHOW VARIABLES    | 服务器配置变量             |





第十四章 MySQL ALTER命令

当我们需要修改数据表名或者修改数据表字段时，就需要使用到MySQL ALTER命令。

14-1 删除、添加或者修改表字段

如下命令使用了ALTER命令以及DROP子句来删除以上创建的 i 字段：

```sql
mysql> alter table A drop i;
```

如果数据库表中只剩余一个字段则无法使用DROP来删除字段。

MySQL中使用ADD子句来向数据表中添加列，如实例在表 A 中添加 i 字段，并定义数据类型：

```sql
mysql> alter table A add i int;
```

执行以上命令后，i 字段会自动添加到数据库字段的末尾。

14-2 修改字段类型及名称

如果需要修改字段类型及名称，你可以在ALTER命令中使用MODIFY或CHANGE子句。

例如：把字段 c 的类型从CHAR(1)改为CHAR(10)，可以执行一下命令：

```sql
mysql> alter table A modify c char(10);
```

14-3 修改表名

如果需要修改数据库表的名称，可以在ALTER TABLE语句中使用RENAME子句来实现。

尝试以下实例将数据库表 A 重命名为 A_tb1：

```sql
mysql> alter table A rename to A_tb1;
```





第十五章 MySQL函数

MySQL有很多的内置函数，以下列出了这些函数的说明。

15-1 MySQL字符串函数

| 函数                                  | 描述                                                         | 实例                                                         |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ASCII(s)                              | 返回字符串s的第一个字符的ASCII码                             | 返回CustomerName字段第一个字母的ASCII码：`SELECT ASCII('CustomerName') AS NumCodeFristChar;`   ---输出结果：67 |
| CHAR_LENGTH(s)                        | 返回字符串s的字符数                                          | 返回字符串itcast的字符数：`SELECT CHAR_LENGTH('itcast') AS LengthOfString;`  ---输出结果：6 |
| CHARACTER_LENGTH(s)                   | 返回字符串s的字符数                                          | 返回字符串itcast的字符数：`SELECT CHARACTER_LENGTH('itcast') AS LengthOfString;`  --输出结果：6 |
| CONCAT(s1，s2,...sn)                  | 字符串s1，s2等多个字符串合并为一个字符串                     | 合并多个字符串：`select concat('SQL','itcast','Google','Facebook') as concatenatedString;`  ---输出结果：SQLitcastGoogleFacebook |
| CONCAT_WS(x，s1，s2...sn)             | 同CONCAT(s1,s2,...sn)函数，但是每一个字符串之接要加上x，x可以是分隔符 | 合并多个字符串，并添加分隔符：`select concat_ws('-','SQL','Tutorial','is','fun!') as concatenatedString;`  ---输出结果：SQL-Tutorial-is-fun! |
| FIELD(s，s1，s2,...)                  | 返回第一个字符串s在字符串列表(s1,s2,...)中的位置             | 返回字符串c在列表中的位置：`select field('c','a','b','c','d','e');`  ---输出结果：3 |
| FIND_IN_SET(s1，s2)                   | 返回在字符串s2中与s1匹配的字符串的位置                       | 返回字符串c在指定字符串中的位置：`select find_in_set('c','a,b,c,d,e');`  ---输出结果：3 |
| FORMAT(x，n)                          | 函数可以将数字x进行格式化“#，###.##”，将x保留小数点后n为，最后一位四舍五入。 | 格式化数字"#,###.##"形式：`select format(250500.563,2);`   ---输出结果：250,500.56 |
| INSERT(s1，x，len，s2)                | 字符串s2替换s1的x位置开始长度为len的字符串                   | 从字符串第一个位置开始的6个字符替换为itcast：`select insert('google.com',1,6,'itcast');`  ---输出结果：itcast.com |
| INSTR(s1，s)                          | 从字符串s1中获取s的开始位置                                  | 获取b在字符串abc中的位置：`select instr('abc','b');`  ---输出结果：2 |
| LEFT(s，n)                            | 返回字符串s的的前n个字符                                     | 返回字符串itcast的前两个字符：`select left('itcast',2);`  ---输出结果：it |
| LOCATE(s1，s)                         | 从字符串s中获取s1的开始位置                                  | 返回字符串abc中b的位置：`select locate('b','abc');`  ---输出结果：2 |
| LOWER(s)                              | 将字符串s的所有字母变成小写字母                              | 字符串ITCAST转为小写：`select lower('ITCAST');`  ---输出结果：itcast |
| LPAD(s1，len，s2)                     | 在字符串s1的开始处填充字符串s2，是使字符串长度达到len        | 将字符串xx填充到abc字符串的开始处：`select lpad('abc',5,'xx');`  --输出结果：xxabc |
| LTRIM(s)                              | 去掉字符串s开始处的空格                                      | 去掉字符串itcast开始处的空格：`select ltrim('    itcast');`  ---输出结果：itcast |
| MID(s，n，len)                        | 从字符串s的start位置截取长度为len的子字符串，同substring(s，n，len) | 从字符串itcast中的第二个位置截取三个字符：`select mid('itcast',2,3);`  ---输出结果：tca |
| POSITION(s1 IN s)                     | 从字符串s中获取s1的开始位置                                  | 返回字符串abc中b的位置：`select position('b' in 'abc');`  ---输出结果：2 |
| REPEAT(s，n)                          | 将字符串s重复n次                                             | 将字符串itcast重复三次：`select repeat('itcast',3);`  ---输出结果：itcastitcastitcast |
| REPLACE(s，s1，s2)                    | 将字符串s2替代字符串s中的字符串s1                            | 将字符串abc中的字符a替换为字符x：`select replace('abc','a','x');`  ---输出结果：xbc |
| REVERSE(s)                            | 将字符串s的顺序反过来                                        | 将字符串abc的顺序颠倒：`select reverse('abc');`  ---输出结果：cba |
| RIGHT(s，n)                           | 返回字符串s的后n个字符                                       | 返回字符串itcast的后两个字符：`select right('itcast',2);`  ---输出结果：st |
| RPAD(s1，len，s2)                     | 在字符串s1的结尾处添加字符串s2，使字符串的长度达到len        | 将字符串xx填充到abc字符串的结尾处：`select rpad('abc',5,'xx');`  ---输出结果：abcxx |
| RTRIM(s)                              | 去掉字符串s结尾处的空格                                      | 去掉字符串itcast的末尾空格：`select rtrim('itcast    ');`  ---输出结果：itcast |
| SPACE(n)                              | 返回n个空格                                                  | 返回2个空格：`select space(2);`  ---输出结果：               |
| STRCMP(s1，s2)                        | 比较字符串s1和s2，如果s1与s2相等返回0，如果s1>s2返回1，如果s1<s2返回-1 | 比较字符串：`select strcmp('itcast','itcast');`  ---输出结果：0 |
| SUBSTR(s，start，length)              | 从字符串s的start位置截取长度为length的子字符串               | 从字符串itcast中的第2个位置截取3个字符：`select substr('itcast',2,3);`  ---输出结果：tca |
| SUBSTRING(s，start，length)           | 从字符串s的start位置截取长度为length的子字符串               | 从字符串itcast中的第2个位置截取3个字符：`select substring('itcast',2,3);`  ---输出结果：tca |
| SUBSTRING_INDEX(s，delimiter，number) | 返回从字符串s的第number个出现的分隔符delimiter之后的子串。如果number是正数，返回第number个字符左边的字符串。如果number是负数，返回第（number的绝对值(从右边数)）个字符右边的字符串。 | `select substring_index('a*b','*',1);`  ---输出结果：a       |
| TRIM(s)                               | 去掉字符串s开始和结尾处的空格                                | 去掉字符串itcast的首尾空格：`select trim('    itcast   ');`  |
| UCASE(s)                              | 将字符串转为大写                                             | 将字符串转itcast转为大写：`select ucase('itcast');`  ---输出结果：ITCAST |
| UPPER(s)                              | 将字符串转为大写                                             | 将字符串转itcast转为大写：`select upper('itcast');`  ---输出结果：ITCAST |

15-2 MySQL数字函数

| 函数名                             | 描述                                                         | 实例                                                         |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ABS(x)                             | 返回x的绝对值                                                | 返回-1的绝对值：`select abs(-1);`  ---输出结果：1            |
| ACOS(x)                            | 求x的反余弦值(参数是弧度)                                    | `select acos(0.25);`                                         |
| ASIN(s)                            | 求反正弦值(参数是弧度)                                       | `select asin(0.25);`                                         |
| ATAN(s)                            | 求反正切值(参数是弧度)                                       | `select atan(0.25);`                                         |
| ATAN2(n，m)                        | 求反正切值(参数是弧度)                                       | `select atan2(-0.8,2);`                                      |
| AVG(expression)                    | 返回一个表达式的平均值，expression是一个字段                 | 返回Products表中Price字段的平均值：`select avg(price) as averagePrice from Products;` |
| CEIL(x)                            | 返回大于或等于x的最小整数                                    | `select ceil(1.5);`   ---输出结果：2                         |
| CEILING(x)                         | 返回大于或等于x的最小整数                                    | `select ceil(1.5)`;   ---输出结果：2                         |
| COS(x)                             | 求余弦值(参数是弧度)                                         | `select cos(2);`                                             |
| COT(x)                             | 求余切值(参数是弧度)                                         | `select cot(6);`                                             |
| COUNT(expression)                  | 返回查询的记录总数，expression是一个字段或*号                | 返回Products表中products字段总共有多少条记录：`select count(*) as numberOfProducts from Products;` |
| DEGREES(x)                         | 求弧度转换为角度                                             | `select degrees(3.1415926535898)`  ---输出结果：180          |
| n DIV m                            | 整除，n为被除数，m为除数                                     | 计算10除于5：`select 10 div 5;`                              |
| EXP(x)                             | 返回e的x次方                                                 | 计算e的三次方：`select exp(3);`                              |
| FLOOR(x)                           | 返回小于或等于x的最大整数                                    | 小于或等于1.5的整数：`select floor(1.5);`  --- 输出结果：1   |
| GREATEST(expr1，expr2，expr3，...) | 返回列表中的最大值                                           | 返回以下数字列表中的最大值：`select greatest(3,12,34,8,25);`  ---输出结果：34          返回以下字符串列表中的最大值：`select greatest("google","itcast","Apple");`  ---输出结果：itcast |
| LEAST(expr1，expr2，expr3，...)    | 返回列表中的最小值                                           | 返回以下数字列表中的最小值：`select least(3,12,34,8,25);`  ---输出结果：3          返回以下字符串列表中的最大值：`select least("google","itcast","Apple");`  ---输出结果：Apple |
| LN(n)                              | 返回数字的自然对数                                           | 返回2的自然对数：`select ln(2);`  ---输出结果：0.6931471805599453 |
| LOG(x)                             | 返回自然对数(以e为底的对数)                                  | `select log(20.085536923188);`  ---输出结果：2               |
| LOG10(x)                           | 返回以10为底的对数                                           | `select log10(100);`  ---输出结果：2                         |
| LOG2(x)                            | 返回以2为底的对数                                            | 返回以2为底6的对数：`select log2(6);`  ---输出结果：2.584962500721156 |
| MAX(expression)                    | 返回字段expression中最大值                                   | 返回Products表中price的最大值：`select max(price) as maxPrice from Products;` |
| MIN(expression)                    | 返回字段expression中最小值                                   | 返回Products表中price的最小值：`select min(price) as minPrice from Products;` |
| MOD(x，y)                          | 返回x除以y以后的余数                                         | 5除于2的余数：`select mod(5,2);`  ---输出结果：1             |
| PI()                               | 返回圆周率                                                   | `select PI();`                                               |
| POW(x，y)                          | 返回x的y次方                                                 | 2的3次方：`select pow(2,3);`  ---输出结果：8                 |
| POWER(x，y)                        | 返回x的y次方                                                 | 2的3次方：`select power(2,3);`  ---输出结果：8               |
| RADIANS(x)                         | 将角度转为弧度                                               | 180度转换为弧度：`select radians(180);`                      |
| RAND()                             | 返回0-1的随机数                                              | `select rand();`                                             |
| ROUND(x)                           | 返回离x最近的整数                                            | `select round(1.23456);`  ---输出结果：1                     |
| SIGN(x)                            | 返回x的符号，x是负数、0、正数分贝返回-1、0和1                | `select sign(-10);`  ---输出结果：-1                         |
| SIN(x)                             | 求正弦值(参数是弧度)                                         | `select sin(randians(30));`  ---输出结果：0.5                |
| SQRT(x)                            | 返回x的平方根                                                | 25的平方根：`select sqrt(25);`  ---输出结果：5               |
| SUM(expression)                    | 返回指定字段的总和                                           | 计算OrderDetails表中字符安Quantity的总和：`select sum(Quantity) from OrderDetails;` |
| TAN(x)                             | 求正切值(参数是弧度)                                         | `select tan(1.75);`                                          |
| truncate(x，y)                     | 返回数值x保留到小数点后y位的值（与ROUND最大的区别是不会进行四舍五入） | `select trucate(1.23456);`  ---输出结果：1.234               |

15-3 日期函数

| 函数名                            | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| adddate(d，n)                     | 计算其实日期d加上n天的日期                                   |
| addtime(t，n)                     | 时间t加上n秒的时间                                           |
| curdate()                         | 返回当前日期                                                 |
| current_date()                    | 返回当前日期                                                 |
| current_time                      | 返回当前时间                                                 |
| current_timestamp()               | 返回当前日期和时间                                           |
| curtime()                         | 返回当前时间                                                 |
| date()                            | 从日期或日期时间表达式中提取日期值                           |
| datediff(d1，d2)                  | 计算日期d1->d2之间间隔的天数                                 |
| date_add(d，INTERAVL expr type)   | 计算起始日期d加上一个时间段后的日期                          |
| date_format(d，f)                 | 按表达式f的要求显示日期d                                     |
| date_sub(date,INTERVAL expr type) | 函数从日期减去指定的时间间隔                                 |
| day(d)                            | 返回日期值d的日期部分                                        |
| dayname(d)                        | 返回日期d是星期几，如Monday、Tuesday                         |
| dayofmonth(d)                     | 返回日期d是本月的第几天                                      |
| dayofweek(d)                      | 日期d今天是星期几，1：星期日，2：星期一，以此类推            |
| dayofyear(d)                      | 计算日期d是本年的第几年                                      |
| extract(type from d)              | 计算日期d中获取指定的值，type指定返回的值。type可取值为：MICROSECONDSECONDMINUTEHOURDAYWEEKMONTHQUAETERYEARSECOND_MCROND |
| rom_days(n)                       | 计算从0000年1月1日开始n天后的日期                            |
| hour(t)                           | 返回t中小时值                                                |
| last_day(d)                       | 返回给定日期的那一月份的最后一天                             |
| localtime()                       | 返回当前日期和时间                                           |
| localtimestamo()                  | 返回当前日期和时间                                           |
| makedate(yeear，day-of-year)      | 基于给定参数年份year和所在年中的天数序号day-of-year返回一个日期 |
| maketime(hour，minute，second)    | 组合时间，参数分别为小时、分钟、秒                           |
| microsecond(date)                 | 返回日期参数所对应的毫秒数                                   |
| minute(t)                         | 返回t中分钟值                                                |
| monthname(d)                      | 返回日期当中月份的名称                                       |
| month(d)                          | 返回日期d中的月份，1到12                                     |
| now()                             | 获取当前时间和日期                                           |
| period_add(period，number)        | 为年-月组合日期添加一个时段                                  |
| period_diff(period1，period2)     | 返回两个时段之间的月份差值                                   |
| quarter(d)                        | 返回日期d是第几季节，返回1到4                                |
| second(t)                         | 返回t中秒钟值                                                |
| sec_to_time(s)                    | 将以秒为单位的时间s转换为时分秒的格式                        |
| timediff(time1，time2)            | 计算时间差值                                                 |
| timestamp(expression，interval)   | 单个参数时，函数返回日期或日期时间表达式：有2个参数时，将参数加和 |
| to_days(d)                        | 计算日期d距离0000年1月1日的天数                              |
| week(d)                           | 计算日期d是本年中第几个星期，范围是0到53                     |
| weekday(d)                        | 日期d是星期几，0代表星期一，1表示星期二                      |
| weekofyear(d)                     | 计算日期d是本年的第几个星期，范围是0到53                     |
| year(d)                           | 返回年份                                                     |
| yearweek(date，mode)              | 返回年份及第几周（0到53），mode中0表示周天，1表示周一，以此类推 |

15-4 MySQL高级函数

| 函数名                                                       | 描述                                                         | 实例                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BIN(x)                                                       | 返回x的二进制码                                              | 15的二进制码：`select bin(15);`  ---输出结果：1111           |
| BINARY(s)                                                    | 将字符串s转换为二进制字符串                                  | `select binary('itcast');`                                   |
| CASE expression WHEN condition1 THEN result1 WHEN condition2 THEN result2  ...  WHEN conditonN THEN resultN ELSE resultEND | CASE表示函数开始，END表示函数结束。如果condition1成立，则返回result1，如果condition2成立，则返回result2。当全部不成立则返回result。而当有一个成立之后，后面的就不执行了。 |                                                              |
| CAST(x AS type)                                              | 转换数据类型                                                 | 字符串日期转换为日期：`select cast('2017-08-29') as date`    |
| COALESCE(expr1，expr2，...)                                  | 返回参数中第一个非空表达式（从左到右）                       | `coalesce(null,null,'itcast.com',null,'google.com');`        |
| CONNECTION_ID()                                              | 返回服务器的连接数                                           | `SELECT CONNECTION_ID();`                                    |
| CONV(x，f1，f2)                                              | 返回f1进制数变成f2进制数                                     | `select conv(15,10,2);`  ---输出结果：1111                   |
| CONVERT(s USING cs)                                          | 函数将字符串s的字符集变成cs                                  | `select charset(convert('abc' using gbk));`  ---输出结果：gbk |
| CUEERNT_USER()                                               | 返回当前用户                                                 | `select current_user();`  ---输出结果：root@localhost        |
| DATABASE()                                                   | 返回当前数据库名                                             | `select database();`                                         |
| IF(expr，v1，v2)                                             | 如果表达式expr成立，返回结果v1，否则，返回结果v2             | `select if(1>0,'YES','NO');`  ---输出结果：YES               |
| IFNULL(v1，v2)                                               | 如果v1的值不为NULL，则返回v1，否则返回v2                     | `select ifnull(null,'Hello Word');`  ---输出结果：Hello Word |
| ISNULL(expression)                                           | 判断表达式是否为空                                           | `select isnull(null);`  ---输出结果：1                       |
| LAST_INSERT_ID()                                             | 返回最近生成的AUTO_INCREMENT值                               | `select last_insert_id();`                                   |
| NULLIF(expr1，expr2)                                         | 比较两个字符串，如果字符串expr1与expr2相等返回NULL，否则返回expr1 | `select nullif(25,25);`  ---输出结果：NULL                   |
| SESSION_USER()                                               | 返回当前用户                                                 | `select SESSION_USER();`  ---输出结果：root@localhost        |
| SYSTEM_USER()                                                | 返回当前用户                                                 | `select SYSTEM_USER();`  ---输出结果：root@localhost         |
| USER()                                                       | 返回当前用户                                                 | `SELECT USER();`  ---输出结果：root@localhost                |
| VERSION()                                                    | 返回数据库的版本号                                           | `SELECT VERSION();`  ---输出结果：5.7.32                     |





第十六章 MySQL索引

​	MySQL索引的建立对于MySQL的高效运行时很重要的，索引可以大大提高MySQL的检索速度。

​	索引分**单列索引和组合索引**。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索引包含多个列。

​	创建索引时，你需要保该索引是应用在SQL查询语句的田间（一般作为WHERE子句的条件）。

​	实际上，索引也是一张表，该表保证了主键与索引字段，并指向实体表的记录。

​	上面都在说使用索引的好处，但过多的使用索引将会造成滥用。因此索引也会有它的缺点，虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。

​	建立索引会占用磁盘空间的索引文件。

16-1 普通索引

1、创建索引

​	这是最基本的索引，它没有任何限制。它有以下几种创建方式：

```sql
create index indexname on mytable(username(length));
```

如果是CHAR、VARCHAR类型，length可以小于字段实际长度；如果是BOLB和TEXT类型，必须指定length。

```sql
create index id on B(A_ID);
```

2、修改表结构（索引）

​	语法：

```
alter table tableName add index indexName(cloumnName);
```

```sql
alter table B add index id(A_ID);
```

3、创建表的时候直接指定

​	语法：

```sql
create table mytable(
	ID int not null,
	username varchar(16) not null,
	index [indexName](username(length))
);
```

4、删除索引的语法

```sql
drop index [indexName] on mytable;
```

16-2 唯一索引

​	它的前面的普通索引类似，不同的就是，索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。它有以下几种创建方式：

1、创建索引

​	语法：

```sql
create unique index indexName on mytable(username(length));
```

2、修改表结构

​	语法：

```sql
alter table mytable add unique [indexName](username(length));
```

3、创建表的时候直接指定

​	语法：

```sql
create table mytable(
	ID int not null,
  username varchar(16) not null,
  unique [indexName](username(length))
);
```

16-3 使用ALTER命令添加和删除索引

有四种方式来添加数据库表的索引：

- `ALTER TABLE tb1_name ADD PRIMARY KEY(column_list)；`该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
- `ALTER TABLE tb1_name ADD UNIQUE index_name(column_list);`这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能出现多次）。
- `ALTER TABLE tb1_name ADD INDEX index_name(column_list);`添加普通索引，索引值可出现多次。
- `ALTER TABLE tb1_name ADD FULLTEXT index_name(column_list);`该语句指定了索引为FULLTEXT，用于全文索引。

以下实例为在表中添加索引。

```sql
mysql> ALTER TABLE testalter_tb1 ADD INDEX (C)： 
```

你还可以在ALTER命令中使用DROP命令子句来删除索引。尝试以下实例删除索引：

```sql
mysql> ALTER TABLE testalter_tb1 DROP INDEX C;
```

16-4 使用ALTER命令添加和删除主键

​	主键只能作用于一个列上，添加主键索引时，你需要确保该主键默认不为空（NOT NULL）。实例如下：

```sql
mysql> ALTER TABLE testalter_tb1 MODIFY	i INT NOT NULL;
mysql> ALTER TABLE testalter_tb1 ADD PRIMARY KEY(i);
```

 	你还可以使用ALTER命令删除主键：

```sql
mysql> ALTER TABLE testalter_tb1 DROP PRIMARY KEY;
```

​	删除主键时只需要指定PRIMARY KEY，但在删除索引时，你必须知道索引名。

16-5 显示索引信息

你可以使用SHOW INDEX命令来列出表中的先关信息。可以通过添加\G的格式化输出信息。

尝试一下实例：

```sql
mysql> SHOW INDEX FROM table_name;\G
```



第十七章 MySQL事务

​	MySQL事务主要用于处理操作量大，复杂度高的数据。比如说，有人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱、文章等等，这样，这些数据库操作语句就构成一个事务！

- ‘在MySQL中只有使用了InnoDB数据库引擎的数据库或表才支持事务。
- 事务处理可以用来维护数据库的完整性，保证成批的SQL语句要么全部执行，要么全部不执行。
- 事务用来管理insert、update、delete语句。

一般来说，事务是必须满足4个条件（ACID）：**原子性（Atomicity，或称不可分割性）、一致性（Consistency）、隔离性（Isolation，又称独立性）、持久性（Durability）**。

- **原子性**：一个事务中的所有操作，要么全部完成，要么全部不完成，在中间某个环节不会结束。事务在执行过程中发生错误，会被回滚到事务开始前的状态，就像这个事务从来没有执行过一样。
- **一致性**：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精准度、串联性以及后续数据库可以自发性地完成预定的工作。
- **隔离性**：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括**读未提交（Read uncommitted）、读已提交（Read committed）、可重复读（Repeatable Read）和串行化（Serializable）**。
- **持久性**：事务处理结束后，对数据的修改就是永久的，几遍系统故障也不会丢失。

17-1 事务控制语句

- `BEGIN或START TRANSACTION`：显式的开启一个事务。
- `COMMIT`：也可使用COMMIT WORK，不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改成为永久性的。
- `ROKKBACK`：有可以使用ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有为提交的修改。
- `SAVEPOINT identifier`：SAVEPOINT允许在事务中创建一个保存点，一个事务中可以有多个SAVEPOINT。
- `RELEASE SAVEPOINT indentifier`：删除一个事务的保存点，当没有指定保存点时，执行该语句会抛出一个异常。
- `ROLLBACK TO identifier`：把事务回滚到标记点。
- `SET TRANSACTION`：用来设置事务的隔离级别。InnoDB存储引擎提供事务的隔离级别有READ_UNCOMMITTED、READ COMMITTED、REPEATABLE READ和SERIALIZABLE。

17-2 MySQL事务处理主要有两种方法

1、用BEGIN、ROLLBACK、COMMIT来实现

- **BEGIN**开始一个事务
- **ROLLBACK**事务回滚
- **COMMIT**事务确认

2、直接用SET来改变MySQL的自动提交模式

- **SET AUTOCOMMIT=0** 禁止自动提交
- **SET AUTOCOMMIT=1** 开启自动提交









数据库高级部分

第一章 编码MySQL

1-1 查看MySQL编码

```sql
mysql> show variables like 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | latin1                     |
| character_set_connection | latin1                     |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | latin1                     |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

1-2 设置mysql编码

```shell
在启动mysql时，使用一下方式启动:
docker run --name develop-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
```



第二章 MySQL的目录及配置文件

- **/etc/mysql/mysql.conf.d**  mysql的主配置文件
- **/var/lib/mysql**  mysql数据库的数据库文件存放位置
- **/var/log**  mysql数据库的日志输出存放位置



第三章 数据库-原理部分

常用术语

3-1 数据模型

​	数据模型（Data Model）是数据库结构的基础，是用来描述数据的一组概念和定义，数据模型主要有三个要素：数据结构、数据操作、数据约束条件。

​	数据结构：对象类型的集合，是对静态属性的描述。

​	数据操作：是对数据库中的各种对象性数据，允许执行的操作集合，如增删改查等；数据操作是对系统动态热性的描述。

​	数据约束条件：是一组完整性规则的集合，也就是说，对于具体的应用必须先遵循特定的语义约束条件。比如：性别只能取“男”或者“女”中的之一。考试成绩：（满分100）只能是0-100的数值。

3-2 数据库

​	数据库（database）是长期存储在计算机外存上有结构，可共享的数据集合；数据库中的数据按照一定的数据模型描述、组织和存储，具有较小的冗余度，较高的数据独立性和可扩展性，并可以为多个用户共享。

​	常见数据库分类：

​		关系型数据库：如MySQL、Oracle、SQLServer

​		非关系型数据库：如Redis、HBase等

3-3 数据库管理系统

​	数据库管理系统（database managerment system，DBMS）是指数据库系统中对数据库进行管理的软件系统，是数据库系统的核心组成部分。数据库的一切操作，如增伤改查以及各种控制，都是通过DBMS进行的。

​	具有以下4个基本功能：

​		① 数据定义功能

​				用户可以通过DBMS提供的数据定义语言对数据库的数据进行定义。

​		② 数据操纵功能

​				用户可以通过数据操纵语言实现对数据库的增删改查操作。

​		③ 数据库运行管理

​				管理数据库的运行时DBMS运行时的核心工作。所有访问数据库的操作都要在DBMS的统一管理下进行，以保证数据的安全性、完整性、一致性以及多用户对数据库的并发使用。

​		④ 数据库的建立和维护

​				建立数据库，包括数据库初始数据的输入与数据转换等。维护数据库，包括数据库的转储与恢复，数据库的重组织，性能监控和分析。

3-4 数据库系统相关人员

​	数据库系统的先关人员是数据库系统的重要组成部分，具体可以可以分为以下的三类人员：

​	① 数据库管理员

​		职责：负责数据库的建立、使用、维护的专门人员。

​	② 应用程序开发人员

​		职责：开发数据库应用程序的人员，可以使用数据库管理系统的所有功能。

​	③ 最终用户

​		职责：一般来说，是通过应用程序使用数据库的人员，最终用户无需自己编写应用程序。

3-5 数据库系统

​	数据库系统（database system DBS）是由硬件系统、数据库管理系统，数据库，数据库应用程序，数据库系统相关人员构成的人-机系统，是指有数据库的整个计算机系统。

说明：在许多场合，数据库、数据库管理系统、数据库系统不做严格区分。



第四章 关系代数分类

4-1 基本运算

​	并、差、笛卡尔积、选择、投影。

​	关系代数的基本操作（原始运算）：“选择”、“投影”、笛卡尔积（也叫做“叉积”或“交叉连接”）、并集、差集和“重命名”。

4-2 组合运算

​	交、连接、自然连接和除。

其中最重要的是交集、除法和自然连接。

4-3 扩充的关系代数操作

​	外连接（左外和右外）、外部并和半连接。





第五章 数据库设计

​	关系型数据库的设计分为以下5个阶段：

5-1 需求分析

​	明确用户需求，到底做什么？

5-2 概念模型设计

​	（1）该阶段是整个数据库设计的关键，它通过对用户需求进行综合、归与抽象。主要通过E-R图表示。

​	（2）优点：

​			① 简单明了，容易理解

​			② 独立于计算机与具体的RDBMS无关

​	（3）E-R模型的基本元素

​			① 实体（Entity）：如学生

​			② 属性（Attribute）：如姓名

​			③ 键码（Key）：如身份证号码

​			④ 关系（Relationship）：如两个实体之间的关系

​				一对一（1:1）：一个人一个身份证号码；一个学校一个校长

​				一对多（1：n）：学校和老师的关系

​				多对多（n：n）：学生选课，一个学生可以选择多门课程，一门课程被多名学生进行选修

E-R符号表示：

![1608278778637](D:\Java\image\1608278778637.png)

5-3 逻辑模式设计

​	（1）该阶段涉及到更多的概念，方法，理论。

​	（2）主要任务：

​			① 与具体的数据库相关

​			② 规范化处理，尽可能的消除关系操作过程中的异常情况。

​			③ E-R图转换如下的关系模式：

​				电影（片名，出品年份，影片长度，影片类型，公司名称）

​				明星（姓名，联系地址，公司名称）

​				扮演（片名，出品年份，姓名，角色）

​				影片公司（公司名称，地址）

​				卡通片（片名，出品年份，设计平台）

5-4 数据库实施

​	创建数据，定义数据结构，组织数据入库，调试数据库并进行数据库的试运行。

5-5 数据库的运行和维护

​	数据库正式运行之后，对数据库运行过程中对其进行评价，调整，修改，调优等。





第六章 数据库设计遵循的原则

6-1 范式概念

概念：范式就是符合某一规范级别的关系模型的集合。

共有7种范式：

​	1NF => 2NF => 3NF => BCNF => 4NF => 5NF => 6NF

1、第一范式（Frist Normal Form）

定义：如果一个关系模式R的所有属性都是不可分割的基本数据项，则这个关系属于第一范式。

举例说明：（学生选课：学号、姓名、系别、系部地址、课程名称、课程成绩）

Student(s_no，s_name，s_dept，s_location，s_course_name，s_grade)

注：1NF是关系模式应具备的最起码的条件，如果数据库设计不能满足第一范式，就不能称作是关系模式；关系数据库设计研究的关系规范化是在1NF基础之上进行的。

2、第二范式（Sencond Normal Form）

定义：若关系模式R属于第一范式，且每个非主属性都是完全函数依赖于主键，则属于第二范式。

说明：从2NF的定义可以看出，从2NF开始讨论的是主键和非主属性之间的函数依赖关系，所以分析关系模式是属于2NF，首先致命关系模式的主键，然后在讨论非主属性和主键之间的函数依赖关系。

例如：选课关系模式

SC（s_no，c_no，score）中，主键为（s_no，c_no），而非主属性scor与主键之间不存在部分函数依赖关系，所以关系模式SC属于2NF。

3、第三范式（Third Normal Form）

定义：若关系模式R属于第一范式，且每个非主属性都不传递函数依赖于主键，则R属于第三范式。

说明：3NF说明的是非主属性和主键之间的函数依赖关系。

例如：选课关系模式

SC（s_no，c_no，score）中，由于除了主键之外，只有一个非主属性score，所以score不可能构成与主键之间的传递函数依赖，所以SC属于3NF。

4、BCNF（Boyce-Codd Normal Form）

定义：若关系模式R属于第一范式，且每个属性都不传递依赖于主键，则R属于BC范式。

说明：也就是说，在关系模式R中，凡是决定因素的属性或属性集包含键码，决定因素是函数依赖的左部属性集，比如X->Y，X称为决定因素。

有BC范式的定义可以得到一下结论，一个满足BC范式的关系模式有：

​	① 所有非主属性对每一个候选码都是完全函数依赖。

​	② 所有的主属性对每一个不包含它的候选码都是完全函数依赖。

​	③ 没有任何属性完全遗爱与非候选码的任何一组属性。





第七章 事务并发操作出现几种问题

​	所谓事务，是用户定义的一个数据库操作序列，是数据库环境中的逻辑工作单元，是一个不可分割的整体。

​	事务的4个特性简称为ACIG特性，事务ACID特性可能遭到破坏的因素有：

​		① 多个事务并发执行，不同事务的操作交叉执行。

​		② 事务在运行过程中被强制终止。

​	如何保证在多个事务并发执行的过程中不发生上述的两种情况，是数据库管理系统并发控制的主要责任。

7-1 丢失修改数据

​	举例：银行卡有100元，事务A取10元，事务B取10元，事务AB两人同时取钱，初始值都是100

7-2 读“脏”数据

​	数据库技术中，如果正常提交的事务A使用事务B未提交的撤销数据，这种数据成为“脏数据”，会造成数据的脏读和污读。

7-3 不一致分析

​	造成这种数据不一致的主要原因是并发执行 的 两个事务中，一个事务在读取数据时，另一个事务正在修改同一个数据。这样就可能导致两个事务的相互干扰“读”事务的错误执行结果。



第八章 数据库并发的控制（了解部分）

8-1 并发调度的可串行化

​	可串行化准则：多个事务的并发执行时正确的，当且仅当其结果按其一次序串行执行它们时的结果相同，这种调度策略称为可串行化调度。可串行化是并发事务正确性的准则，一个给定的并发调度，当且仅当它是串行化的，才认为是正确的。

8-2 封锁

封锁是实现并发控制的非常重要的技术。封锁是指某事务在对某数据对象进行操作前，先请求系统对其加锁，成功加锁之后该事务对该数据对象有了控制权，只有该事务对其进行解锁之后，其他的事务才能更新它，DBMS有两种锁：

① 排它锁（也称作X锁）

​	如果事务T在对某个数据对象实施了X锁，那么其他的事务必须要等到T事务接触对该数据对象的X锁之后，才能对这个数据进行加锁。

② 共享锁（也称为S锁）

​	如果事务T在对某个数据对象实施了S锁，那么其他事务也可能对该数据对象实施S锁，但是对这个数据对象施加的所有S锁都接触之前不允许任何事务对该数据对象实施X锁。

8-3 死锁

封锁技术可以避免一些并发操作引起的不一致错误，但也会产生其他的一些问题：活锁和死锁。

① 活锁

​	如果某个事务在永远等待的状态，得不到封锁的机会，这种现象为活锁，避免这种锁最好的方法就是采用先来先服务的策略。

② 死锁

​	两个或两个以上的事务处于等待状态每个事务都在等待对方事务接触封锁，它才能继续执行下去，这样任何事务都处于等待状态而无法继续执行的现象称为死锁。

解决死锁问题方法有两类：

A、死锁的预防

B、死锁的诊断与预防





数据库的优化篇

第一章 为什么要进行数据库优化？

1-1 避免网站页面出现访问错误

​	由于数据库连接timeout产生页面5xx错误

​	由于慢查询造成页面无法加载

​	由于阻塞造成数据无法提交

1-2 增加数据库的稳定性

​	很多数据库问题都是由于低效的查询引起的

1-3 优化用户体验

​	流畅页面的访问速度

​	良好的网站功能体验

第二章 MySQL数据库优化

​	可以从哪几个方面进行数据库的优化？如图所示：

![1608282723615](D:\Java\image\1608282723615.png)

2-1 SQL及索引优化

​	根据需求写出良好的SQ，并创建有效的索引，实现某一种需求可以多种写法，这时候我们就要选择一种效率最高的写法。这个时候就要了解sql优化。

2-2 数据库表结构优化

​	根据数据库的范式，设计表结构，表结构设计的好直接关系到写SQL语句。

2-3 系统配置优化

​	大多数运行在Linux机器上，如tcp连接的限制、打开文件数的限制】安全性限制，因此我们要对这些配置进行相应的优化。

2-4 硬件配置优化

​	选择适合数据库服务的cpu，更快的IO，更高的内存；cpu并不是越多越好，某些数据库版本有最大的限制，IO操作并不是减少阻塞。

注：通过上图可以看出，该金字塔中，优化的成本从下而上逐渐增高，而优化的效果会逐渐降低。



第三章 SQL及索引优化

‘3-1 查看mysql的版本

```sql
mysql> select version();
+-----------+
| version() |
+-----------+
| 5.7.32    |
+-----------+
1 row in set (0.00 sec)
```

3-2 准备数据

```sql
在网址https://dev.mysql.com/doc/sakila/en/sakila-installation.html中下在数据库数据
```

3-3 表结构关系



3-4 如何发现有问题的SQL

​	MySQL慢查日志的开启方式和存储格式。

1、检查慢查日志是否开启

```sql
#1、检查是否开启慢查日志
mysql> show variables like 'slow_query_log';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | OFF   |
+----------------+-------+
1 row in set (0.00 sec)

#2、查询慢查日志文件slow_query_log_file存储的位置
mysql> show variables like '%log%';
| relay_log_info_file                        | relay-log.info                              |
| relay_log_info_repository                  | FILE                                        |
| relay_log_purge                            | ON                                          |
| relay_log_recovery                         | OFF                                         |
| relay_log_space_limit                      | 0                                           |
| slow_query_log                             | OFF                                         |
| slow_query_log_file                        | /var/lib/mysql/49d9e9e794f7-slow.log        |
| sql_log_bin                                | ON                                          |
| sql_log_off                                | OFF                                         |
| sync_binlog                                | 1                                           |
| sync_relay_log                             | 10000                                       |
| sync_relay_log_info                        | 10000                                       |

#3、开启慢查查询日志，首先需要先设置如下两个
mysql> set global log_queries_not_using_indexes=on;

#4、大于1秒钟的数据记录到慢日志中，如果设置为默认0，则会有大量的信息存储在磁盘中，磁盘很容易满掉
mysql> set global long_query_time=1;


#5、开启慢查询日志
mysql> set global slow_query_log=on;

#6、验证慢查询日志是否开启
进入slow_query_log_file所在的位置，进入查看是否有执行的SQL语句即可，例如：
use sakila;
set timestamp=160829237;
select * from actor;


```

2、查看所有日志的变量信息

```sql
mysql> show variables like '%log%';
查询部分内容如下：
+--------------------------------------------+---------------------------------------------+
| Variable_name                              | Value                                       |
+--------------------------------------------+---------------------------------------------+
| back_log                                   | 80                                          |
| binlog_cache_size                          | 32768                                       |
| binlog_checksum                            | CRC32                                       |
| binlog_direct_non_transactional_updates    | OFF                                         |
| binlog_error_action                        | ABORT_SERVER                                |
| binlog_format                              | ROW                                         |
| binlog_group_commit_sync_delay             | 0                                           |
| binlog_group_commit_sync_no_delay_count    | 0                                           |
| binlog_gtid_simple_recovery                | ON                                          |
| binlog_max_flush_queue_time                | 0                                           |
| binlog_order_commits                       | ON                                          |
| binlog_row_image                           | FULL                                        |
| binlog_rows_query_log_events               | OFF                                         |
| binlog_stmt_cache_size                     | 32768                                       |
| binlog_transaction_dependency_history_size | 25000                                       |
| binlog_transaction_dependency_tracking     | COMMIT_ORDER                                |
| expire_logs_days                           | 0                                           |
| general_log                                | OFF                                         |
| general_log_file                           | /var/lib/mysql/49d9e9e794f7.log             |
| innodb_api_enable_binlog                   | OFF                                         |
| innodb_flush_log_at_timeout                | 1                                           |
| innodb_flush_log_at_trx_commit             | 1                                           |
| innodb_locks_unsafe_for_binlog             | OFF                                         |
| innodb_log_buffer_size                     | 16777216                                    |
| innodb_log_checksums                       | ON                                          |

```

3、MySQL慢查日志的存储格式

```sql
通过进入slow_query_log_file所在的位置，我们可以看到如下的内容：

# Time: 2020-12-18T12:23:57.3244098Z   ------查询执行的时间
# User@Host: root[root] @ localhost []  id:   13   ------执行sql的主机信息
# Query_time: 0.001283 Lock_time: 0.000347 Rows_sent: 200 Rows_exaimned: 200   -------SQL的执行信息
		Query_time:SQL的查询时间
		Lock_time:锁定时间
		Rows_sent:所发送的行数
		Rows_examined:所扫描的行数
use sakile;  -----SQL执行内容
SET timestamp=1608294237;    ------SQL执行时间
select * from actor;  -----SQL执行内容
```



第四章 MySQL慢查日志分析工具（mysqldumpslow）

4-1 介绍

​	如何进行查看慢查询日志，如果开启了慢查询日志，就会生成很多的数据，然后我们就可以通过对日志的分析，生成分析表，然后通过报表进行优化。

4-2 用法

​	接下来我们查看一下这个工具的用法：

​		注意：在mysql数据库所在的服务器上，而不是在mysql>命令行中

​	该工具该如何使用：`mysqldumpslow -h`命令

如下是在docker下mysql镜像实现的：

```shell
[root@192 ~]# docker exec -it develop-mysql mysqldumpslow -h
Option h requires an argument
ERROR: bad option

Usage: mysqldumpslow [ OPTS... ] [ LOGS... ]

Parse and summarize the MySQL slow query log. Options are

  --verbose    verbose
  --debug      debug
  --help       write this text to standard output

  -v           verbose
  -d           debug
  -s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default
                al: average lock time
                ar: average rows sent
                at: average query time
                 c: count
                 l: lock time
                 r: rows sent
                 t: query time  
  -r           reverse the sort order (largest last instead of first)
  -t NUM       just show the top n queries
  -a           don't abstract all numbers to N and strings to 'S'
  -n NUM       abstract numbers with at least n digits within names
  -g PATTERN   grep: only consider stmts that include this string
  -h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
               default is '*', i.e. match all
  -i NAME      name of server instance (if using mysql.server startup script)
  -l           don't subtract lock time from total time
```

查看verbase信息：

```shell
[root@192 ~]# docker exec -it develop-mysql mysqldumpslow -h --v
Can't determine basedir from 'my_print_defaults mysqld' output: --skip-host-cache
--skip-name-resolve
--pid-file=/var/run/mysqld/mysqld.pid
--socket=/var/run/mysqld/mysqld.sock
--datadir=/var/lib/mysql
--symbolic-links=0
```

查看慢查询日志的前10个mysqldumpslow分析的结果如下：

```shell

```

