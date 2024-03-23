---
title: Vue3快速上手
date: 2024-03-15 21:03:14
tags:
---

# Vue3快速上手

<img src="https://user-images.githubusercontent.com/499550/93624428-53932780-f9ae-11ea-8d16-af949e16a09f.png" style="width:200px" />



## 1.Vue3简介

- 2020年9月18日，Vue.js发布3.0版本，代号：One Piece（海贼王）
- 耗时2年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99位贡献者](https://github.com/vuejs/vue-next/graphs/contributors) 
- github上的tags地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0

## 2.Vue3带来了什么

### 1.性能的提升

- 打包大小减少41%

- 初次渲染快55%, 更新渲染快133%

- 内存减少54%

  ......

### 2.源码的升级

- 使用Proxy代替defineProperty实现响应式

- 重写虚拟DOM的实现和Tree-Shaking

  ......

### 3.拥抱TypeScript

- Vue3可以更好的支持TypeScript

### 4.新的特性

1. Composition API（组合API）

   - setup配置
   - ref与reactive
   - watch与watchEffect
   - provide与inject
   - ......
2. 新的内置组件
   - Fragment 
   - Teleport
   - Suspense
3. 其他改变

   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除keyCode支持作为 v-on 的修饰符
   - ......

# 一、创建Vue3.0工程

## 1.使用 vue-cli 创建

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

## 2.使用 vite 创建

官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite官网：https://vitejs.cn

- 什么是vite？—— 新一代前端构建工具。
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。
- 传统构建 与 vite构建对比图

<img src="https://cn.vitejs.dev/assets/bundler.37740380.png" style="width:500px;height:280px;float:left" /><img src="https://cn.vitejs.dev/assets/esm.3070012d.png" style="width:480px;height:280px" />

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

# 二、常用 Composition API

官方文档: https://v3.cn.vuejs.org/guide/composition-api-introduction.html

## 1.拉开序幕的setup

1. 理解：Vue3.0中一个新的配置项，值为一个函数。
2. setup是所有<strong style="color:#DD5145">Composition API（组合API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。
4. 组件中所用到的：数据、方法等等，均要配置在setup中。
5. setup函数的两种返回值：
   1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
   2. <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span>
6. 注意点：
   1. 尽量不要与Vue2.x配置混用
      - Vue2.x配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup中的属性、方法。
      - 但在setup中<strong style="color:#DD5145">不能访问到</strong>Vue2.x配置（data、methos、computed...）。
      - 如果有重名, setup优先。
   2. setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）

##  2.ref函数

- 作用: 定义一个响应式的数据
- 语法: ```const xxx = ref(initValue)``` 
  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference对象，简称ref对象）</strong>。
  - JS中操作数据： ```xxx.value```
  - 模板中读取数据: 不需要.value，直接：```<div>{{xxx}}</div>```
- 备注：
  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠``Object.defineProperty()``的```get```与```set```完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了Vue3.0中的一个新函数—— ```reactive```函数。

## 3.reactive函数

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用```ref```函数）
- 语法：```const 代理对象= reactive(源对象)```接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy的实例对象，简称proxy对象）</strong>
- reactive定义的响应式数据是“深层次的”。
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

## 4.Vue3.0中的响应式原理

### vue2.x的响应式

- 实现原理：
  - 对象类型：通过```Object.defineProperty()```对属性的读取、修改进行拦截（数据劫持）。
  
  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。
  
    ```js
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```

- ==存在问题==：
  
  - 新增属性、删除属性、界面不会更新。
  
  - 直接通过下标修改数组, 界面不会自动更新。
  
    这些问题vue2需要人工解决，解决方法是？

### Vue3.0的响应式

- 实现原理: 
  - 通过Proxy（代理）:  拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
  - 通过Reflect（反射）:  对源对象的属性进行操作。
  - MDN文档中描述的Proxy与Reflect：
    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
    
    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
    
      ```js
      new Proxy(data, {
      	// 拦截读取属性值
          get (target, prop) {
          	return Reflect.get(target, prop)
          },
          // 拦截设置属性值或添加新属性
          set (target, prop, value) {
          	return Reflect.set(target, prop, value)
          },
          // 拦截删除属性
          deleteProperty (target, prop) {
          	return Reflect.deleteProperty(target, prop)
          }
      })
      
      proxy.name = 'tom'   
      ```

## 5.reactive对比ref

-  从定义数据角度对比：
   -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
   -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
   -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。
-  从原理角度对比：
   -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
   -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。
-  从使用角度对比：
   -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
   -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

## 6.setup的两个注意点

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。
  
- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。


## 7.计算属性与监视

### 1.computed函数

- 与Vue2.x中computed配置功能一致

- 写法

  ```js
  import {computed} from 'vue'
  
  setup(){
      let person = reactive({...})
      ...
  	//计算属性——简写，默认只有getter，没有考虑setter的情况
      //这里可以直接写person.fullName = computed...，这样不需要return fullName只要return person就行，如果按照下面的写法那么还是需要return fullName
      let fullName = computed(()=>{                   
          return person.firstName + '-' + person.lastName
      })
      //计算属性——完整
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
      return {
          person,
          fullName
      }
  }
  ```

### 2.watch函数

- ==官方文档中监听ref时，直接写变量名而不是箭头函数，监听reactive时使用箭头函数(不论是监听reactive整体还是内部某个属性)，这三种情况deep和immediate都奏效==

- ==同时监听多个数据源，如果是ref直接加[]，如果是reactive则先箭头函数后加[]==

  ```JavaScript
  //正常情况：监视ref定义的响应式数据，监视多个ref定义的响应式数据，箭头函数监听reactive整体，箭头函数监听单个reactive内部属性，箭头函数监听多个reactive内部属性
  watch(sum,(newValue,oldValue)=>{
  	console.log('sum变化了',newValue,oldValue)
  },{immediate:true})
  
  watch([sum,msg],(newValue,oldValue)=>{
  	console.log('sum或msg变化了',newValue,oldValue)
  }) 
  
  watch(() => person,(newValue,oldValue)=>{
  	console.log('person变化了',newValue,oldValue)
  })//oldValue无法正确获取
  
  watch(()=>person.job,(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  }) 
  
  watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  })
  ```

- 几个小“坑”：

  - 监听reactive整体但是不用箭头函数时：oldValue无法正确获取、强制开启了深度监视（immediate有效，deep失效）。

    ```JavaScript
    /* 监视reactive定义的响应式数据
    			若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
    			若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
    */
    watch(person,(newValue,oldValue)=>{
    	console.log('person变化了',newValue,oldValue)
    },{immediate:true,deep:false}) //此处的deep配置不再奏效
    ```

  - 监听reactive整体时使用箭头函数：oldValue无法正确获取，immediate和deep有效

    ```JavaScript
    //箭头函数监听reactive整体
    watch(() => person,(newValue,oldValue)=>{
    	console.log('person变化了',newValue,oldValue)
    })//oldValue无法正确获取
    ```

  - 只有在监听的reactive是一个数组且使用箭头函数时将数组...解构再加上[]成为一个数组时，oldValue才可以正确获取

    ```JavaScript
    const numbers = reactive([1, 2, 3, 4])
    
    watch(
      () => [...numbers],
      (numbers, prevNumbers) => {
        console.log(numbers, prevNumbers)
      }
    )
    
    numbers.push(5) // logs: [1,2,3,4,5] [1,2,3,4]
    ```

  - 监听reactive内部属性但是不用箭头函数：watch失效，但是immediate:true时可以执行一次watch内语句

    ```JavaScript
    //监听reactive内部属性但不用箭头函数
    watch(person.job,(newValue,oldValue)=>{
    	console.log('person的job变化了',newValue,oldValue)
    },{immediate:true}}//watch直接失效，但是immediate:true可以执行一次watch内部语句
    ```

  - 监听ref时使用箭头函数：watch失效，但是immediate:true时可以执行一次watch内语句

    ```javascript
    //箭头函数监听ref
    watch(()=> sum,(newValue,oldValue)=>{
        console.log('sum变化了',newValue,oldValue)
    },{immediate:true}) //watch直接失效，但是immediate:true可以执行一次watch内部语句
    ```

  - ref是对象时，直接监听：需要开启deep:true才能深度监听，但oldValue无法正确获取

    ```javascript
    //ref是对象时直接监听
    let person = ref({
    ...
    })
    watch(person,(newValue,oldValue)=>{
        console.log(newValue,oldValue)
    },{immediate:true,deep:true})//开启deep:true能深度监听，但oldValue无法正确获取
    ```

  - ref是对象时，直接监听它的value：强制开启深度监听，oldValue无法正确获取

    ```javascript
    //ref是对象时直接监听它的value
    let person = ref({
    ...
    })
    watch(person.value,(newValue,oldValue)=>{
        console.log(newValue,oldValue)
    },{immediate:true})//开启deep:true能深度监听，但oldValue无法正确获取
    ```

  - ref是对象时，箭头函数监听它：返回的是两个RefImpl

    ```javascript
    //ref是对象时箭头监听它
    let person = ref({
    ...
    })
    watch(() => person,(newValue,oldValue)=>{
        console.log(newValue,oldValue)
    },{immediate:true})//返回两个RefImpl
    ```

  - ref是对象时，箭头函数监听它的value：返回两个proxy

    ```javascript
    //ref是对象时箭头监听它的value
    let person = ref({
    ...
    })
    watch(() => person.value,(newValue,oldValue)=>{
        console.log(newValue,oldValue)
    },{immediate:true})//返回两个proxy
    ```

    

    

    

### 3.watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect有点像computed：

  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(()=>{
      const x1 = sum.value
      const x2 = person.age
      console.log('watchEffect配置的回调执行了')
  })
  ```

## 8.生命周期

<div style="border:1px solid black;width:380px;float:left;margin-right:20px;"><strong>vue2.x的生命周期</strong><img src="https://cn.vuejs.org/images/lifecycle.png" alt="lifecycle_2" style="zoom:33%;width:1200px" /></div><div style="border:1px solid black;width:510px;height:985px;float:left"><strong>vue3.0的生命周期</strong><img src="https://v3.cn.vuejs.org/images/lifecycle.svg" alt="lifecycle_2" style="zoom:33%;width:2500px" /></div>





































1

- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
  - ```beforeDestroy```改名为 ```beforeUnmount```
  - ```destroyed```改名为 ```unmounted```
- Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
  - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

- 如果同时使用原生生命周期钩子和composition api形式的生命周期钩子，两者是后者优先级更高，更先执行，原生不用引入，composition api形式需要引入

## 9.自定义hook函数

- 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。

- 类似于vue2.x中的mixin。

- 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。



## 10.toRef

为reactive内部的一个属性创建ref，ref会被传递，并且保持和原reactive内部的属性的响应式关系，而如果使用const fooRef = ref(state.foo)，则相当于是只讲state.foo这个变量代表的值传给了ref()，这样fooRef和state.foo之间没有响应式关系(相当于一次基本类型数据赋值)

![image-20220713145342316](images/image-20220713145342316.png)


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```

  ![image-20220713150958248](images/image-20220713150958248.png)

  ![image-20220713151013672](images/image-20220713151013672.png)

  此处是对象解构，更简单的方法是用一个变量接住toRefs返回值然后再在return中用...直接解构这个变量

  ![image-20220713151257384](images/image-20220713151257384.png)

  


# 三、其它 Composition API

## 1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

## 2.readonly 与 shallowReadonly

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

## 3.toRaw 与 markRaw

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。(==readonly是没有资格更改数据，markRaw是可以更改但是不会响应式==)
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

## 4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

  ```vue
  <template>
  	<input type="text" v-model="keyword">
  	<h3>{{keyword}}</h3>
  </template>
  
  <script>
  	import {ref,customRef} from 'vue'
  	export default {
  		name:'Demo',
  		setup(){
  			// let keyword = ref('hello') //使用Vue准备好的内置ref
  			//自定义一个myRef
  			function myRef(value,delay){
  				let timer
  				//通过customRef去实现自定义
  				return customRef((track,trigger)=>{
  					return{
  						get(){
  							track() //告诉Vue这个value值是需要被“追踪”的
  							return value
  						},
  						set(newValue){
  							clearTimeout(timer)
  							timer = setTimeout(()=>{
  								value = newValue
  								trigger() //告诉Vue去更新界面
  							},delay)
  						}
  					}
  				})
  			}
  			let keyword = myRef('hello',500) //使用程序员自定义的ref
  			return {
  				keyword
  			}
  		}
  	}
  </script>
  ```

  

## 5.provide 与 inject

<img src="https://v3.cn.vuejs.org/images/components_provide.png" style="width:300px" />

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```js
     setup(){
     	......
         let car = reactive({name:'奔驰',price:'40万'})
         provide('car',car)
         ......
     }
     ```

  2. 后代组件中：

     ```js
     setup(props,context){
     	......
         const car = inject('car')
         return {car}
     	......
     }
     ```

## 6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

# 四、Composition API 的优势

## 1.Options API 存在的问题

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。

<div style="width:600px;height:370px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f84e4e2c02424d9a99862ade0a2e4114~tplv-k3u1fbpfcp-watermark.image" style="width:600px;float:left" />
</div>
<div style="width:300px;height:370px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5ac7e20d1784887a826f6360768a368~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;width:560px;left" /> 
</div>















## 2.Composition API 的优势

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

<div style="width:500px;height:340px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc0be8211fc54b6c941c036791ba4efe~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>
<div style="width:430px;height:340px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cc55165c0e34069a75fe36f8712eb80~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>













# 五、新的组件

## 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

## 2.Teleport

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。

  ```vue
  <teleport to="移动位置">
  	<div v-if="isShow" class="mask">
  		<div class="dialog">
  			<h3>我是一个弹窗</h3>
  			<button @click="isShow = false">关闭弹窗</button>
  		</div>
  	</div>
  </teleport>
  ```

## 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```

# 六、其他

## 1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。
  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |
  

## 2.其他改变

- data选项应始终被声明为一个函数。

- 过渡类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......

#  七、组合式api中使用mapState等

> https://juejin.cn/post/7044460212522057736
>
> 首先我们先看官方的用例。
>
> [![img](images/2022032707485384.png)](https://www.wangxuelong.vip/wp-content/uploads/2022/03/2022032707485384.png)
>
> 
>
> 当然也可以展开列表格式的。
>
> ```js
> ...mapState(['count''name'])
> ```
>
> 
>
> 在setup里，官方依然是使用计算属性，但是当我们在一个页面中需要很多state数据时，代码就会变得很臃肿。
>
> [![img](images/202203270752329.png)
>
> 
>
> 封装前的分析
>
> 
>
> 既然官方都是放在计算属性里，那我们不妨先从计算属性开始分析。
>
> ```js
> computed:{
> 
>  // funName:function(){return 'value'}
> 
>  ...mapState(['count','name']),
> 
> }
> ```
>
> 从上边的代码可以看出，计算属性里面都是一个一个函数。（关于计算属性可以参考这篇文章[计算属性](https://www.wangxuelong.vip/3585.html#计算属性computed)），那么我们就可以得出count也是一个函数。
>
> 
>
> mapState返回的是一个对象，对象里的一个个key对应一个个函数
>
> 
>
> ```js
> computed:{
> 
>  // funName:function(){return 'value'}
> 
>  ...mapState(['count','name']),
> 
>  // 说明count就一个这样的函数
> 
>  // count:function(){}
> 
> }
> ```
>
> 根据上边的分析后，再结合vue2，假如我们理想中的便捷写法如下。
>
> ```vue
> <template>
> 
> <h2>{{count}}</h2>
> 
> </template>
> 
> <script>
> 
> import {mapState,useStore} from 'vuex'
> 
> export default {
> 
> name: 'App',
> 
> setup(props) {
> 
>  const store = useStore()
> 
>  const storeState = mapState(['count','name'])
> 
>  return{
> 
>    ...storeState
> 
>  }
> 
> }
> 
> }
> 
> </script>
> ```
>
> 但是结果却不是我们想要的结果。
>
> [![img](images/2022032708041362.png)](https://www.wangxuelong.vip/wp-content/uploads/2022/03/2022032708041362.png)
>
> 可以看出来这是一个函数。
>
> 接下来我们再根据官方的这句代码以及结合上边的解释
>
> ```js
> const sCount = computed(()=>{store.state.count}) //computed ref对象
> ```
>
> 然后我们尝试往计算属性里批量存入这些函数。
>
> ```js
> setup(props) {
> 
>  const storeStateFns = mapState(['count','name'])
> 
>  const storeState = {}
> 
>  Object.keys(storeStateFns).forEach(fnnKey=>{
> 
>    const fn = storeStateFns[fnnKey]
> 
>    // {count:ref,name:ref}
> 
>    storeState[fnnKey]=computed(fn)
> 
>    //这里读fn的时候是根据this.$store.state.count来读的，所以会报错，而且在setup里没有this。
> 
>  })
> 
>  return{
> 
>    ...storeState
> 
>  }
> 
> }
> ```
>
> 但是最后还是爆了错。
>
> [![img](images/2022032708255599.png)](https://www.wangxuelong.vip/wp-content/uploads/2022/03/2022032708255599.png)
>
> 报错的愿意就是读fn的时候是根据this.$store.state.count来读的，所以会报错，而且在setup里没有this。
>
> 所以我们可以使用bind（可以参考[bind](https://www.wangxuelong.vip/3450.html#bind)）
>
> ```js
> setup(props) {
> const store = useStore()
> const storeStateFns = mapState(['count','name'])
>  const storeState = {}
>  Object.keys(storeStateFns).forEach(fnnKey=>{
>    const fn = storeStateFns[fnnKey].bind({$store:store})
>    storeState[fnnKey]=computed(fn)
>  })
>  return{ 
>    ...storeState
>  }
> }
> ```
>
> 此时就可以正常使用了
>
> [![img](images/2022032708374210.png)](https://www.wangxuelong.vip/wp-content/uploads/2022/03/2022032708374210.png)
>
> ## 封装辅助函数
>
> ```js
> import {mapState,useStore} from 'vuex'
> import {computed} from 'vue'
> export function useState(mapper){
>  //获取到对应的对象的function:{name:function,count:function}
>  const storeStateFns = mapState(mapper)
>  //把一个个数据转换成ref对象
> const store = useStore()
>  const storeState = {}
>  Object.keys(storeStateFns).forEach(funKey=>{
>    const fn = storeStateFns[funKey].bind({$store:store})
>    storeState[funKey]=computed(fn)
>  })
>  return storeState
> 
> }
> ```
>
> 现在我们就可以像开头期待那样使用了。
>
> ```vue
> <template>
> 
> <h2>{{count}}</h2>
> 
> </template>
> 
> <script>
> import {useState} from './hooks/useState.js'
> export default {
> name: 'App',
> setup(props) {
>  //可以传数组、对象
> const storeState = useState(['count','name'])
>  return{ 
>    ...storeState
>  }
> }
> } 
> </script>
> ```
>
> [![img](images/2022032709034284.png)](https://www.wangxuelong.vip/wp-content/uploads/2022/03/2022032709034284.png)