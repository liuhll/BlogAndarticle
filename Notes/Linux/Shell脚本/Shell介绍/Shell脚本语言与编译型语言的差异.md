# Shell脚本语言与编译型语言的差异

- 程序设计语言可以分为**两类**：
   1. 编译型语言
   2. 解释型语言


## 编译型语言
- 需要一个`编译过程`
- 这类语言需要预先将我们写好的`源代码`(`source code`)转换成`目标代码`(`object code`)
- 运行程序时，直接读取`目标代码`(`object code`)
- 优点 
  - 编译后的`目标代码`(`object code`)非常接近计算机底层，因此**执行效率很高**
- 缺点 
  - 由于编译型语言多半运作于底层，所处理的是字节、整数、浮点数或是其他机器层级的对象，往往**实现一个简单的功能需要大量复杂的代码**

## 解释型语言
- `解释型语言`也被称作`脚本语言`

- 执行这类程序时，`解释器`(`interpreter`)需要读取我们编写的`源代码`(`source code`)，并将其转换成`目标代码`(`object code`)，再由计算机运行 

- 缺点 
  -  每次执行程序都多了`编译`的过程，因此**效率有所下降**，它们的效率通常**不如**`编译型语言`

- 优点
  - 多半**运行在比编译型语言还高的层级**，能够*轻易处理文件与目录之类的对象*

- 通常使用脚本编程还是值得的
   - 脚本执行的速度已经**够快**了，快到足以让人*忽略它性能上的问题*
   