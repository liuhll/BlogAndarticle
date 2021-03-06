# Framework 设计准则
- [dotNet设计原则](https://msdn.microsoft.com/zh-cn/library/ms229042(v=vs.110).aspx)

## 命名准则


## 类型设计准则

## 成员设计准则


## 扩展性设计
- 可扩展性机制 — `子类化`、 `事件`、 `虚拟成员`、`回调`等等

### 未密封的类
`密封的类`**不能**被继承，并会**阻止**可扩展性。 与此相反，可以从继承的类称为`未密封的类`

### 受保护的成员
`受保护的成员`本身**不提供任何可扩展性**，
但这些用户可**通过子类化可扩展性功能更强大**。 它们可以用于公开高级自定义选项，而不必要地复杂化的主公共接口  
任何人都可以继承类，以及访问受保护的成员。
- 使用受保护的高级自定义的成员
- 处理为在为`进行安全`、 `文档`和`兼容性分析`的公共未密封类**为受保护的成员**

### 事件和回调
`回调`将扩展点，允许一个框架，用于回调到用户代码中通过一个`委托`。 通常，这些委托将传递到框架通过一种方法的参数

- ✓ 请考虑 使用回调以允许用户提供自定义代码要执行的框架。
- ✓ 请考虑 使用事件以允许用户自定义一个框架，而无需了解的面向对象设计的行为。
- ✓ 执行 事件想要优先选择普通回调，因为它们是更广泛的开发人员更熟悉，与 Visual Studio 语句完成功能集成。
- X 避免 性能敏感的 Api 中使用回调。
- ✓ 执行 使用新 `Func<...>`, ，`Action<...>`, ，或 `Expression<...>` 而**不是在回调中定义的 `自定义Api`** 时的自定义委托的类型。
`Func<...>` 和 `Action<...>` 表示**泛型委托**。 `Expression<...>` 表示**函数定义**，可以编译并随后在运行时调用但可还被序列化并传递到远程进程。
- ✓ 执行 测量和了解使用性能影响的 `Expression<...>`, ，而不是使用 `Func<...>`和 `Action<...>` 委托。
   - `Expression<...>`类型是在逻辑上等同于大多数情况下 `Func<...>` 和 `Action<...>` 委托。 主要区别在于，委托将用于本地进程内方案;表达式适用于的情况下十分有用和可能在远程进程或计算机中的表达式进行求值。
- ✓ 执行 了解，通过调用一个委托，正在执行任意代码，并可能具有安全、 正确性和兼容性的负面影响。

### 虚成员


### 抽象 （抽象类型和接口）

### 用于实现抽象基类


### 如果要密封





## 异常设计准则

## 使用准则

## 常见设计模式