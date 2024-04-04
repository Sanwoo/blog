---
title: es6
date: 2024-03-15 21:03:14
tags:
---

# let const

# 解构赋值

> ```js
> const num = [1, 2, 3, 4]
> const [a, b, c, d] = num
> console.log(a); // 1
> console.log(b); // 2
> console.log(c); // 3
> console.log(d); // 4
> ```
>
> ```js
> const zhao = {
>     name: 'zhaobenshan',
>     age: 'dont know',
>     xiaopin: () => {
>         console.log('表演一个小品');
>     }
> }
> const {name, age, xiaopin} = zhao
> console.log(name); // zhaobenshan
> console.log(age); // dont know
> xiaopin() // 表演一个小品
> ```

# 模板字符串``

## 可以声明字符串
> ```js
> const a = `i am`
> console.log(a); // i am
> ```

## 可以出现换行符
> 如果是''和""这样写则会报错
>
> ```js
> const names = `<ul>
>                 <li>夏竹</li>
>                 <li>王多鱼</li>
>                 </ul>`
> ```

## 字符串拼接
> ```js
> const a = 'jay'
> const b = `我喜欢听${a}的歌`
> console.log(b); //我喜欢听jay的歌
> ```

# 对象里面写函数作为对象属性，省略: function

> ```js
> const a = {
>     todo: function() {
>         console.log('todo');
>     }
> }
> 
> //es6写法如下
> 
> const a = {
>     todo() {
>         console.log('todo');
>     }
> }
> ```

# 箭头函数

> this指向箭头函数的父作用域中的this，且不会被apply、call和bind改变
>
> 不能作为构造实例化对象，普通的函数可以用new 函数名作为构造函数使用，箭头函数则不可以
>
> 不能使用arguments变量
>
> 形参有且只有一个时，可以省略小括号
>
> 花括号中的函数体只有一条return xxx语句时，可以省略花括号和return关键字
>
## 与普通函数的区别
声明方式不同
> 普通函数用function，箭头函数小括号加箭头

匿名函数
> 普通函数既可以匿名也可以具名，箭头函数只能匿名然后赋值给变量

this指向
> 普通函数this指向最后一次调用它的对象，箭头函数指向父级上下文的this
箭头函数的this永远不会改变，即使是call、apply、bind
箭头函数没有原型prototype
箭头函数没arguments，只有rest
箭头函数不能作为构造函数new
箭头函数的参数不能重名

# 函数参数赋值初始值

> 赋初值后如果没有传这个参数就用赋值初始值来运行，传了就用传的值运行
>
> ```js
> function a(b, c, d=10){
>     console.log(b + c +d)
> }
> ```

# rest参数

> rest参数必须要放到最后，不然报错，必须加...，不然不会返回后面的数组而是顺序的下一个
>
> rest参数可以任意取变量名，一般取args，我取rest
>
> ```js
> function add(a, b, ...rest) {
>  console.log(a);
>  console.log(b);
>  console.log(rest);
> }
> 
> add(1, 2, 3, 4, 5, 6)
> // 1
> // 2
> // [3, 4, 5, 6]
> ```

# ...扩展运算符

> 将数组解构
>
> ```js
> const a = [1, 2, 3, 4]
> console.log(...a); // 1, 2, 3, 4 
> ```

# Symbol

> Symbol的值是唯一的，用来解决命名冲突的问题，Symbol() 函数每次都会返回新的一个 symbol
>
> ```js
> const a = Symbol('me')
> const b = Symbol('me')
> console.log(a === b); // false
> console.log(typeof a); // symbol
> 
> const c = Symbol.for('you')
> const d = Symbol.for('you')
> console.log(c === d) // true
> ```
>
> 和 `Symbol()` 不同的是，用 `Symbol.for()` 方法创建的的 symbol 会被放入一个全局 symbol 注册表中。`Symbol.for()` 并不是每次都会创建一个新的 symbol，它会首先检查给定的 key 是否已经在注册表中了。假如是，则会直接返回上次存储的那个。否则，它会再新建一个。
>
> Symbol的值不能与其他的值进行运算

# 生成器Generator与迭代器
## 最简单生成器实例
>```js
>function* foo() {
>            
>}
>const res = foo()
>console.log(res)
>```
>![img](images/generator1.png)
## 使用迭代器
>```js
>function* foo() {
>    yield "hello"
>    yield "world"
>    yield "welcome"
>}
>
>const res = foo()
>const res1 = res.next()
>const res2 = res.next()
>const res3 = res.next()
>const res4 = res.next()
>console.log(res1, res2, res3, res4)
>```
>![img](images/generator2.png)
>```js
>function* foo() {
>    console.log('111')
>    yield "hello"
>    console.log('222')
>    yield "world"
>    console.log('333')
>    yield "welcome"
>}
>const res = foo()
>const res1 = res.next()
>console.log(res1)
>const res2 = res.next()
>console.log(res2)
>const res3 = res.next()
>console.log(res3)
>const res4 = res.next()
>console.log(res4)
>```
>![img](images/generator3.png)
>练习，输出1-9数字
>```js
>function* foo() {
>    for (let index = 0; index < 10; index++) {
>        yield index
>    }
>}
>const res = foo()
>for (let index = 0; index < 10; index++) {
>    console.log(res.next().value);
>}
>```

# Promise

# Set

> size
>
> 返回集合的元素个数
>
> add
>
> 增加一个新元素，返回当前集合
>
> delete
>
> 删除元素，返回布尔值
>
> has
>
> 检测集合中是否包含某个元素，返回布尔值

# Map

> size
>
> 返回Map的元素个数
>
> set
>
> 增加一个新元素，返回当前Map
>
> get
>
> 返回键名对象的键值
>
> has
>
> 检测Map中是否包含某个元素，返回布尔值
>
> clear
>
> 清空集合返回undefined

# class类

> class声明类
>
> constructor定义构造函数初始化，也就是构造方法(==这里写实例对象的属性==)
>
> ```js
> class me {
>     constructor(name){
>         this.name = name
>     }
> }
> const a = new me('ljh')
> console.log(a.name); // 'ljh'
> ```
>
> 添加实例对象方法
>
> ```js
> class phone {
>     constructor(brand, price){
>         this.brand = brand
>         this.price = price
>     }
>     log(){
>         console.log(this);
>     }
> }
> 
> const a = new phone('apple', '10000')
> a.log() 
> // phone { brand: 'apple', price: '10000' }
> ```
>
> extends继承父类，super调用父级构造方法
>
> ```js
> class phone {
>     constructor(brand, price){
>         this.brand = brand
>         this.price = price
>     }
>     log(){
>         console.log(this);
>     }
> }
> 
> class smartphone extends phone {
>     constructor(brand, price, color, size){
>         super(brand, price)
>         this.color = color
>         this.size = size
>     }
> }
> 
> const a = new smartphone('apple', '10000', 'black', 'big')
> a.log()
> // smartphone {brand: 'apple', price: '10000', color: 'black', size: 'big'}
> ```
>
> static定义静态方法和属性
>
> ```js
> class me {
>     static name = 'ljh'
> }
> 
> const a = new me()
> console.log(me.name); // ljh
> console.log(a.name); // undefined
> ```
>
> 父类方法可以重写
>
> 在子类中重写重名方法将其覆盖即可
>
> ```js
> class phone {
>     constructor(brand, price){
>         this.brand = brand
>         this.price = price
>     }
>     log(){
>         console.log(this);
>     }
> }
> 
> class smartphone extends phone {
>     constructor(brand, price, color, size){
>         super(brand, price)
>         this.color = color
>         this.size = size
>     }
>     log(){
>         console.log('我重写啦');
>     }
> }
> 
> const a = new smartphone('apple', '10000', 'black', 'big')
> a.log() // '我重写啦'
> ```
>
> getter和setter
>
> ```js
> class phone {
>     get price(){
>         console.log('getter辣');
>         return 'hoho'
>     }
> 
>     set price(newprice){
>         console.log('setter辣');
>     }
> }
> const a = new phone()
> console.log(a.price);// 'getter辣'  //'hoho'
> a.price = 'free' // 'setter辣'
> ```

# 数字扩展

> Number.EPSILON
>
> > **`Number.EPSILON`** 属性表示 1 与[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)可表示的大于 1 的最小的浮点数之间的差值。
> >
> > EPSILON` 属性的值接近于 `2.2204460492503130808472633361816E-16`，或者 `2^-52。
>
> 支持二进制和八进制写法
>
> ```js
> const a = 0b1010
> const b = 0o777
> const c = 100
> console.log(a, b, c); // 10 511 100
> ```
>
> Number.isFinite判断数是不是有限的
>
> ```js
> console.log(Number.isFinite(100)); // true
> console.log(Number.isFinite(100/0)); // false
> console.log(Number.isFinite(Infinity)); // false
> ```
>
> Numebr.isNaN 判断是不是NaN
>
> ```js
> console.log(Number.isNaN(100)); // false
> console.log(Number.isNaN(NaN)); // true
> ```
>
> Number.parseInt和Number.parseFloat将字符串转整数
>
> Number.isInteger 判断是否为整数
>
> Math.trunc将数字小数部分抹掉
>
> Math.sign判断一个数到底为整数还是负数还是零，正数返回1，负数返回-1，零返回0

# 对象方法拓展

> Object.is判断两个值是否完全相等，大致上相当于===，但是NaN是个特例
>
> ```js
> console.log(NaN === NaN); //false
> console.log(Object.is(NaN, NaN)); // true
> ```
>
> Object.assign将除第一个参数以外的其他参数合并到第一个参数对象身上，相同的属性覆盖，不同的属性添加
>
> Object.setPrototypeOf()方法设置一个指定的对象的原型 ( 即，内部 [[Prototype]] 属性）到另一个对象或 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/null)。
>
> ```js
> const a = {
>     name: 'ljh'
> }
> 
> const b = {
>     age: 18
> }
> 
> Object.setPrototypeOf(a , b)
> console.log(a);
> ```
>
> ![image-20220816185159388](picture/image-20220816185159388.png)

# import export

## 命名导出（Named exports）
> 命名导出无论怎样导出，引入的时候都需要{}
>
> ```js
> //------ lib.js ------
> const sqrt = Math.sqrt;
> function square(x) {
>     return x * x;
> }
> function diag(x, y) {
>     return sqrt(square(x) + square(y));
> }
> 
> export {sqrt, square, diag}
> 
> //------ main.js ------
> import { square, diag } from 'lib';				
> console.log(square(11)); // 121
> console.log(diag(4, 3)); // 5
> ```
>
> ```js
> //------ lib.js ------
> export const sqrt = Math.sqrt;
> export function square(x) {
>     return x * x;
> }
> export function diag(x, y) {
>     return sqrt(square(x) + square(y));
> }
> 
> //------ main.js ------
> import { square, diag } from 'lib';				
> console.log(square(11)); // 121
> console.log(diag(4, 3)); // 5
> ```
>
## 别名引入（Aliasing named imports）
> 当从不同模块引入的变量名相同的时候
>
> ```javascript
> import {speak} from './cow.js'
> import {speak} from './goat.js'	
> 复制代码
> ```
>
> 这些写法显然会造成混乱
>
> 正确的方法是这样的
>
> ```javascript
> import {speak as cowSpeak} from './cow.js'
> import {speak as goatSpeak} from './goat.js'
> 复制代码
> ```
>
> 可是，当从每个模块需要引入的方法很多的时候，这种写法就显得十分的繁琐、冗长，例如
>
> ```javascript
> import {
>   speak as cowSpeak,
>   eat as cowEat,
>   drink as cowDrink
> } from './cow.js'
> 
> import {
>   speak as goatSpeak,
>   eat as goatEat,
>   drink as goatDrink
> } from './goat.js'
> 
> cowSpeak() // moo
> cowEat() // cow eats
> goatSpeak() // baa
> goatDrink() // goat drinks
> 复制代码
> ```
>
> 解决方案就是命名空间引入了
>
## 命名空间引入（Namespace imports）
> ```js
> import * as cow from './cow.js'
> import * as goat from './goat.js'
> 
> cow.speak() // moo
> goat.speak() // baa
> ```
>
## 默认导出（Default exports）
> 默认导出就不需要name了，但是一个js文件中只能有一个export default。
>
> ```javascript
> //------ myFunc.js ------
> export default function () { ... };
> 
> //------ main1.js ------
> import myFunc from 'myFunc';
> myFunc();
> ```
>
> 虽然export default只能有一个，但也可以导出多个方法。
>
> ```kotlin
> export default {
>   speak () {
>     return 'moo'
>   },
>   eat () {
>     return 'cow eats'
>   },
>   drink () {
>     return 'cow drinks'
>   }
> }
> 复制代码
> ```
>
> 引入与命名空间引入类似
>
> ```javascript
> import cow from './default-cow.js'
> import goat from './default-goat.js'
> 
> cow.speak() // moo
> goat.speak() // baa
> ```

# Bigint

> **`BigInt`** 是一种内置对象，它提供了一种方法来表示大于 `2^53 - 1` 的整数。这原本是 Javascript 中可以用 [`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 表示的最大数字。**`BigInt`** 可以表示任意大的整数。
>
> 可以用在一个整数字面量后面加 `n` 的方式定义一个 `BigInt` ，如：`10n`，或者调用函数 `BigInt()`（但不包含 `new` 运算符）并传递一个整数值或字符串值。