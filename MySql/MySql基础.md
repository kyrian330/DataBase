### 一、数据准备

#### 建数据库

mysql -uroot -p123456;

create database MyDb;



#### 建数据表

-- 学生表
-- student
-- 学号
-- 姓名
-- 性别
-- 出生年月日
-- 所在班级

```sql
create table student(
	snum varchar(64) primary key,
	sname varchar(64) not null,
	ssex varchar(8) not null,
	sbirth datetime,
	class varchar(64)
);
```

------



-- 教师表
-- teacher
-- 教师编号
-- 教师名字
-- 教师性别
-- 出生年月日
-- 职称
-- 所在部门

```sql
create table teacher(
	tnum varchar(64) primary key,
	tname varchar(64) not null,
	tsex varchar(8) not null,
	tbirth datetime,
	profes varchar(64),
	depart varchar(64) not null
);
```

------



-- 课程表
-- course
-- 课程号
-- 课程名称
-- 教师编号

```sql
create table course(
	cnum varchar(64) primary key,
	cname varchar(64) not null,
	tnum varchar(64) not null,
	foreign key(tnum) references teacher(tnum) 
);
```

------



-- 成绩表
-- score
-- 学号
-- 课程号
-- 成绩

```sql
create table score(
	snum varchar(64) not null,
	cnum varchar(64) not null,
	mark decimal,
	foreign key(snum) references student(snum),
	foreign key(cnum) references course(cnum),
	primary key(snum, cnum)
);
```

------



#### 添加数据

```sql
# 学生信息
insert into student values('101','张三','男','1999-09-30','95033');
insert into student values('102','李四','男','2000-09-30','95033');
insert into student values('103','小妹','女','2003-01-10','95031');
insert into student values('104','大福','男','2001-03-30','95033');
insert into student values('105','小红','女','2000-12-30','95032');
insert into student values('106','布鲁斯','男','2001-07-30','95031');
insert into student values('107','富贵','男','2001-08-30','95033');
insert into student values('108','红孩儿','女','2000-11-01','95032');
insert into student values('109','陈胜','男','2001-01-30','95033');
```



```sql
#教师信息
insert into teacher values('804','李进','男','1979-12-12','助教','物电学院');
insert into teacher values('856','王旭东','男','1985-10-12','副教授','计信学院');
insert into teacher values('825','老唐','男','1969-7-12','副教授','计信学院');
insert into teacher values('831','王花','女','1989-3-12','讲师','文学院');
```



```sql
#课程信息
insert into course values('3-105','文学赏析','831');
insert into course values('3-245','大学物理','804');
insert into course values('3-166','编译原理','856');
insert into course values('3-888','Java课程设计','825');
```



```sql
#成绩
insert into score values('103','3-105','97');
insert into score values('105','3-105','70');
insert into score values('109','3-888','78');
insert into score values('103','3-888','67');
insert into score values('105','3-888','91');
insert into score values('109','3-245','97');
insert into score values('101','3-245','97');
insert into score values('102','3-166','57');
insert into score values('103','3-166','87');
```



### 二、查询练习

```sql
-- 1.查询教师所有的单位即不重复的depart列（distinct去重）
-- distinct 排除重复
mysql> select distinct depart from teacher;
+----------+
| depart   |
+----------+
| 物电学院 |
| 计信学院 |
| 文学院   |
+----------+
```



```sql
-- 2.  查询score表中成绩在60到80之间的所有记录
mysql> select * from score where mark between 60 and 80;
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 103  | 3-888 |   67 |
| 105  | 3-105 |   70 |
| 109  | 3-888 |   78 |
+------+-------+------+
```

```sql
-- 或者
mysql> select * from score where mark > 60 and mark < 80;
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 103  | 3-888 |   67 |
| 105  | 3-105 |   70 |
| 109  | 3-888 |   78 |
+------+-------+------+
```



```sql
-- 3.  查询score表中成绩为57，87，或97的记录
mysql> select * from score where mark in(57, 87, 97);
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 101  | 3-245 |   97 |
| 102  | 3-166 |   57 |
| 103  | 3-105 |   97 |
| 103  | 3-166 |   87 |
| 109  | 3-245 |   97 |
+------+-------+------+
```



```sql
-- 4.  查询student表中"95031"班或者性别为"女"的记录。
mysql> select * from student where class='95031' or ssex='女';
+------+--------+------+---------------------+-------+
| snum | sname  | ssex | sbirth              | class |
+------+--------+------+---------------------+-------+
| 103  | 小妹   | 女   | 2003-01-10 00:00:00 | 95031 |
| 105  | 小红   | 女   | 2000-12-30 00:00:00 | 95032 |
| 106  | 布鲁斯 | 男   | 2001-07-30 00:00:00 | 95031 |
| 108  | 红孩儿 | 女   | 2000-11-01 00:00:00 | 95032 |
+------+--------+------+---------------------+-------+
```



```sql
-- 5.  以cnum升序、mark降序查询score表的所有记录。
-- (sql语句逗号后不加空格)。
mysql> select * from score order by cnum asc,mark desc;
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 103  | 3-105 |   97 |
| 105  | 3-105 |   70 |
| 103  | 3-166 |   87 |
| 102  | 3-166 |   57 |
| 101  | 3-245 |   97 |
| 109  | 3-245 |   97 |
| 105  | 3-888 |   91 |
| 109  | 3-888 |   78 |
| 103  | 3-888 |   67 |
+------+-------+------+
```



```sql
-- 6.  查询"95033"班的学生人数。
mysql> select count(*) from student where class='95033';
+----------+
| count(*) |
+----------+
|        5 |
+----------+
```



```sql
-- 7.  查询score表中的最高分学生的学号、程号、分数。
mysql> select snum,cnum,mark from score where mark=(select max(mark) from score);
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 101  | 3-245 |   97 |
| 103  | 3-105 |   97 |
| 109  | 3-245 |   97 |
+------+-------+------+
```



```sql
-- 8.  查询每门课的平均成绩(一条语句，按照课程号分组)
mysql> select avg(mark),cnum from score group by cnum;
+-----------+-------+
| avg(mark) | cnum  |
+-----------+-------+
|   83.5000 | 3-105 |
|   72.0000 | 3-166 |
|   97.0000 | 3-245 |
|   78.6667 | 3-888 |
+-----------+-------+
```



```sql
-- 9.  查询score表中至少有2名学生选修并以'3-1'开头的课程的平均分。
mysql> select cnum,avg(mark),count(*) from score group by cnum
       having count(*)>=2 and cnum like '3-1%';
+-------+-----------+----------+
| cnum  | avg(mark) | count(*) |
+-------+-----------+----------+
| 3-105 |   83.5000 |        2 |
| 3-166 |   72.0000 |        2 |
+-------+-----------+----------+
```



```sql
-- 10.  查询所有学生的 sname、 cnum 和 mark 列(多表查询)。
mysql> select snum,sname from student;
+------+--------+
| snum | sname  |
+------+--------+
| 101  | 张三   |
| 102  | 李四   |
| 103  | 小妹   |
| 104  | 大福   |
| 105  | 小红   |
| 106  | 布鲁斯 |
| 107  | 富贵   |
| 108  | 红孩儿 |
| 109  | 陈胜   |
+------+--------+

mysql> select snum,cnum,mark from score;
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 101  | 3-245 |   97 |
| 102  | 3-166 |   57 |
| 103  | 3-105 |   97 |
| 103  | 3-166 |   87 |
| 103  | 3-888 |   67 |
| 105  | 3-105 |   70 |
| 105  | 3-888 |   91 |
| 109  | 3-245 |   97 |
| 109  | 3-888 |   78 |
+------+-------+------+


mysql> select sname,cnum,mark from student,score
       where student.snum = score.snum;
+-------+-------+------+
| sname | cnum  | mark |
+-------+-------+------+
| 张三  | 3-245 |   97 |
| 李四  | 3-166 |   57 |
| 小妹  | 3-105 |   97 |
| 小妹  | 3-166 |   87 |
| 小妹  | 3-888 |   67 |
| 小红  | 3-105 |   70 |
| 小红  | 3-888 |   91 |
| 陈胜  | 3-245 |   97 |
| 陈胜  | 3-888 |   78 |
+-------+-------+------+
```



```sql
-- 11.  查询所有学生的snum、 cname 和mark列。
mysql> select cnum,cname from course;
+-------+--------------+
| cnum  | cname        |
+-------+--------------+
| 3-105 | 文学赏析     |
| 3-166 | 编译原理     |
| 3-245 | 大学物理     |
| 3-888 | Java课程设计 |
+-------+--------------+


mysql> select cnum,snum,mark from score;
+-------+------+------+
| cnum  | snum | mark |
+-------+------+------+
| 3-245 | 101  |   97 |
| 3-166 | 102  |   57 |
| 3-105 | 103  |   97 |
| 3-166 | 103  |   87 |
| 3-888 | 103  |   67 |
| 3-105 | 105  |   70 |
| 3-888 | 105  |   91 |
| 3-245 | 109  |   97 |
| 3-888 | 109  |   78 |
+-------+------+------+


mysql> select snum,cname,mark from course,score
       where course.cnum = score.cnum;
+------+--------------+------+
| snum | cname        | mark |
+------+--------------+------+
| 101  | 大学物理     |   97 |
| 102  | 编译原理     |   57 |
| 103  | 文学赏析     |   97 |
| 103  | 编译原理     |   87 |
| 103  | Java课程设计 |   67 |
| 105  | 文学赏析     |   70 |
| 105  | Java课程设计 |   91 |
| 109  | 大学物理     |   97 |
| 109  | Java课程设计 |   78 |
+------+--------------+------+
```



```sql
-- 12.  查询所有学生的 sname、 cname 和 mark 列。
-- sname 来自 student
-- cname 来自 course
-- mark 来自 score (三表查询)

mysql> select sname,cname,mark from student,course,score
       where student.snum=score.snum and course.cnum=score.cnum;
+-------+--------------+------+
| sname | cname        | mark |
+-------+--------------+------+
| 张三  | 大学物理     |   97 |
| 李四  | 编译原理     |   57 |
| 小妹  | 文学赏析     |   97 |
| 小妹  | 编译原理     |   87 |
| 小妹  | Java课程设计 |   67 |
| 小红  | 文学赏析     |   70 |
| 小红  | Java课程设计 |   91 |
| 陈胜  | 大学物理     |   97 |
| 陈胜  | Java课程设计 |   78 |
+-------+--------------+------+
```



```sql
-- 13.  查询"95033"班学生每门课的平均分。
mysql> select snum from student where class='95033';
+------+
| snum |
+------+
| 101  |
| 102  |
| 104  |
| 107  |
| 109  |
+------+


mysql> select * from score
       where snum in (select snum from student where class='95033');
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 101  | 3-245 |   97 |
| 102  | 3-166 |   57 |
| 109  | 3-245 |   97 |
| 109  | 3-888 |   78 |
+------+-------+------+


mysql> select cnum,avg(mark) from score
       where snum in (select snum from student where class='95033')
       group by cnum;
+-------+-----------+
| cnum  | avg(mark) |
+-------+-----------+
| 3-166 |   57.0000 |
| 3-245 |   97.0000 |
| 3-888 |   78.0000 |
+-------+-----------+
```



```sql
-- 14.  查询课程"3-888"(Java程序设计)里，成绩高于"103"号(小妹)的所有同学记录。

mysql> select mark from score where snum='103' and cnum='3-888';
+------+
| mark |
+------+
|   67 |
+------+


mysql> select * from score where cnum='3-888' and mark > 67;
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 105  | 3-888 |   91 |
| 109  | 3-888 |   78 |
+------+-------+------+
```



```sql
-- 15.  查询"王花"老师任课的学生成绩
mysql> select tnum from teacher where tname='王花';
+------+
| tnum |
+------+
| 831  |
+------+


mysql> select * from course where tnum = 831;
+-------+----------+------+
| cnum  | cname    | tnum |
+-------+----------+------+
| 3-105 | 文学赏析 | 831  |
+-------+----------+------+


mysql> select * from score where cnum='3-105';
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 103  | 3-105 |   97 |
| 105  | 3-105 |   70 |
+------+-------+------+
```



```sql
-- 15.  查询选修人数大于1的课程的教师姓名。
mysql> select cnum from score group by cnum having count(*)>1;
+-------+
| cnum  |
+-------+
| 3-105 |
| 3-166 |
| 3-245 |
| 3-888 |
+-------+


mysql> select tnum from course where cnum in ('3-105','3-166','3-245','3-888');
+------+
| tnum |
+------+
| 804  |
| 825  |
| 831  |
| 856  |
+------+


mysql> select tname from teacher where tnum in ('804','825','831','856');
+--------+
| tname  |
+--------+
| 李进   |
| 老唐   |
| 王花   |
| 王旭东 |
+--------+
-- (实际项目里满足条件的课程不只一个，所以应该用in)
```



```sql
-- 16.  查询"计信学院"教师所教课程的成绩表
mysql> select * from teacher where depart='计信学院';
+------+--------+------+---------------------+--------+----------+
| tnum | tname  | tsex | tbirth              | profes | depart   |
+------+--------+------+---------------------+--------+----------+
| 825  | 老唐   | 男   | 1969-07-12 00:00:00 | 副教授 | 计信学院 |
| 856  | 王旭东 | 男   | 1985-10-12 00:00:00 | 副教授 | 计信学院 |
+------+--------+------+---------------------+--------+----------+


mysql> select * from course where tnum in (select tnum from teacher where depart='计信学院');
+-------+--------------+------+
| cnum  | cname        | tnum |
+-------+--------------+------+
| 3-888 | Java课程设计 | 825  |
| 3-166 | 编译原理     | 856  |
+-------+--------------+------+


mysql> select * from score where cnum in (select cnum from course where tnum in (select tnum from teacher where depart='计信学院'));
+------+-------+------+
| snum | cnum  | mark |
+------+-------+------+
| 103  | 3-888 |   67 |
| 105  | 3-888 |   91 |
| 109  | 3-888 |   78 |
| 102  | 3-166 |   57 |
| 103  | 3-166 |   87 |
+------+-------+------+
```



```sql
-- 17.  查询所有教师和同学的name、 sex和birth。
mysql> select tname,tsex,tbirth from teacher
       union
       select sname,ssex,sbirth from student;
+--------+------+---------------------+
| tname  | tsex | tbirth              |
+--------+------+---------------------+
| 李进   | 男   | 1979-12-12 00:00:00 |
| 老唐   | 男   | 1969-07-12 00:00:00 |
| 王花   | 女   | 1989-03-12 00:00:00 |
| 王旭东 | 男   | 1985-10-12 00:00:00 |
| 张三   | 男   | 1999-09-30 00:00:00 |
| 李四   | 男   | 2000-09-30 00:00:00 |
| 小妹   | 女   | 2003-01-10 00:00:00 |
| 大福   | 男   | 2001-03-30 00:00:00 |
| 小红   | 女   | 2000-12-30 00:00:00 |
| 布鲁斯 | 男   | 2001-07-30 00:00:00 |
| 富贵   | 男   | 2001-08-30 00:00:00 |
| 红孩儿 | 女   | 2000-11-01 00:00:00 |
| 陈胜   | 男   | 2001-01-30 00:00:00 |
+--------+------+---------------------+
```



```sql
mysql> select tname as 姓名,tsex as 性别,tbirth as 生日 from teacher
       union
       select sname,ssex,sbirth from student;
+--------+------+---------------------+
| 姓名   | 性别 | 生日                |
+--------+------+---------------------+
| 李进   | 男   | 1979-12-12 00:00:00 |
| 老唐   | 男   | 1969-07-12 00:00:00 |
| 王花   | 女   | 1989-03-12 00:00:00 |
| 王旭东 | 男   | 1985-10-12 00:00:00 |
| 张三   | 男   | 1999-09-30 00:00:00 |
| 李四   | 男   | 2000-09-30 00:00:00 |
| 小妹   | 女   | 2003-01-10 00:00:00 |
| 大福   | 男   | 2001-03-30 00:00:00 |
| 小红   | 女   | 2000-12-30 00:00:00 |
| 布鲁斯 | 男   | 2001-07-30 00:00:00 |
| 富贵   | 男   | 2001-08-30 00:00:00 |
| 红孩儿 | 女   | 2000-11-01 00:00:00 |
| 陈胜   | 男   | 2001-01-30 00:00:00 |
+--------+------+---------------------+
```



```sql
-- 18.  查询student表里每个学生的姓名和年龄。
mysql> select sname,year(now()),year(sbirth),year(now()) - year(sbirth) from student;
+--------+-------------+--------------+----------------------------+
| sname  | year(now()) | year(sbirth) | year(now()) - year(sbirth) |
+--------+-------------+--------------+----------------------------+
| 张三   |        2022 |         1999 |                         23 |
| 李四   |        2022 |         2000 |                         22 |
| 小妹   |        2022 |         2003 |                         19 |
| 大福   |        2022 |         2001 |                         21 |
| 小红   |        2022 |         2000 |                         22 |
| 布鲁斯 |        2022 |         2001 |                         21 |
| 富贵   |        2022 |         2001 |                         21 |
| 红孩儿 |        2022 |         2000 |                         22 |
| 陈胜   |        2022 |         2001 |                         21 |
+--------+-------------+--------------+----------------------------+
```



```sql
-- 19.  假设使用如下命令建立了一个grade表(成绩成绩表);
create table grade(
	low int(3),
	hight int(3),
	grade char(1)
);

-- 插入数据
insert into grade values(90,100,'A');
insert into grade values(80,89,'B');
insert into grade values(70,79,'C');
insert into grade values(60,69,'D');
insert into grade values(0,59,'E');


-- 查询所有同学的snum、 cnum、 mark和grade列。
mysql> select snum,cnum,mark,grade from score,grade where mark between low and hight;
+------+-------+------+-------+
| snum | cnum  | mark | grade |
+------+-------+------+-------+
| 101  | 3-245 |   97 | A     |
| 102  | 3-166 |   57 | E     |
| 103  | 3-105 |   97 | A     |
| 103  | 3-166 |   87 | B     |
| 103  | 3-888 |   67 | D     |
| 105  | 3-105 |   70 | C     |
| 105  | 3-888 |   91 | A     |
| 109  | 3-245 |   97 | A     |
| 109  | 3-888 |   78 | C     |
+------+-------+------+-------+
```



### 三、连接查询

- 内连接

  **inner join** 或者 **join**

- 外连接

  1.左连接 left join 或者 left outer join

    2.右连接 right join 或者 right outer join

    3.完全外连接 full join 或者 full outer join

#### 建数据表

-- person 表
id
name
carId

```sql
create table person(
	id int,
	name varchar(64),
	carId int
);
```

-- card 表
id
name,

```sql
create table card(
	id int,
	name varchar(64)
);
```



#### 添加数据

```sql
insert into person values(1,'张三',1);
insert into person values(1,'李四',3);
insert into person values(1,'王五',6);

insert into card values(1,'饭卡');
insert into card values(2,'建行卡'); 
insert into card values(3,'农行卡'); 
insert into card values(4,'工商卡'); 
insert into card values(5,'邮政卡'); 

-- 注意: 上面两张表并没有创建外键
```



#### 表连接

```sql
mysql> select * from person;
+------+------+-------+
| id   | name | carId |
+------+------+-------+
|    1 | 张三 |     1 |
|    1 | 李四 |     3 |
|    1 | 王五 |     6 |
+------+------+-------+
```

```sql
mysql> select * from card;
+------+--------+
| id   | name   |
+------+--------+
|    1 | 饭卡   |
|    2 | 建行卡 |
|    3 | 农行卡 |
|    4 | 工商卡 |
|    5 | 邮政卡 |
+------+--------+
```



```sql
-- 1.  inner join
mysql> select * from person inner join card on person.carId=card.id;
+------+------+-------+------+--------+
| id   | name | carId | id   | name   |
+------+------+-------+------+--------+
|    1 | 张三 |     1 |    1 | 饭卡   |
|    1 | 李四 |     3 |    3 | 农行卡 |
+------+------+-------+------+--------+
```



```sql
-- 2.  left join
mysql> select * from person left join card on person.carId=card.id;
+------+------+-------+------+--------+
| id   | name | carId | id   | name   |
+------+------+-------+------+--------+
|    1 | 张三 |     1 |    1 | 饭卡   |
|    1 | 李四 |     3 |    3 | 农行卡 |
|    1 | 王五 |     6 | NULL | NULL   |
+------+------+-------+------+--------+

-- 左连接，会取出左表的所有数据
```



```sql
-- 3.  right join
mysql> select * from person right join card on person.carId=card.id;
+------+------+-------+------+--------+
| id   | name | carId | id   | name   |
+------+------+-------+------+--------+
|    1 | 张三 |     1 |    1 | 饭卡   |
|    1 | 李四 |     3 |    3 | 农行卡 |
| NULL | NULL |  NULL |    2 | 建行卡 |
| NULL | NULL |  NULL |    4 | 工商卡 |
| NULL | NULL |  NULL |    5 | 邮政卡 |
+------+------+-------+------+--------+
```



```sql
-- 4.  full join
-- select * from person full join card on person.carId=card.id;

-- mysql 不支持 full join
```



```sql
-- 用union实现
mysql> select * from person left join card on person.carId=card.id
       union
       select * from person right join card on person.carId=card.id;
+------+------+-------+------+--------+
| id   | name | carId | id   | name   |
+------+------+-------+------+--------+
|    1 | 张三 |     1 |    1 | 饭卡   |
|    1 | 李四 |     3 |    3 | 农行卡 |
|    1 | 王五 |     6 | NULL | NULL   |
| NULL | NULL |  NULL |    2 | 建行卡 |
| NULL | NULL |  NULL |    4 | 工商卡 |
| NULL | NULL |  NULL |    5 | 邮政卡 |
+------+------+-------+------+--------+
```

### 四、事务



```sql
-- 1.  mysql 默认是开启事务的(自动提交)

mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+

-- 默认事务开启的作用是什么?
-- 当我们执行一个sql 语句时，效果会立即体现，且不能回滚。
```



- 数据准备

```sql
-- 建库 bank
create database bank;
use bank;


-- 建用户表
create table user(
	id int primary key,
	name varchar(64),
	money int
);

-- 插入数据
insert into user values(1,'a',1000);
```



```sql
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+

-- 事务回滚；撤销sql 语句。
mysql> rollback;


mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+
-- 无法回滚
```



- 设置mysql 自动提交为 false

```sql
mysql> set autocommit=0;


mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            0 |
+--------------+
-- 已经关闭自动提交
```



- 回滚成功

```sql
mysql> insert into user values(2,'b',2000);

mysql> rollback;

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+
```





- 提交之后 无法回滚

```sql
mysql> insert into user values(2,'b',2000);

mysql> commit;  -- 提交

mysql> rollback;

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  2000 |
+----+------+-------+
-- 第二条数据无法回滚
```



- 自动提交	@@autocommit=1
- 手动提交    commit
- 事务回滚    rollback



#### 模拟转账



- b 向 a转账500

```sql
update user set money=money-500 where name='b';

update user set money=money+500 where name='a';

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1500 |
|  2 | b    |  1500 |
+----+------+-------+

-- 此时并没有提交
```



- 回滚成功，事务提供了一个返回的机会。

```sql
mysql> rollback;

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  2000 |
+----+------+-------+
```



- 还事务原默认状态（自动提交）

```sql
set autocommit=1;


mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
-- 现在已无法回滚
```



- 再次转账

```sql
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  2000 |
+----+------+-------+


update user set money=money-500 where name='b';
update user set money=money+500 where name='a';

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1500 |
|  2 | b    |  1500 |
+----+------+-------+
```



- 尝试回滚，发现回滚失败

```sql
mysql> rollback;


mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1500 |
|  2 | b    |  1500 |
+----+------+-------+
```



- 尝试手动开启事务
- begin  或者  start transaction

```sql
begin;
update user set money=money+100 where name='b';
update user set money=money-100 where name='a';


mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1400 |
|  2 | b    |  1600 |
+----+------+-------+


mysql> rollback;

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1500 |
|  2 | b    |  1500 |
+----+------+-------+
-- 回滚成功
```



- start transaction同理

```sql
start transaction;
update user set money=money-1000 where name='b';
update user set money=money+1000 where name='a';


mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  2500 |
|  2 | b    |   500 |
+----+------+-------+

mysql> rollback;

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1500 |
|  2 | b    |  1500 |
+----+------+-------+
```



#### 事务的隔离性

1. **read uncommitted**;        读未提交的
2. **read committed**;			 读已经提交的
3. **repeatable read**;             可以重复读
4. **serializable**;                      串行化





- 一、read uncommitted

```sql
insert into user values(3,'小明',1000);
insert into user values(4,'淘宝店',1000);

mysql> select * from user;
+----+--------+-------+
| id | name   | money |
+----+--------+-------+
|  1 | a      |  1500 |
|  2 | b      |  1500 |
|  3 | 小明    |  1000 |
|  4 | 淘宝店  |  1000 |
+----+--------+-------+

-- 假设小明在淘宝店买了800的鞋，转账后店里要看800是否已经到账
```





- 如何查看数据库的隔离级别  以mysql 5.7为例

```sql
-- 系统级别
select @@transaction_isolation;

-- 会话级别
select @@global.transaction_isolation;
```



```sql
mysql> select @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
-- 或者 select @@global.tx_isolation;
```



- 修改隔离级别

```sql
mysql> set global transaction isolation level read uncommitted;


mysql> select @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| READ-UNCOMMITTED               |
+--------------------------------+
-- 修改成功
```



- 现在回到小明买鞋

```sql
-- 开启事务
begin;
update user set money=money-800 where name='小明';
update user set money=money+800 where name='淘宝店';

mysql> select * from user;
+----+--------+-------+
| id | name   | money |
+----+--------+-------+
|  1 | a      |  1500 |
|  2 | b      |  1500 |
|  3 | 小明    |   200 |
|  4 | 淘宝店   |  1800 |
+----+--------+-------+
-- 淘宝店发现确实有转账信息，于是给小明发货。



-- 如果此时小明 rollback
rollback;
mysql> select * from user;
+----+--------+-------+
| id | name   | money |
+----+--------+-------+
|  1 | a      |  1500 |
|  2 | b      |  1500 |
|  3 | 小明    |  1000 |
|  4 | 淘宝店   |  1000 |
+----+--------+-------+
-- 钱就不会转到店里


-- 上述情况：淘宝店读到了小明没有提交的事务数据(脏读)
-- 在隔离级别为 READ-UNCOMMITTED 情况会出现脏读
-- 实际开发不允许出现脏读现象
```



- 二、  read committed; 

  ​    -- **会发生不可重复读现象**



- 三、  repeatable read;
       -- **可以重复读, 但会发生幻读现象**

  

**注意**

-- 幻读和不可以重复读现象略复杂, 可以上网搜资料
-- 打开两个mysql终端模拟实际开发更佳