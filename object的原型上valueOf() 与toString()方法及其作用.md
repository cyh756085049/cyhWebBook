### object的原型上valueOf()与toString()方法及其作用

各种引用对象都继承或最终继承于Object，使用着Object的原型，所以它们不管何时都有 toString() 和 valueOf() 方法，valueOf和toString是Object.prototype的方法。一般很少直接调用，但是在使用对象参与运算的时候就会调用这两个方法了。只不过有些类型的原型重写了这两个方法，比如 Function 实例的原型就重写了 toString() 方法，按照原型链的规则，如果方法和属性在原型链的各原型中有重名，则优先使用最近的方法和属性。

- valueOf: 返回对象的原始值表示
- toString: 返回对象的字符串表示



###### 常见的引用类型：

- Function 重写了 toString()
- Date 重写了 toString() 也重写了 valueOf()
- Array 重写了 toString()

> https://blog.csdn.net/weixin_42476799/article/details/89330017



### == 和 ===的区别，隐式的类型转换

===要求数据类型相同， ==会进行隐式类型转换

