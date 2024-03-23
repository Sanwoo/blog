---
title: vue2$vue3
date: 2024-03-15 21:03:14
tags:
---

##### vue2

# vue2补充

> vue2和vue3的cdn不可以互用
>
> vue2 script标签可以不加type也能识别
>
> vue前面的new不能丢掉
>
> vue2基础案例
>
> > ```
> > <body>
> >     <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
> >     <div id="container">
> >         <h1>插值语法</h1>
> >         <hr>
> >         <h1>hello,{{name}}</h1>
> >         <hr>
> >         <h1>指令语法</h1>
> >         <hr>
> >         <a :href="url">点我进入</a>
> >     </div>
> >     <script type="text/javascript">
> >         new Vue({
> >             el: '#container',
> >             data: {
> >                 name: 'ounce',
> >                 url: 'http://www.baidu.com',
> >             }
> >         })
> >     </script>
> > </body>
> > 
> > ```
>
> 上述案例中出现过忘记加data的错误
>
> new vue写进div里面的错误
>
> 网址没有加http://无法访问的错误
>
> new Vue的V一定要大写小写v会报错
>
> vue实例的字面量表示法本身是简写的，原生的为'name': 'ounce'，一般省略了name的''
>
>  `{{name}}`可以写成`{{_data.name}}`但是不能写成`{{vm. _data.name}}`和`{{vm.name}}`，因为这些已经在vue的容器里面了写这些会让vue误以为你在调用vue中的名为vm的属性或者方法(因为`{{}}`调用时的执行上下文是vue实例，默认会在vue实例里面去寻找vm)，即使你将vm初始化为vm了，但在控制台中调用name则必须带上vm因为控制台的执行上下文是全局上下文
>
> > ![image-20220409165403610](images/image-20220409165403610.png)
>
> 在const vm = new Vue(){}里面可以使用vm.来调用，因为这相当于是vue实例里面调用自己(this)
>
> key/value对都可以简写成函数式
>
> ==插值语法中插入showInfo则是插入整个函数，插入showInfo()则是把这个函数的返回值插入进去==
>
> ==空路由会报错，并且导致app.vue中的内容不显示==
>
> 使用图片记得引用，并且盒子记得设置宽高
>
> 取消语法检查
>
> > vue.config.js加入lintOnSave: false
>
> 关于mounted拿不到data数据在钩子函数部分中有解释
>
> 
>
> > 模板在渲染时候，读取对象中的某个对象的属性值时，这个对象不存在，说通俗点就是三层表达式a.b.c，在对象a中没有对象b，那么读取对象a.b.c中的值，自然会报错。如果是两层表达式a.b则不会报错，返回的是undefined。
> >
> > **原因：**
> > 我们发现这里的queryData是vuex中state管理加载的数据，异步调用显示，然后vue渲染机制中：
> >
> > **异步数据先显示初始数据，再显示带数据的数据，**
> >
> > 所以上来加载queryData时候还是一个空数组
> >
> > 当渲染完成后，才加载异步数据
> > 所以在渲染时，出现的三层表达式在queryData中取queryData[0]数组中的下标为0的对象还不存在，再在这个对象中取其他值自然会报错，但是渲染完成后，queryData中的值加载好了，自然可以取到，这也就解释了为什么界面正常显示，但开发者工具会报错的原因
> >
> > 【解决方案】：
> > 在上面一个div中添加v-if判断条件，如果queryData[0]取不到，则不加载该div即可解决。（注意，不能用v-show，v-show的机制是加载后，根据条件判断是否显示）
> > ————————————————
> > 版权声明：本文为CSDN博主「蓝胖子的多啦A梦」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> > 原文链接：https://blog.csdn.net/Maxueyingying/article/details/123225053
> >
> > [修复报错 Error in render: “TypeError: Cannot read properties of undefined (reading ‘ipconfig‘)“_蓝胖子的多啦A梦的博客-CSDN博客](https://blog.csdn.net/Maxueyingying/article/details/123225053)
>
> 因为vue异步导致的dom节点中拿不到data中的三层及以上表达式的问题，v-if并不能完全解决问题，必要时候需要妥协多设置data项从而避免使用三层及以上表达式（a.b.c）
>
> 将data项写空时不要写null，即使后面通过api数据将null覆盖掉了，因为异步的原因控制台会报错null没有反应
>
> > ![image-20220521223232376](images/image-20220521223232376.png)
>
> 
>
> 通过v-if和v-else实现类似于播放暂停按钮的二者相斥图标显示
>
> 
>
> ==api文件夹将所有api调用方法写成js文件统一保存供全局使用，而vuex是将所有组件都需要的全局数据统一保存==
>
> 
>
> 由于异步的原因，图片链接在刚开始时是null导致控制台get类型报错，解决方法是条件操作符，异步数据没有成型时，将图片链接赋值为''，异步数据成型后将数据传给图片链接
>
> > ```
> > <img :src="`${songpics[index]? songpics[index]:''}`" alt="">
> > ```
> >
> > [vue GET http://localhost:8080/undefined/ 404 (Not Found)(已解决)_写bug给别人改的博客-CSDN博客](https://blog.csdn.net/weixin_46258925/article/details/123890919)
>
> 
>
> vue路由相关
>
> > 实现某个路由组件刚进入网站页面就显示只需要将它的path写成path:'/'
> >
> > 通过重定向可以让某个二级及以上路由默认显示，重定向也可以传参，但是还不知道怎么传动态参数，只知道怎么传静态参数？
>
> 
>
> > vuex通过mapState和mapGetters传过来的数据就相当于data和computed一样的，可以直接用
> >
> > 例如mapState传来playListIndex，可以直接在模板中用`{{playListIndex}}`，或者script中this.playListIndex来使用
> >
> > 但如果想要更改map传过来的数据，则需要通过this.$store.state.playListIndex = xxx来更改，或者dispatch和commit来方法更改数据，直接this.playListIndex = xxx更改是不奏效的
>
> 
>
> ==vue2使用vuex-persistedstate来实现vuex数据刷新后保留==
>
> > 本质上是利用sessionStorage来实现数据保留
> >
> > 安装：`npm install vuex-persistedstate --save`
> >
> > 在store下的index.js中，引入并配置
> >
> > ```text
> > import createPersistedState from "vuex-persistedstate"
> > 
> > const store = new Vuex.Store({
> >   // ...
> >   plugins: [createPersistedState()]
> > })
> > ```
> >
> > 此时可以选择数据存储的位置，可以是localStorage/sessionStorage/cookie，此处以存储到sessionStorage为例，配置如下：
> >
> > ```text
> > import createPersistedState from "vuex-persistedstate"
> > const store = new Vuex.Store({
> >   // ...
> >   plugins: [createPersistedState({
> >       storage: window.sessionStorage
> >   })]
> > })
> > ```
> >
> > 存储指定的state
> >
> > vuex-persistedstate默认持久化所有state，指定需要持久化的state,配置如下：
> >
> > ```text
> > import createPersistedState from "vuex-persistedstate"
> > const store = new Vuex.Store({
> >   // ...
> >   plugins: [createPersistedState({
> >       storage: window.sessionStorage,
> >       reducer(val) {
> >           return {
> >           // 只储存state中的user
> >           user: val.user
> >         }
> >      }
> >   })]
> > ```
> >
> > 此刻的val 对应store/modules文件夹下几个js文件存储的内容，也就是stor/index中import的几个模块，可以自己打印看看。希望哪一部分的数据持久存储，将数据的名字在此配置就可以，目前我只想持久化存储user模块的数据。
> >
> > 注意：如果此刻想配置多个选项，将plugins写成一个一维数组，不然会报错。
> >
> > ```text
> > import createPersistedState from "vuex-persistedstate"
> > import createLogger from 'vuex/dist/logger'
> > // 判断环境 vuex提示生产环境中不使用
> > const debug = process.env.NODE_ENV !== 'production'
> > const createPersisted = createPersistedState({
> >   storage: window.sessionStorage
> > })
> > export default new Vuex.Store({
> >  // ...
> >   plugins: debug ? [createLogger(), createPersisted] : [createPersisted]
> > })
> > ```
> >
> > [如何解决vuex页面刷新数据丢失问题？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/453359434)
>
> 
>
> vue的双击事件@dblclick
>
> audio的结束事件@ended 当前歌曲时长更新事件@timeupdate
>
> 
>
> vuex >> props >eventbus>emit>pubsub.js>mixin>query>params
>
> params刷新后参数丢失，query不会，但params和query都会因路由跳转丢失数据？(==路由传参只适合做登录验证等功能==)
>
> https://blog.csdn.net/liyunkun888/article/details/83269343
>
> 
>
> 倘若想用query或者params传输对象或者数组可以使用JSON.stringify()转为字符串再在子路由中通过JSON.parse()转换回来
>
> [vue路由传递对象数组，刷新页面，数据丢失[object Object](问题篇)_skyblue_afan的博客-CSDN博客_vue路由传数组](https://blog.csdn.net/skyblue_afan/article/details/119956020)
>
> 
>
> vue实现当前页面刷新的三种方法
>
> 这里是我本来想通过在输入框的回车事件中加入强制刷新来实现每次在输入框内回车搜索结果页面都刷新一次来解决vue路由跳转同一页面携带不同参数但是页面中用到该动态参数的地方不实时刷新这一问题，但是这样会导致输入框内的文本在单纯的路由跳转过程中本来会保留变的不保留(因为是相当于ctrl+f5强制刷新了一次)，而且provide/inject方法中要求不使用kee-alive或者是keep-alive中限制生效组件不包含搜索页面，在我自己测试中需要将==isRouterAlive==设置为vuex的state并将reload()方法中的==this.isRouterAlive==改写为==this.$store.state.isRouterAlive==才比较好用，限制条件过多步骤过于繁琐
>
> > location.reload和this.$router.go(0)，这种属于强制刷新页面，相当于ctrl+f5，会出现瞬间空白页面
> >
> > 
> >
> > 新建一个空白页面，先进入这个空白页面再跳转回来实现刷新页面，相当于空白页面做了中转
> >
> > 
> >
> > provide/inject组合
> >
> > ![image-20220601225226300](images/image-20220601225226300.png)
> >
> > 通过声明reload方法，控制router-view的显示或隐藏，从而控制页面的再次加载，这边定义了
> >
> > ```
> > isRouterAlive //true or false 
> > ```
> >
> > 然后在需要当前页面刷新的页面中注入App.vue组件提供（provide）的 reload 依赖，然后直接用this.reload来调用就行
> >
> > ![image-20220601225323652](images/image-20220601225323652.png)
> >
> > [vue项目刷新当前页面的三种方法_Linux小百科的博客-CSDN博客_vue刷新当前页面,重载页面数据](https://blog.csdn.net/yaxuan88521/article/details/123307992)
>
> 
>
> vue跳转相同路由 但是query参数不同不刷新问题
>
> 与上文不同的是，上文是想通过强制刷新来解决query参数不同不刷新的问题(毕竟刷新一次就行了)，这里主要是想通过监听$route，通过query参数变化来触发监听下的事件，并在该事件中进行赋值改变来实现页面数据更新，但是问题在于如果该页面下存在嵌套路由，低一级的路由间进行跳转也会导致query参数变化，但是其实该页面接收数据的来源并没有更新传过来的数据，只是低一级的路由间跳转默认将query和params重置为空了，所以这里的方法也存在局限性
>
> > 跳转路由的时候 如果url地址相同 但是参数不一样 跳转的时候不会刷新页面数据 因为当前页面的mounted已经执行完毕 参数变化不会触发mounted或created钩子 所以页面数据不会刷新
> >
> > （跳转的页面的url地址与原页面url只有参数不同 页面不刷新）
> >
> > 解决方法：添加侦听器watch侦听路由参数的变化 重置data里面的数据[Object.assgin(this.$data,this.$options.data())] 重新初始化数据
> >
> > ![image-20220601230714428](images/image-20220601230714428.png)
> >
> > ==此方法试过无效，this.init()报错，尚未明白原理，或许在app.vue中添加有效？==
> >
> > [vue跳转相同路由 但是query参数不同不刷新问题 - 简书 (jianshu.com)](https://www.jianshu.com/p/b715a841bff7)
> >
> > 
> >
> > 解决思路：
> >
> > 虽然下一个点击还是当前路由不会刷新当前路由，但我们知道r o u t e 路 由 数 据 此 时 是 发 生 了 变 化 的 ， 比 如 route路由数据此时是发生了变化的，比如route路由数据此时是发生了变化的，比如route里传递的参数信息会随着每次不同的点击发生变化，那么我们利用这一点使用vue的watch去检测这样的变化然后再重新请求数据请求接口即得到想要的数据
> >
> > ![image-20220601231015478](images/image-20220601231015478.png)
> >
> > ==此方法尚未尝试==
> >
> > [vue路由，解决同一路由页面多次触发不刷新页面【vue开发】_徐徐如升_的博客-CSDN博客_vue同一路由](https://blog.csdn.net/slow097/article/details/124199779)
> >
> > 
> >
> > 第一
> > 需要在路由配置页面进行判断，这段直接复制即可，参数无需改动
> >
> > ```
> > /** 解决跳转重复路由报错问题 */ 
> > const routerPush = router.push;
> > router.push = path => {
> >   // 判断下当前路由是否就是要跳转的路由
> >   if (router.currentRoute.fullPath.includes(path)) return;
> >   return routerPush.call(router, path);
> > }第二
> > 在你跳转到的页面里添加监听方法
> > ```
> >
> >  第二
> >
> >     watch: {
> >         '$route' (to, from) {
> >             //你在create里的方法
> >             //你在mounted里的方法
> >    
> >          	}
> >          },
> > ==此方法尚未尝试，但是看起来好像是三个方法里面最靠谱的==
> >
> > [解决vue路由跳转同一页面，携带不同参数，页面不刷新的问题_小蒋必须努力的博客-CSDN博客_vue同一个页面参数不同](https://blog.csdn.net/m0_60397142/article/details/120482070)
>
> 
>
> 想让某个低级路由默认显示，可以在它的父级路由上使用重定向redirect，目标设置为你想默认显示的低级路由，这里会因为重定向的原因网址路径上多一个/
>
> ```
> http://localhost:8081/#/search?keyworks=jay
> http://localhost:8081/#/search/?keyworks=jay   
> 
> //search后多出了一个/就是因为重定向，此处是子路由path配置为空，所以/后面会直接跟?，如果path配置不为空，那么path后面配置的内容会显示在/和?之间
> ```
>
> 不使用重定向就是第一个网址的样式，在不使用重定向时如果想实现默认显示某个子路由则需要将该子路由的path配置为空，如果同级子路由的path都为空，则需要将你想默认显示的子路由放在children配置项中的第一位，但是这里又会产生一个新问题，在子路由path为空时vuerouter将它视为默认子路由，如果你在拥有默认子路由的同时默认子路由的父级路由还进行了命名name的配置，控制台就会报错并且进入该父级路由默认子路由不会响应并渲染，那么这里只能去掉父级路由的name配置项转而在编程式导航中选择用path去push，这样又添加了不必要的麻烦
>
> ![image-20220601233541996](images/image-20220601233541996.png)

# vue2基本理解

> 想让vue工作，必须先创建一个vue实例，且要传入一个配置对象(通过选择器传入)
>
> root容器中的代码依然符合html规范，只不过混入了一些特殊的vue语法
>
> root容器里面的东西成为vue模板
>
> vue实例和容器是一一对应的关系
>
> 真实开发中只有一个vue实例，并且会配合着很多组件来使用
>
> `{{xxx}}`中的xxx要写js表达式，xxx可以是vue实例中的任意内容，只要它是js表达式
>
> 注意区分js表达式和js代码(语句)的区别
>
> js表达式: 一个表达式会产生一个返回值，放在任何一个需要返回值的地方
>
> js代码(语句): if(){} while(){}
>
> > 由vue管理的函数，不能写箭头函数，否则this就不再是vue实例而是window了
> >
> > 但不是只要在vue实例里面的函数就是vue管理的函数，很典型的settimeout定时器的回调函数、Ajax的回调函数和promise的回调函数就是可以在vue实例里面但不是由vue管理的函数(它们是由js引擎管理的)，对于这些函数反而要写成箭头函数，否则this就会变成window而不是vue实例

# vue2模板(插值和指令)

> 插值语法:
>
> > 功能: 用于解析标签体内容
> >
> > 写法: `{{xxx}}` xxx是js表达式，可以直接读取到data、computed等中的所有属性
>
> 指令语法:
>
> > 功能: 用于解析标签(包括：标签属性、标签体内容、绑定事件......)
> >
> > 举例: v-bind:href="xxx"或简写为:href="xxx",xxx同样要写js表达式，且可以直接读取到data、computed等里面的所有属性
> >
> > 备注: vue中有很多指令，且形式都是v-xxx,此处只是拿v-bind举例子
>
> 插值语法和指令语法的区别
>
> > 两者都可以调用data、computed等中的属性，插值语法需要用`{{}}`，而指令语法则需要使用v开头的指令去调用
>
> ==插值和指令如果调用的是data和methods中函数则应该写showInfo，如果是调用的函数中的返回值则应该写showInfo()==
>
> 模板内套娃
>
> > ```
> > <body>
> >     <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
> >     <div id="container">
> >         <h1>插值语法</h1>
> >         <hr>
> >         <h1>hello,{{name}}</h1>
> >         <hr>
> >         <h1>指令语法</h1>
> >         <hr>
> >         <a :href="other.url">点我进入{{other.name}}</a>
> >     </div>
> >     <script>
> >         new Vue({
> >             el: '#container',
> >             data: {
> >                 name: 'ounce',
> >                 other: {
> >                     url: 'http://www.baidu.com',
> >                     name: '百度',
> >                 }
> >             }
> >         })
> >     </script>
> > </body>
> > 
> > ```
> >
> > 其中other下还有key: value数据，在div里面想要调出这些other下的keyvalue数据则需要添加other.，我称之为套娃

# 单向数据绑定和双向数据绑定(v-bind&v-model)

> 单向数据绑定v-bind
>
> > v-bind:value=''可以简写为:value=""
> >
> > v-bind数据只能从data流向页面(在data里面改属性值，页面里面的属性值会随之发生变化，反之不会发生变化)
>
> 双向数据绑定v-model
>
> > v-model:value=''可以简写为v-model=""(因为v-model默认收集的就是value值)
> >
> > v-model的数据可以从data流向页面，也可以从页面流向data(在data里面改属性值，页面里面的属性值会随之发生变化,在页面里面更改属性值，data里面也会随之发生变化，且会更进一步影响v-bind)
> >
> > v-model只能应用在表单类元素上(如input、select等)(就是必须要有value的地方才能用v-model)

# el与data的两种写法

> el的两种写法
>
> > new vue的时候配置el属性
> >
> > 先创建vue实例，随后再通过v.$mount('')指定el的值
> >
> > ```
> > <script>
> >         const v = new Vue({
> >             data: {
> >                 name: 'ounce',
> >                 other: {
> >                     url: 'http://www.baidu.com',
> >                     name: '百度',
> >                 }
> >             }
> >         })
> >         console.log(v);
> >         v.$mount('#container');
> >     </script>
> > ```
>
> data的两种写法
>
> > 对象式，即默认写法
> >
> > 函数式
> >
> > ```
> > data: function() {
> >              return {
> >                  name: 'ounce',
> >                  other: {
> >                      url: 'http://www.baidu.com',
> >                      name: '百度',
> >                  }
> >              }
> >          }
> >  //还可以使用es6写法进行简写
> >  data() {
> >              return {
> >                  name: 'ounce',
> >                  other: {
> >                      url: 'http://www.baidu.com',
> >                      name: '百度',
> >                  }
> >              }
> >          }
> > ```
> >
> > 函数式更复杂，但以后都要用函数式，因为对象式在后面会报错(==但并不是所有的配置项都可以写成函数式，典型的反例就是computed写成函数式会报错==)
>

# MVVM模型

> ![image-20220405143358424](images/image-20220405143358424.png)
>
> ![image-20220405143447846](images/image-20220405143447846.png)
>
> vue中经常使用vm(viewmodel)这个变量名来表示vue实例
>
> ==data中的所有属性到最后都会显示在vue实例中，然后模板中可以对这些vue实例的属性以及方法直接调用(包括data里面没有的但是vue实例有的属性和方法也可以调用)==
>
> ==(这就是个model->viewmodel->view的过程也就是plain JavaScript objects->vue->DOM的过程)==

# 数据代理和Object.defineProperty

> ![image-20220409103627953](images/image-20220409103627953.png)
>
> 数据代理的底层是通过Object.defineProperty的getter和setter实现的
>
> ![image-20220409110934882](images/image-20220409110934882.png)
>
> 最简单的通过Object.defineProperty实现的数据代理如下所示
>
> ![image-20220409104020245](images/image-20220409104020245.png)

# 事件处理(v-on@)

> ![image-20220409110609805](images/image-20220409110609805.png)
>
> v-on:可以简写为@

# 事件修饰符(所有事件都可用的修饰符，不是只有click可以用修饰符可以连续写)

> ![image-20220409114400488](images/image-20220409114400488.png)
>
> 例如@click.stop="showInfo"
>
> ![image-20220409114545599](images/image-20220409114545599.png)

# 键盘事件(keyup&keydown推荐后者)

> ![image-20220409133751479](images/image-20220409133751479.png)
>
> 举例
>
> ![image-20220409134028503](images/image-20220409134028503.png)
>
> 原生写法
>
> ![image-20220409134423446](images/image-20220409134423446.png)
>
> 补充:
>
> > 对于ctrl、alt、shift和meta四个系统修饰键，可以指定按下的其他键例如ctrl+y触发事件
> >
> > <input type="text" @keyup==.ctrl.y==="showInfo">
> >
> > 对于tab键，应该使用keydown，使用keyup会因为tab本身的作用将焦点转移导致keyup无法触发

# 计算属性computed

> 计算属性会进行缓存，相比使用methods和插值直接拼接更有优势
>
> computed不能写成函数式，对computed中的属性进行调用只能写成fullName而不能写成fullName()，即便fullName写成了函数形式(==这一点与data、methods中的属性等不同，它们中的属性被调用成fullName是调用函数，调用成fullName()是调用函数的返回值==)，因为computed中的属性都是要进行计算后再将计算值赋给vm的同名的属性，例如此处的fullName计算完成后会==把值给vm初始化一个同名的属性vm.fullName==
>
> ![image-20220409173622607](images/image-20220409173622607.png)
>
> fullName: "张-三"这张图是控制台vue实例里面的，它证明了确实将计算值赋给了vm的同名属性初始化
>
> ![image-20220409173636213](images/image-20220409173636213.png)
>
> 使用computed计算属性时如果想实现双向数据代理则需要手动设置getter和setter(由vue底层的Object.defineProperty提供)
>
> ![image-20220409170107698](images/image-20220409170107698.png)
>
> 第二段中所说的fullName写成了函数形式就是简写方式，fullName自己充当getter
>
> ![image-20220409174214659](images/image-20220409174214659.png)

# 监视属性watch

> 被监视的属性(即data、methods等中的属性)发生了变化时，自动进行相关操作
>
> 监视属性有两种写法:new Vue时传入watch配置 通过vm.$watch这个实例方法监视
>
> > ![image-20220411151626775](images/image-20220411151626775.png)
>
> > ![image-20220411151716292](images/image-20220411151716292.png)
>
> 深度监视
>
> > 加入选项deep: true 即可开启深度监视，即被监视的属性的子属性发生了变化，也算该属性发生了变化
> >
> > deep默认为false
>
> 补充
>
> > 如果想要单个监视套娃属性中的子属性，例如data中的numbers中的a，应该用字面量的原生写法
> >
> > ```
> > 'numbers.a':{
> > ......
> > ......
> > }
> > ```
> >
> > 如果使用不加引号的简写法则会报错
> >
> > 如果直接用a也会报错，因为vm实例里面没有a只有numbers，要通过vm实例下的numbers再找到a，不能越级
>
> 此处key/value对简写为函数式时不能使用选项immediate和deep

# watch和computed的区别

> ![image-20220411161817744](images/image-20220411161817744.png)

# 绑定class和绑定style

> 字符串写法
>
> > 通过点击事件函数实现class的值的替换从而实现class绑定的切换
> >
> > ![image-20220417123216990](images/image-20220417123216990.png)
>
> 数组写法
>
> > 通过给class绑定一个数组，并初始化数组为class值的集合，从而实现class的绑定(这样可以随时增加减少或者更改class值)
> >
> > ![image-20220417123720285](images/image-20220417123720285.png)
>
> 对象写法
>
> > 和数组写法类似，实际上就是把数组改写为字面量表达式对象(key:value对)
> >
> > ![image-20220417124030205](images/image-20220417124030205.png)
>
> 绑定style非常少用，由两种写法分别是对象写法和数组写法，对象写法和绑定class的对象写法一样都是字面量表达式对象(key:value对)，而数组写法则是在数组里面配置对象，且每一个对象都是用字面量表达式的方法表示出来(非常麻烦属于是画蛇添足了)
>
> [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=27)

# 条件渲染(v-if v-else-if v-else v-show)

> v-show值为false时，将该节点隐藏(底层通过添加display: none来实现)
>
> v-if值为false时，将该节点去除(这个节点直接消失，而不是通过display: none隐藏)
>
> v-if v-else-if v-else对应的是if elseif else的逻辑
>
> > v-if满足了就不会看v-else-if
> >
> > v-if不满足再看v-else-if是否满足
> >
> > 如果v-if和v-else-if都不满足，则v-else执行(v-else不需要加条件，加了也没用)
> >
> > v-if v-else-if v-else三者配合使用时不能被打断，必须是连续的
>
> template配合v-if可以不破坏结构来实现整个模块的条件渲染控制(template配合v-show无效)
>
> > ![image-20220417132321604](images/image-20220417132321604.png)

# 列表渲染(v-for遍历)与key的作用原理(==key是虚拟dom的唯一标识==)

> ![image-20220417133246500](images/image-20220417133246500.png)
>
> 其中遍历次数是指2.中的xxx是一个数字，然后从1到xxx遍历
>
> ![image-20220417133537805](images/image-20220417133537805.png)

# 列表过滤

> [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=31)

# 监测数据原理-对象

> [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=34)
>
> 底层运用Object.defineProperty来实现data中数据变化页面重新解析的效果
>
> 以下是原理概要(并非完整的原理)
>
> > 将data传入函数observer，函数内置Object.defineProperty里面的getter和setter将data的属性传给接收函数observer的变量obs，再将obs赋给vm._data和data，将data和vm. _data联系起来，vm. _data的改变使页面重新解析,实现监测数据对象
> >
> > (直接用Object.defineProperty监视data并将data等于vm._data表面上也能实现相同效果，但实际上因为data改变会导致Object.defineProperty生效再导致setter和getter改变data从而导致了无限递归报错，所以上面的方法相当于使用obs来充当Object.defineProperty和data之间的中转站，可以避免无限递归，再通过obs赋给vm. _data和data来实现data和vm. _data之间的联系)
> >
> > ```
> >  <script>
> >         let data = {
> >             name : '123',
> >             address: '456'
> >         };
> >         const obs = new Observer(data);
> >         console.log(obs);
> >         let vm = {};
> >         vm._data = data = obs;
> >         function Observer (obj) {
> >             const keys = Object.keys(obj);
> >             keys.forEach((k)=>{
> >                 Object.defineProperty(this,k,{
> >                     get(){
> >                         return obs[k];
> >                     },
> >                     set(val){
> >                         obs[k] = val;
> >                     }
> >                 })
> >             })
> >         }
> >     </script> //简易原理概要代码
> > ```

# Vue.set()

> [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=35)
>
> vue3中已取消

# 监测数据原理-数组

> 本质上是将侦听数组变更的方法进行了包裹(加入了模板的自动重新解析)，即不仅进行了数组变更，还进行了模板的自动重新解析，这些被包裹过的方法有push、pop、shift、unshift、splice、sort、reverse
>
> 对于那些非变更数组的方法(即返回一个新数组而不是改变原数组的方法)，例如filter、concat、slice，可以原则将新数组替换原数组
>
> (这些方法的使用规则和原生js的方法使用规则完全相同)
>
> ![image-20220419151536167](images/image-20220419151536167.png)

# 收集表单数据

> ![image-20220420094423534](images/image-20220420094423534.png)
>
> v-model.number一般和input type="number"一起使用，v-model.number负责将输入的转换为有效数字，input type="number"负责限定只能输入数字
>
> (除了.number以外v-model还有.lazy和.trim两个内置修饰符)
>
> JSON.stringify()将js对象或者值转换为json字符串

# 过滤器filters

> ![image-20220420110530127](images/image-20220420110530127.png)
>
> filter==s==与data computed methods watch同级别
>
> new Vue({filter==s==})局部过滤器，只限于它所属的vm实例
>
> Vue.filter(name,callback)是全局过滤器(注意这里是filter而不是filters)
>
> ```
> <h3>{{time | timeFormater}}<h3> //没有括号，也默认传入time
> <h3>{{time | timeFormater('YYYY_MM_DD')}}<h3> //括号内的内容对应下文中的str
> 
> ...
> filters: {
> timeFormater(value,str='YYYY年MM月DD日 HH:mm:ss'){
> return dayjs(value).format(str)
> }
> } //此处str='YYYY年MM月DD日 HH:mm:ss'是es6新增的形参默认值，当有形参传入时使用传入的形参，没有传入的形参时将使用此处写下的默认形参值
>   //dayjs在BootCDN网站下载
> ...
> ```
>
> 过滤器内的函数不管任何时候什么样的形式都是默认将|前面的参数传入作为第一个参数，在括号内写入的内容则是后续的其他参数(==第一个参数总是|前面的参数==)

# v-text

> v-text="xxx" xxx会替换掉标签的文本内容
>
> 没有插值语法好用，插值语法更加灵活

# v-html

> ![image-20220420130433753](images/image-20220420130433753.png)
>
> 和v-text一样都会替换掉节点内的内容

# v-cloak

> ![image-20220420133155802](images/image-20220420133155802.png)
>
> 此处的css是指选中所有v-cloak属性并执行display: none

# v-once

> ![image-20220427115303860](images/image-20220427115303860.png)
>
> 加入了v-once属性的节点只进行一次渲染，里面的插值语法就算改变了内容也不会随着进行二次渲染
>
> 记得和事件修饰符中的once区别开来

# v-pre

> ![image-20220427115521996](images/image-20220427115521996.png)
>
> 加了v-pre属性的节点不会进行渲染

# 自定义指令

> ![image-20220427123642316](images/image-20220427123642316.png)
>
> 自定义指令可以写为函数式和字面量表示法，字面量表示法中可以使用回调函数
>
> 实际上自定义指令是coder自己将底层dom的操作封装成vue中的一个指令来自己使用

# 生命周期

> ![image-20220427130656642](images/image-20220427130656642.png)
>
> 生命周期钩子是叫法最多的
>
> vue中有许多钩子函数，分别对应了vue一个生命周期中的不同时间节点(就像人的18岁成年一样的是一个时间节点必然会发生的事情，在这个时间节点的钩子函数中加入自己想要实现的内容，vue就会在这个时间点执行这些内容)

# 钩子函数(8个4对)(beforeCreate和created)(beforeMount和Mounted)(beforeUpdate和updated)(beforeDestroy和destroyed)

> 在beforeMount中的对dom的操作最终都不会奏效，因为会被mount这个过程中发生的虚拟dom转为真实dom这一事件中的真实dom覆盖掉，所以对dom的操作其实实现了，但是被覆盖掉了，所以最终都没奏效
>
> destroy过程中只会卸载掉组件自定义事件，v-on绑定的原生事件是destroy卸载不掉的js(虽然卸载不掉但是它运行了不会对其他东西(例如数据)产生任何影响)
>
> beforeDestroy中可以进行对方法和对象的调用，但是对于数据的更新不会起到任何作用
>
> beforeUpdate中数据是已经更新的，但是模板是还未更新的
>
> ![生命周期](images/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
>
> 注意:==mouted中如果拿后台动态响应data数据而不是写死的data数据，需要定时器等待created把数据获取完==
>
> > ![image-20220516215540701](images/image-20220516215540701.png)
> >
> > 定时器里面必须要箭头函数，否则无效(尚不明白原因)
> >
> > [Vue created/mounted 异步获取不到data中的数据_何壹时的博客-CSDN博客_mounted拿不到vuex异步数据](https://blog.csdn.net/huisoul/article/details/121713618)

# 非单文件组件(Vue.extend)

> ![image-20220429131300934](images/image-20220429131300934.png)
>
> 组件中的data写成key:value对时会在复用时影响到数据之间的独立性(类似于一种双向绑定)(和前面的data必须写成函数式相对应，写成函数式时每次调用都是返回的返回值，返回值不会被修改，而keyvalue对是可以被修改的)

# 组件的注意点

> ![image-20220430124658515](images/image-20220430124658515.png)
>
> components中的template写结构模板应使用``来包住
>
> 'name': name简写为name: name再简写为(es6新特性，key和value一样的话去掉:只写一个)name

# 组件嵌套

> 嵌套关系
>
> 子级组件定义时要写在父级组件的前面(从上到下渲染)

# VueComponent

> ![image-20220430131228525](images/image-20220430131228525.png)

# vm和vc的内置关系

> ![image-20220430152450668](images/image-20220430152450668.png)
>
> ==在组件实例对象要获取原型上的东西时直接拿不需要加.prototype==
>
> 例如Vue.prototype.$api =api
>
> 然后组件实例对象中直接this.$api即可
>
> 
>
> vm和vc两点区别:
>
> > vc的data要写成函数式
> >
> > vc不能指定el
>
> vc在课堂外应该叫组件实例对象
>
> ![image-20220430152737793](images/image-20220430152737793.png)

# 单文件组件

> 实际开发中不会使用非单文件组件开发，全是单文件组件开发(非单文件组件开发没法将css样式组件化)
>
> 需要脚手架cli来让浏览器识别.vue文件和export以及import指令
>
> export和import是es6模块化指令，总共有三种暴露方式，一般使用默认暴露export default {}
>
> 需要将子组件都存放在app.vue组件中，再将app.vue组件引入进main.js(入口文件)中，main.js再被引入进index.html中
>
> ![image-20220502172044754](images/image-20220502172044754.png)
>
> render: h => h(APP)是箭头函数的简写形式，目的是为了在引入残缺版的vue时解析main.js(残缺版的vue中没有模板解析器)
>
> 而在.vue文件中有vue-template-compiler(vue2的模板解析器，vue3中为@vue/compiler-sfc)模板解析器去解析

# 更改默认配置

> [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=64)

# ref属性

> ![image-20220503134004792](images/image-20220503134004792.png)
>
> 与id的区别在于ref可以获取到组件标签的组件实例对象(vc)，而通过id的dom操作只能获取到该组件实例对象内具体的dom结构
>
> 有ref属性的节点会在组件实例对象身上的$refs里面找到它
>
> ==注意添加的属性是ref=""而不是$ref=""==

# props(父组件给子组件传数据最简单的方式)

> ![image-20220503140954830](images/image-20220503140954830.png)
>
> props与data相比优先级更高(即两个相同的变量名不同的值显示props的那个值)
>
> ==props 可以通过 `v-bind` 或简写 `:` 动态赋值==
>
> props起作用的前置条件为两个组件之间是父子关系(父给子传按上述方法，子给父传需要父先给子传一个函数(这样子vc上就有了这个函数)，再由子用methods创建一个函数获取vc上的父传过来的函数再传参回去(因为子在父里面，所以传给这个函数的参会被父自动接收到))

# mixin

> ![image-20220503142505734](images/image-20220503142505734.png)
>
> 将不同子组件中多次使用的部分内容整合成一个对象在一个.js文件中，通过暴露(export必定伴随着import)传给其他文件，.vue文件中可以通过局部混入来接受这些内容，main.js可以通过全局混入来接收这些内容(局部混入则让这些局部混入的子组件的组件实例对象(vc)得到这些内容，而全局混入则让所有vm和vc都得到这些内容不管你使不使用它)

# 插件

> ![image-20220503144805043](images/image-20220503144805043.png)
>
> 本质上是将各种全局命令(即Vue.xxx)整合成一个函数对象放到一个.js文件中，通过暴露(export必定伴随着import)传给其他文件，再到main.js中用Vue.use()来使用这个插件，括号内写,js文件的名称(例如plugins.js  Vue.use(plugins))
>
> 简写
>
> ```
> export default {
> install(Vue){
> //全局过滤器
> //全局指令
> //全局混入
> ...
> }
> }
> ```

# scoped

> 在子组件的style中添加scoped属性可以将该style的作用域局限在该子组件中，防止不同子组件中相同的变量名互相冲突

# 一般组件化开发流程

> ![image-20220503165322217](images/image-20220503165322217.png)

# localStorage和sessionStorage

> ![image-20220504110820719](images/image-20220504110820719.png)
>
> localStorage和sessionStorage的api完全一样

# 组件自定义事件(子组件给父组件传数据的方式/props子传父的方法见props)

> ![image-20220504132224194](images/image-20220504132224194.png)
>
> 解绑组件自定义事件的三种方法
>
> > ![image-20220504133117677](images/image-20220504133117677.png)
>
> 组件上绑定原生事件需要在子组件内使用$emit触发，否则不生效(vue在组件标签上的事件都默认为组件自定义事件)
>
> 绑定组件自定义事件好像要一个一个emit(?尚未证实)

# 全局事件总线(可以实现父子兄弟组件间通信)

> ![image-20220505193827914](images/image-20220505193827914.png)
>
> Vue.Component.prototype.```_```proto```_```===Vue.prototype
>
> 组件实例对象vc可以访问到Vue原型身上的属性和方法

# 消息订阅与发布(第三方库pubsub-js)(可以实现父子兄弟组件间通信)

> ![image-20220506095802047](images/image-20220506095802047.png)

# $nextTick(也是一种生命周期钩子)

> 当一个回调中更改了dom之后并不会立刻去更新模板，而是等这个回调全部执行完后再去更新模板
>
> ![image-20220506100630083](images/image-20220506100630083.png)
>
> 是钩子但不能像mounted那样和data等并列，只能作为钩子写在函数里面

# vue动画以及第三方库animate.css

> ![image-20220506104433628](images/image-20220506104433628.png)
>
> ![image-20220506104451598](images/image-20220506104451598.png)
>
> 给transition标签配置了name后v-enter等语句要将v改成对应的name属性的值
>
> 可以自己写原生动画@keyframes来替代v-enter、v-enter-to、v-leave以及v-leave-to，也可以不写原生动画，v-enter-active和v-leave-active则集成了原生中的animation-duration等属性(原生css中过渡transition是动画@keyframes的一种低阶表现形式，所以vue中没有过渡而是将过渡transition变成了动画的一个标签)
>
> animate.css的应用
>
> > [尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=94)

# 配置代理

> ![image-20220506121109706](images/image-20220506121109706.png)
>
> ![image-20220506121157202](images/image-20220506121157202.png)
>
> cors是解决跨域问题一劳永逸的方法
>
> (需要和谁进行交换数据就把谁写在target里面)

# 插槽slot

> ![image-20220508131644877](images/image-20220508131644877.png)
>
> ![image-20220508131736940](images/image-20220508131736940.png)
>
> 其中v-slot只能用在组件的template标签，其他标签会编译失败
>
> ![image-20220508131931041](images/image-20220508131931041.png)
>
> ![image-20220508132026604](images/image-20220508132026604.png)
>
> ![image-20220508132051546](images/image-20220508132051546.png)
>

# vuex

> 在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。
>
> 在多个组件间需要进行数据共享时使用
>
> vue2对应vuex3 vue3对应vuex4
>
> ![vuex](images/vuex.png)
>
> 实际上actions、mutations和state外面还有一层store然后才是vuex
>
> ==当state中的数据需要经过加工后再使用时，可以使用getters加工。==
>
> ==state相当于data，是vuex保存数据的地方==
>
> ==actions异步，mutations同步==
>
> 使用了vuex后就带有了$store
>
> 
>
> 下载npm i vuex(vue2用vuex3 vue3用vuex4)
>
> 使用
>
> > 再src下创建store文件夹内创建.js文件，文件内需要引用vue和vuex，然后再Vue.use(Vuex)(因为必须在创建store实例之前执行Vue.use(Vuex)，但是由于import的类似变量提升的特性，所以在main.js中始终无法做到这一点，只能在store文件夹内的.js文件完成这一步骤)
> >
> > .js文件内需要const actions、mutations、state、getters，并将它们放在一个new的Vue.Store内再暴露
> >
> > 
> >
> > src下.js文件
> >
> > 创建文件：```src/store/index.js```
> >
> > ```
> > //引入Vue核心库
> > import Vue from 'vue'
> > //引入Vuex
> > import Vuex from 'vuex'
> > //应用Vuex插件
> > Vue.use(Vuex)
> > 
> > //准备actions对象——响应组件中用户的动作
> > const actions = {
> >     //响应组件中加的动作
> > 	jia(context,value){
> > 		// console.log('actions中的jia被调用了',miniStore,value)
> > 		context.commit('JIA',value)
> > 	},
> > }
> > //准备mutations对象——修改state中的数据
> > const mutations = {
> >     //执行加
> > 	JIA(state,value){
> > 		// console.log('mutations中的JIA被调用了',state,value)
> > 		state.sum += value
> > 	}
> > }
> > //准备state对象——保存具体的数据
> > const state = {}
> > //准备getters
> > //当state中的数据需要经过加工后再使用时，可以使用getters加工,getters就像vuex里的computed
> > const getters = {}
> > 
> > //创建并暴露store
> > export default new Vuex.Store({
> > 	actions,
> > 	mutations,
> > 	state,
> > 	getters
> > })
> > ```
> >
> > 在```main.js```中创建vm时传入```store```配置项
> >
> > ```
> > ......
> > //引入store
> > import store from './store'
> > ......
> > 
> > //创建vm
> > new Vue({
> > 	el:'#app',
> > 	render: h => h(App),
> > 	store
> > })
> > ```
>
> 四种map方法在使用前记得要从vuex中import
>
> ```
> import {mapActions,mapMutations,mapState,mapGetters} from 'vuex'
> ```
>
> <strong>mapState方法：</strong>用于帮助我们映射```state```中的数据为计算属性
>
> ```js
> computed: {
>     //借助mapState生成计算属性：sum、school、subject（对象写法）
>     //...es6新特性，将keyvalue对对象解构
>      ...mapState({sum:'sum',school:'school',subject:'subject'}),
>          
>     //借助mapState生成计算属性：sum、school、subject（数组写法）
>     ...mapState(['sum','school','subject']),
> },
> ```
>
> <strong>mapGetters方法：</strong>用于帮助我们映射```getters```中的数据为计算属性
>
> ```js
> computed: {
>     //借助mapGetters生成计算属性：bigSum（对象写法）
>     ...mapGetters({bigSum:'bigSum'}),
> 
>     //借助mapGetters生成计算属性：bigSum（数组写法）
>     ...mapGetters(['bigSum'])
> },
> ```
>
> mapState和mapGetters都写在computed里面
>
> <strong>mapActions方法：</strong>用于帮助我们生成与```actions```对话的方法，即：包含```$store.dispatch(xxx)```的函数
>
> ```js
> methods:{
>     //靠mapActions生成：incrementOdd、incrementWait（对象形式）
>     ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
> 
>     //靠mapActions生成：incrementOdd、incrementWait（数组形式）
>     ...mapActions(['jiaOdd','jiaWait'])
> }
> ```
>
> <strong>mapMutations方法：</strong>用于帮助我们生成与```mutations```对话的方法，即：包含```$store.commit(xxx)```的函数
>
> ```js
> methods:{
>     //靠mapActions生成：increment、decrement（对象形式）
>     ...mapMutations({increment:'JIA',decrement:'JIAN'}),
>     
>     //靠mapMutations生成：JIA、JIAN（对象形式）
>     ...mapMutations(['JIA','JIAN']),
> }
> ```
>
> > 备注：mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数(即在模板中加括号，括号内加参数)，否则参数是事件对象。
>

# vuex模块化和命名空间(namespaced)

> 1. 目的：让代码更好维护，让多种数据分类更加明确。
>
> 2. 修改```index.js```(==此处可以再进行一次模块化，将index.js改写为index.js+count.js+person.js==)
>
>    ```javascript
>    const countAbout = {
>      namespaced:true,//开启命名空间
>      state:{x:1},
>      mutations: { ... },
>      actions: { ... },
>      getters: {
>        bigSum(state){
>           return state.sum * 10
>        }
>      }
>    }
>    
>    const personAbout = {
>      namespaced:true,//开启命名空间
>      state:{ ... },
>      mutations: { ... },
>      actions: { ... }
>    }
>    
>    const store = new Vuex.Store({
>      modules: {
>        countAbout,
>        personAbout
>      }
>    })
>    ```
>
> 3. 开启命名空间后，组件中读取state数据：
>
>    ```js
>    //方式一：自己直接读取
>    this.$store.state.personAbout.list
>    //方式二：借助mapState读取：
>    ...mapState('countAbout',['sum','school','subject']),
>    ```
>
> 4. 开启命名空间后，组件中读取getters数据：
>
>    ```js
>    //方式一：自己直接读取
>    this.$store.getters['personAbout/firstPersonName']
>    //方式二：借助mapGetters读取：
>    ...mapGetters('countAbout',['bigSum'])
>    ```
>
> 5. 开启命名空间后，组件中调用dispatch
>
>    ```js
>    //方式一：自己直接dispatch
>    this.$store.dispatch('personAbout/addPersonWang',person)
>    //方式二：借助mapActions：
>    ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
>    ```
>
> 6. 开启命名空间后，组件中调用commit
>
>    ```js
>    //方式一：自己直接commit
>    this.$store.commit('personAbout/ADD_PERSON',person)
>    //方式二：借助mapMutations：
>    ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
>    ```
>

# 路由route理解

> 理解： 一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。
>
> 前端路由：key是路径，value是组件。
>
> 使用路由后路由组件就带有了```$route```和```$router```，每个路由组件身上的```$router```都是一样的，而$route则各不相同
>
> vue2对应vue-router3 vue3对应vue-router4
>
> 路由组件放在pages或者router/routes文件夹下，一般组件放在components文件夹下

# 路由route基本使用

> 1. 安装vue-router，命令：```npm i vue-router```
>
> 2. 应用插件：
>
>    > ```
>    > //在main.js中引入VueRouter
>    > 
>    > import VueRouter from 'vue-router'
>    > 
>    > //使用VueRouter
>    > 
>    > Vue.use(VueRouter)
>    > ```
>
> 3. 编写router配置项:
>
>    ```js
>    //引入VueRouter
>    import VueRouter from 'vue-router'
>    //引入路由组件
>    import About from '../components/About'
>    import Home from '../components/Home'
>    
>    //创建router实例对象，去管理一组一组的路由规则
>    const router = new VueRouter({
>    	routes:[
>    		{
>    			path:'/about',
>    			component:About
>    		},
>    		{
>    			path:'/home',
>    			component:Home
>    		}
>    	]
>    })
>    
>    //暴露router
>    export default router
>    ```
>
> 4. 实现切换（active-class可配置高亮样式）
>
>    ```vue
>    <router-link active-class="active" to="/about">About</router-link>
>    ```
>
> 5. 指定展示位置
>
>    ```vue
>    <router-view></router-view>
>    ```
>

# 多级路由children

> 配置路由规则，使用children配置项：
>
> ```js
> routes:[
> 	{
> 		path:'/about',
> 		component:About,
> 	},
> 	{
> 		path:'/home',
> 		component:Home,
> 		children:[ //通过children配置子级路由
> 			{
> 				path:'news', //此处一定不要写：/news
> 				component:News
> 			},
> 			{
> 				path:'message',//此处一定不要写：/message
> 				component:Message
> 			}
> 		]
> 	}
> ]
> ```
>
> 跳转（要写完整路径）：
>
> ```vue
> <router-link to="/home/news">News</router-link>
> ```
>
> ==此处要特别注意子路由的path和to中路径写法上的区别==

# 路由参数query(ajax?)

> 传递参数
>
> ```vue
> <!-- 跳转并携带query参数，to的字符串写法 -->
> <li v-for="m in messageList" :key="m.id">
> <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">跳转</router-link>
>     //es6新特性
> </li>	
> 
> <!-- 跳转并携带query参数，to的对象写法 -->
> <li v-for="m in messageList" :key="m.id">
> <router-link 
> 	:to="{
> 		path:'/home/message/detail',
> 		query:{
> 		   id:m.id,
>          title:'m.title'
> 		}
> 	}"
> >跳转</router-link>
> </li>
> ```
>
> ```vue
> <script>
> export default {
> name:"message",
> data(){
>  return {
>      message:[
>          {id:'001',title:'xiaoxi1'},
>          {id:'002',title:'xiaoxi2'},
>          {id:'003',title:'xiaoxi3'}
>      ]
>  }
> }
> }
> </script>
> ```
>
> 接收参数：
>
> ```js
> $route.query.id
> $route.query.title
> ```

# 命名路由

> 1. 作用：可以简化路由的跳转。
>
> 2. 如何使用
>
>    1. 给路由命名：
>
>       ```js
>       {
>       	path:'/demo',
>       	component:Demo,
>       	children:[
>       		{
>       			path:'test',
>       			component:Test,
>       			children:[
>       				{
>                             name:'hello' //给路由命名
>       					path:'welcome',
>       					component:Hello,
>       				}
>       			]
>       		}
>       	]
>       }
>       ```
>
>    2. 简化跳转：
>
>       ```vue
>       <!--简化前，需要写完整的路径 -->
>       <router-link to="/demo/test/welcome">跳转</router-link>
>       
>       <!--简化后，直接通过名字跳转 -->
>       <router-link :to="{name:'hello'}">跳转</router-link>
>       
>       <!--简化写法配合传递参数 -->
>       <router-link 
>       	:to="{
>       		name:'hello',
>       		query:{
>       		   id:666,
>                   title:'你好'
>       		}
>       	}"
>       >跳转</router-link>
>       ```
>

# 路由参数params

> 1. 配置路由，声明接收params参数
>
>    ```js
>    {
>    	path:'/home',
>    	component:Home,
>    	children:[
>    		{
>    			path:'news',
>    			component:News
>    		},
>    		{
>    			component:Message,
>    			children:[
>    				{
>    					name:'xiangqing',
>    					path:'detail/:id/:title', //使用占位符声明接收params参数
>    					component:Detail
>    				}
>    			]
>    		}
>    	]
>    }
>    ```
>
> 2. 传递参数
>
>    ```vue
>    <!-- 跳转并携带params参数，to的字符串写法 -->
>    <li v-for="m in messageList" :key="m.id">
>    <router-link :to="`/home/message/detail/${m.id}/${m.title}`">跳转</router-link>
>     //es6新特性
>    </li>
>    
>    <!-- 跳转并携带params参数，to的对象写法 -->
>    <router-link 
>    	:to="{
>    		name:'xiangqing',
>    		params:{
>    		   id:666,
>                title:'你好'
>    		}
>    	}"
>    >跳转</router-link>
>    ```
>
>    > 特别注意：==路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！==
>
> 3. 接收参数：
>
>    ```js
>    $route.params.id
>    $route.params.title
>    ```
>    
>    params刷新会丢失数据，query不会
>    
>    https://blog.csdn.net/liyunkun888/article/details/83269343
>

# 路由的props

> 作用：让路由组件更方便的收到参数
>
> ==第一种方法传死数据无意义，第二种和第三种方法本质上还是query和params传参，只是通过props打包过后在子路由组件中可以更方便的使用==
>
> > ```
> > props:['id','title']
> > 
> > //下面的例子通过props将query或者params传给子路由组件，在子路由组件中就要这样来接收，有点类似于...mapState这样的形式
> > ```
>
> ```js
> {
> 	name:'xiangqing',
> 	path:'detail/:id',
> 	component:Detail,
> 
> 	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
>     //只能传死数据
> 	props:{a:900}
> 
> 	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
>     //第二种写法可以在组件内写编程式导航将params传过去，在子路由中接收(比较符合我的习惯)
>     //第二种写法不适用于query
> 	props:true
> 	
> 	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
>     //第三种写法在router的index.js中将query或者params结构后再输出给子路由
>     //这是第三种写法的最语义化的写法
> 	props($route){
> 		return {
> 			id:$route.query.id,
> 			title:$route.query.title
> 		}
> 	}
>  //这是第三种写法的解构赋值写法
>  props({query}){
> 		return {
> 			id:query.id,
> 			title:query.title
> 		}
> 	}
>  //这是第三种写法的解构赋值连续写法
>  props({query:{id,title}}){
> 		return {
> 			id,
> 			title
> 		}
> 	}
> }
> ```
>

# router-link的replace属性

> 作用：控制路由跳转时操作浏览器历史记录的模式
>
> 浏览器的历史记录有两种写入方式：分别为```push```和```replace```，```push```是追加历史记录，```replace```是替换当前记录。路由跳转时候默认为```push```(开启replace后，浏览器的后退就回不去了)
>
> 如何开启```replace```模式：```<router-link replace .......>News</router-link>```
>
> 需要知道的是单页面应用中跳转页面后前一个页面会直接被销毁destroy

# 编程式路由导航

> 作用：不借助```<router-link> ```实现路由跳转，让路由跳转更加灵活
>
> ==```<router-link> ```默认转化为a标签，在有些场景下我们的交互点并不是a标签所以不能使用```<router-link> ```，例如我们需要利用button实现路由跳转，此时使用编程式路由导航将方法绑定到对应dom节点的click事件上==
>
> 具体编码：
>
> ```js
> //$router的两个API
> this.$router.push({
> 	name:'xiangqing',
> 		params:{
> 			id:xxx,
> 			title:xxx
> 		}
> })
> 
> this.$router.replace({
> 	name:'xiangqing',
> 		params:{
> 			id:xxx,
> 			title:xxx
> 		}
> })
> this.$router.forward() //前进
> this.$router.back() //后退
> this.$router.go() //可前进也可后退，根据括号内数字来顶，+n就是前进n步，-n就是后退n步
> ```

# 缓存路由组件

> 作用：让不展示的路由组件保持挂载，不被销毁。
>
> ==记住keep-alive要放在router-view的外面，如果不写include，则所有在这个router-view展示的路由组件都会keep-alive==
>
> 具体编码(下列代码写在父路由组件中用来展示子路由组件的地方)：
>
> ```vue
> <keep-alive include="News"> 
>  <router-view></router-view>
> </keep-alive>
> ```
>
> 需要注意的是include后面接的是==组件名==，而不是路由名等其他名字
>
> 当不写include的时候，默认所有组件都不会销毁，想缓存多个路由组件则在include后面写数组

# 两种路由独有的生命周期钩子(activated deactivated)

> ```activated```路由组件被激活时触发。
>
> ```deactivated```路由组件失活时触发。

# 路由守卫(通过路由守卫内加入判断逻辑实现鉴权)

> 路由信息元(meta)，可以用来存放各种信息
>
> 作用：对路由进行权限控制
>
> 分类：全局守卫、独享守卫、组件内守卫
>
> 全局守卫(==写在new VueRouter中，与routes平级==):
>
> ```js
> //全局前置守卫：初始化时执行、每次路由切换前执行
> router.beforeEach((to,from,next)=>{
> 	console.log('beforeEach',to,from)
> 	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
> 		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
> 			next() //放行
> 		}else{
> 			alert('暂无权限查看')
> 			// next({name:'guanyu'})
> 		}
> 	}else{
> 		next() //放行
> 	}
> })
> 
> //全局后置守卫：初始化完成后执行、每次路由切换完成后执行
> router.afterEach((to,from)=>{
> 	console.log('afterEach',to,from)
> 	if(to.meta.title){ 
> 		document.title = to.meta.title //修改网页的title
> 	}else{
> 		document.title = 'vue_test'
> 	}
> })
> ```
>
> 独享守卫(无后置守卫，==写在new VueRouter下的routes下的具体的路由组件内==):
>
> ```js
> beforeEnter(to,from,next){
> 	console.log('beforeEnter',to,from)
> 	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
> 		if(localStorage.getItem('school') === 'atguigu'){
> 			next()
> 		}else{
> 			alert('暂无权限查看')
> 			// next({name:'guanyu'})
> 		}
> 	}else{
> 		next()
> 	}
> }
> ```
>
> 组件内守卫(==直接写在路由的组件内，而不是在router文件夹下的index.js文件中配置==)：
>
> ```js
> //进入守卫：通过路由规则，进入该组件时被调用
> beforeRouteEnter (to, from, next) {
> },
> //离开守卫：通过路由规则，离开该组件时被调用
> beforeRouteLeave (to, from, next) {
> }
> ```

# 路由器两种模式history和hash

> 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值。
>
> hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器。
>
> hash模式：
>
> > 地址中永远带着#号，不美观 。
> >
> > 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
> >
> > 兼容性较好。
>
> history模式：
>
> > 地址干净，美观 。
> >
> > 兼容性和hash模式相比略差。
> >
> > 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。
> >
> > ```js
> > const router = new VueRouter({
> >   mode: 'history',
> >   routes: [...]
> > })
> > ```
>
> 在nodejs服务器端使用connect-history-fallback解决history模式404