---
title: MySQL常见约束
date: 2021-12-22 15:42:35
tags: 
- 笔记
- MySQL
cover: https://s2.loli.net/2021/12/29/6Vcr3mzG7FuSQyv.gif
top_img: /img/20.jpg
---

Tips：

> 1. 检查约束(check):
MySQL中不支持检查约束，但是`Oracle`中是好使的。就在这前面简单介绍一下吧！笔记不嫌多。哈哈哈哈！
我在大学的课程是SQL Server，这玩意也支持Check约束，语句贴下面，简单感受一下吧。
还有另外两种约束：
自增长约束(AUTO_INCREMENT)
非负约束(unsigned)
相信你们都晓得这自增约束，自增长约束在下面详解代码有涉及到，不知道的自行百度。链接放这。[怎么实现自增长约束](https://blog.csdn.net/weixin_44050355/article/details/103948501)
2. 创建检查约束：
``` SQL
create table emp8(
    id int(10),
    name varchar(20),
    salary Double(10,2),
    CONSTRAINT emp8_salary_ck CHECK(salary > 3000)   #工资大于3000不允许往工资里加
);
```

# 什么是约束？约束使用规则？

* 约束是在表中定义的用于维护数据库完整性的一些规则。

* 通过为表中的列定义约束可以防止将错误的数据插入表中，也可以保持表之间数据的一致性

* 若某个约束条件只作用于单独的列，可以将其定义为列级约束也可定义为表级约束；

* 若某个约束条件作用域多个列，则必须定义为表级约束。




# Mysql中支持以下约束

### * 一、非空约束：not null

### * 二、唯一约束：unique 

### * 三、主键约束：primary key

### * 四、外键约束： foreign key

# 约束类型详细用法

## * 一、非空约束(NOT NULL)

###  (1)  not null约束的字段不允许为NULL，必须有值；非空约束是列级约束，如果想设置多行非空，必须每行都添加非空约束。

### 语法格式：

``` SQL
create table student(
    id int,
    name varchar(30) not null      #添加学生的姓名非空约束，不允许姓名是空值
);
```

### (2)  为已创建的表添加非空约束：

``` SQL
alter table student
modify student varchar(30) not null;  #modify 字段名 数据类型 not null;
```
### (3)  取消非空约束

``` SQL
alter student
modify student varchar(30) null;  #把原来的not null改为null即可
```


## * 二、唯一约束(UNIQUE)

### (1)  UNIQUE, 规定某个表中的某个字段的值是唯一的，不能重复，但是可以为null。unique放在一个列上，是列级约束；unique放在多个列上，是表级约束。

### 列级约束语法格式：

``` SQL
create table student(
    id int,
    name varchar(30) unique,   #添加学生的姓名唯一约束，不允许重名
    age int
);
```
### 表级约束语法格式：

``` SQL
create table emp3(
    id int(10),
    name varchar(20),
    phone carcahr(30),
    CONSTRAINT emp3_phone_un UNIQUE(phone)  #CONTRAINT后接约束名(随便取，一般取名格式是表_字段名_约束缩写，这样比较直观)
)
```

### (2)  组合唯一约束

``` SQL
create table emp3(
    id int(10),
    name varchar(20),
    phone carcahr(30),
    CONSTRAINT emp3_email_un UNIQUE(phone,name) #必须电话跟姓名同时一样才不能添加，其中一个字段一样，一个字段不一样，是允许的。
```

### (3)  删除唯一约束语法：

``` SQL
alter table emp3
drop index emp3_email_un;   #drop index后接需要删除的约束名
```

## * 三、主键约束(primary key)

### (1)  主键约束修饰的字段，非空且唯一，一张表中只能有一个主键。通常利用主键确定唯一一条数据。

### 列级约束语法格式：

``` SQL
create table student(
    id int(10) AUTO_INCREMENT primary key,   #通常给主键约束添加自增约束AUTO_INCREMENT
    name varchar(30)
)
```
### 表级约束语法格式：

``` SQL
create table emp6(
    id int(10),
    name varchar(20),
    CONTRAINT emp6_id_pk primary key(id)
);
```

### (2)  为已创建的表添加主键约束：

``` SQL
alter table student
add CONSTRAINT emp6_id_pk primary key(id);   #关键字ADD CONSTRAINT
```

### (3)  删除主键约束：

``` SQL
alter table emp6    #删除表emp6的主键约束
drop primary key;  #删除主键比较特别，不用加主键的名称
```

## * 四、外键约束(FOREIGN key)

### (1)  外键约束：通常关联另一个表的主键，出现在外键约束的数据肯定也在主键约束表中

### (2)  所以外键约束涉及到两个表的关联关系，通常从表的外键关联主表的主键

### (3)  外键约束也是这几个约束类型中最复杂，最难理解的约束，建议在mysql中建表多试试。

``` SQL
create table dept1(
dept_id int PRIMARY KEY,
dept_name varchar(20)
);

select * from dept1;

create table emp7(
id int(10),
name varchar(20),
depart_id int(10),
constraint emp7_departId_fk foreign key(depart_id) references dept1(dept_id)
);
```
### (4)  添加了外键约束的两张表有什么特点：

#### * 出现在外键约束的数据肯定也在主键约束表中：
所以你想在emp7中添加数据，首先你要保证dept1表中有这个数据。

#### * 如果你想删除主表中的数据，你运行delete语句一定报完整性错误，所以删除dept1表中的数据之前，你得先删除从表emp7中的这个数据。

``` SQL
insert into tep1
values(10,'IT');

insert into emp7
values(101,'张三',10)

delete from dept1
where dept id = 101;   #你先运行这个一定报错

delete from emp7
where id = 10    #你应该先删除外键从表emp7的数据，才能成功删除主表tep1中的对应关联的数据
```

### (5)级联删除(on delete cascode) 跟 级联置空(on delete set null)

#### 阅读到这你会发现，我们每次要删除数据的话还要先把相关联的外键一个个删除以后才能去删这个主表的数据，相当的麻烦，就是再来学两个好用的删除方法。

#### * 级联删除：当父表中的列被删除时，子表中相对应的列数据也被删除

``` SQL
create table emp7(
    id int(10),
    name varchar(20),
    depart_id int(10),
    constraint emp7_departId_fk foreign key(depart_id) references dept1(dept_id)
    on delete cascode #删除主表数据时，从表也会跟着同时删除
);
```

#### * 级联置空：当父表中的列数据被删除时，子表中相应的列置空

``` SQL
create table emp7(
    id int(10),
    name varchar(20),
    depart_id int(10),
    constraint emp7_departId_fk foreign key(depart_id) references dept1(dept_id)
    on delete set null #删除主表数据时，从表数据置空
);
```

### (6)为已创建的表添加外键约束：

``` SQL
alter tale emp7
ADD CONSTRAINT emp7_departId_fk FOREIGN KEY(depart_id) REFERENCES dept1(dept_id);
```

### (7)删除外键约束

``` SQL
alter tale emp7
drop FOREIGN KEY emp7_departId_fk;
```

### 结尾

 约束理解起来没什么难的点吧，除了外键约束要复杂一点，create语句，insert语句都贴在上面了，只要粘贴到Mysql中跑一下，然后对照其他sql语句，就能自己看到效果了。外键约束如果日后忘记了的话，时空隧道放着，多多复习。

[外键约束视频教学](https://www.bilibili.com/video/BV1GP4y1h7Zj?p=47),笔记就是这么个笔记，整理起来是真的费劲。

### 最后，感谢阅读。