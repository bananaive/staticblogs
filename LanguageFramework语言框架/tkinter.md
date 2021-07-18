# tkinter简介
一个python自带的GUI图形库

优点:教程多易上手,适合写一些小工具   
缺点:功能可能相对薄弱

# 基本使用
1. 创建窗口实例
2. 创建控件(也就是那些菜单元素)
3. 指定这些空间的master
4. 进入消息循环
```python
# Python3.x 导入方法
from tkinter import * 
root = Tk()                     # 创建窗口对象的背景色
                                # 创建两个列表
li     = ['C','python','php','html','SQL','java']
movie  = ['CSS','jQuery','Bootstrap']
listb  = Listbox(root)          #  创建两个列表组件
listb2 = Listbox(root)
for item in li:                 # 第一个小部件插入数据
    listb.insert(0,item)
 
for item in movie:              # 第二个小部件插入数据
    listb2.insert(0,item)
 
listb.pack()                    # 将小部件放置到主窗口中
listb2.pack()
root.mainloop()                 # 进入消息循环
```