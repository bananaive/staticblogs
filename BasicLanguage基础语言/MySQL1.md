# MySQL

## DDL

### create

- create database 数据库名

	- 用给定的名字创建一个数据库

- create table 表名
- create index 索引名 on 表名(字段)

	- 在表的制定字段创建索引

### show

- show databases

	- 列出在MySQL服务器主机上的数据库

- show tables 数据库名

	- 显示当前数据库中已有的数据库

### drop

- drop database 数据库

	- 删除数据库中所有表和数据库

- drop table 表名
- drop index 索引名 on 表名

	- 删除索引

### use 数据库名

- 转换数据库

### rename table 表名 to 新表名

### desc 表名

- 把指定数据库表的结构显示出来

### truncate 表名

- 删除表里的全部数据，保留表结构

### alter table 表名

- add（列名1 列类型，列名2 列类型）
- modify 列名 列类型
- drop 列名

	- 删除列

- change 旧列名 新列名 列类型
- rename 新表名
- drop index 约束名

	- 删除唯一约束

- drop foreign key 约束名

	- 删除外键约束

- drop primary key

	- 删除表的主键约束

- drop foreign key 约束名

	- 删除表的外键约束

## 对象

### 表 table

- 最基本的数据存储单元

### 约束 constraint

- 执行数据校验的规则，用于保证数据完整性
- not null 非空约束
- unique 唯一约束
- primary key 主键约束
- foreign key 外键约束
- check 检查约束

	- 在MySQL中并未实现

### 视图 view

- 一个或者多个数据表里数据的逻辑显示，视图并不存储数据

### 索引 index

- 用于提高查询性能，相当于目录，独立于数据库对象，创建后由数据库系统自动维护，不同数据表中的索引可以重名，频繁使用dml语句时，维护索引会增加系统开销，存储索引也需要磁盘空间

### 函数 function

- 用于完成一次特定的计算，具有一个返回值
- 单行函数

	- 字符函数
	- 数值函数
	- 时间日期函数
	- 流程函数
	- 其他函数

- 多行函数

	- avg
	- sum
	- count
	- max
	- min
	- 分组函数

		- group by 列名

			- 针对指定的重复数据，进行分组操作

	- 过滤分组

		- having 过滤条件

			-  使用过滤条件对分组的结果进行过滤

	- limit 起始位置,显示的总数

		- 限制返回结果的行数，起始位置默认为0

### 存储过程 procedure

- 用于完成一次完整的业务处理，没有返回值，但可通过传出参数将多个值传给调用环境

## 数据的特殊设置

### auto_increment 自动增长

### default 默认值

## DML

### insert into 列表名{(column [,column...])} values (value [,value...])

- 插入单行

### insert into 列表名{(column [,column...])} values (value [,value...]), (value [,value...])

- 插入多行

### update 列表名 set column = value [,column = value]...[where condition]

- 可以同时修改多列

### delete [from] 列表名 [where condition]

- 删除表中记录，如果没有where限定则删除所有记录

## DQL

### select

- select {*,column [alias],...} from 列表名 
where 条件 
order by 排序字段1 ASC,排序字段2 ASC

	- 排序

		- ASC升序
		- DESC降序

	- 多关键词排序

		- 优先按第一个字段升/降序，若第一个字段有相同的记录，则按后面的字段升/降序

- select {*,column [alias],...} from 列表名

	- 查询

		- 可以在查询的基础上做运算再显示
		- 也可以在查询的结果上将列的标题头改为别名再显示

- select distinct 关键字1,关键字2 from 列表名

	- 在查询结果中去除关键字内容完全相同的内容

- select {*,column as [别名],...} from 列表名

	- 查询

		- 可以在查询的基础上做运算再显示
		- 也可以在查询的结果上将列的标题头改为别名再显示

- 多表查询

	- 交叉连接/笛卡尔交集

		- select ... from 表1 
cross join  表2
on 连接条件
		- select ... from 表1,表2

	- 内连接

		- 等值连接

			- select 字段列表 from 表1，表2 where 表1.属性 = 表2.属性
			- select 字段列表 from 表1 join 表2 on 表1.属性 = 表2.属性
			- select 字段列表 from 表1，表2，表3 where 条件1 and 条件2

		- 非等值连接

			- select 字段列表 from 表1，表2 where 非等值条件

	- 外连接

		- 左外连接

			- select ... left join ... on ...

				- 左边的表中的行全部显示

		- 右外连接

			- right join ... on ...

				- 右边的表的行全部显示

		- 全外连接

			- 左右外连接的数据都显示
在MySQL中没有实现

	- 自连接

		- 将自身看作一个新的数据表进行多表连接查询

- 子查询

	- select语句的嵌套
效率不如多表查询
要用括号来提高优先级

		- select ... from ... where ...>(select ... from ...where 条件 )

	- select 单行数据
from 多行数据
where 单行数据

## MySQL特殊之处

### 比较运算符

- <>表示不等于
- between...and...在两个值之间
- in(list)匹配所有列出的值
- like匹配一个字符模式

	- % 表示0个或者多个字符_表示一个字符

- is null是空值

