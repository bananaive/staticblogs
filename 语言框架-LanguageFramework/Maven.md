# Maven
## 为什么需要maven

简单的说就是，maven可以帮助我们管理jar包的导入，以及项目的打包，Maven可以根据现有依赖自行添加其需要的jar包

它的定义是一个**项目架构管理工具**

他的核心思想就是**约定大于配置**
## 项目结构
```
│  javaweb-01-maven.iml
│  pom.xml--Maven的配置文件
│
├─src
│  └─main
│      ├─java--用来保存所有的源代码
│      ├─resources--用来保存服务器相关的资源
│      └─webapp--项目初始化生成的web范例
│          │  index.jsp
│          │
│          └─WEB-INF
│                  web.xml
│
└─target--Maven构建出的内容(包括打包出的jar文件)
```

## pom.xml
* project-Maven的版本和头文件
* groupId,artifactId,version-在Idea新建项目时填写的信息就在这里
* packaging-项目的打包方式(一般情况下**jar**是JAVA应用,**war**是JavaWeb应用)
* properties-配置信息都在这里
   *  project.build.sourceEncoding-项目的默认构建编码
   *  maven.compiler.source-项目的编码版本
   *  maven.compiler.target-项目的编码版本
*  dependency-项目依赖
   *  dependency-每一个依赖就是一个dependency,内含如下信息
      *  groupId
      *  artifactId
      *  version
*  build-项目在构建时用到的的插件等
## 父子工程
父工程中会有
```xml
<modules>
   <module>servlet01</module>
</modules>
```
子工程会有
```xml
<parent>
   <artifactId>javaweb-01-maven</artifactId>
   <groupId>top.gobanana</groupId>
   <version>1.0-SNAPSHOT</version>
</parent>
```
父项目中的所有jar包,子项目可以直接使用
## Maven环境优化
1. 修改web.xml为最新
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         >
   </web-app>
   ```
2. 将Maven的结构搭建完整(指创建java和src文件夹)
## 常见问题
* Maven在导出文件时,有些文件不一定会被导出,因为它约定了在项目的哪些目录中应该只有哪些文件,不在pom中修改配置,它不会进行导出
