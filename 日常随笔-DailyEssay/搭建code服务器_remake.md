# 本文目录

1. 应用场景
2. 实现方式及利弊

## 应用场景
通过搭建vscode服务器，将代码放置在服务器，通过web浏览器进行编辑、调试。你可以使用一个平板，或者一个笔记本，就能可以进行编程

你可以不用带着沉重的笔记本电源适配器，甚至只带一个平板，就可以随时随地进行编程

但是不要报太高期望,markdown**无法预览**,**无法debug**,只能平时写一些算法练习

鸡肋,鸡肋,食之无味,弃之可惜
## 实现方式及利弊
1. **源码运行**.下载vscode源代码，``yarn``编译之后,通过``yarn web``启动.缺点是,不能身份验证,除非你只在服务器的局域网范围内使用web服务,否则你的服务器就开放给公网了,因为vscode中可以打开终端,就相当于通过ssh连进服务器了.
2. **基于vscode的code-server**.使用一个开源的项目,它将vscode针对web服务进行一些优化(主要是账号登陆和插件市场两部分),开箱即用

## 理想的安装流程

**vscode版**

1. ``git clone https://github.com/microsoft/vscode.git``下载源代码
2. ``yarn``编译
3. ``yarn web``运行web应用
4. ``http://127.0.0.1:8080``打开浏览器进入

可能出现的问题

1. 插件市场无法使用

    在多次尝试之后仍不成功

1. 没有安装``nodejs`` 以ubuntu为例
    来源[风起时博客][1]
    ```shell
    # 在ubuntu中添加一个nodejs源
    curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
    # 可选操作:使用清华源进行安装
    vim /etc/apt/sources.list.d/nodesource.list
    # 将https://deb.nodesource.com/node/替换为https://mirrors.tuna.tsinghua.edu.cn/nodesource/deb/
    # 或者将https://deb.nodesource.com/node_16.x/改为https://mirrors.tuna.tsinghua.edu.cn/nodesource/deb_16.x
    sudo apt update
    sudo apt install nodejs
    ```
2. ``yarn``时提示版本需要为最新版
    来自[漂洋过海的油条的博客][0]
    ```shell
    # 移除cmdtest,yarn
    sudo apt remove cmdtest
    sudo apt remove yarn

    # 更新ubuntu中的yarn源
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

    # 更新源并安装
    sudo apt update
    sudo apt install yarn
    ```
3. ``yarn``时报错,如下
    ```
    Package xcb was not found in the pkg-config search path.
    Perhaps you should add the directory containing `xcb.pc'
    to the PKG_CONFIG_PATH environment variable
    Package 'xcb', required by 'x11', not found
    ```
    参考[南有木兮木不知][3]

    意思就是``xcb.pc``这个包找不到,那就把这个包定位出来,然后从源目录拷贝进``/usr/lib/pkgconfig/``目录下就好
4. ``yarn``时,占用的内存越来越大,最后卡死失败
    解决办法：虚拟内存

    参考这个[盗草屋][6]

    大概意思就是创建一个指定大小的文件,用来代替内存进行读写
    ```shell
    mkdir swap
    cd swap
    sudo dd if=/dev/zero of=swapfile bs=40960 count=100000
    # 把生成的文件(4个G)转换成 Swap 文件
    sudo mkswap swapfile
    # 激活 Swap 文件。
    sudo swapon swapfile

    ```
    然后查看``free -m``swap那一行对应的就是虚拟内存大小以及使用情况

    如果需要卸载这个 swap 文件，可以进入建立的 swap 文件目录。执行下列命令。

    ``sudo swapoff swapfile``

**code-server**

1. ``uname -a``查看本机的架构
2. ``https://gitee.com/mirrors/code-server/blob/master/docs/install.md``进入官方帮助文档,找到适合自己的安装方式

可能出现的问题

1. markdown语法无法预览
    鬼知道为啥,气死了,甚至后台没报错

2. docker安装并运行后,无法安装插件且一旦容器重启就无法正常使用
    个人推测为docker持久化存储有问题,重启之后部分重要内容被重置掉了

1. 打开的工作文件夹中文件过多时,后台报错``监听文件限制``
    来自[花红叶绿莫笑秋][2]
    ```shell
    # 修改系统文件
    sudo vim /etc/sysctl.conf
    # 在最下面一行添加fs.inotify.max_user_watches=524288
    # 刷新配置文件
    sudo sysctl -p
    ```
2. linux的架构是非主流的
    比如``arrch``(PS:没错我的云主机就是这个)
    官网讲,用``yarn``安装,官方文档也有写

[0]:https://blog.csdn.net/weixin_40533355/article/details/81135112
[1]:https://www.cnblogs.com/forheart/p/13203249.html
[2]:https://blog.csdn.net/weixin_43760383/article/details/84326032?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.base&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.base
[3]:https://blog.csdn.net/mhsszm/article/details/82683189
[6]:https://www.cnblogs.com/daocaowu/p/3410472.html