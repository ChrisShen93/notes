# MySQL入门

标签（空格分隔）： 数据库 MySQL

---

# 数据类型

## 整数

TINYINT：使用一个字节，即八位进行存储

SMALLINT：使用两个字节

MEDIUMINT：三个字节

INT：四个字节

BIGINT：八个字节


## 实数

### 不精确类型实数

FLOAT：四个字节

DOUBLE：八个字节

### 精确类型实数

DECIMAL：可指定小数点前后的最大位数
    eg：DECIMAL(20,2)表示小数点后存储两个数字，小数点存储十八个数字


## 字符串

VARCHAR：用于保存可变长度的字符串，只规定最大长度值。可节省磁盘空间

CHAR：固定长度的字符串，范围为0~255，会以空格补充长度

TEXT：有四种子类型，一般被看做非二进制字符串，即字符字符串，有字符集，通过字符集进行比较和排序
    TINYTEXT
    TEXT
    MEDIUMTEXT
    LONGTEXT

BLOB：四种子类型，一般被看做二进制字符串，即字节字符串，没有字符集，比较和排序都是基于字节的
    TINYBLOB
    BLOB
    MEDIUMBLOB
    LONGBLOB


## 日期

DATETIME：能保存大范围的值，从2001~9999，精度为秒，使用八个字节进行存储

TIMESTAMP：时间戳类型，保存从1970.1.1以来的秒数，使用四个字节进行存储，只能保存1970~2038年


# 数据类型选择准则

最小原则

简单原则

避免索引列上的NULL



# MySQL常用操作

```mysql
CREATE TABLE tbl_user (
    user_name VARCHAR(20),
    age INT,
    signup_date DATE
);

insert into tbl_user values('chris', '22', '2016-07-24');

SELECT * FROM tbl_user;

insert into tbl_user values('jikexueyuan', 18, '2016-07-24');

select * from tbl_user;

select * from tbl_user where user_name = 'chris';

select * from tbl_user where user_name = 'chris' and age = '22';

update tbl_user set age = 23 where user_name = 'chris';

delete from tbl_user where user_name = 'chris';

alter table tbl_user add email varchar(50);

alter table tbl_user drop email;

alter table tbl_user change age user_age int;

alter table tbl_user change user_age user_age tinyint(1) not null;

alter table tbl_user rename user_tbl;
```



## MySQL事务的基本概念

### MySQL事务处理的基本概念

> 事务（Transaction）：作为一个单独单元的一个或者多个SQL语句组成。这个单元中的每个SQL语句是相互依赖的，而且单元作为一个整体是不可分割的。如果单元中的一个语句不能成功地完成，整个单元就会回滚，所有影响到的数据库将返回事务开始以前的状态。因此，只有事务中所有语句都被成功地执行才能说这个事务被成功地执行。

事务与ACID属性：

* 原子性Atomicity
    * 每个事务都必须被认为是一个不可分割的单元
* 一致性Consistency
    * 不管事务是完全成功完成还是中途失败，当事务使系统处于一致的状态时存在一致性
* 孤立性Isolation
    * 每个事务在它自己的空间发生，和其他发生在系统中的事务隔离，而且事务的结果只有在它完全被执行时才能看到
* 持久性Durability
    * 即使系统崩溃，一个提交的事务仍然被保持

### MySQL事务处理的生命周期

```mysql
start transaction;

-- sql codes here

commit;  -- rollback;
```

### MySQL事务处理的自动提交

```mysql
select @@autocommit;  -- 查看是否开启自动提交

set @@autocommit = 0;  -- 将自动提交关闭。此时不执行commit的话，事务不会被提交
```


## MySQL事务的隔离级别与锁

### MySQL事务隔离级别的概念与序列化隔离

1. serializable（序列化隔离）
2. repeatable（可重复读）
3. read committed（提交读）
4. read uncommitted（未提交读）

### MySQL事务隔离级别可重复读，提交读和未提交读

### MySQL性能与非事务表的表锁定
