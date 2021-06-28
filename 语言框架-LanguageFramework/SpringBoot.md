## 一个思想---好莱坞模式  
  **("不要给我们打电话，我们会给你打电话(don‘t call us, we‘ll call you)")**


  好莱坞原则强调**高层对低层的主动作用**   
  即低层只需要管好自己的工作----**具体实现**   
  而高层自有它自己的工作----**这就是管理低层的逻辑，或者说从client到具体实现的一系列中间逻辑**     
  在不需要用到某个低层的时候，高层并不会调用到这个具体低层，低层永远不需要向高层作出表示，说它需要被调用   
  ----即在所有的处于使用者与现有代码的中间的，用于隔离和解偶二者的那些中间逻辑中，低层逻辑永远不要涉入高层的实现，而只要高层通过某个逻辑去涉入低层的实现，也即**低层不要去调用高层，只有高层才会去调用低层**，这才是合理的，我们应**尽量避免向上调用和相互调用**
## JAVA编程+好莱坞模式=**IoC**
  * IoC是什么

    Ioc—Inversion of Control，即“控制反转”，不是什么技术，而是一种设计思想。在Java开发中，**Ioc意味着将你设计好的对象交给容器控制**，而不是传统的在你的对象内部直接控制。如何理解好Ioc呢？理解好Ioc的关键是要明确“谁控制谁，控制什么，为何是反转（有反转就应该有正转了），哪些方面反转了”，那我们来深入分析一下：


    **谁控制谁，控制什么**：传统Java SE程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由Ioc容器来控制对象的创建；谁控制谁？当然**是IoC 容器控制了对象**；控制什么？那就是**主要控制了外部资源获取**（不只是对象包括比如文件等）。

    **为何是反转，哪些方面反转了**：有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象；为何是反转？因为由容器帮我们查找及注入依赖对象，**对象只是被动的接受依赖对象**，所以是反转；哪些方面反转了？依赖对象的获取被反转了。
  * IoC能做什么

    IoC不是一种技术，只是一种思想，一个重要的面向对象编程的法则，它能指导我们如何设计出**松耦合**、更优良的程序。传统应用程序都是由我们在类内部主动创建依赖对象，从而导致类与类之间高耦合，难于测试；有了IoC容器后，**把创建和查找依赖对象的控制权交给了容器**，由容器进行注入组合对象，所以对象与对象之间是松散耦合，这样也方便测试，利于功能复用，更重要的是使得程序的整个体系结构变得非常灵活。

    其实IoC对编程带来的最大改变不是从代码上，而是从思想上，发生了“**主从换位**”的变化。应用程序原本是老大，要获取什么资源都是主动出击，但是在IoC/DI思想中，应用程序就变成**被动**的了，被动的等待IoC容器来创建并注入它所需要的资源了。

    IoC很好的体现了面向对象设计法则之一—— 好莱坞法则：“别找我们，我们找你”；即由IoC容器帮对象找相应的依赖对象并注入，而不是由对象主动去找。
  * IoC的方法之一---**DI**

    DI—Dependency Injection，即*“**依赖注入**”*：是**组件之间依赖关系由容器在运行期决定**，形象的说，即由容器动态得将某个依赖关系注入到组件之中。依赖注入的目的并非为软件系统带来更多功能，而是为了**提升组件重用的频率**，并为系统搭建一个灵活、可扩展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。

    理解DI的关键是：“谁依赖谁，为什么需要依赖，谁注入谁，注入了什么”，那我们来深入分析一下：

    * **谁依赖于谁**：当然是应用程序依赖于IoC容器；

    * **为什么需要依赖**：应用程序需要IoC容器来提供对象需要的外部资源；

    * **谁注入谁**：很明显是IoC容器注入应用程序某个对象，应用程序依赖的对象；

    * **注入了什么**：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）。

    IoC和DI由什么关系呢？其实它们是**同一个概念的不同角度描述**，由于控制反转概念比较含糊（可能只是理解为容器控制对象这一个层面，很难让人想到谁来维护对象关系），所以2004年大师级人物Martin Fowler又给出了一个新的名字：“依赖注入”，相对IoC 而言，“依赖注入”明确描述了“被注入对象依赖IoC容器配置依赖对象”。
    
    ### Spring IoC容器的**依赖注入工作**
    1. 阶段一----收集和注册    
        第一个阶段可以认为是**构建和收集 bean 定义**的阶段，在这个阶段中，我们可以通过 XML 或者 Java 代码的方式定义一些 bean，然后通过手动组装或者让容器基于某些机制自动扫描的形式，将这些 bean 定义收集到 IoC 容器中。
    2. 阶段二----分析和组装   
        当第一阶段工作完成后，我们可以先暂且认为 IoC 容器中充斥着一个个**独立的 bean**，它们之间没有任何关系。

        但实际上，它们之间是有依赖关系的，所以，IoC 容器在第二阶段要干的事情就是**分析**这些已经在 IoC 容器之中的 bean，然后根据它们之间的依赖关系先后**组装**它们。

        如果 IoC 容器发现某个 bean 依赖另一个 bean，它就会将这**另一个 bean 注入给依赖它的那个 bean**，直到所有 bean 的依赖都注入完成，所有 bean 都“**整装待发**”，整个 IoC 容器的工作即算完成。

        至于分析和组装的依据，Spring 框架最早是通过 XML 配置文件的形式来描述 bean 与 bean 之间的关系的，随着 Java 业界研发技术和理念的转变，基于 Java 代码和 Annotation **元信息**(PS:在springboot2中彻底使用注解完成大部分的书写)的描述方式也日渐兴盛，但不管使用哪种方式，都只是为了简化绑定逻辑描述的各种“表象”，最终都是为本阶段的最终目的服务。

        在springboot2中XML形式的组件依赖描述被取代   
        *很多 Java 开发者一定认为 Spring 的 XML 配置文件是一种配置（Configuration），但本质上，这些配置文件更应该是一种代码形式，XML 在这里其实可以看作一种 DSL，它用来表述的是 bean 与 bean 之间的依赖绑定关系，如果没有 IoC 容器就要自己写代码新建（new）对象并配置（set）依赖。*
  * IoC的方法之二---DL
    DL-Dependency Lookup，即“**依赖查找**”，是**当前软件实体主动去某个服务注册地查找其依赖的那些服务**
## OOP面向对象编程的**延续**----AOP面向切面编程
  * 先上听不懂的：可以通过预编译方式和运行其动态代理实现在**不修改源代码**的情况下给程序动态统一添加功能的一种技术   
    然后是大白话：像性能统计这种功能，跟业务逻辑无关，所以最好是**不修改源代码**就能统一添加功能咯，另外要是能动态调整就更好了
  * 主要意图：将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。
  * 为什么要用AOP，OOP的接口不行么？
    AOP和定义良好的 OOP 的接口可以说都是用来解决并且实现需求中的横切问题的方法。但是对于 OOP 中的接口来说，它仍然需要我们在相应的模块中去**调用该接口中相关的方法**，这是 OOP 所无法避免的，并且一旦接口不得不进行修改的时候，所有事情会变得一团糟；AOP 则不会这样，你只需要修改相应的 Aspect，再重新编织（weave）即可。 当然，AOP 也绝对不会代替 OOP。核心的需求仍然会由 OOP 来加以实现，而 AOP 将会和 OOP 整合起来，以此之长，补彼之短。
* 文件目录  
```
src
├─main
│  ├─java
│  │  └─com
│  │      └─example
│  │          └─demo
│  └─resources
│      ├─static
│      └─templates
└─test
    └─java
        └─com
            └─example
                └─demo
```
* 程序的入口``@SpringBootApplication``   
  将这个类中的main方法以固定写法写出即可
* 
* 组件之间的依赖    
  * 在组件A中用到了组件B,即组件A依赖组件B
* @Configuration(proxyBeanMethods = true)   
  * 作用：
    由于所有的bean生成的对象都由IoC(**容器**)管理，所以**所有的bean**，都会在IoC内部生成一个**代理对象**     
    而如果想要两个对象A与B在IoC眼中出现依赖关系，可以让A对象在内部**使用B对象**   
    如果标题的注解中，值为**false**，那么A类对应的IoC中的代理对象同样会调用B类，只是调用出来的B类的对象是生成出的**新的B对象**，不是IoC中的代理对象，故依赖关系**不成立**    
    如果标题的注解中，值为**true**，那么当A类对应的IoC中的代理对象使用B类时，IoC总会检查所有代理对象中有没有B类对应的那个，也就是保证B类为**单例状态**，所以依赖关系才**成立**
  * 如何选择?
    1. 如果不需要依赖关系,那就调``false``,也就是**Lite**模式,这样不需要容器检查并保持单例,启动会比较快
    2. 如果需要有依赖关系,那就调``true``,也就是**Full**模式,这样能保证它依赖的就是容器中的组件(代理对象)
## 注册组件的方式(注解标注在一个类上就代表某一类的组件)
  1. @Bean
     1. 自定义组件名字@Bean("组件名")
  2. @Component--组件
  3. @Controller--控制器
  4. @Service--业务逻辑组件
  5. @Repository--数据库层组件
  6. @ComponentScan--配置包扫描
  7. @Import--导入组件
     1. 使用位置:标记了组件的类的头顶
     2. 例子: ``@Import({User.class, DBHelper.class})``
     3. 调用所选类的有参无参构造器,给容器中自动创建出这两个类型的组件,默认组件的名字就是全类名
  8. @Conditional--条件装配
     1. 只有满足某种**条件**之后才能执行组件注入,有许多子注解,用来表示众多的逻辑条件
     2. 例子``@ConditionalOnBean(name="tom")``只有容器中有tom这个组件的时候才会注册这个注解修饰的类
  9. @ImportResoutce()--导入原生XML类型定义的组件
     1.  当项目中还有用XML写的组件时,可以用这个去导入
     2.  例子``@ImportResource("classpath:beans.xml")``导入在``resources``文件夹下的``beans.xml``文件

## 配置绑定   
使用java读取到``properties``文件中的内容并将其封装到``javaBean``中,以供使用



1. ``@Component`` + ``@ConfigurationProperties``--**bean中绑定->controller读取**
   1. **组件定义**:使用``@Component``注册组件,并在这个类中定义与配置文件中相同的**类变量**    
    然后用``@ConfigurationProperties(perfix = "名称")``与配置文件中的字段进行绑定
   2. **数据的取出**:自定义一个Controller   
    import上文中定义的类   
    使用自动注入注解``@Autowired``定义一个类变量
   3. 详细例子
    ```XML
    src/main/resources/application.properties

    // 数据源头
    mycar.brand=BYD
    mycar.price=100000
    ```
    ```JAVA
    src\main\java\com\myhelloworld\boot\bean\Car.java

    import org.springframework.boot.context.properties.ConfigurationProperties;
    import org.springframework.stereotype.Component;

    // 数据绑定
    // 只有在容器中得组件才能使用数据绑定
    @Component
    @ConfigurationProperties(prefix = "mycar")
    public class Car {
        private String brand;
        private Integer price;

        public String getBrand(){return brand;}

        public void setBrand(String brand) {this.brand = brand;}

        public Integer getPrice(){return price;}

        public void setPrice(Integer price){this.price = price;}

        @Override
        public String toString(){
            return "Car{"+"brand='"+brand+'\''+",price="+price+'}';
        }
    }
    ```
    ```JAVA
    src/main/java/com/myhelloworld/boot/controller/HelloController.java

    package com.myhelloworld.boot.controller;
    import com.myhelloworld.boot.bean.Car;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HelloController {

        // 数据读取
        @Autowired
        Car car;

        @RequestMapping("/car")
        public Car car(){
            return  car;
        }

        @RequestMapping("/hello")
        public String handle01(){
            return "你好";
        }
    }
    ```
  2. ``@EnableConfigurationProperties``+``ConfigurationProperties``    
   bean中绑定配置但不注册进容器(**即没有``@Component``注解**)-**config辅助绑定**-controller正常读取

     1. **适用情况**:使用第三方包的时候,无法给它的组件加上``@Component``
     2. config部分:``@EnableConfigurationProperties(类名.class)``

## 自动配置原理
1. SpringBoot先加载所有的自动配置类   
2. 每个自动配置类按照条件来生效,并默认绑定配置文件指定的值
3. 生效的配置类就会给容器中装配很多组件   
4. 只要容器中有这些组件,相当于这些功能就有了   
5. 只要用户有自己配置的,那就以用户的优先   
6. 定制化配置
  * 用户自己直接@Bean替换底层的文件
  * 用户去看这个组件获取的配置文件什么值就去修改

## SpringBoot怎么开发
1. 引入场景依赖
2. 查看自动配置了那些
   * 自己分析引入场景对应的自动配置一般都是生效的
   * 在``application.properties``中``debug=true``,会开启自动配置报告
3. 是否需要修改
   * 参照文档修改配置项
     * 参考配置文档https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
     * 自己分析到xxxxproperties绑定了哪些
   * 自定义加入或者替换组件
     * @Bean,@Component以替换组件

## controller/service/mapper/model四层结构
### model层

  **model层**即数据库实体层，也被称为entity层，pojo层。

  一般数据库一张表对应一个实体类，类属性同表字段一一对应。

### dao层

  dao层即数据持久层，也被称为**mapper层**。

  dao层的作用为访问数据库，向数据库发送sql语句，完成数据的增删改查任务。

### service层
  **service层**即业务逻辑层。

  service层的作用为完成功能设计。

  service层调用dao层接口，接收dao层返回的数据，完成项目的基本功能设计。

### controller层

  **controller层**即控制层。

  controller层的功能为请求和响应控制。

  controller层负责前后端交互，接受前端请求，调用service层，接收service层返回的数据，最后返回具体的页面和数据到客户端。

作者：斋藤飞鸽
链接：https://www.jianshu.com/p/dcaaa8b9d47f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。