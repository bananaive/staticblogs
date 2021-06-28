* JAVA的字符串中的**单双引号**
  * 单引号是用来写**字符型**，而双引号时用来写**字符串**的，所以在单引号中写字符串会报错   
* JAVA类中的构造方法
  * 特点
    1. 函数名与类名相同
    2. 不用定义返回值类型
    3. 不可以写return语句
    4. 一般的函数不能调用构造函数，只有构造函数才能调用函数
    5. 一个对象建立后，构造函数仅只运行一次
    6. 当一个类中没有定义构造函数时，系统会给该类一个**默认的**、空参数的**构造函数**
    7. 可以有多个构造函数对应不同的传入参数，也就是说，构造函数有**重载**的能力
  * 示例
    ```
    package javastudy;

    public class ConfunDemo {
        public static void main(String[] args) {
            //输出Hello World。new对象一建立，就会调用对应的构造函数Confun()，并执行其中的println语句。
            Confun c1=new Confun();            
            
        }
    }
    class Confun{        
        Confun(){        
            //定义构造函数，输出Hello World
            System.out.println("Hellow World");
        }
    }
    ————————————————
    版权声明：本文为CSDN博主「打杂人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
    原文链接：https://blog.csdn.net/aerchi/article/details/90759744
    ```
* JAVA中传参的问题   
  不能用``new User(name: "zhangsan",age: 18);``传参   
  只能按顺序``new User("zhangsan",18);``传参   
* 几个互相嵌套的概念    
  容器->组件

* 更正：编辑出得文件是``.class``文件,直接``java 名字``即可


# 缺少的知识点
## 文档注释
## 数据类型
   1. 基本数据类型
      1. 整数
         1. byte占1个字节
         2. short占2个
         3. int(默认)占4个
         4. long占8个
      2. 浮点数
         1. float占4个
         2. double(默认)占8个
      3. 字符char占2个
      4. 布尔值占1位
   2. **引用数据类型**
      1. 类
      2. 接口
      3. 数组
## 类型转换
   1. 自动类型转换
      1. 短字节自动转长字节
      2. 子类自动转为父类
   2. 强制类型转换
      1. 与上面的相反
## 金融类的计算不能用double,需要用**BigDecimal**
## ``if(a)``等价于``if(a==true)``
## 在定义变量时,可以一行定义多个值
## 变量作用域
   1. 类变量``static 变量类型 变量名;``
   2. 实例变量``变量类型 变量名;``
   3. 局部变量/成员变量(也就是方法内的变量)
## 定义一个常量``final 名字全大写 = 值;``
## 运算符
   1.  算术运算符
   2.  赋值运算符
   3.  关系运算符``instanceof``
   4.  逻辑运算符``&&与 ||或 !非``
   5.  位运算符``&与 |或 !非 ^异或 ~取反 >> << >>>三个都是位运算``
   6.  **条件运算符**``三元运算符? :``
   7.  扩展运算符``*= /=``
## 包的命名,是公司域名的倒写,包的声明应该在第一行
## javadoc   java文档常用的注解
## switch语句的**case穿透现象**,以及支持String类型
## 带标签的continue如何实现
## 方法的重载-名字相同参数不相同
## 可变长参数如何实现必须是最后一个参数``...``
## 对象都默认有一个无参的构造方法,但是如果手动添加了一个有参的构造方法,需要同时添加无参的构造方法
## 单例模式---构造器私有
## 对象的创建(栈存放引用,堆存放具体的对象)
## 多态-父类的引用指向子类的对象``Person person = new Student()``
## instanceof关键字,如果匹配,可以进行类型之间的转换
## 修饰符
    1.  public
    2.  protected
    3.  private
    4.  static
    5.  static
    6.  abstract
    7.  final
## 抽象类
## 接口``interface``是一种约束,
    1.  只能定义方法名
    2.  子类实现接口必须重写其中的方法
    3.  只有一个方法的接口叫函数式接口,可以使用lambda简化
    4.  与抽象类的区别就是,接口比抽象类更抽象
## JAVA是单继承,**但是一个类可以实现多个接口??**
## 内部类
    1.  局部内部类
    2.  静态内部类
    3.  匿名内部类(重点)-lambda
## 异常之间的关系
    1.  throwable
        1.  exception运行/检查时产生的异常
            1.  运行时出现的异常
                1.  1/0
                2.  classnotfound
                3.  空指针异常
                4.  未知类型
                5.  下标越界异常
                6.  等
            2.  检查时出现的异常
        2.  error
            1.  界面渲染错误/AWT异常
            2.  虚拟机异常/JVM异常
                1.  栈溢出异常StackOverFlow
                2.  java内存移除OutOfMemory
    2.  处理异常的关键字
        1.  try()
        2.  catch(){}先抓小异常再抓大异常
        3.  finally()
        4.  throw内部手动抛出异常
        5.  throws方法抛出异常
    3.  自定义异常,继承Exception类即可
## 常用类
    1.  Object类
        1.  hashcode()
        2.  toString()
        3.  clone()
        4.  getClass()
        5.  notify()
        6.  wait()
        7.  equals()
    2.  Math类-常见的数学运算
    3.  Random类-生成随机数-UUID
    4.  File类
        1.  创建文件
        2.  查看文件
        3.  修改文件
        4.  删除文件
    5.  包装类-**自动装箱拆箱**
    6.  Date类-时间日期类
        1.  Date
        2.  日期转换SimeDateFormat
        3.  Calendar
    7.  String类-不可变性final-**操作量较少**用这个
    8.  StringBuffer-可变长-append()-**多线程数据量较大要用这个**
        1.  效率低但是安全
    9.  StringBuilder-可变长-**单线程数据量较大**,字符串缓冲区大,用这个
        1.  效率高但是不安全
## 迭代器lterator
## 集合框架
1. collection
     1.  list-**有序可重复**
         1.  arraylist数组构建的
             1.  add添加
             2.  remove删除
             3.  contains
             4.  size大小
         2.  linkedlist链表
             1.  getFirst()获取头节点
             2.  getlast()获取尾节点
             3.  removeFirst()
             4.  addFirst()
         3.  Vector
         4.  Stack栈
    1.  set-**无序不可重复**
         1.  hashset
         2.  treeset
  1.  map
      1.  hashmap-底层
          1.  存储逻辑-按照规则生成地址然后存入元素-如果地址冲突,在此地址后面生成一个链表
          2.  数据结构
              1.  在JDK1.7之前,是数组加链表
              2.  在JDK1.8之后,数组+链表+红黑树
                  1.  **为什么要加红黑树?**   
                  如果按照1.7的方式用链表的方式解决地址冲突问题,那么在链表长度超过8的时候,以(红黑树平衡二叉树)的方式存储冲突的内容,会更有效率
                  2. **为什么hash表的长度必须是2的幂?**
                  因为在源码中,为了将hash表中的数据均匀分配给链表/红黑树,需要进行取模,而最快的取模运算就是位运算hash&(length-1)
          3. 加载因子 
      2.  treemap
  2.  Collections工具类
  3.  **泛型<>约束??**避免类型转换之间的问题
## IO流
数据源(input)-->处理流(把二进制转换为程序可读的类型)-->输出(程序接收数据)
IO流分类
1. 字节流
   1. 输入InputStream
   2. 输出OutputStream
2. 字符流
   1. 输出Writer
   2. 输入Reader   
#### 到此为止是常用的四个,以下的几个是在特殊场景中,经过封装然后使用 
3. 处理流
   1. buffer(bufferInputStream/bufferOutputStream/bufferWriter/bufferReader)
   2. data(dataInputStream/dataOutputStream)
   3. 转换流(InputstreamReader/OutputStreamWriter)
   4. Filter(跟buffer一样是四个)
   5. print(Writer/PrintStream)
   6. object流(四个)
   7. 序列化与反序列化(Serializable)
      1. 如果某个变量在序列化过程中不需要序列化,那么可以用关键字**transient**修饰
4. 节点流
   1. (CharArrayReader/Writer/inputstream/outputstream)
   2. StringReader/Writer
   3. pipe(管道流)PipedOutStream
## 多线程
### 进程和线程
### 单线程run/多线程start
### 线程创建的方法
1. Tread   
   其实现是由**start0**办到的,这是本地方法,JAVA**无权**调用,需要通过底层的C语言进行处理
2. Runnable   
   函数式接口,可以用lambda简化(也就是**箭头函数**)
3. Callable   
   可以有返回值
4. 静态代理   
   new Thread(Runnable).start();
5. lambda表达式   
   属于函数式编程,**为了避免内部类过多,降低代码的可读性**
6. 线程的状态
   1. 新建
   2. 就绪
   3. 运行
   4. 阻塞
   5. 死亡
7. 常用的方法
   1. sleep
   2. join
   3. yield礼让
   4. isLive存活
   5. start
   6. 优先级setPriority
   7. interrupt中断
   8. 不建议使用stop
8. 线程同步(锁与排队)
   1. 并发
   2. Synchronized
      1. 同步方法,将方法整个锁住,会有许多不必要的资源被上锁(优先级低)
      2. **同步代码块**(优先级中)
      3. 出现死锁的条件(只要缺一个条件就不会死锁)
         1. 互斥
         2. 请求与保持
         3. 不剥夺
         4. 循环等待
   3. Lock(有限级高)
      1. ReentrantLock
         1. lock
         2. trylock
         3. unlock
9. 线程通信
   1.  缓冲区(消息队列)
   2.  标志位(就是红绿灯)
   3.  wait()
   4.  notifyAll()唤醒全部
10. 线程池
    1.  池化技术
    2.  池的大小
    3.  最大连接数
    4.  保持时间
## 网络编程
## GUI
## 注解与反射
### 注解
1. 元注解
2. 内置注解
3. 自定义注解
4. 反射读取注解
### 反射
1. class类
2. 类加载机制
3. Mrthod
4. Field
5. Construct
6. **破环私有关键字**
7. 性能分析(正常>检测关闭的反射>默认的反射)
8. 反射获得注解,泛型...