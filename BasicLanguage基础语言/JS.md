* 变量
  1. let 只在命令所在的代码块有效
  2. const 声明一个只读变量，不可修改
  3. var 可以是全局的也可以是函数内部的
     1. 函数外部的变量，在函数内部重新命名再修改，函数外的变量不会变
    tips：
    * js声明的变量取值的原则：就近原则
    * js是弱类型语言，不同的数据类型可以用同一个变量名表示
    * 函数内部声明的变量，不会影响函数外部同名的变量的值
    * 为了避免对全局的变量进行污染，要适当使用let以及闭包的手段