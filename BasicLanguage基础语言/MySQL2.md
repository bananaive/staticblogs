# MySQL

## 结构

### 数据库

### 数据表

### 按行数据

- 属性-列

## 语法

### 不区分大小写，建议关键字大写

### 每句话用；结尾

### 关键词不能缩写不能分行

### 用缩进提高语句的可读性

### 各子句一般分行写

### 注释

- 单行注释：#注释内容
- 单行注释：-- 注释文字
- 多行注释：/*多行注释内容*/

## 常用语句

### 登入

- mysql -h 主机名 -P 端口号 -u 用户名 -p 密码

### show

- show database

	- 显示所有数据库

- show tables

	- 显示数据表

### use

- use 数据库名

	- 进入某个数据库

### select

- select database()

	- 查看当前所在的数据库

- select * from 数据表名

	- 查看数据表的所有数据

- select version()

	- 查看数据库版本

### create

- create table 数据表名(
列名 列类型,
列名 列类型,
。。。。。。
)

	- 创建数据表

### desc

- desc 数据表名

	- 查看数据表的表结构

### 函数必须有返回值

## DQL数据查询语言

### 基础查询

- 概念

	- select 查询列表
from 表名;
	- 查询列表可以是：表中的字段、常量值、表达式、函数
	- 查询结果是一个虚拟的表格

- 查询表中的单个字段

	- SELECT 字段名 FROM 表名；

- 查询表中的多个字段

	- SELECT 字段1，字段2，字段3 FROM 表名；

		- 顺序与字段顺序一致

- 查询表中的所有字段

	- SELECT * FROM 表名；

		- 顺序与原始表中的顺序完全一致

- 查询常量值

	- SELECT 常量值；

		- 返回内容为常量值

- 查询表达式

	- SELECT 100*90；

- 查询函数

	- SELECT VERSION()；

		- 相当于调用函数

- 为字段起别名

	- SELECT 查询列表 AS 别名
	- SELECT 查询列表 空格 别名
即：SELECT username “别名”

- 去重

	- SELECT DISTINCT 查询列表 FROM 表名称

- 数值运算-+号

	- 只能作为运算符来用
也就是数值+数值
如果一方为字符型，它会使图转换为数值型
且如果转换失败，则视为0
只要其中一方为NULL，则结果为NULL
	- SELECT 字段名+字段名 FROM 表名

- 字符串拼接-CONCAT函数

### 条件查询

- 语法

	- SELECT 
        查询列表
FROM
        表名
WHERE
        筛选条件；
	- 执行顺序：先FROM再WHERE最后SELECT

- 按照条件表达式筛选

	- 条件表达式

		- > < = != <> >= <=
		- <> !=

			- 不等于

- 按照逻辑表达式筛选

	- 逻辑运算符

		- &&

			-  并且

		- ||

			- 或者

		- !

			- 非

		-  and

			- 并且

		- or

			- 或者

		- not

			- 非

	- 模糊查询

		- like

			- 一般和通配符搭配使用
			- 通配符

				- %任意多个字符包含0
				- _任意单个字符

			- MySQL也支持转义字符

		- between and

			- 可以提高简洁度
			- 包含两个端点
			- 等价于大于等于前一个值，小于等于后一个值

		- in

			- 判断某字段的只是否属于in列表中的某一项
			- 提高简洁度
			- in列表的值类型必须统一
			- 并不支持通配符
			- 等价于好几个 =

		- is null

			- =或者<>不能用于判断null值
只能用is null或者is not null

		- <=>

			- 安全等于
			- 可以判断null值以及普通的数值

## DML增删改语言

## DDL表的操作

## TCL事务控制语言

