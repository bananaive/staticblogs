# 环境
* JDK1.8
* MySQL5.7(MySQL两个版本很经典,5.7和8.0)
* maven3.6.1
* IDEA
# 前情回顾
* JDBC
* MySQL
* Java基础
* Maven
* junit(单元测试)
# Mybatis相关
官方中文文档

https://mybatis.org/mybatis-3/zh/index.html

官方源码

https://github.com/mybatis/mybatis-3

Maven引入
```XML
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
</dependency>
```

# 持久化与持久层
持久化   
就是将数据存储起来的动作

持久层   
就是完成数据存储的部分代码
# 小试牛刀
1. 配置环境   
   创建maven项目,在其中导入junit/MySQL驱动/Mybatis三个包

2. 创建一个模块
   * 编写Mybatis的核心配置文件``mybatis-config.xml``
  ```XML
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
        <!-- 环境标签,同一个项目中可以有多个环境 -->
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
                    <!-- 驱动类型,这里是MySQL的驱动 -->
                    <property name="driver" value="com.mysql.jdbc.Driver"/>
                    <!-- 连接MySQL的URL,设定了许多参数,包括地址,端口,数据库,是否启用SSL等 -->
                    <property name="url" value="jdbc:mysql://124.70.34.168:3306/ypgtdb?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;useJDBCCompliantTimezoneShift=true&amp;useLegacyDatetimeCode=false&amp;serverTimezone=Asia/Shanghai"/>
                    <!-- 用户名密码 -->
                    <property name="username" value="ypgt"/>
                    <property name="password" value="Ypgt_2021"/>
                </dataSource>
            </environment>
        </environments>
        <mappers>
            <!-- 所有的实现Dao接口的Mapper.xml文件都需要在这里注册 -->
            <mapper resource="top/gobanana/dao/UserMapper.xml"/>
        </mappers>
    </configuration>
  ```
   * 编写一个类用来绑定核心配置文件,获取``SqlSession``对象(通常放在utils包下)
  ```JAVA
    package top.gobanana.utils;

    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;

    import java.io.IOException;
    import java.io.InputStream;

    public class MybatisUtils {
        private static SqlSessionFactory sqlSessionFactory;
        static {
            try {
                // 绑定上面的核心xml文件
                String resource = "mybatis-config.xml";
                InputStream inputStream = Resources.getResourceAsStream(resource);
                // 创建工厂类对象
                sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            } catch (IOException e){
                e.printStackTrace();
            }
        }

        public static SqlSession GetSqlSession(){
            // 使用工厂类对象生产出包含所有MySQL操作的SqlSession对象
            return sqlSessionFactory.openSession();
        }
    }
  ```
   * 创建符合数据库结构的pojo类(实体类),用来以对象为容器存储查找出的一行数据(通常在pojo包下)
  ```JAVA
  package top.gobanana.pojo;

    public class User {
        // 这里的字段,取出多少字段就写多少,用来在Mapper.xml文件中规定返回类型,以及项目中其他位置的调用
        private int id;
        private String username;
        private String password;
        private String create_time;
        private String status;
        private String salt;
        private String end_time;
        private String realname;
        // 其中有参构造方法,无参构造方法,重写toString()方法的部分省略
    }
  ```
   * 创建pojo类对象对应的dao接口类,接口中定义此对象相关的各个操作
  ```JAVA
    package top.gobanana.dao;

    import top.gobanana.pojo.User;

    import java.util.List;

    public interface UserDao {
        // 这里以一个获取所有字段为例子
        List<User> getUserList();
    }
  ```
   * 在``resources``文件夹下的相同路径,创建dao类对应的``***Mapper.xml``文件
  ```XML
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <!-- namespace使用完成路径绑定本xml文件要实现的dao接口类 -->
    <mapper namespace="top.gobanana.dao.UserDao">
        <!-- id填写接口类中方法的名字;resultType规定了返回值的类型,如果返回的是许多行,那么就写元素的类型 -->
        <select id="getUserList" resultType="top.gobanana.pojo.User">
            select * from ypgtdb.SYS_USER_INFO
        </select>
    </mapper>
  ```
   * 在``test``文件夹下相同的路径编写测试类,用来验证数据库的读写是否成功
  ```JAVA
    package top.gobanana.dao;

    import org.apache.ibatis.session.SqlSession;
    import org.junit.Test;
    import top.gobanana.pojo.User;
    import top.gobanana.utils.MybatisUtils;

    import java.util.List;

    public class UserDaoTest {
        // 声明这是一个测试方法
        @Test
        public static void main(String[] args) {
            // 用MybatisUtils类中的方法获取核心对象sqlSession
            SqlSession sqlSession = MybatisUtils.GetSqlSession();
            // 利用sqlSession的getMapper方法,获取mapper对象(这里是Userdao,只是命名的区别)
            UserDao mapper = sqlSession.getMapper(UserDao.class);
            // 运行方法,返回结果,输出
            List<User> userList = mapper.getUserList();
            for (User user : userList) {
                System.out.println(user);
            }
            sqlSession.close();
        }
    }
  ```
  参考资料:[官方入门文档][1]

# 稍微进阶

* 如何在``mapper.xml``的SQL语句中使用接口类中传入的变量?    
  用``#{变量名}``代替相应位置即可(类似于模板语法)   
  当参数只有一个时,变量名可以为任意字符(不推荐这么做,可读性差)
    ```XML
    <insert id="addUser" parameterType="int">
        select * from database.USERINFO where id = #{id}
    </insert>
    ```
* 那传入参数为对象怎么办?   
  对象中的属性可以用上面的方式直接获得
    ```XML
    <insert id="addUser" parameterType="top.gobanana.pojo.User">
        insert into datebase.USERINFO (id,name,pwd) values (#{id},#{name},#{pwd})
    </insert>
    ```
* 小tips:在JAVA中调用mapper接口类中方法之后,需要提交事务才能在数据库中实现更改
  ```JAVA
    // 先提交事务
    sqlSession.commit()
    // 再释放连接
    sqlSession.close()
  ```
* 当传递的参数只有几个时，能不能不用pojo类的对象传参？   
  可以，用map对象传参即可    

* 总结一下传参的模式
  * 实体类的对象传参
  * map传参
  * 单个传参

# 配置文件详解
``mybatis-config.xml``

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：
```
configuration（配置）
    properties（属性）
    settings（设置）
    typeAliases（类型别名）
    typeHandlers（类型处理器）
    objectFactory（对象工厂）
    plugins（插件）
    environments（环境配置）
        environment（环境变量）
            transactionManager（事务管理器）
            dataSource（数据源）
    databaseIdProvider（数据库厂商标识）
    mappers（映射器）
```
## 环境配置environments

尽管可以配置**多个环境**，但每个 SqlSessionFactory 实例只能选择**一种环境**。
* 使用``default="development"``设置使用某个环境,其中``development``对应的是环境的id（比如：``id="development"``）
* 在dataSource中,设置数据源类型(UNPOOLED|POOLED|JNDI)默认的是**POOLED**
  * UNPOOLED– 这个数据源的实现会每次请求时打开和关闭连接
  * **POOLED**– 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求
  * JNDI – 比较老旧,没什么人用

## 属性properties
 
利用这个标签可以读取配置文件中的信息(也就是``***.properties``文件)    
在核心配置文件中,**第一个标签应该是属性properties**
```properties
driver=com.mysql.jdbc.Driver
url=jdbc://******************
username=root
password=123456
```

```XML
<!-- 导入配置文件即可 -->
<properties resource="org/mybatis/example/config.properties">
  <!-- 可以自行添加变量,但如果与上一行引入的外部配置文件冲突,配置文件优先 -->
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
<dataSource type="POOLED">
  <!-- 读取变量的语法就是${driver} -->
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```
## 类别名typeAliases
类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。例如：

方法一:挨个起别名
```XML
<typeAliases>
  <!-- 给你的实体类起别名,就不用每次都写全类名了 -->
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```
方法二:整个包导入
```XML
<!-- 或者,把你的实体类这个包搞进来,让它自己搜索 -->
<!-- 它自动生成的别名就是类名的首字母小写 -->
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```
方法二的补充:用注解起别名,这样用包扫描的方法时,就可以自定义别名了
```JAVA
@Alias("author")
public class Author {
    ...
}
```
参考资料:[官方配置文档][2]

[1]: https://mybatis.org/mybatis-3/zh/getting-started.html
[2]: https://mybatis.org/mybatis-3/zh/configuration.html