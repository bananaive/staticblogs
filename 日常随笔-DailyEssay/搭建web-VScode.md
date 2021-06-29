主要参考内容

[一个视频][0]

# 安装运行库
总共需要gcc g++ py3 make pkg-config libx11-dev libxkbfile-dev libsecret-1-dev yarn Dependencies(这个我没搞懂是什么意思)下面挑些不好安的写一下过程
## pkg-config
它的作用是在编译源码时,提供包的索引,从而不需要修改编译时的配置文件(大概是这么个意思)

多版本下载地址[官网][2](已经在2017年停止更新,最新版本0.29.2)

安装流程,参考[博文][3]
```shell
wget https://pkgconfig.freedesktop.org/releases/pkg-config-0.29.2.tar.gz
tar -zxvf ***.tar.gz
cd 解压后的文件夹
./configure
make
make check
make install 
测试
pkg-config --version
```
如果在./configure阶段报错,去安装对应的包

我这里是缺了``glib-2.0 >= 2.16``

运行命令``sudo apt-get install libglib2.0-dev``

## GCC
```shell
sudo apt install build-essential
它会安装安装一堆新包，包括gcc，g ++和make
```
## yarn
我的yarn版本安装时是1.22.5

```shell
卸载yarn
sudo apt remove cmdtest
sudo apt remove yarn
更换镜像源文件source.list源地址
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
安装yarn
sudo apt update
sudo apt install yarn
```
## nodejs
nodejs的版本需要大于14小于16(因为我装了12的,报错信息是这么讲的)
[多个版本地址][5]
```shell
wget https://nodejs.org/dist/v15.0.0/node-v15.0.0.tar.gz
解压
tar -zxvf ***.tar.gz
cd 解压后的文件夹
配置安装
./configure
make&&make install
```
# 编译并运行VScode-web

## 安装VScode
```shell
git clone https://www.github.com/ymj68520/vxcode.git
进入目录,合并官方代码,并安装
cd vscode
git checkout main
git pull https://github.com/microsoft/vscode.git main
yarn
```
## 运行vscode-web
先``yarn watch``构建客户端

再执行``yarn web``运行web版vscode

默认界面端口为8080
默认服务端口为8081

# 出现的问题
在vscode第一次``yarn``时
```shell
Package xcb was not found in the pkg-config search path.
Perhaps you should add the directory containing `xcb.pc'
to the PKG_CONFIG_PATH environment variable
Package 'xcb', required by 'x11', not found
```
参考[博客][4]

意思就是``xcb.pc``这个包找不到,那就把这个包定位出来,然后从源目录拷贝进``/usr/lib/pkgconfig/``目录下就好

我这里发现,好多缺失的包都在一个目录下,且目标``/usr/lib/pkgconfig/``目录下为空,故整体cp到目标目录,然后再yarn,就没有报错了

哦对,如果yarn的过程中,因为超时卡住,不要慌,再yarn一次

最后的 yarn watch似乎需要**很多**的内存...我服务器是4G的,构建了几次都不太行

## 如何使用

首次打开的密码是1314



[0]:https://www.bilibili.com/video/BV1LV411x7nG?from=search&seid=2044032549096715626
[1]:https://www.cnblogs.com/albizzia/p/10803032.html
[2]:https://pkgconfig.freedesktop.org/releases/
[3]:https://blog.csdn.net/sinat_23084397/article/details/84321046?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162493583116780274164083%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162493583116780274164083&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-84321046.first_rank_v2_pc_rank_v29&utm_term=ubuntu%E5%AE%89%E8%A3%85pkgconfig&spm=1018.2226.3001.4187
[4]:https://blog.csdn.net/mhsszm/article/details/82683189
[5]:https://nodejs.org/dist/