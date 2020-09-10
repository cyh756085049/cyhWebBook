### `for-of`和`for-in`的区别

#### 1、`for-of`

遍历可迭代的对象（部署了`Symbol.iterator`），包括 `Array`，`Map`，`Set`，`String`，`arguments`等，遍历的是每个数组的值。

示例代码：

```js
  const arr = [1, 2, 'a'];
	for (let value of arr) {
    console.log(value);
  }
	// 输出： 1 2 a
```

#### 2、`for-in`

遍历一个对象的可枚举属性

> - 不可遍历Symbol属性
> - 可以遍历到原型上的属性
> - 顺序是任意的,不固定的

示例代码：

```js
    const data = {
        [Symbol.for('a')]: 'a',
        name: 'name',
        '2': '2',
        '1': '1',
        b: 'b',
        a: 'a'
    }
    data.__proto__.sex = 'sex'
    for (const k in data) {
        console.log(k) // 1 2 name b a sex
    }
```

#### 3、总结

##### for-of

- 通常用于遍历`数组对象`,不可遍历`普通对象`
- 只能遍历可迭代的对象（部署了`Symbol.iterator`

##### for-in

- 不可遍历Symbol属性
- 为普通对象设计的，遍历顺序是任意的,数组索引顺序很重要,不适用于数组遍历
- 遍历出来的key是string类型
- 可以遍历到原型上的属性

