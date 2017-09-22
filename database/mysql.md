# MySQL 重新入门

标签（空格分隔）： MySQL 数据库

---

### 常用命令

```mysql
## 终端登录
mysql -u root -p

## 查看数据库
show databases;

## 创建数据库
create database database_name;

## 切换到数据库
use database_name;

## 查看数据库中的表
show tables;

## 创建表
create table table_name (column_name value_type(value_length) [, column_name value_type(value_length)]);
## eg:
create table blog_list (blog_title varchar(20), create_time varchar(20));

## 建表的复杂形式，eg
create table table_name (
        id int not null auto_increment,
        name char(20) not null,
        address char(50) null,
        city char(50) null default 'china',
        age int not null,
        primary key(id)
    )engine=InnoDB;

## 显示表的结构
show columns from table_name;
## 快捷方式
describe table_name;

## 更改表结构
### 增加一列
alter table table_name add column_name char(100) null;
### 删除一列
alter table table_name drop column column_name;

## 查看记录
select * from table_name;

## 插入数据
insert into table_name values ('colunm_1_value', 'column_2_value', ...);

## 删库
drop database database_name;

## 删表
drop table table_name;

##清空表记录
delete from table_name;
```


### 数据类型

#### 数字类型

整数：

1. tinyint
2. smallint
3. mediumint
4. int
5. bigint

浮点数：

1. float
2. double
3. real
4. decimal

#### 日期和时间

1. date
2. time
3. datetime
4. timestamp
5. year

#### 字符串类型

字符串：

1. char
2. varchar

文本：

1. tinytext
2. text
3. mediumtext
4. longtext

二进制：

1. tinyblob
2. blob
3. mediumblob
4. longblob
