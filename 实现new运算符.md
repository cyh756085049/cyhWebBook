### 实现new运算符

```js
/**
 * 实现 new 运算符
 * 作用：创建一个用户定义的对象类型的实例或者具有构造函数的内置对象的实例
 * 步骤：
 * 1、创建一个空的简单的js对象
 * 2、链接该对象（即该对象的构造函数）到另一个对象
 * 3、将步骤1新创建的对象作为this的上下文
 * 4、如果该函数没有返回对象，则返回this
 */
function newObject() {
    // 创建一个空的对象
    let obj = new Object();
    // 获取构造函数
    let Con = [].shift.call(arguments);
    // 链接到原型
    obj.__proto__ = Con.prototype;
    // 绑定this执行构造函数
    let result = Con.apply(obj, arguments);
    // 确保new出来的是一个对象
    return typeof result === 'object' ? result : obj;
}

/**
 * 优化的 new 实现
 * @returns {*}
 */
function newObject1() {
    // 获取构造函数，同时删除arguments 中的第一个参数
    Con = [].shift.call(arguments);
    // 创建一个空的对象并链接到原型，obj可以访问构造函数原型中的属性
    let obj = Object.create(Con.prototype);
    // 绑定this实现继承，obj可以访问到构造函数中的属性
    let ret = Con.apply(obj, arguments);
    // 优化返回构造函数返回的对象
    return ret instanceof Object ? ret : obj;
}

/**
 * 带参数的 new 实现
 * @param fn
 * @param args
 * @returns {*}
 */
function myNew(fn, ...args) {
    const obj = {};
    obj.__proto__ = fn.prototype;
    const res = fn.call(obj, ...args);
    return typeof res === 'object' ? res : obj;
}
```

