# MYSQL



SHOW DATABASES;

CREATE DATABASE 数据库名;

USE 数据库名;

SELECT DATABASE();

DROP DATABASE 数据库名; 

表结构创建

```mysql
create table emp(
	id int comment '编号'，
	worknum varchar(10),
    name varchar(10),
    gender char(1),
    age tinyint unsigned,
    idcard char(18),
    entrydate date
) comment '员工表';
```

表结构修改                                                                                                                                                                                                                                        

1. 添加字段

ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束]  

2. 修改字段

ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）

3. 修改字段名和字段类型

ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释] [约束]  

4. 删除字段

ALTER TABLE 表名 DROP 字段名

5. 修改表名

ALTER TABLE 表名 RENAME TO 新表名

6. 删除

DROP TABLE [IF EXISTS] 表名

删除指定表并重新创建该表

TRUNCATE TABLE 表名

## DML

Data Manipulation Language

INSERT/UPDATE/DELETE

INSERT INTO 表名 （字段名1， 字段名2， …） VALUES （值1， 值2， 值3， …）;

INSERT INTO 表名  VALUES （值1， 值2， 值3， …）;

批量添加方法

INSERT INTO 表名 （字段名1， 字段名2， …） VALUES （值1， 值2， 值3， …） （值1， 值2， 值3， …） （值1， 值2， 值3， …）;



插入数据时， 指定的字段顺序与值顺序一一对应

字符串和日期型数据应该包含在引号之间

插入的数据大小， 应该在字段的规定范围内。



 DML-修改数据

UPDATE 表名 SET 字段名1=值1， 字段名2=值2，…[WHERE 条件]

**没有条件就修改整张表**

DELETE FROM 表名 [WHERE 条件]

没有where就删除全部



## 查询参数

select id from users union select email-id from users

union 列数必须相同。

group by 分组

select * from users group by 2

order by 默认升序排列 desc 变降序

limit 限制输出内容数量

select * from users limit 0, 3；

group_concat 合并到一行显示





注入：构造精巧语句，查询得到的信息。

union注入， 报错注入， 布尔注入， 时间注入

注入点： 访问数据库的连接。



如何判断字符型注入还是数字型注入？

使用and 1 = 1 和 and 1 = 2 来判断。

> 加号容易被理解成空格

如何判断闭合方式?

‘ “ ‘) “) 其他

输入?id=1‘’’报错为near 1多了一个单引号