# web

## web server

### 指用来运行web应用的平台
专门处理http请求

### nginx

- 并发能力强
- 内存占用小

### apache

- 跨平台
- 安全性

### kangle

- 专为虚拟主机研发

## B/S模型

### 浏览器请求，服务器响应的工作模式

## XML格式

### 一种文件存储、传输格式

### 用途

- 创建新的互联网语言，比如XHTML

## 面向服务SOA

### 服务之间通过简单又精确的接口进行通信

### 粗粒度、松散耦合

## 分布式系统消息通信技术

### RPC

- C/S
- 同步的
- 面向过程
- 跨平台跨语言

### COPBA

- 在概念上扩展了RPC
- 面向对象
- 企业级

### RMI

- 面向对象的JAVA RPC

### WebService

- 基于web
- 同步调用，实时性要求高

### 消息中间件MOM

- 特点

	- 利用高效可靠的消息传递机制进行平台无关的数据交流
	- 屏蔽各种平台与协议之间的特性，实现应用的协同

- 主流标准

	- JMS

		- JAVA平台
		- 面向接口
		- 消息规范
		- 一套API标准
		- 没有考虑异构系统

	- AMQP

		- 消息的 producer 将 Message 发送给 Exchange，Exchange 负责交换 / 路由，将消息正确地转发给相应的 Queue。消息的 Consumer 从 Queue 中读取消息。

	- STOMP

- 常见MQ

	- Qpid

		- 有安全认证特性

	- RabbitMQ

		- 重量级
		- 消息队列
		- broker
		- 支持多种协议
		- 可扩展性差、速度慢

	- ZeroMQ

		- 非常轻量级
		- 支持高级消息场景但是需要实现各个块
		- 仅提供非持久性队列

	- Jafka/Kafka

		- 快速持久化、高吞吐、完全的分布式系统
		- 支持Hadoop数据并行加载

	- ActiveMQ

		- 介于RabbitMQ与ZeroMQ之间

	- Apollo
	- redis

		- 一个Key-Value的数据库
		- NoSQL
		- 因为支持MQ功能所以可作为一个轻量级队列服务
		- 入队数据小于10k时表现良好、出队时任意大小数据均有很好的性能
		- RabbitMQ出队性能远低于redis
		- 盛大使用

	- Metamorphosis

		- Kafka的JAVA版
		- 阿里在2017年对其定制与优化

- 此部分的来源

	- https://www.cnblogs.com/huojg-21442/p/7601380.html

- 待看的部分

	- https://blog.csdn.net/qq_42459181/article/details/88061028?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase

## 应用服务器

### 通过多种协议为应用提供服务

## Http

## 2017年的内容

