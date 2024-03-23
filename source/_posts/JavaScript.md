---
title: JavaScript
date: 2024-03-15 21:03:14
tags:
---

# JS补充

>JavaScript区分大小写
>
>api接口是对象有属性和方法
>
>事件(click)是对象有属性无方法(click是一种事件/click()是一种元素对象的方法)
>
>原始值（Undefined、Null、Boolean、Number、String、Symbol和BigInt）是对象有属性无方法
>
>引用值（Object）是对象有属性有方法
>
>在html模板上使用了形参(典型的click点击事件)也要在对应的js里面的定义函数的地方写形参
> 
>在必须使用/来读取文件的地方不能一起使用.，此时应该将带有/的语句用[]括起来然后可以和.一起使用
>
> ![image-20220510091016145](images/image-20220510091016145.png)
>
> 一个事件监听器只能有一个事件处理程序，addEventListener可以添加多个自定义监听器(事件处理程序)，传统的的事件监听器例如onclick算一个事件监听器，同一个dom节点同时可以有多个不同的事件监听器
>
> e.target和this的区别，前者是指向我们点击的的对象，无论点击事件是否冒泡，后者则是指向绑定事件的对象，没有事件冒泡时和前者一样，有事件冒泡时会返回冒泡的终点也就是绑定了事件的那个对象
>
>箭头函数的函数体如果只有一个return语句的话，可以省略花括号和return关键字(注意要省略必须两个一起省略，不能只省略一个)
>```
>const arrow = x => x + 1 
>```
>
> 函数内的语句执行时只要碰到return就立刻停止执行并且退出，return不带返回值时会返回undefined，带返回值时只能返回一个值，见书p81
>
> 函数的传参数目不确定时利用arguments代替，arguments本身是一个伪数组，它没有真正数组的一些方法，例如pop() push()等
>
> 将star函数当作构造器使用，返回值类型为object(==即下面ldh是一个object而不是function==)
>
> ```js
> function star(name, age, sex) {
>     this.name = name; //必须用; 逗号会报错
>     this.age = age;
>     this.sex = sex;
> }
> let ldh = new star("刘德华", 18, "male");
> console.log(ldh);
> ```
>
> 字符串也可以像数组一样索引取值
>
> 字符串类型的数字当作数组或字符串的下标索引会被直接转换为数字类型
> ```js
> let map = [1,2,34]
> let s = 'abcdf'
> let a = '2'
> console.log(map[a]);//34
> console.log(s[a]);//c
> ```
> ```js
>'cat'[1]=a
>'cat'.charAt(1)=a
>```
> 
>```
> null == undefined //true
>
> null === undefined //false
>```
>```javascript
>let a = 'hello world';
>let b = ['h','e','l','l','o','','w','o','r','l','d'];
>console.log(a[5]);// ' '
>console.log(b[5]);// ' '
>
>console.log(a.indexOf(''));// 0
>console.log(a.lastIndexOf(''));// 11
>console.log(b.indexOf(''));// 0
>console.log(b.lastIndexOf(''));// 11
>
>console.log(a.indexOf(' '));// 5
>console.log(a.lastIndexOf(' '));// 5
>console.log(b.indexOf(' '));// 5
>console.log(b.lastIndexOf(' '));// 5
>//这里需要注意的是空格也算一个字符，那么在console.log(a[5])中返回的是' '而不是''，前者比后者两个引号中间多了空格，对于indexOf和lastIndexOf，如果传入参数是''空字符则分别返回0和字符串的length，如果是' '则返回字符串中空格的索引号
>```
> ```javascript
> let a = '123456'
> let b = ['1','2','3','4','5','6']
> let c = [1,2,3,4,5,6]
> 
> console.log(a.lastIndexOf(6)) //5
> console.log(a.lastIndexOf('6')) //5
> console.log(b.lastIndexOf('6')) //5
> console.log(b.lastIndexOf(6)) //-1
> console.log(c.lastIndexOf('6')) //-1
> console.log(c.lastIndexOf(6)) //5
> ```
>
> 对于indexOf和lastIndexOf，要根据要找的值是什么数据类型来填写()中的值
>
> 这里的console.log(a.lastIndexOf(6))对6进行了类型转换，而数组中的没有进行类型转换

## 构造器/构造函数与转型函数书p114
``` js
 new Number();//构造函数

 Number();//转型函数
```
## 在字面量表示法中，对象的属性/方法如果初始化为函数，可以将function省略掉
``` js
function showInfo(){}
// 可以简写为 
showInfo(){}
 
get:function(){}
// 可以简写为
get(){}
```

## 构造函数和普通函数的区别？
>？


# 0.1+0.2 != 0.3
> 根据语言规范，JavaScript 采用“遵循 IEEE 754 标准的双精度 64 位格式”（"double-precision 64-bit format IEEE 754 values"）表示数字。——在 JavaScript（除了[`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)）当中，**并不存在整数/整型 (Integer)。**
>
> ```
> 0.1 + 0.2 = 0.30000000000000004
> ```

# 数组转字符串join、toString
>数组转字符串join(注意join和toString的区别，前者用''空字符得到没有,的字符串，不用''得到有,分割的字符串，后者不管用不用''都得到有,分隔的字符串)
> ``` js
>const elements = ['Fire', 'Air', 'Water'];
>
>console.log(elements.join());
>// Expected output: "Fire,Air,Water"
>
>console.log(elements.join(''));
>// Expected output: "FireAirWater"
>
>console.log(elements.join('-'));
>// Expected output: "Fire-Air-Water"
>
>const array1 = [1, 2, 'a', '1a'];
>console.log(array1.toString());
>// Expected output: "1,2,a,1a"
>```
# 字符串转数组split、Array.from()
>split,注意split()和split('')和split(' ')这三者的区别，
>
>Array.from(),效果和split('')一样
>
> ```javascript
> const str = 'The quick brown fox jumps over the lazy dog.';
> 
> const words = str.split(' ');
> console.log(words);
> // expected output: Array ["The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog."]
> 
> const chars = str.split('');
> console.log(chars);
> // expected output: Array ["T", "h", "e", " ", "q", "u", "i", "c", "k", " ", "b", "r", "o", "w", "n", " ", "f", "o", "x", " ", "j", "u", "m", "p", "s", " ", "o", "v", "e", "r", " ", "t", "h", "e", " ", "l", "a", "z", "y", " ", "d", "o", "g", "."]
> 
> const strCopy = str.split();
> console.log(strCopy);
> // expected output: Array ["The quick brown fox jumps over the lazy dog."]
>
> console.log(Array.from(str))
> // expected output: Array ["T", "h", "e", " ", "q", "u", "i", "c", "k", " ", "b", "r", "o", "w", "n", " ", "f", "o", "x", " ", "j", "u", "m", "p", "s", " ", "o", "v", "e", "r", " ", "t", "h", "e", " ", "l", "a", "z", "y", " ", "d", "o", "g", "."]
> ```
# 两数组拼接concat
>``` js
>const array1 = ['a', 'b', 'c'];
>const array2 = ['d', 'e', 'f'];
>const array3 = array1.concat(array2);
>
>console.log(array3);
>// Expected output: Array ["a", "b", "c", "d", "e", "f"]
>```

# 两字符串拼接concat
>```
>const str1 = 'Hello';
>const str2 = 'World';
>
>console.log(str1.concat(' ', str2));
>// Expected output: "Hello World"
>
>console.log(str2.concat(', ', str1));
>// Expected output: "World, Hello"
>```

# 字符串修改下标
> js字符串不能像数组一样直接利用下标来进行字符修改，所以执行出错，要想实现下标修改需要用到slice
>``` js
> const str = 'The quick brown fox jumps over the lazy dog.';
> 
> console.log(str.slice(31));
> // Expected output: "the lazy dog."
> 
> console.log(str.slice(4, 19));
> // Expected output: "quick brown fox"
> 
> console.log(str.slice(-4));
> // Expected output: "dog."
> 
> console.log(str.slice(-9, -5));
> // Expected output: "lazy"
>```

# 数组与字符串长度属性length差异
> js数组可以直接通过修改length长度实现切割或者扩容(扩容出来的位置为空，打印出来是undefined)，而字符串不行
>
> ```javascript
> let arr = [1,2,3,4,5]
> arr.length = 3
> let s = '12345'
> s.length = 3
> console.log(arr);
> console.log(s);
> //[ 1, 2, 3 ]
> //12345
> ```

# if中执行语句导致else if满足条件(if执行语句之前不满足条件)，else if也不会触发
> ```javascript
> let n = 1, m = 0
> if(n > 0){
>  m++
>  console.log(m);
> }else if(m > 0){
>  console.log('done');
> }
> // 1
> ```
>
> ```javascript
> let n = 1, m = 0
> if(n > 0){
>  m++
>  console.log(m);
> }
> if(m > 0){
>  console.log('done');
> }
> //1 done
> ```

# for中执行语句改变了判定条件属性会影响循环次数
>
> ```js
> let n = 10
> for(let i = 0; i < n; i++){
>  console.log(1);
>  n--
> }
> //11111
> ```
> ![image-20220710142349099](images/image-20220710142349099.png)
>
> ![image-20220710142503690](images/image-20220710142503690.png)
>
# 数组解构赋值不加封号会导致报错或意料之外的结果
>```js
> let x = document.getElementById(j)	
> let y = document.getElementById(j+1); //这里不加封号会导致将这行代码和下行代码合并成一句代码执行，这样在y的赋值语句中调用y会报错
> [x.style.left, y.style.left] = [y.style.left, x.style.> left]; //此处不加封号同理会汇合并成一句代码执行，导致意外结果
> [x.id, y.id] = [y.id, x.id]
>```

# forEach和map
> 数组的forEach和map虽然都可以传入currentValue，但是forEach的是当前处理项的值而不是当前处理项，而map的则是当前处理项，两者最明显的的区别是forEach不能用currentValue来对数组元素进行值的更改(例如x => x*x)，而map可以
>
> 所以forEach想更改值只能通过给arr[index]赋值的方式来进行
>
> forEach没有返回值，map返回一个新数组

# map和reduce
> map返回值是一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。
>
> reduce() 方法对数组中的每个元素按序执行一个由您提供的 reducer 函数，每一次运行 reducer 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值。第一次执行回调函数时，不存在“上一次的计算结果”。如果需要回调函数从数组索引为 0 的元素开始执行，则需要传递初始值。否则，数组索引为 0 的元素将被作为初始值 *initialValue*，迭代器将从第二个元素开始执行（索引为 1 而不是 0）。

# forEach中return
> 数组forEach中使用return不会终止循环，只会终止当前的这一轮操作，对当前这一轮操作后面的循环不会有影响
>
>  ![image-20220711131933843](images/image-20220711131933843.png)

# 引用值的内存地址问题
> ```js
> const map = new Map()
> map.set([1,2],0)
> console.log(map.get([1, 2]));//undefined
> ```
> 跟['a'] === ['a']判断为false是一样的，数组和对象得将引用用变量保存起来，后面才能调用到相同的地址，每次使用[]都是new Array
>
> 所以数组也可以作为键，但是必须使用一个变量将该数组存储起来(这种变量保存一个引用数据类型是一种浅拷贝)，否则会出现上面的情况
>
> [js关于map的一个问题？为什么时undefine - SegmentFault 思否](https://segmentfault.com/q/1010000007838432)
>
> 
>
>```javascript
>const map = new Map()
>const map2 = map
>map.set('name',1)
>map2.set('age',2)
>console.log(map);
>console.log(map2);
>```
> 
> ![image-20220712185223090](images/image-20220712185223090.png)
> 
> 这种直接等于引用数据类型的是浅拷贝
> 
>
> 
>
# Map中的每个keyvalue对都是一个数组
>
> ```javascript
> const map = new Map([
>     ['name',1],
>     ['age',2],
>     ['sex',3]
> ])
> for (const arr of map) {
>     console.log(arr);
> }
> //[ 'name', 1 ]
> //[ 'age', 2 ]
> //[ 'sex', 3 ]
> ```
>
> ```javascript
> const map = new Map()
> map.set('name',1)
> map.set('age',2)
> map.set('sex',3)
> for (const arr of map) {
>     console.log(arr);
> }
> //[ 'name', 1 ]
> //[ 'age', 2 ]
> //[ 'sex', 3 ]
> ```

# 预编译
> 在JavaScript中，当一般变量、函数、函数参数重名时，它们的权重为：
> 已初始化的变量声明 > 函数 > 函数参数 > 未初始化的变量声明
>
> 从预编译角度理解问题
>
> 函数预编译(覆盖原则)
>
>> 创建AO对象，执行期上下文（后面更新关于执行期上下文详解）。
>>
>> 寻找函数的形参和变量声明，将变量和形参名作为AO对象的属性名，值设定为undefined.(最先预编译，所以未初始化的变量声明权重最低)
>> 
>> 将形参和实参(也就是外部传进的参数)相统一，即更改形参后的undefined为具体的形参值。(第二个预编译，所以函数参数权重倒数第二低)
>>
>> 寻找函数中的函数声明，将函数名作为AO属性名，值为函数体。(倒数第二，所以函数声明权重第二)
>>
>> 最后开始逐行顺序执行代码(执行了初始化赋值，所以初始化变量声明权重最高，因为最后执行覆盖了前面的)
>
>全局预编译
>
>> 创建GO（Global Object，全局执行期上下文，在浏览器中为window）对象；
>>
>>寻找var变量声明，并赋值为undefined；(==先预编译，所以权重低于函数==)
>>
>>寻找function函数声明，并赋值为函数体；(==后预编译，所以权重高于函数==)
>>
>>执行代码。
https://blog.csdn.net/qq_51066068/article/details/124015286

# js数组排序和求和
## 升序
>
> ```javascript
> numbers.sort((a, b) => a - b);
> console.log(numbers);
> ```
>
> ```js
> nums.sort((a, b) => {Math.abs(a) - Math.abs(b)})
> // 绝对值升序
> ```
>
## 降序
>
> ```javascript
> numbers.sort((a, b) => b - a);
> console.log(numbers);
> ```
>
> ```js
> nums.sort((a, b) => {Math.abs(b) - Math.abs(a)})
> // 绝对值降序
> ```
## 求和(for循环和for of除外)
> ```js
> const sum = arr.reduce((a, b) => a + b)
> // reduce求和
> ```

# 循环判断if中的条件，即使条件不符合也要运行
>
> > ```js
> > const nums = [-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1]
> > let k = 0
> > 
> > for(let i = 0; i < nums.length; i++){
> >     if(nums[i] < 0 && k-- > 0){
> >         nums[i] = -nums[i]
> >     }
> > }
> > 
> > console.log(k);
> > // k为-25，即使k不大于0也还是要运行--
> > ```


# 数组和对象都可以通过[]来获取内部值
>
> > ```js
> > const a = [1,2,3]
> > const b = {a: 1, b: 2, c: 3}
> > console.log(a[0]) //1
> > console.log(b['a']) //1
> > ```
>
> 需要注意的是对象通过[]获取需要注意加''，因为key是一个string，而使用obj.xxx的方式获取则不需要加''

# Array.prototype.slice.call()
Array.prototype.slice.call()可以将一个类数组对象/集合转换为一个新数组，这个类数组需要length属性，[].slice.call效果相同，一般用来将arguments转换为数组
>
> ```js
> Array.prototype.slice.call({length:3, '0': 9, '1': 8, '2': 7})
> // [9, 8, 7]
> Array.prototype.slice.call({'0': 9, '1': 8, '2': 7})
> // []
> ```
>
> 其他转数组方法
>
> [...arguments]
>
> Array.from(arguments)
>
> ```js
> var toArray = function(s){
>     try{
>         return Array.prototype.slice.call(s);
>     } catch(e){
>         var arr = [];
>         for(var i = 0,len = s.length; i < len; i++){
>             //arr.push(s[i]);
>                arr[i] = s[i];  //据说这样比push快
>         }
>          return arr;
>     }
> }
> ```

# 数组函数添加属性
> 数组和函数添加属性不能像对象一样直接写在内部，只能在外面赋值，这就导致了作用域的差异
>
> > ```js
> > const a = function(){
> > 
> > }
> > a.b = function(){
> >     console.log(1)
> > }
> > ```
> >
> > ```js
> > const a = []
> > a.foo = 'foo'
> > ```
>
> 
>
# 连续赋值
>
> ```js
> let a = b = 3
> // 先执行b = 3，沿作用域链找b，没有找到则是向不存在的变量赋值，则隐式地声明一个全局变量b并赋值为3，b = 3的返回值为3，然后执行let a = 3
> console.log(window.a, window.b)
> // undefined 3
> ```

# 隐式声明
>
> ```js
> console.log(window.a)
> // undefined
> console.log(a)
> // Uncaught ReferenceError: a is not defined
> a = 3
> // 不使用var let const的就是隐式声明，隐式声明会在预编译时在window上挂载一个属性a = undefined，但是没有变量提升所以不会创造全局变量a，所以console.log(a)会报错
> ```

# 立即执行函数/自执行函数(函数加小括号)
> ```js
> var fact = (function f(){ 
>     console.log(1); 
>     return 2
>     })
> console.log(fact);
> // Function: f
> // function f本身不执行
> ```
>
> ```js
> var fact = (function f(){ 
>     console.log(1); 
>     return 2
>     })()
> console.log(fact);
> // 1 2
> // function f执行，并将返回值赋给fact变量
> 
> var fact = (function f(){ 
>     console.log(1); 
>     return 2
>     }())
> console.log(fact);
> // 和上面等价
> ```
>
> ==自执行函数的this不管在哪都指向全局对象window/global==
>
> ```js
> let obj = {
>     num: 5,
>     func: function () {
>         let that = this;
>         that.num *= 2;
>         (function () {
>             this.num *= 3;
>             console.log(this);
>             that.num *= 4;
>             return function () {
>                 this.num *= 5;
>                 that.num *= 6;
>                 // 这里return的function不会执行而是作为返回值返回，这里没有变量来接
>             }
>         })()
>     }
> }
> var num = 2;
> obj.func();
> console.log(num);
> console.log(obj.num);
> //浏览器下 window 6 40
> //node下 global 2 40 因为node下var不会把变量挂载到global身上
> ```

# process.nextTick
>  在一次事件循环结束后，下一轮事件循环的宏任务开始执行前调用，是一个微任务，且永远比promise.then微任务更快

# 块级作用域
> ```js
> console.log(window.a,a);
> // undefined undefined
> if(true){
>   console.log(window.a,a);
>   // undefined ƒ a() {}
>   function a() {}
>   console.log(window.a,a);
>   // ƒ a() {} ƒ a() {}
> };
> console.log(window.a,a)
> // ƒ a() {} ƒ a() {}
> ```

# JS输入输出语句

> ![image-20220324125316723](C:%5CUsers%5CHasee%5CDesktop%5C%E7%AC%94%E8%AE%B0%5Cimages%5Cimage-20220324125316723.png)

# var let const

> var与let的区别
>
> > var声明范围是函数作用域，而let声明范围是块作用域
> >
> > 例如
> >
> > ```js
> > if(true) {
> >  var name = 'matt';
> >  console.log(name); // matt
> > }
> > console.log(name);  // matt
> > ```
> >
> > ```js
> > if(true) {
> >     let name = 'matt';
> >     console.log(name); // matt
> > }
> > console.log(name);  // ReferenceError: name is not defined
> > ```
> >
> > var的“声明提升hoist”，let没有声明提升(因为暂时性死区的存在)
> >
> > ```js
> > function foo() {
> >     console.log(age);
> >     var age = 30;
> > }
> > foo(); // undefined
> > 
> > // 会报出undefined而不是报错嗯低原因是因为系统运行时会把它看成等价于如下代码
> > 
> > function foo() {
> >     var age;
> >     console.log(age);
> >     age = 30;
> > }
> > foo(); // undefined
> > ```
> >
> > ```js
> > function foo() {
> >     console.log(age);
> >     let age = 30;
> > }
> > foo(); // ReferenceError: Cannot access 'age' before initialization
> > ```
> >
> > var可以重复声明同一变量，let不可以，var let混用对同一变量重复声明也会报错
> >
> > ```js
> > var name;
> > var name;
> > ```
> >
> > ```js
> > let name;
> > let name; // SyntaxError: Identifier 'name' has already been declared
> > ```
> >
> > ```
> > let name;
> > var name; // SyntaxError: Identifier 'name' has already been declared
> > ```
> >
> > ```
> > var name;
> > let name; // SyntaxError: Identifier 'name' has already been declared
> > ```
> >
> > var在全局作用域声明的变量会变成window对象的属性，let则不会(window需要在浏览器环境下才有效，在nodejs环境下无效)
> >
> > ```
> > var name = 'matt';
> > console.log(window.name); //matt
> > ```
> >
> > ```
> > let name = 'matt';
> > console.log(window.name); // undefined
> > ```
> >
> > 使用var的for循环定义的迭代变量会渗透到循环体外部，且var会导致迭代变量的奇怪声明和修改，而let则不会
> >
> > ```
> > for (var i = 0; i < 5; ++i ) {
> > 
> > }
> > console.log(i); // 5
> > ```
> >
> > ```
> > for (let i = 0; i < 5; ++i ) {
> > 
> > }
> > console.log(i); // ReferenceError: i is not defined
> > ```
> >
> > for循环执行逻辑: 先执行for里面的n次循环再执行n次块里面的语句
> >
> > var: 在退出循环时，迭代变量保存的是导致最终退出循环的那一个值即5，所以在执行块里面的语句就会出现5个5
> >
> > let: 用let声明迭代变量时，JavaScript引擎会在后台为每一次迭代循环声明一个新的迭代变量，每一个setTimeout引用的都是不同的新的迭代变量，所以console.log输出的是我们所期望的01234
> >
> > ```
> > for (var i = 0; i < 5; ++i ) {
> > setTimeout(() => console.log(i), 0)
> > }
> > // 55555
> > ```
> >
> > ```
> > for (let i = 0; i < 5; ++i ) {
> > setTimeout(() => console.log(i), 0)
> > }
> > // 01234
> > ```
> >
> > const声明与let基本一致，少数区别如下
> >
> > const声明变量时必须同时初始化变量，且尝试修改const声明的变量会报错
> >
> > ```
> > const age; // SyntaxError: Missing initializer in const declaration
> > ```
> >
> > ```
> > let age;
> > ```
> >
> > ```
> > const age = 20;
> > age =30; //TypeError: Assignment to constant variable.
> > ```
> >
> > ```
> > let age = 20;
> > age =30; // 30
> > ```
> >
> > 虽然修改const声明的变量会报错，但修改该变量内部的属性并不违反const的限制
> >
> > ```
> > const person = {};
> > person.name = 'matt'; // matt
> > ```
> >
> > 修改const声明的变量会报错，所以不能使用const声明for循环中的迭代变量，因为迭代变量会出现数值的变化，违反了const的限制，但如果const声明的变量在for循环中不被改变数值，那么该变量还是可以被使用在for循环中的
> >
> > 应该主要使用const声明变量，let次之，var尽可能的不使用
>
> [var 描述 - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/var)
>
> [let - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/let)
>
> [const - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/const)

# typeOf操作符

> 判断变量是否为字符串、数值、布尔值或者undefined，函数、对象和null则会返回object(严格来讲函数并不是一种数据类型，且返回function还是object因浏览器而异，见书p87/null在此处被逻辑上认为是一种空对象的引用)
>
> ```
> let message = 'some string';
> console.log(typeof message);   // string
> console.log(typeof (message)); //string
> console.log(typeof 99);        //number
> ```
>
> [typeof - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

# 基本数据类型
## Undefined类型
> undefined类型只有一个值，就是特殊值undefined，当var和let声明了变量但没有初始化时，变量就被赋予了undefined值
>
> ```
> let message;
> console.log(message == undefined); // true
> 
> // 这个例子等同于下例
> 
> let message = undefine;
> console.log(message == undefined); // true
> ```
>
> (没有必要显式地以undefined来初始化，因为没有初始化的变量会自动负值undefined)
>
> 包含undefined值得变量和没有定义的变量有本质区别
>
> ```
> let message;
> console.log(message == undefined); // true
> console.log(age); //ReferenceError: age is not defined
> ```
>
> 注意，无论是未定义的变量还是没有初始化的变量，它们用typeof操作符返回的值都是undefined，逻辑上是对的但是严格上两者有根本性差异
>
> 建议每个变量在声明的同时进行初始化，这样可以根据typeof返回undefined来确定变量是否声明，而不是是否初始化
>
> ```
> let message;
> console.log(typeof message); // undefined
> console.log(typeof age); //undefined
> ```
>
> [undefined - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)
>
## Null类型
> Null类型只有一个特殊值，即特殊值null，逻辑上null表示一个空的对象指针，这是typeof null会返回object的原因
>
> 建议将未来要保存对象值的变量初始化为null，这样只需检查这个值是不是bull就可以知道这个变量是否在后来被赋予了新的值
>
> ```
> let message = null;
> console.log(message != null);
> ```
>
> undefined是由null派生而来的，表面上它们相等
>
> ```
> let message;
> console.log(message == null); //true
> ```
>
> [null - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)

## Boolean类型
> Boolean类型有true和false两个特殊值
>
> | 数据类型  | 转换为true的值         | 转化为false的值 |
> | --------- | ---------------------- | --------------- |
> | Boolean   | true                   | false           |
> | string    | 非空字符串             | ""(空字符串)    |
> | Number    | 非零数值(包括infinity) | 0、NaN          |
> | Object    | 任意对象               | null            |
> | Undefined | N/A(不存在)            | undefined       |
>
> 理解上述转换十分重要，在if等流控制语句中会自动执行其他类型值到boolean值的转换
>
> [Boolean - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## Number类型
> 科学计数法
>
> ```
> let floatNum = 3.125e7; //等于31250000
> let floatNum1 = 3.125e-7; //等于0.0000003125
> ```
>
> 因为使用IEEE 754数值，0.1+0.2并不等于0.3，因为在JavaScript中计算会先将10进制数字转换成2进制数字相加再转换为10进制来显示，但是因为IEE754数值得精度只达到17位小数，但是0.1和0.2转换成2进制数字是无穷尽的，这样0.1和0.2转换过来的2进制数值只能是近似值而不是准确值，所以两个近似值相加得到的结果也是近似值从而不会等于0.3，所以只有在2进制下能用17位小数表示准确值的数相加才能得到准确值
>
> ```
> const a = 0.1;
> const b = 0.2;
> console.log(a + b); //0.30000000000000004
> ```
>
> 因为内存限制，JavaScript的数值有上下限，最大值保存在Number.MAX_VALUE中，这个值为1.7976931348623157e308，最小值保存在Number.MIN_VALUE中，这个值为5e-324，如果某个值超过了最大值，则自动转换为Infinity，如果某个值小于最小值，则自动转换为-Infinity，正负Infinity不能用于计算，可以使用isFinite()函数来确定一个值是不是有限大
>
> ```
> const a = Number.MAX_VALUE;
> const b = Number.MIN_VALUE;
> console.log(a); //1.7976931348623157e+308
> console.log(b); //5e-324
> ```
>
> 使用Number.NEGATIVE_INFINITY和Number.POSITIVE_INFINITY也可以获得正负Infinity
>
> ```
> const a = Number.POSITIVE_INFINITY;
> const b = Number.NEGATIVE_INFINITY;
> console.log(a); //Infinity
> console.log(b); //-Infinity
> ```
>
> NaN，不是数字(Not a Number)
>
> ```
> console.log(0/0); //NaN
> console.log(-0/+0); //NaN
> console.log(5/0); //Infinity
> console.log(5/-0); //-Infinity
> ```
>
> 任何使用NaN参与的计算返回值都是NaN，且NaN不是数字，所以NaN == NaN返回值是false
>
> 可以使用isNaN来判断一个参数是不是NaN(不常用，常用valueOf和toString)
>
> [isNaN() - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)
>
> [Number - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)

## String类型
> 字符串转换，使用String()进行字符串转换，遵循以下规则:
>
> - 如果该值有toString()方法，则调用toString()方法
>
> - 如果值是null，则返回“null”
>
> - 如果是undefined，则返回“undefined”
>
>   [String - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
>
>   模板字面量，字符串插值，模板字面量标签函数，原始字符串(String.raw()标签函数获取如换行符和Unicode字符本身而不是被转换后的字符表示)，这些内容见书p40-p44

## Symbol类型(es6新增)
>
>  见书p44-p55

## Bigint类型(es10新增)

# 引用数据类型

## Object类型

 > 使用new 创造出来的都是object
 >
 > let 数组名 = new object();
 >
 > 字面量创建对象
 >
 > ```js
 > let 数组名 = {
 > 
 > name: 'ounce',  //必须用逗号
 > 
 > age:18,
 > 
 > sex:'male',
 > 
 > sayName() {
 > 
 > console.log(this.name);
 > 
 > }
 > 
 > } ;
 > ```
 >
 > 见书p56

# 数值转换
## Number()
>
> 转换规则
>
> - Boolean值，true转换为1，false转换为0
>
> - 数值直接返回
>
> - null，返回为0
>
> - undefined，转换为NaN
>
> - 字符串包含数值字符(包括数值前有正负号的情况)，将转换为十进制数字(数值字符前带零的忽略零) 浮点值格式的转换为浮点值格式 十六进制格式的转换为十进制 空字符串转换为0 除去这些情况外返回NaN
>
> - 对象，调用valueOf()方法，并按照上述规则转换返回的值，如果转换结果是NaN，则调用toString()方法，再按照转换字符串的规则转换
>
>  [Object.prototype.valueOf() - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)
>
> [Object.prototype.toString() - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
>  
>  [Number - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)
>
## parseInt()
>
> 更专注于字符串是否包含数值
>
> 字符串前面的空格会被忽略，从第一个字符开始转换，如果第一个字符不是数值字符，加号减号，则立刻返回NaN，==这意味着空字符串也会返回NaN(这一点和Number()不同，它返回0)==，如果第一个字符是数值字符和+-号，则依次检测每个字符直到碰到非数值字符或者字符串末尾，也可以传入第二个参数告诉parseInt这是一个几进制的数，以便于更好地解析，一个数值也会因传入的第二个参数的不同解析结果不同
>```js
> let z = parseInt("1234blue"); //1234
>
> let a = parseInt("AF", 16); //175
>
> let b = parseInt("AF"); //NaN
>
> let c = parseInt("10", 2); //2 二进制解析
>
> let d = parseInt("10", 8); //8 八进制解析
>```
> [parseInt - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

## parseFloat()
> parseFloat从第一个字符开始转换，自动忽略掉开头的0，转换直到第二个小数点或者非数值字符，然后自动结束解析并忽略掉第二个小数点或者非数值字符和之后的字符
>```js
> parseFloat只解析十进制的数
>
> let z = parseFloat("1234blue"); //1234
>
> let a = parseFloat("0xA"); //0
>
> let b = parseFloat("22.34.5"); //22.34
>
> let c = parseFloat("0908.5"); //908.5
>```
> [parseFloat - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)

# 一元操作符

> 前缀递增++a前缀递减--a
>
> 前缀递增递减都会在语句被求值之前改变(副作用)
>
> ```js
> let a = 2;
> let b = 20;
> let c = --a + b;
> let d = a + b;
> console.log(a);//1
> console.log(c);//21
> console.log(d);//21
> ```
>
> 后缀递增a++后缀递减a--
>
> 后缀递增递减不会在语句被求值之前改变
>
> ```
> let a = 2;
> let b = 20;
> let c = a-- + b;
> let d = a + b;
> console.log(a);//1
> console.log(c);//22
> console.log(d);//21
> ```
>
> 前缀后缀递增递减对于可以转换为数值的执行递增递减，对于不能转换为数值的返回NaN

# 位操作符

## 按位非~
> 作用是返回数值的一补数(一补数的定义见书p60)，是二进制数学操作符
>
> 效果为对数值取反并减1
>
> ```js
> let a = 25;
> let b = ~a;
> console.log(b); //-26
> ```
>
>
## 按位与&
>
> 是二进制数学操作符
>
> | 第一个数值的位 | 第二个数值的位 | 结果 |
> | -------------- | -------------- | ---- |
> | 1              | 1              | 1    |
> | 1              | 0              | 0    |
> | 0              | 1              | 0    |
> | 0              | 0              | 0    |
>
> 按位与操作在两位都是1时返回1，在任何一位是0时返回0
>
> 举例:
>
> ```js
> let a =25 & 3;
> console.log(a); //1
> ```
>
> 25 & 3计算过程如下:
>
> 将25转换为二进制为:11001，将3转化为二进制为:11
>
> 则将11001和00011对应的每一位进行一次&运算则可得1
>
> 因此25&3的结果为1
>
## 按位或|
>
> 是二进制数学操作符
>
> | 第一个数值的位 | 第二个数值的位 | 结果 |
> | -------------- | -------------- | ---- |
> | 1              | 1              | 1    |
> | 1              | 0              | 1    |
> | 0              | 1              | 1    |
> | 0              | 0              | 0    |
>
> 按位或操作在至少一位是1时返回1，两位都是0时返回0
>
> 举例:
>
> ```js
> let a = 25 | 3;
> console.log(a); //27
> ```
>
> 25 | 3计算过程如下:
>
> 将25转换为二进制为: 11001，将3转化为二进制为:11
>
> 则将11001和00011对应的每一位进行一次|运算则可得11011即为27
>
> 因此25|3的结果是27
>
## 按位异或^
> 是二进制数学操作符
>
> | 第一个数的位 | 第二个数的位 | 结果 |
> | ------------ | ------------ | ---- |
> | 1            | 1            | 0    |
> | 1            | 0            | 1    |
> | 0            | 1            | 1    |
> | 0            | 0            | 0    |
>
> 按位异或操作只在一位上是1的时候返回1，两位都是1或者两位都是0时返回0
>
> 举例
>
> ```js
> let a = 25 ^ 3;
> console.log(a); //26
> ```
>
> 将25转换为二进制为: 11001，将3转换为二进制为: 11
>
> 则将11001和00011对应的每一位进行一次^操作则可得到11010即为26
> 
> 因此25^3的结果为26
>
## 左移<<
>左移n位相当于乘2的n次方(其中对被操作数进行一次Math.floor确保是整数，对结果再进行一次Math.floor)
> 二进制数学操作符
>
> 将数字转换为二进制后按照给定的位数向左移动
>
> 举例
>
> ```js
> let a = 2;
> let b = 2 << 5;
> console.log(b); //64
> ```
>
> 将2转换为二进制为10，再将10向左移动5位得1000000
>
> 左移会保留所操作数值的符号，例如-2左移5位会变成-64而不是64
>
## 有符号右移>>
> 右移n位相当于除2的n次方(其中对被操作数进行一次Math.floor确保是整数，对结果再进行一次Math.floor)
>
> 二进制数学操作符
>
> 将数字转换为二进制后按照给定的位数向右移动
>
> 举例
>
> ```js
> let a = 64;
> let b = a >> 5;
> console.log(b); //2
> ```
>
> 将64转换为二进制为1000000，再将其右移5位得到10
>
> 右移会保留所操作值的符号，例如-64右移5位会变成-2而不是2
>
## 无符号右移>>>
>
> 正数的无符号右移与整数的有符号右移相同，而对于负数来说却不是一样的(因为无符号右移会将所有二进制数都当成正数)，详情见书p63-p64

# 布尔操作符

## 逻辑非!
>
> 逻辑非始终返回布尔值，将操作的数进行布尔值转换，再对齐进行取反
>
> 详细运算规则见书p64
>
> 同时使用两个感叹号!!相当于调用了转型函数Boolean()，无论操作数是什么类型，第一个感叹号总是返回操作数的布尔值的取反，第二个感叹号再将该取反再取反得到操作数真正的布尔值

## 逻辑与&&
> | 第一个操作数 | 第二个操作数 | 结果  |
> | ------------ | ------------ | ----- |
> | true         | true         | true  |
> | true         | false        | false |
> | false        | true         | false |
> | false        | false        | false |
>
> 如果有操作数不是布尔值，则遵循如下规则返回：
>
> - 如果第一个操作数是对象，则返回第二个操作数
> - 如果第二个操作数是对象，则只有在第一个操作数求值为true才会返回对象
> - 如果两个操作数都是对象，则返回第二个操作数
> - 如果有一个操作数是null，则返回null
> - 如果有一个操作数是NaN，则返回NaN
> - 如果有一个操作数是undefined，则返回undefined
>
> 逻辑与&&具有短路特性: 意思就是如果第一个操作数决定了结果，那么就永远不会对第二个操作数求值。体现在逻辑与&&身上就表现为如果第一个值是false那么就不会对第二个操作数求值而是直接输出，如果第一个操作数的值为true，那么才会对第二个操作数求值
>
> 举例：
>
> ```js
> let a = true;
> let b = a && undeclared; // 报错，因为undeclared没有事先声明 ReferenceError: unclared is not defined
> console.log(b); // 这一行没有被执行
> ```
>
> ```
> let a = false;
> let b = a && undeclared; // 没有报错，即使undeclared没有声明，但是因为短路特性而且a是false，所以直接略过了对undeclared这个变量的计算，直接执行了下一行并成功输出结果
> console.log(b); // 被执行 ， 结果为false
> ```
>
## 逻辑或||
> | 第一个操作数 | 第二个操作数 | 结果  |
> | ------------ | ------------ | ----- |
> | true         | true         | true  |
> | true         | false        | true  |
> | false        | true         | true  |
> | false        | false        | false |
>
> 如果有操作数不是布尔值，则遵循如下规则:
>
> - 如果第一个操作数是对象，则返回第一个操作数
> - 如果第一个操作数求值为false，则返回第二个操作数
> - 如果两个操作数都是对象，则返回第一个操作数
> - 如果两个操作数都是null，则返回null
> - 如果两个操作数都是NaN，则返回NaN
> - 如果两个操作数都是undefined，则返回undefined
>
> 与逻辑与相同，逻辑或也有短路特性，不同的是，如果逻辑或的第一个数是true，则不会对第二个操作数进行计算
>
> 举例
>
> ```js
> let a = false;
> let b = a && undeclared; // 报错，因为undeclared没有事先声明 ReferenceError: unclared is not defined
> console.log(b); // 这一行没有被执行
> ```
> ```js
> let a = true;
> let b = a || undeclared; //没有报错，即使undeclared没有声明，但是因为短路特性而且a是true，所以直接略过了对undeclared这个变量的计算，直接执行了下一行并成功输出结果
> console.log(b); //被执行，结果为true
> ```
>
> 逻辑或的短路特性使得逻辑或可以用来避免对变量赋值null或者undefined
>
> 举例
>
> ```js
> let a = b || c;
> ```
>
> 在这个例子中，a会被赋予b或者c的值，如果a不是null或者undefined，则a被赋予b的值，如果b是null或者undefined，则a会被赋予c的值

# 乘性操作符

> 会将一些非数值用Number()进行自动的类型转换，这意味着空字符串会返回0，而true会返回1

## 乘法操作符*
>  计算两个数值的乘积，更多特殊的规则见书p66-p67

## 除法操作符/
> 计算第一个操作数除以第二个操作数的商，更多特殊行为见书p67

## 取模操作符%
> 计算第一个操作数初一第二个操作数的余数，更多特殊行为见书p67

# 指数操作符**
## Math.pow()现在有了自己的操作符**
> ```js
> console.log(Math.pow(3,2)); //9
> console.log(3 ** 2 ); //9
> ```
>
## 指数赋值操作符**=
> ```js
> let a = 3;
> a **= 2;
> console.log(a); //9
> ```

# 加性操作符
## 加法操作符+
> 用于求两个数的和，也可以用来拼接字符串，其他特殊行为见书p68
>
> 此处重点是字符串拼接的用法，如果两个操作数都是字符串，则直接将第二个操作数拼接在第一个操作数后面，如果两个操作数中只有一个是字符串，那么会自动将另外一个操作数转换成字符串，然后再按先后顺序进行拼接
>
> 举例
>
> ```js
> let a = 5 + 5;
> console.log(a); // 10
> let b = 5 + "5";
> console.log(b); // "55"
> ```
>
> 最容易犯的错误就是忽略加法操作符中的数据类型
>
> ```
> let a = 5;
> let b = 10;
> let message = "the sum of 5 and 10 is" + a + b;
> console.log(message); //"the sum of 5 and 10 is 510"
> let message1 = "the sum of 5 and 10 is" + (a + b);
> console.log(message1); // "the sum of 5 and 10 is 15"
> ```
>
> 会出现上述例子的情况是因为加法操作符从左到右，message中是前面的字符串先和a相加所以a变成了字符串"5"而不是数值5，然后再和b相加b也变成了字符串，所以变成了510而不是15，而message1中将a+b用括号括了起来，它们的运算有优先级，所以结果是15
>
## 减法操作符-
> 与加法操作符不同的是，它不能剪切字符串，也就是如果两个操作数中有字符串会将字符串转换为数值来进行减法运算
>
> 其他特殊行为见书p69

# 关系操作符

> 一共四个关系操作符分别是< > <=  >=，用法与正常数学用法一致
>
> 特殊行为见书p70
>
> ==其中如果两个操作数都是字符串那么会逐个比较两者的字符串对应字符的编码大小==
>
> 例如
>
> ```
> let a = "brick" > "alpha";
> console.log(a); //true
> ```
>
> 这里注意字母大小写的编码大小不一样，所以会出现下面这种情况
>
> ```
> let a = "Brick" > "alpha";
> console.log(a); //false
> ```
>
> 这是因为B的编码为66，而a的编码为97
>
> 另外，在进行比较时，如果有一个操作数是Number类型，那么会将另外一个操作数也转换成Number类型来进行比较，例如
>
> ```js
> let a = "23" < "3";
> let b = "23" < 3;
> console.log(a); //true
> console.log(b); //false
> ```
>
> 任何有NaN参与的比较都返回false

# 相等操作符

## 等于和不等于== !=
>
> 用于比较两个操作数是否相等，返回值为true和false
>
> 会自动转换为Numbr类型，任何有NaN参与的比较==都会返回false !=都会返回true
>
> 特殊行为见书p71

## 全等和全不等 !== ===
>
> 全等和等于的区别就是全等不会自动转换操作数的数据类型，全不等和不等的区别同理

# 条件操作符

> variable = boolean_expression ? true_value : false_value;
>
> 举例
>
> ```js
> let max = (a > b)? a : b; //如果a>b成立，那么将a赋值给max，反之将b赋值给max
> let max = (a > b)? b : a; //如果a>b成立，那么将b赋值给max，反之将a赋值给max
> ```

# 赋值操作符=

> 简单的赋值操作符用=表示，复合型赋值操作符见书p73

# 逗号操作符,

> let a = 1, b =2 ,c = 3;
>
> 在一条语句中执行多个操作
>
> 辅助赋值
>
> ```js
> let a = (5, 4, 3, 2, 1); // a = 1
> ```
>
> 这种辅助赋值的用法并不常见

# if语句

> if (condition) statement else statement2
>
> 举例
>
> ```js
> if (i > 25) {
>     console.log("greater than 25")
> } else if (i < 0) {
>     console.log("less than 0")
> } else {
>     console.log("between 0 and 25, inclusive.")
> }
> ```

# do-while语句

> do-while语句是一种后测试循环，先执行循环体中的代码，再对条件求值，换句话说，循环体中的代码至少执行一次
>
> do {
>
> statement
>
> } while(expression);
>
> ```js
> let a =0;
> do {
> i += 2;
> } while (i < 10);
> ```

# while语句

> while语句是先测试循环，循环体内的语句可能一次都不会被执行
>
> while (expression) statement
>
> 举例
>
> ```js
> let i = 0;
> while (i < 10) {
> i += 2;
> }
> ```

# for语句

> for语句是先测试语句
>
> for (initialization; expression; post-loop-expression) statement
>
> 举例
>
> ```js
> let a = 10;
> for (let i = 0; i< a ; i++) {
> console.log(i);
> }
> //执行顺序为let i = 0/ 判断i<a /符合条件则console.log(i) /最后i++  此时一个循环周期结束
> ```

# for-in语句

> 严格的迭代语句，用于枚举对象中的非符号键属性
>
> for (property in expression) statement
>
> 举例
>
> ```js
> for (const propName in window) {
> document.write(propName);
> }
> ```
>
> 循环显示了BOM对象window的所有属性，每次执行循环，都会给变量propName赋予一个window对象的属性作为值，直到window的所有属性都被枚举一遍
>
> 对象的属性是无序的，for-in语句不能保证返回对象属性的顺序，所有可以枚举的属性都会返回一次，返回顺序因浏览器而异
>
> for-in迭代的变量是null或undefined，则不执行循环体

# for-of语句

> 严格的迭代语句，用于遍历可迭代对象的元素
>
> for (property of expression) statement
>
> 举例
>
> ```js
> for (const el of [2,4,6,8]) {
> document.write(el);
> }
> ```
>
> 书p76

# break和continue语句

> break用于立即退出循环，强制执行循环后的下一条语句，continue也用于立刻退出循环，但会再次从循环顶部开始执行，再退出循环
>
> 书p76-p78

# with语句

> 主要适用场景是针对一个对象反复操作
>
> with (expression) statement;
>
> 举例
>
> ```js
> let qs = location.search.substring(1);
> let hostname = location.hostname;
> let url = location.href;
> 
> 可改写为
> 
> with(location) {
> let qs = search.substring(1);
> let hostName = hostname;
> let url = href;
> }
> ```

# switch语句
>```js
> switch (expression) {
>
>  case value1:
>
>   statement
>
>    break;
>
>  case value2:
>
>   statement
>
>    break;
>
>  case value3:
>
>   statement
>
>    break;
>
>  case value4:
>
>   statement
>
>    break;
>
> } 
>```
> switch在比较条件的值时使用的是全等于
>
> 书p78-p80                                                                                                                 
# 函数声明/函数封装function
> https://juejin.cn/post/7073348417652523021
## 函数声明提升
> 只有函数声明方式有函数声明提升，函数声明会在任何代码执行之前先被读取并添加到执行上下文；js引擎会在执行代码时先扫描一遍，把发现的函数声明提升到源代码树的顶部，因此即使函数定义在调用这个函数的代码后面，引擎也会把函数声明提升到顶部；但如果是函数表达式的话，没有函数声明提升，那么就会报错
>
> ```js
> console.log(sum(1,2));
> function sum(a,b) {  //函数声明提升
>     return a+b
> }
> //3
> ```
>
> ```js
> console.log(sum(1,2));
> let sum = function (a,b) {  //函数表达式声明
>     return a+b
> }
> //Cannot access 'sum' before initialization
> ```
>
### 1 函数声明
>
> ```js
> function  sum(a ,b){ return a + b}
> ```
>
### 2 函数表达式声明  匿名函数  （兰姆达函数）
>
> ```js
> let sum= function(a,b){ return a + b};   // 这里的函数末尾的分号，和其他变量初始化是一样的。 
> ```
>
### 3 构造器声明
>
> ```js
> let sum = new Function("num1", "num2", "return num1 + num2");
> ```
>
### 4 箭头函数
>
> ```js
> let sum = (num1, num2) => { return num1 + num2; };
> ```
>
### 5 对象中函数简写
>
> ```js
> const o = {
> say() {
>  return "Hello!";
> }
> };
> 
> // 等同于
> 
> const o = {
> say: function() {
>  return "Hello!";
> }
> };
> ```
>
### var
> 当用var声明变量并接住函数时，并且再用方法一声明一个同名的函数时，该名字函数代表的是前者
>
> 因为重名时一般变量优先级>函数
>
> 当用let和const则不会出现这种情况，会直接报错
>
> ```js
> function  sum(){ console.log(1);}
> var sum= function(){console.log(2);};
> var sum = new Function('console.log(3)');
> var sum = () => { console.log(4) };
> sum()
> //第一二行同时运行结果为2
> //第一三行同时运行结果为3
> //第一四行同时运行结果为4
> //剩下的排列组合遵循覆盖原则
> ```

# 原始值与引用值
## 原始值与引用值
> 原始值是指6种数据类型的数据: undefined、null、Boolean、number、string和symbol，==原始值不能有属性(但是可以有方法)，给原始值添加属性时，会在那段代码运行时临时创建一个该原始值的对象，在这段代码过后该对象就被销毁了书p114==，按值访问
>
> 引用值是(原型链) 引用值可以有属性，JavaScript不允许直接访问内存位置，不能直接操作对象所在的内存空间，所以操作对象是操作该对象的引用而不是对象本身，==按引用(by reference)==访问
>
> ```js
> let a = "ounce";
> let b = new String("sanwoo");
> a.age = 19;
> b.age = 20;
> console.log(a.age);  //undefined
> console.log(b.age);  //20
> console.log(typeof(a));  //string
> console.log(typeof(b));  //object
> ```
>
## 复制值
>
> > 对于原始值来说，原始值的复制值和原始值之间是相互独立的互不干扰
>
> ```js
> let a = 1;
> let b = a;
> console.log(a);  //1
> console.log(b);  //1
> b = a + 1;
> console.log(a);  //1
> console.log(b);  //2
> ```
>
> 对于引用值来说，引用值的复制值和引用值都是指向内存中的同一对象的指针，复制值和引用值互相影响，值始终一样
>
> ```js
> let a = new Object();
> let b = a;
> console.log(a.name); //undefined
> console.log(b.name); //undefined
> b.name = "ounce";
> console.log(a.name); //ounce
> console.log(b.name); //ounce
> 
> /* 此处用b.name的原因是用b = "ounce"会导致b的类型变成string而不是object */
> ```

## 传递参数
>JavaScript种所有的函数的传递参数都是按值传递，但在传入的参数是引用值时有时可能发生意想不到的情况p86

## 确定类型
>instanceof操作符，书p87(涉及到原型链) 

# 内置对象

> Object、Array、String、Global、Math

# offset系列

> ![image-20220603174844988](images/image-20220603174844988.png)
>
> ![image-20220603175116292](images/image-20220603175116292.png)

# 原型和原型链

> ![image-20220604164009276](images/image-20220604164009276.png)
>
> ![image-20220604164034601](images/image-20220604164034601.png)
>
> 所有的原型链最后都指向对象object，这就是js的万物皆对象
>
> 上述是new的过程，要注意new和没有new的区别
>
> 例如
>
> > ![image-20220604164251651](images/image-20220604164251651.png)
>
> new一个对象的过程(==构造函数都是默认返回return this 在new的过程中this指向对象 在没有new的过程中this指向单纯的值==)
>
> > ![image-20220604164719197](images/image-20220604164719197.png)

# DOM选择器 getElementById getElementsByTagName querySelector querySelectorAll

## getElementById
> 返回一个element对象(不是伪数组)，只有document有这个方法，body和其他元素上没有
>
> var element = document.getElementById(id);
>
> 举例
>
> html:
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
>   <title>getElementById example</title>
> </head>
> <body>
>   <p id="para">Some text here</p>
>   <button onclick="changeColor('blue');">blue</button>
>   <button onclick="changeColor('red');">red</button>
> </body>
> </html>
> ```
>
> javascript:
>
> ```js
> function changeColor(newColor) {
>   var elem = document.getElementById('para');
>   elem.style.color = newColor;
> }
> ```
>
> [document.getElementById - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)
>
## getElementByTagName
> 返回元素集合，以伪数组形式显示(即便只有0个1个元素，都返回集合用伪数组表示)
>
> var elements = document.getElementsByTagName(name);
>
> 在调用getElementsByTagName这个api时前面的必须是一个指定的父元素，例如上述中的document就是一个父元素(全局文档)，那么数组中的元素想调用就应该用数组下标定位数组中具体的元素(例如下述例子中的lis[0])
>
> ```js
> <body>
>     <ol>
>         <li>1</li>
>         <li>2</li>
>         <li>3</li>
>     </ol>
>     <script src="ceshijs.js"></script>
> </body>
> 
> let lis = document.getElementsByTagName("ol");
> console.log(lis[0].getElementsByTagName("li"));
> ```
>
> [Document.getElementsByTagName() - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName)
>
## getElementsByClassName
>与getElementByTagName基本相似
>
>[Element.getElementsByClassName() - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getElementsByClassName)
>
## querySelector
> 根据指定选择器返回第一个元素对象(不是伪数组)
>
> element = document.querySelector(selectors); //selectors可以是标签名、id、class等
>
> ```js
> <body>
>     <ol>
>         <li>1</li>
>         <li>2</li>
>         <li>3</li>
>     </ol>
>     <script src="ceshijs.js"></script>
> </body>
> 
> 
> console.log(document.querySelector("li"));
> ```
>
> [document.querySelector() - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector)
>
## querySelectorAll
> 根据选择器返回所有元素对象集合，并以伪数组的形式表现出来
>
> ```js
> elementList = parentNode.querySelectorAll(selectors);
> ```
>
> [Document.querySelectorAll - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll)

# 事件

> 事件三要素
>
> > 事件源: 指的是发起这个事件的主语
> >
> > 事件类型: 如何触发，什么事件
> >
> > 事件处理程序/事件监听器/事件处理器: 触发后执行什么,==是全局事件处理器的一种属性==
>
> 事件对象(event)
>
> > 传给事件处理器的唯一参数
>
> 事件类型
>
> > 用户界面事件
> >
> > 焦点事件
> >
> > 鼠标事件
> >
> > > mouseover实现事件冒泡，而mouseenter不实现事件冒泡
> >
> > 滚轮事件
> >
> > 输入事件
> >
> > 键盘事件
> >
> > 合成事件
>
> ![image-20220603172512548](images/image-20220603172512548.png)
>
> 事件代理/事件委托/事件委派
>
> > 不在每个子节点设置事件监听器，而是在它们共同的父节点上设置事件监听器，然后利用事件冒泡原理影响到每个子节点
>
> e.target和this的区别，前者是指向我们点击的的对象，无论点击事件是否冒泡，后者则是指向绑定事件的对象，没有事件冒泡时和前者一样，有时间冒泡时会返回冒泡的终点也就是绑定了事件的那个对象

# DOM和节点

> document.querySelector()中的document是指的document节点同时也是一种接口，element同理
>
> 
>
> DOM树中所有内容都是节点Node类型
>
> 这些内容除了都是Node类型外还各自有自己的细分类型分别为Document类型(包含了HTMLDocument类型)、Element类型(包含了HTMLElement类型)、Text类型、Comment类型、CDATASection类型、DocumentType类型、DocumentFragment类型、Attr类型
>
> (同时HTMLDocument类型又包含了HTMLElement类型)