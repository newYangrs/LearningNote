## attribute用法

- __attribute__可以设置函数属性(Function Attribute)、变量属性(Variable Attribute)和类型属性(Type Attribute)。

- __attribute__前后都有两个下划线，并且后面会紧跟一对原括弧，括弧里面是相应的__attribute__参数
  __attribute__语法格式为：attribute ( ( attribute-list ) )

- 如果函数被设定为**constructor属性**，**则该函数会在main（）函数执行之前被自动的执行**；若函数被设定为**destructor属性**，**则该函数会在main（）函数执行之后或者exit（）被调用后被自动的执行。**

