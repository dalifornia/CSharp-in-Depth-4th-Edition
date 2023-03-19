# default运算符，typeof运算符，可空值类型

Sun Mar 19 2023

### defualt运算符

* default(x) ，x为类型名或类型形参，返回该类型的默认值
  * 什么是默认值？
    * 非可空值类型返回其“0值”，例如0, 0.0, false等等
    * 可空值类型返回该类型的null值

### typeof运算符

* typeof(T), 返回一个封闭的、已构造的类型
* 特殊的是例如要获取`Dictionary<T1,T2>`的类型，要使用`typeof(Dictionary<,>)`

### 可空值类型

* Why using `null`
  * 表示某个变量为空
  * 表示某个变量尚未赋值
  * 虽然上述两个用途都可以用bool变量或某个特殊值来实现，但明显使用`null`来表示空值是更优雅的做法
* ps：Tony hoare在1965年的algol语言中首次引进null引用的概念，此后无数开发人员饱受NullReferenceException的折磨
* `Nullable<T>`
  * `Nullable<T> where T：struct`
    * T被称为基础类型，例如`Nullable<int>`此时基础类型是int
    * 本质上是用一个bool变量`HasValue`在内部表示自己是否被赋值，Value属性表示该对象的值
  * 可调用的方法：
    * `GetValueOrDefault()` 返回结构体中的值，若`HasValue == false`，则返回default值
    * `GetValueOrDefault(T defaultValue)` 返回结构体中的值，若`HasValue == false`，则返回defaultValue
    * 重写了`Equals()`，`GetHashCode()`，当两个可空值类型的对象比较时，先比较HasValue，只有两个对象的HasValue均为true时再比较Value属性是否相等。
  * 语言层面的支持：
    * 如何声明一个可空值类型对象？
      * `Nullable<int> a = null`
      * `？`后缀: `int? a = new int?() ` 
    * 允许将`null`对象或一个可空值类型对象转换为另一个可空值类型对象，但不允许从可空值类型对象转换到非可空值类型对象
  * 提升运算符
    * 空合并运算符 `??` 
      * `first ?? second` 先计算first表达式，若first不为null，则整个表达式结果等于first的计算结果
      * 若first计算为null，则接着计算second，整个表达式的结果等于second的计算结果
      * 需要注意的是如果first或second只要有一个为非空值类型，最终first ?? second返回的结果就是非空值类型
