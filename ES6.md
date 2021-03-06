# ES6语法

## 数组的扩展

### 类数组的转换数组 Array.from()

类数组：本质特征是必有length属性

常用的类数组有：arguments，DOM返回的NODELIST集合

```js
// arguments对象
function foo() {
var args = Array.from(arguments);
// ...
}
```

### 扩展运算符 ...

1. 将空位转为undefined
2. 将某些数据结构转化为数组，比如类数组arguments
3. 主要还是用作函数的调用，将一个数组转化成参数序列
```js
console.log(...[1, 2, 3])  // 1 2 3
```
4. 数组的合并

```js
// ES5  
[1, 2].concat(more)  
// ES6  
[1, 2, ...more] 
```
5. 与解构复制结合进行数组生成,但是必须是最后一个参数要不就报错

```js
const [first, ...rest] = [1,2,3,4,5]
// first = 1
// rest = [2,3,4,5]
```
6. 把字符串转为数组

```js
[...'hello']  
// [ "h", "e", "l", "l", "o" ] 
```
7.  和对象的解构的结合

可以把对象中可遍历的，尚未读取的属性分配到指定对象上

```js
let {x,y,...z} = {x:1,y:2,a:2,b:5}
// z = {a:2,b:5}

//注意:解构必须是最后一个参数，否则会报错，右边等号的值不能是undefined和null;另外解构的拷贝是浅拷贝，只是拷贝了引用
```

扩展运算符用于取出参数对象可遍历的所有属性，并复制到新的对象上

## 函数的扩展

1. 可以给参数赋默认值，若没有传参就使默认值
2. 优点:简洁，便于阅读，便于维护和修改
3. 已经定义了的参数变量不需要再声明
4. 与解构赋值默认值结合使用

```js
function fetch(url, { body = '', method = 'GET', headers = {} })
{
console.log(method);
} f
etch('http://example.com', {})
// "GET"
fetch('http://example.com')
// 报错
```

## 对象的扩展({}包起来的就成对象了)

1. 属性的简洁表示

```js
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}
// 等同于
var baz = {foo: foo};
```

2. 方法的简洁表示

```js
var birth = '2000/01/01';
var Person = {
name: '张三',
//等同于birth: birth
birth,
// 等同于hello: function ()...
hello() { console.log('我的名字是', this.name); }
};
```
3. 模块导出的简洁表示

```js
var ms = {};
function getItem (key) {
return key in ms ? ms[key] : null;
} f
unction setItem (key, value) {
ms[key] = value;
} f
unction clear () {
ms = {};
} m
odule.exports = { getItem, setItem, clear };
// 等同于
module.exports = {
getItem: getItem,
setItem: setItem,
clear: clear
};
```
4. 可以把表达式作为键，必须写在方括号内；也可以用字符串作键

### 对象常用方法

1. Object.keys(obj) --> 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。不包含symol属性
1. Object.values(obj) --> 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。不包含symol属性，若是使用create添加，默认是不能被遍历到的，但是键名的属性描述对象enumerable改成true就可以了。若是参数是字符串，那么会返回字符组成的数组
1. Object.entries(obj)：返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。如果原对象的属性名是一个 Symbol 值，该属性会被忽略。还可以把一个对象转换为真正的MAP结构

## Proxy 

使用new Proxy(target, handler) -- > 外界对该对象的访问都必须经过这层拦截，

## Generator

Generator函数是一个状态机，封装了多个内部状态。执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态
机，还是一个遍历器对象生成函数。

### 特点

1. function关键字和函数名之间有一个星号
2. 内部使用yield语句定义不同的内部状态

```js

```
