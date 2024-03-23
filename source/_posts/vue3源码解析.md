---
title: Vue3源码解析
date: 2024-03-15 21:03:14
tags:
---

##### reactivity模块

###### 响应式大概逻辑流程

> ![image-20220721213347873](images/image-20220721213347873.png)
>
> 其中fn是用户行为函数

###### reactive的实现

> ```js
> import { track, trigger } from "./effect"
> 
> export function reactive(raw) {
>  const proxy = new Proxy(raw, {
>      get(target, key, receiver) {
>          const res = Reflect.get(target, key, receiver)
>          // 收集依赖，收集依赖是为了去触发依赖，不做收集和触发依赖时就可以实现获取数据但不能实现更新数据(此时的get可以不搭配effect函数直接Object.xxx获取，也可以搭配a = Object.xxx + 1这样的来获取)，只有收集触发后配合effect函数才能实现更新数据
>          // 这里的target就是被get的reactive对象本身，key则是reactive对象内具体被get的某个属性
>          track(target, key)
>          return res
>      },
>      set(target, key, value, receiver) {
>          const res = Reflect.set(target, key, value, receiver)
>          // TODO:触发依赖
>          trigger(target, key)
>          return res
>      }
>      // set执行完毕后，如果有地方get了set改变的变量，那么get会在set执行完毕后再执行
>      // 直接赋值obj.xx = xx是直接set，而obj.xx++是先get再set(因为自增相当于 obj.xx = obj.xx + 1)
>  })
>  return proxy
>  //将代理对象proxy返回
> }
> ```

###### effect的实现(==创建一个ReactiveEffect的类，保存传入的fn并执行==)

> 尚未明白为什么用户行为会触发effect函数，可能和运行时有关？
>
> ```js
> class ReactiveEffect {
>     constructor(fn) {
>         this.fn = fn
>         //将fn作为方法绑定在effect实例身上
>     }
>     run() {
>         this.fn()
>     }
>    }
> export function effect(fn) {
>  //fn就是用户行为函数
>     const _effect = new ReactiveEffect(fn)
>     _effect.run()
>    }
> ```

###### targetMap的建立(==用于存储effect==)与track的实现(==用于收集依赖==)

> ![image-20220721223459850](images/image-20220721223459850.png)
>
> ```js
> let activeEffect
> // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> 
> class ReactiveEffect{...}
> export function effect(fn){...}
> 
> const targetMap = new WeakMap()
> export function track(target, key) {   
>     let depsMap = targetMap.get(target);
>     if (!depsMap) {
>         targetMap.set(target, (depsMap = new Map()));
>     }
>     let dep = depsMap.get(key);
>     if (!dep) {
>         depsMap.set(key, (dep = new Set()));
>     }
>     dep.add(activeEffect)
>     //通过activeEffect全局变量取到effect实例，并将其存储在dep中
> }
> ```
>

###### trigger的实现(==用于触发依赖==)

> ```js
> export function trigger(target, key) {
>        let depsMap = targetMap.get(target)
>        let dep = depsMap.get(key)
>        for(const effect of dep){
>            effect.run()
>        }
> }
> ```

###### effect返回runner的实现

> effect调用会返回runner函数，runner函数其实就是effec实例身上的run()，并且run()返回fn用户行为函数的返回值，也就是runner调用会返回fn用户行为函数返回值
>
> ==effect(fn) => function(runner) => \_effect.run() => fn => return this._fn()==
>
> > 在上面effect的实现部分进行添加：
> >
> > ```js
> > let activeEffect
> > // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> > class ReactiveEffect {
> >  constructor(fn) {
> >      this.fn = fn
> >      //将fn作为方法绑定在effect实例身上
> >  }
> >  run() {
> >      activeEffect = this
> >      //将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它来实现依赖收集
> >      const result = this.fn()
> >      return result
> >      //在这里为了实现返回fn用户行为函数的返回值将返回值用result接住并return
> >  }
> > }
> > export function effect(fn) {
> >  //fn就是用户行为函数
> >  const _effect = new ReactiveEffect(fn)
> >  _effect.run()
> >  const runner = _effect.run.bind(_effect)
> >  return runner
> >  // bind将run()内的this指针指向_effect，而不是activeEffect（在上面的class ReactiveEffect的run()中将this指向了activeEffect，这里为了返回正常runner，用bind修改this指向），然后将_effect.run()返回给runner并将runner return
> > }
> > ```

###### effect的scheduler功能的实现

> 通过effect函数传入的第二个参数scheduler
>
> effect第一次执行时还是会执行fn
>
> 当响应式对象的set触发时不会执行fn而是执行scheduler
>
> 当你手动执行runner函数时还是会执行fn的
>
> > 在effect返回runner的实现的基础上对effect的实现进行再添加
> >
> > ```js
> > let activeEffect
> > // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> > class ReactiveEffect {
> >  constructor(fn, scheduler = null) {
> >      //传入scheduler，scheduler并不是必须的，所以初始化为null
> >      this.fn = fn
> >      //将fn作为方法绑定在effect实例身上
> >      this.scheduler = scheduler
> >      //将scheduler作为方法绑定在effect实例身上
> >  }
> >  run() {
> >      activeEffect = this
> >      //将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它来实现依赖收集
> >      const result = this.fn()
> >      return result
> >      //在这里为了实现返回fn用户行为函数的返回值将返回值用result接住并return
> >  }
> > }
> > 
> > export function effect(fn, options) {
> >  //fn就是用户行为函数
> >  const scheduler = options.scheduler
> >  const _effect = new ReactiveEffect(fn, scheduler)
> >  //effect传入第二个参数options，options中的scheduler对象传给ReactiveEffect来创建effect实例
> >  _effect.run()
> >  const runner = _effect.run.bind(_effect)
> >  return runner
> >  // bind将run()内的this指针指向_effect，而不是activeEffect（在上面的class ReactiveEffect的run()中将this指向了activeEffect，这里为了返回正常runner，用bind修改this指向），然后将_effect.run()返回给runner并将runner return
> > }
> > ```
> >
> > 在trigger的实现的基础上进行添加
> >
> > ```js
> > export function trigger(target, key) {
> >     let depsMap = targetMap.get(target)
> >     let dep = depsMap.get(key)
> >     for(const effect of dep){
> >         if(effect.scheduler){
> >             effect.scheduler()
> >         } else{
> >             effect.run()
> >         }
> >         //触发依赖时如果有scheduler就执行scheduler，没有就执行run
> >     }
> > }
> > ```

###### effect的stop和onStop功能的实现

> effect实例中添加stop()方法，调用stop传入runner函数(==stop(runner)==)实现set操作无效(==即响应式对象的数据更改了但不引起set操作，视图数据不更新==)，具体操作是将effect实例从dep中删除即可，需要注意的是，==stop以后是这个reactive对象内的所有属性都停止更新，而不是单个属性数据停止更新==，stop后再手动runner()即可取消stop(==代码中的体现就是再次初始化了一个effect实例，deps不为空了==)
>
> this.active用来控制是否清除effect实例
>
> > 在effect的scheduler功能的实现和track的实现的基础上添加
> >
> > ```js
> > let activeEffect
> > // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> > class ReactiveEffect {
> >     constructor(fn, scheduler = null) {
> >     //传入scheduler，scheduler并不是必须的，所以初始化为null
> >         this.fn = fn
> >         //将fn作为方法绑定在effect实例身上
> >         this.scheduler = scheduler
> >         //将scheduler作为方法绑定在effect实例身上
> >         this.deps = []
> >         //初始化一个deps数组属性，方便track中实现effect实例反向收集deps
> >         this.active = true
> >         //用来控制是否需要清除effect实例
> >     }
> >     run() {
> >         activeEffect = this
> >         //将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它来实现依赖收集
> >         const result = this.fn()
> >         return result
> >         //在这里为了实现返回fn用户行为函数的返回值将返回值用result接住并return
> >     }
> >     stop(){
> >     //添加stop方法，删除effect实例  
> >         if(this.active){
> >         //性能优化，防止多次连续stop浪费性能(连续多次stop效果等同于一次stop)
> >             cleanupEffect(this)
> >             this.active = false
> >         }
> >     }
> > }
> > 
> > function cleanupEffect(effect){
> >     // 清除函数，将deps中的effect实例全部删除
> >     const { deps } = effect;
> >     if (deps.length) {
> >         for (let i = 0; i < deps.length; i++) {
> >             deps[i].delete(effect);
> >         }
> >         deps.length = 0;
> >     }
> > }
> > 
> > export function effect(fn, options) {
> > //fn就是用户行为函数
> >     const scheduler = options.scheduler
> >     const _effect = new ReactiveEffect(fn, scheduler)
> >     //effect传入第二个参数options，options中的scheduler对象传给ReactiveEffect来创建effect实例
> >     _effect.run()
> >     const runner = _effect.run.bind(_effect)
> >     runner.effect = _effect
> >     // 将effect实例反向挂载在runner身上，因为stop方法中只传入一个runner，所以通过反向挂载方式获取effect实例
> >     return runner
> >     // bind将run()内的this指针指向_effect，而不是activeEffect（在上面的class ReactiveEffect的run()中将this指向了activeEffect，这里为了返回正常runner，用bind修改this指向），然后将_effect.run()返回给runner并将runner return
> > }
> > 
> > export function stop(runner){
> >     runner.effect.stop()
> >     // 调用effect实例身上的stop方法
> > }
> > 
> > const targetMap = new WeakMap()
> > export function track(target, key) {
> >     let depsMap = targetMap.get(target);
> >     if (!depsMap) {
> >         targetMap.set(target, (depsMap = new Map()));
> >     }
> >     let dep = depsMap.get(key);
> >     if (!dep) {
> >         depsMap.set(key, (dep = new Set()));
> >     }
> >     dep.add(activeEffect)
> >     //通过activeEffect全局变量取到effect实例，并将其存储在dep中
> >     activeEffect.deps.push(dep)
> >     //effect实例反向收集当前的deps集合，方便stop功能删除所有effect实例
> > }
> > ```
>
> onStop就是通过effect函数的第二个参数传给effect，在stop执行后会被调用一次，也就是调用stop之后的回调函数
>
> ![image-20220728002309648](images/image-20220728002309648.png)
>
> > 在function effect的基础上修改
> >
> > ```js
> > import { extend } from '@vue/shared'
> > // shared是src文件夹下的文件夹，里面放置所有在所有模块下都通用的工具函数
> > ...
> > 
> > export function effect(fn, options) {
> > //fn就是用户行为函数
> >  //const scheduler = options.scheduler
> >  const _effect = new ReactiveEffect(fn)
> >  //此时使用了object.assign将options放在effect实例里面，scheduler也是options内的一个属性，那么就不需要将scheduler作为形参传入ReactiveEffect
> >  if(options){
> >      // object.assign将options挂载在effect实例上
> >      extend(_effect, options);
> >  }
> >  //如果有options，就将其挂载到effect实例下，options中目前可能包括scheduler和onStop
> >  _effect.run()
> >  const runner = _effect.run.bind(_effect)
> >  runner.effect = _effect
> >  // 将effect实例反向挂载在runner身上，因为stop方法中只传入一个runner，所以通过反向挂载方式获取effect实例
> >  return runner
> >  // bind将run()内的this指针指向_effect，而不是activeEffect（在上面的class ReactiveEffect的run()中将this指向了activeEffect，这里为了返回正常runner，用bind修改this指向），然后将_effect.run()返回给runner并将runner return
> > }
> > ```
> >
> > shared
> >
> > ```js
> > export const extend = Object.assign
> > ```
> >
> > 在class ReactiveEffect基础上修改
> >
> > ```js
> > let activeEffect
> > // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> > class ReactiveEffect {
> >  constructor(fn, scheduler = null) {
> >  //传入scheduler，scheduler并不是必须的，所以初始化为null
> >      this.fn = fn
> >      //将fn作为方法绑定在effect实例身上
> >      this.scheduler = scheduler
> >      //将scheduler作为方法绑定在effect实例身上
> >      this.deps = []
> >      //初始化一个deps数组属性，方便track中实现effect实例反向收集deps
> >      this.active = true
> >      //用来控制是否需要清除effect实例
> >  }
> >  run() {
> >      activeEffect = this
> >      //将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它来实现依赖收集
> >      const result = this.fn()
> >      return result
> >      //在这里为了实现返回fn用户行为函数的返回值将返回值用result接住并return
> >  }
> >  stop(){
> >  //添加stop方法，删除effect实例  
> >      if(this.active){
> >      //性能优化，防止多次连续stop浪费性能(连续多次stop效果等同于一次stop)
> >          cleanupEffect(this)
> >          if(this.onStop){
> >              this.onStop()
> >          }
> >          // 清除deps后完成stop功能，此时判断是否有onStop，有就调用onStop()
> >          this.active = false
> >      }
> >  }
> > }
> > 
> > function cleanupEffect(effect){
> >  // 清除函数，将deps中的effect实例全部删除
> >  const { deps } = effect;
> >  if (deps.length) {
> >      for (let i = 0; i < deps.length; i++) {
> >          deps[i].delete(effect);
> >      }
> >      deps.length = 0;
> >  }
> > }
> > ```

###### 解决activeEffect可能为undefined的问题

> 在没有effect函数时get(==例如直接Object.xxx==)的时候进入track()有可能出现因为没有effect没有new ReactiveEffect导致activeEffect为undefined，此时track()中又要activeEffect.deps.push(dep)用effect实例反向收集deps就会报错
>
> 
>
> 在function track基础上修改
>
> ```js
> const targetMap = new WeakMap()
> export function track(target, key) {
>     let depsMap = targetMap.get(target);
>     if (!depsMap) {
>         targetMap.set(target, (depsMap = new Map()));
>     }
>     let dep = depsMap.get(key);
>     if (!dep) {
>         depsMap.set(key, (dep = new Set()));
>     }
>     if(!activeEffect){
>         return
>     }
>     //判断activeEffect是否为空，为空则return，不进行下面的操作
>     dep.add(activeEffect)
>     //通过activeEffect全局变量取到effect实例，并将其存储在dep中
>     activeEffect.deps.push(dep)
>     //effect实例反向收集当前的deps集合，方便stop功能删除所有effect实例
> }
> ```

###### readonly功能的实现

> readonly功能只能get，set会报错
>
> 在这一环节将之前的reactive.js进行重构
>
> reactive.js
>
> ```js
> import { mutableHandlers, readonlyHandlers } from "./baseHandlers";
> 
> export function reactive(target) {
>     // 通过mutableHandlers创建reactive对象
>     return createReactiveObject(target, mutableHandlers)
> }
> 
> export function readonly(target) {
>     // 通过readonlyHandlers创建readonly对象
>     return createReactiveObject(target, readonlyHandlers)
> }
> 
> function createReactiveObject(target, baseHandlers) {
>     // 创建proxy对象
>     return new Proxy(target, baseHandlers)
> }
> ```
>
> baseHandlers.js
>
> ```js
> import { track, trigger } from "./effect";
> 
> const get = createGetter()
> const set = createSetter()
> const readonlyGet = createGetter(true)
> // 避免多次创建getter和setter，手动缓存进行性能优化
> 
> function createGetter(isReadonly = false) {
>     return function get(target, key, receiver) {
>         const res = Reflect.get(target, key, receiver)
>         if (!isReadonly) {
>             // 判断isreadonly来确定是否收集依赖
>             track(target, key)
>         }
>         return res
>     }
> }
> 
> function createSetter() {
>     return function set(target, key, value, receiver) {
>         const res = Reflect.set(target, key, value, receiver)
>         // 触发依赖
>         trigger(target, key)
>         return res
>     }
> }
> 
> export const mutableHandlers = {
>     get,
>     set,
>     // 想要set先get，set执行完毕后，如果有地方get了set改变的变量，那么get会在set执行完毕后再执行
> }
> 
> export const readonlyHandlers = {
>     get: readonlyGet,
>     set(target, key, value, receiver) {
>         console.warn(`[Vue warn] Set operation on key "${String(key)}" failed: target is readonly.`, target)
>         // readonly时调用set触发警告
>         return true
>     }
> }
> ```

###### isReactive和isReadonly功能的实现

> 具体思路为在reactive.js中写isReactive函数，函数内获取value的属性值来调用getter，在getter内判断key是否为isReactive函数内调用的属性值，是则返回!isReadonly(==因为一个对象是reactive就必不可能是readonly==)
>
> isReadonly函数同理
>
> 将各个判断函数内调用value的属性集合成一个对象
>
> 
>
> reactive.js文件内添加
>
> ```js
> export const ReactiveFlags = {
>     IS_REACTIVE : '__v_isReactive',
>     IS_READONLY : "__v_isReadonly",
> }
> 
> ...
> 
> export function isReactive(value) {
>     // 通过获取value中的ReactiveFlags.IS_REACTIVE来调用getter，在getter中返回!isReadonly来完成判断
>     return !!value[ReactiveFlags.IS_REACTIVE]
>     // 当value不是proxy时应该返回false，但不加!!时会返回undefined
>     // 因为不是proxy时不会调用getter，而value本身又没有ReactiveFlags.IS_REACTIVE这个key，所以使用!!将undefined变为false，让value不是proxy的情况也返回false
> }
> 
> export function isReadonly(value) {
>     // 与isReactive同理
>     return !!value[ReactiveFlags.IS_READONLY]
> }
> ```
>
> baseHandlers.js文件内修改function createGetter
>
> ```js
> function createGetter(isReadonly = false) {
>     return function get(target, key, receiver) {
>         if(key === ReactiveFlags.IS_REACTIVE){
>             // 用于isReactive函数来判断是否为reactive对象，直接返回!isReadonly(当一个对象是reactive对象那么必然不是readonly对象)
>             return !isReadonly
>         }else if(key === ReactiveFlags.IS_READONLY) {
>             // 用于isReadonly函数来判断是否为readonly对象，直接返回isReadonly
>             return isReadonly
>         }
>         const res = Reflect.get(target, key, receiver)
>         if (!isReadonly) {
>             // 判断isreadonly来确定是否收集依赖
>             track(target, key)
>         }
>         return res
>     }
> }
> ```

###### 优化stop

> 当stop后进行自增操作时，发现set操作有效数据会改变，似乎stop失效
>
> 实际上是因为自增相当于是先get再set，直接赋值obj.xx = xx是直接set，而obj.xx++是先get再set(因为自增相当于 obj.xx = obj.xx + 1)，先get则会收集依赖，则之前stop将依赖清除之后又出现了依赖，所以set操作正常。
>
> 因此我们需要stop时既清除依赖==也不能进行依赖收集==
>
> 
>
> 使用全局变量shouldTrack来控制是否应该收集依赖
>
> class ReactiveEfeect进行添加
>
> ```js
> let activeEffect
> // 维护一个全局变量activeEffect，并在class ReactiveEffect的run中将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它
> let shouldTrack
> // 维护一个全局变量shouldTrack，在stop后在track中控制是否进行依赖收集
> 
> class ReactiveEffect {
>  constructor(fn, scheduler = null) {
>      // 传入scheduler，scheduler并不是必须的，所以初始化为null
>      this.fn = fn
>      // 将fn作为方法绑定在effect实例身上
>      this.scheduler = scheduler
>      // 将scheduler作为方法绑定在effect实例身上
>      this.deps = []
>      // 初始化一个deps数组属性，方便track中实现effect实例反向收集deps
>      this.active = true
>      // 用来控制是否需要清除effect实例
>  }
>  run() {
>      if (!this.active) {
>          // 判断此时是否在stop状态，是stop状态则直接return this.fn()
>          return this.fn()
>      }
>      shouldTrack = true
>      // 当不是在stop状态时，将shouldTrack赋值为true则可以进行track收集依赖
>      activeEffect = this
>      // 将activeEffect指向this，这样当一个effect函数run时就可以通过activeEffect这个全局变量取到它来实现依赖收集
>      const result = this.fn()
>      // 在这里为了实现返回fn用户行为函数的返回值将返回值用result接住并return
>      shouldTrack = false
>      // run执行完毕后将shouldTrack = false使得不能进行依赖收集
>      return result
>  }
>  stop() {
>      // 添加stop方法，删除effect实例  
>      if (this.active) {
>          // 性能优化，防止多次连续stop浪费性能(连续多次stop效果等同于一次stop)
>          cleanupEffect(this)
>          if (this.onStop) {
>              this.onStop()
>          }
>          // 清除deps后完成stop功能，此时判断是否有onStop，有就调用onStop()
>          this.active = false
>      }
>  }
> }
> ```
>
> function track进行性能优化和添加
>
> ```js
> const targetMap = new WeakMap()
> export function track(target, key) {
>  if (!isTracking) {
>      // 通过isTracking判断是否要进行track
>      return
>  }
>  let depsMap = targetMap.get(target);
>  if (!depsMap) {
>      targetMap.set(target, (depsMap = new Map()));
>  }
>  let dep = depsMap.get(key);
>  if (!dep) {
>      depsMap.set(key, (dep = new Set()));
>  }
>  if (dep.has(activeEffect)) {
>      // 性能优化，判断是否已经收集了当前的activeEffect也就是当前的effect实例，如果已经收集了则return
>      return
>  }
>  dep.add(activeEffect)
>  //通过activeEffect全局变量取到effect实例，并将其存储在dep中
>  activeEffect.deps.push(dep)
>  //effect实例反向收集当前的deps集合，方便stop功能删除所有effect实例
> }
> 
> export function isTracking() {
>  // 通过shouldTrack和activeEffect !== undefined来判断是否要进行track
>  // 同时防止了stop后进行依赖收集和没有effect函数直接get时activeEffect为空时进行依赖收集
>  return shouldTrack && activeEffect !== undefined
> }
> ```

###### 实现reactive和readonly的嵌套对象转换功能

> 对一个多层级的对象进行reactive()时，该对象内部的属性如果是一个对象那么也应该将该属性变成reactive对象，实现向下渗透
>
> readonly同理
>
> 在createGetter中实现(==为什么要在createGetter中实现而不是createSetter呢，其一是readonly没有set，其二是Reflect.get返回的是get的属性的值，而Reflect.set返回的是布尔值，只有在getter中能拿到get的属性的值然后将其变为reactive==)(所以reactive对象内的属性只有get它时才会变成reactive对象?)
>
> 
>
> function createGetter添加
>
> ```js
> function createGetter(isReadonly = false) {
>     return function get(target, key, receiver) {
>         if(key === ReactiveFlags.IS_REACTIVE){
>             // 用于isReactive函数来判断是否为reactive对象，直接返回!isReadonly(当一个对象是reactive对象那么必然不是readonly对象)
>             return !isReadonly
>         }else if(key === ReactiveFlags.IS_READONLY) {
>             // 用于isReadonly函数来判断是否为readonly对象，直接返回isReadonly
>             return isReadonly
>         }
>         const res = Reflect.get(target, key, receiver)
>         if (!isReadonly) {
>             // 判断isreadonly来确定是否收集依赖
>             track(target, key)
>         }
>         if(isObject(res)){
>             // 判断res是否为Object，是则判断isReadonly是否为true，true则将readonly(res)，实现readonly对象的向下渗透，false则reactive(res)，实现reactive对象的向下渗透
>             return isReadonly ? readonly(res) : reactive(res)
>         }
>         return res
>     }
> }
> ```
>
> shared添加
>
> ```js
> export const isObject = (val) => {
>     return val !== null && typeof val === 'object'
> }
> ```

###### shallowReadonly功能的实现

> shallowReadonly是只有自身的属性为readonly(==第一层的属性==)，属性为对象时这个对象内部则不是readonly
>
> 
>
> reactive.js添加和修改
>
> ```js
> import { mutableHandlers, readonlyHandlers, shallowReadonlyHandlers } from "./baseHandlers";
> 
> ...
> 
> export function shallowReadonly(target) {
>     // 通过shallowReadonlyHandlers创建shallowReadonly对象
>     return createReactiveObject(target, shallowReadonlyHandlers)
> }
> ```
>
> function createGetter添加
>
> ==这里要十分注意的是if (!isReadonly)、if(shallow)、if(isObject(res))的判断顺序，如果按照视频的顺序则这里的shallow只代表shallowReadonly，如果还想添加shallowReactive和shallowRef的话则需要按照下面的顺序进行判断==
>
> ```js
> const get = createGetter()
> const set = createSetter()
> const readonlyGet = createGetter(true)
> const shallowReadonlyGet = createGetter(true, true)
> // 避免多次创建getter和setter，手动缓存进行性能优化
> 
> function createGetter(isReadonly = false, shallow = false) {
>     return function get(target, key, receiver) {
>         if(key === ReactiveFlags.IS_REACTIVE){
>             // 用于isReactive函数来判断是否为reactive对象，直接返回!isReadonly(当一个对象是reactive对象那么必然不是readonly对象)
>             return !isReadonly
>         }else if(key === ReactiveFlags.IS_READONLY) {
>             // 用于isReadonly函数来判断是否为readonly对象，直接返回isReadonly
>             return isReadonly
>         }
>         const res = Reflect.get(target, key, receiver)
>         if (!isReadonly) {
>             // 判断isreadonly来确定是否收集依赖
>             track(target, key)
>         }
>         if(shallow) {
>             // 判断是否为shallow
>             return res
>         }
>         if(isObject(res)){
>             // 判断res是否为Object，是则判断isReadonly是否为true，true则将readonly(res)，实现readonly对象的向下渗透，false则reactive(res)，实现reactive对象的向下渗透
>             return isReadonly ? readonly(res) : reactive(res)
>         }
>         
>         return res
>     }
> }
> ```
>
> baseHandlers.js添加
>
> ```js
> export const shallowReadonlyHandlers = extend({}, readonlyHandlers, {
>     get: shallowReadonlyGet,
> })
> // 因为shallowReadonlyHandlers的setter和readonlyHandlers的setter一样，所以使用extend将readonlyHandlers挂载，再用get: shallowReadonlyGet将readonlyHandlers的getter覆盖掉
> ```

###### isProxy功能实现

> 函数判断是否为reactive或者readonly构造的proxy
>
> 
>
> reactive.js添加
>
> ```js
> export function isProxy(value) {
>     // 函数判断是不是由reactive或者readonly构造的proxy
>     return isReactive(value) || isReadonly(value)
> }
> ```

###### ref功能实现

> get和set以及依赖收集和触发依赖，set前判断新旧value是否相同，不同则set，相同则不set
>
> 当传入ref的是基本数据类型，则正常get和set，如果传入的是引用数据类型，则将其reactive()(==需要注意的是如果reactive()后就不能进行hasChanged的比较了，需要维护一个变量rawValue来进行存储中继==)
>
> 
>
> 此部分对function track和function trigger进行了重构
>
> ```js
> const targetMap = new WeakMap()
> export function track(target, key) {
>     if (!isTracking) {
>         // 通过isTracking判断是否要进行track
>         return
>     }
>     let depsMap = targetMap.get(target);
>     if (!depsMap) {
>         targetMap.set(target, (depsMap = new Map()));
>     }
>     let dep = depsMap.get(key);
>     if (!dep) {
>         depsMap.set(key, (dep = new Set()));
>     }
>     trackEffects(dep)
> }
> 
> export function trackEffects(dep) {
>     // 将依赖收集的过程抽离成新的函数
>     if (dep.has(activeEffect)) {
>         // 性能优化，判断是否已经收集了当前的activeEffect也就是当前的effect实例，如果已经收集了则return
>         return
>     }
>     dep.add(activeEffect)
>     //通过activeEffect全局变量取到effect实例，并将其存储在dep中
>     activeEffect.deps.push(dep)
>     //effect实例反向收集当前的deps集合，方便stop功能删除所有effect实例
> }
> 
> export function isTracking() {
>     // 通过shouldTrack和activeEffect !== undefined来判断是否要进行track
>     // 同时防止了stop后进行依赖收集和没有effect函数直接get时activeEffect为空时进行依赖收集
>     return shouldTrack && activeEffect !== undefined
> }
> 
> export function trigger(target, key) {
>     let depsMap = targetMap.get(target)
>     let dep = depsMap.get(key)
>     triggerEffects(dep)
> }
> 
> export function triggerEffects(dep) {
>     // 将触发依赖的过程抽离成新的函数
>     for (const effect of dep) {
>         if (effect.scheduler) {
>             effect.scheduler()
>         } else {
>             effect.run()
>         }
>         //触发依赖时如果有scheduler就执行scheduler，没有就执行run
>     }
> }
> ```
>
> shared添加
>
> ```js
> export const hasChanged = (value, oldValue) => {
>     // 判断value是否发生了改变
>     return !Object.is(value, oldValue)
> }
> ```
>
> 创建ref.js
>
> ```js
> import { hasChanged, isObject } from "@vue/shared"
> import { isTracking, trackEffects, triggerEffects } from "./effect"
> import { reactive } from "./reactive"
> 
> class RefImpl {
>     constructor(value) {
>         this._rawValue = value
>         // 维护rawValue避免当value为对象时将其reactive()后变成proxy无法进行hasChanged判断
>         this._value = convert(value)
>         // 当value是对象时将其reactive()
>         this.dep = new Set()
>         // 初始化dep用于依赖收集
>     }
> 
>     get value() {
>         trackRefValue(this)
>         // ref的依赖收集
>         return this._value
>         // refObject.value实现get
>     }
> 
>     set value(newValue) {
>         if(hasChanged(newValue, this._rawValue)){
>             // 判断set的是否是和原value不同的新value，是则进行数据更新和依赖收集
>             this._rawValue = newValue
>             this._value = convert(newValue)
>             // 当newValue是对象时将其reactive()
>             triggerEffects(this.dep)
>         }
>     }
> }
> 
> function convert(value) {
>     // 将判断value是否为object，是则将其reactive()，不是则原值返回的逻辑抽离成新函数
>     return isObject(value) ? reactive(value) : value
> }
> 
> function trackRefValue(ref) {
>     if (isTracking()) {
>         // 此处主要用于判断activeEffect是否为空
>         trackEffects(ref.dep)
>         // trackEffects函数实现依赖收集
>     }
> }
> 
> export function ref(value) {
>     return new RefImpl(value)
> }
> ```

###### isRef和unRef的功能实现

> isRef用于判断传入参数是否为ref，在RefImpl中初始化一个属性__v_isRef = true作为标识，通过判断这个属性是否为true来判断是否为ref，需要注意的是非ref去获取这个属性返回的是undefined，需要对其!!取布尔值再返回
>
> unRef则是返回ref的ref.value或者非ref的本值
>
> ==这里需要注意的是const a = ref(1)中a才是ref对象而1不是==
>
> 
>
> 在ref.js中添加
>
> ```js
> export function isRef(ref) {
>  // 判断是否为ref，是则返回true，否则返回false
>  return !!ref.__v_isRef
> }
> 
> export function unRef(ref) {
>  // ref则返回ref.value，非ref则原样返回
>  return isRef(ref) ? ref.value : ref
> }
> ```
>
> class RefImpl中添加this.__v_isRef = true
>
> ```js
> class RefImpl {
>  constructor(value) {
>      this._rawValue = value
>      // 维护rawValue避免当value为对象时将其reactive()后变成proxy无法进行hasChanged判断
>      this._value = convert(value)
>      // 当value是对象时将其reactive()
>      this.dep = new Set()
>      // 初始化dep用于依赖收集
>      this.__v_isRef = true
>      // 初始化__v_isRef标识为true，用来实现isRef和unRef
>  }
> 
>  get value() {
>      trackRefValue(this)
>      // ref的依赖收集
>      return this._value
>      // refObject.value实现get
>  }
> 
>  set value(newValue) {
>      if(hasChanged(newValue, this._rawValue)){
>          // 判断set的是否是和原value不同的新value，是则进行数据更新和依赖收集
>          this._rawValue = newValue
>          this._value = convert(newValue)
>          // 当newValue是对象时将其reactive()
>          triggerEffects(this.dep)
>      }
>  }
> }
> ```

###### proxyRefs功能实现

> 在template中调用ref时不需要写.value，使用proxy进行劫持，在get中有两种情况，一种是被获取的属性是单纯的属性，那么直接return，另一种是被获取的属性是ref对象，那么返回ref.value
>
> 更改proxyRefs的属性时分两种情况判断，一种是==被修改的属性是ref但是新数据不是ref==，直接将.value = newvalue即可，其他情况都可以用reflect.set进行替换
>
> 
>
> ref.js中添加
>
> ```js
> export function proxyRefs(objectWithRefs) {
>     return new Proxy(objectWithRefs, {
>         // 使用proxy进行数据劫持加工
>         get(target, key, receiver) {
>             return unRef(Reflect.get(target, key, receiver))
>             // unref判断get的数据是否为ref，是则返回ref.value，否则返回value，这样在template中不用写.value
>         },
>         set(target, key, value, receiver) {
>             const oldValue = target[key]
>             if (isRef(oldValue) && !isRef(value)) {
>                 // set的新数据不是ref而原数据是ref时，直接将新值赋给旧值的.value
>                 oldValue.value = value
>                 return true
>             } else {
>                 // 其他情况就直接替换
>                 return Reflect.set(target, key, value, receiver)
>             }
>         }
>     })
> }
> ```

###### computed功能的实现

> 类似ref，computed.value获取到值
>
> 懒执行，不去get(computed.value)，fn函数都不会调用
>
> 缓存，调用一次get之后，值被缓存，下次调用则不会再调用执行fn函数而是直接返回缓存的值(==初始化属性value和dirty = true，dirty为true则将getter运行值赋给value，然后dirty变false，返回value==)
>
> 在调用一次getter后，fn函数也就是effect里的参数值如果发生改变，那么computed也对应发生改变，并且不需要重复调用getter，也就是getter只调用初始的一次，然后重新进行缓存(==正常来说fn参数值改变了会重新调用一次getter，因为相当于是一个新getter，但是这里通过scheduler实现不调用getter并且重新缓存==)(==例如fn是返回一个reactive内的属性值，这个属性值发生了改变，那么computed的值也发生改变==)
>
> ```js
> import { ReactiveEffect } from "./effect"
> 
> class ComputedRefImpl {
>     constructor(getter){
>         this._getter = getter
>         this._dirty = true
>         // 用于标识配合_value实现缓存，第一次getter后缓存给_value
>         this._value
>         // 缓存第一次getter的值
>         this._effect = new ReactiveEffect(getter, () => {
>             if(!this._dirty){
>                 this._dirty = true
>             }
>             // 用scheduler实现fn内参数值发生变化但getter不重复调用并且实现数据更新和缓存
>         })
>     }
> 
>     get value(){
>         if(this._dirty){
>             this._dirty = false
>             this._value = this._effect.run()
>             // 将getter传给effect实例，用effect实例身上的run()实现getter
>         }
> 
>         return this._value
>     }
> }
> 
> export function computed(getter) {
>     return ComputedRefImpl(getter)
> }
> ```

###### runtime-core初始化

> ![image-20220803170425187](images/image-20220803170425187.png)

> > createApp中传入的参数是rootComponent也就是App对象也就是组件对象(==里面只有render和setup两个方法==)，mount中传入的参数则是#app的容器，在mount方法中用createVNode方法用组件实例对象创建一个虚拟节点vnode，再调用render函数(render中传入参数vnode和容器container)
> >
> > 下图为App组件实例对象
> >
> > ![image-20220803160055275](images/image-20220803160055275.png)
> >
> > 下图为根据该实例对象创建出来的vnode，可以看到因为createVNode的时候只传入了rootComponent而形参有三个，所以创建的vnode中只有type属性有值且值为有render和setup两个方法的对象
> >
> > ![image-20220803162112567](images/image-20220803162112567.png)
>
> > render中调用patch，传入参数vnode和container，patch中进行vnode.type判定，是object则processComponent组件初始化，是string则processElement元素初始化(==第一次进行到这里时vnode.type是一个有render和setup两个方法的对象，而后面会调用这个render方法创建type为string的vnode再传入patch，所以后面回溯来初始化element的时候vnode.type就是string==)，这里我们只涉及到组件初始化，processComponent再调用mountComponent并传入参数vnode和container
>
> > mountComponent中会调用createComponentInstance并传入vnode来创建instance
> >
> > ![image-20220803165548562](images/image-20220803165548562.png)
> >
> > ![image-20220803164228778](images/image-20220803164228778.png)
> >
> > 创建完毕后调用setupComponent并传入刚创建的instance来初始化instance，这里可以做initProps和initSlots以及setupStatefulComponent，此处我们只涉及到setupStatefulComponent
>
> > setupStatefulComponent中我们发现此前vnode的type就是App组件对象，这里我们返回到createComponentInstance中将vnode.type赋给instance.type
> >
> > ![image-20220803164452701](images/image-20220803164452701.png)
> >
> > 然后执行
> >
> > ![image-20220803164632597](images/image-20220803164632597.png)
> >
> > 通过instance.type将App组件对象拿到手为Component，再从组件对象身上拿到setup方法，判断setup是否存在(==因为有可能有人不写setup==)，存在即运行setup并将返回值赋给setupResult，并将instance和setupResult传给handleSetupResult进行setup结果的数据处理
>
> > handleSetupResult中判断setupResult是否为object，是则将其挂载到instance身上，最后调用finishComponentSetup完成组件初始化
> >
> > ![image-20220803165115930](images/image-20220803165115930.png)
>
> > finishComponentSetup中通过instance.type拿到之前的App组件对象，然后判断组件对象上是否有render，有则将render挂载到instance身上
> >
> > ![image-20220803165827149](images/image-20220803165827149.png)
>
> > setupComponent完毕后接着调用setupRenderEffect执行渲染副作用，调用instance身上的render方法，将结果赋给subTree，并将subTree和container一起传给patch进行回溯
> >
> > ![image-20220803170153445](images/image-20220803170153445.png)
> >
> > ![image-20220803170005693](images/image-20220803170005693.png)
>
> createApp.js
>
> ```js
> import { render } from "./render"
> import { createVNode } from "./vnode"
> 
> export function createApp(rootComponent) {
>  return {
>      mount(rootContainer){
>          const vnode = createVNode(rootComponent)
> 
>          render(vnode, rootContainer)
>      }
>  }
> }
> ```
>
> vnode.js
>
> ```js
> export function createVNode(type, props, children) {
>      const vnode = {
>          type,
>          props,
>          children
>      }
>      return vnode
> }
> ```
>
> render.js
>
> ```js
> import { createComponentInstance, setupComponent } from "./component"
> 
> export function render(vnode, container) {
>  patch(vnode, container)
> }
> 
> function patch(vnode, container) {
>  // 判断是不是element类型
>  processComponent(vnode, container)
> }
> 
> function processComponent(vnode, container) {
>  mountComponent(vnode, container)
> }
> 
> function mountComponent(vnode, container) {
>  const instance = createComponentInstance(vnode)
>  setupComponent(instance)
>  setupRenderEffect(instance, container)
> }
> 
> function setupRenderEffect(instance, container) {
>  const subTree = instance.render()
>  // vnode => patch
>  // vnode => element => mountElement
>  patch(subTree, container)
> }
> ```
>
> component.js
>
> ```js
> export function createComponentInstance(vnode) {
>  const instance = {
>      vnode,
>      type: vnode.type
>  }
>  return instance
> }
> 
> export function setupComponent(instance) {
>  // initProps
>  // initSlots
> 
>  setupStatefulComponent(instance)
> }
> 
> function setupStatefulComponent(instance) {
>  const Component = instance.type
> 
>  const { setup } = Component
> 
>  if(setup){
>      const setupResult = setup()
>      handleSetupResult(instance, setupResult)
>  }
> }
> 
> function handleSetupResult(instance, setupResult) {
>  if(typeof setupResult === 'object'){
>      instance.setupState = setupResult
>  }
> 
>  finishComponentSetup(instance)
> }
> 
> function finishComponentSetup(instance) {
>  const Component = instance.type
>  if(Component.render){
>      instance.render = Component.render
>  }
> }
> ```

###### rollup

> package.json添加
>
> ```js
> {
>   "name": "ounce-vue3",
>   "version": "1.0.0",
>   "description": "",
>   "main": "index.js",
>   "scripts": {
>     "dev": "node scripts/dev.js reactivity -f global",
>     "build": "rollup -c rollup.config.js"
>   },
>   "keywords": [],
>   "author": "",
>   "license": "ISC",
>   "devDependencies": {
>     "esbuild": "^0.14.51",
>     "minimist": "^1.2.6"
>   },
>   "dependencies": {
>     "rollup": "^2.77.2"
>   }
> }
> ```
>
> rollup.config.js创建
>
> ```js
> export default {
>     input: "./packages/index.js",
>     output: [
>         {
>             format: "cjs",
>             file: "lib/ounce-vue.cjs.js"
>         },
>         {
>             format: "es",
>             file: "lib/ounce-vue.esm.js"
>         },
>     ],
>     plugins: {
> 
>     }
> }
> ```
>
> 在packages下创建index.js
>
> ```js
> export * from "./runtime-core/src/index"
> ...
> //这里只写了runtime-core一个模块，还有其它模块格式一样
> ```
>
> 在runtime-core下创建index.js
>
> ```js
> export { createApp } from "./createApp";
> export { h } from "./h"
> // 将runtime-core模块中要打包出去的东西写在这里
> ```

###### 初始化element

> 在组件初始化完毕后会运行render，并将结果赋给subtree，再将subtree和container传入patch回溯
>
> ![image-20220803170005693](images/image-20220803170005693.png)
>
> 因为render中返回的是h函数也就是createVNode，所以subTree也是一个vnode，==区别在于这个vnode的children可能是一个数组里面包含了另外的h函数，就像下图==，但是这种情况不是本部分重点，下面继续patch
>
> ![image-20220803204702818](images/image-20220803204702818.png)
>
> 在subTree传入patch后对其进行判定，如果typeof vnode.type是string类型则processElement进行元素初始化，是object则processComponent进行组件初始化，此处进行元素初始化
>
> ![img](images/1658998685821-0f747411-a7dc-4409-ad67-255b352137aa.png)
>
> processElement方法中主要是分已挂载和未挂载的情况，我们这里先写未挂载的情况，调用mountElement进行元素挂载
>
> ![img](images/1658998809278-ebdf1906-137c-4e85-ba9c-69840ac31e44.png)
>
> 而mountElement内，可以分为三部分：创建元素，处理props，处理children
>
> > 创建元素：
> >
> > 根据vnode.type来createElement并赋给el，render过后这里的vnode.type就是div、p这样的元素标签，那么el就是页面中的一个元素了
> >
> > ![image-20220803205905242](images/image-20220803205905242.png)
>
> > 处理props
> >
> > 先拿到vnode中的props，再for in遍历拿到key，再通过props[key]拿到value，将key、value通过setAttribute方法给el元素设置属性(==这里的key就是id、class之类的，value就是css中给id和class取的名字==)，最后通过append将el插入进container也就是id为app的div
> >
> > ![image-20220803210019717](images/image-20220803210019717.png)
>
> > 处理children
> >
> > 首先判断children是字符串还是object，字符串则直接通过textContent将字符串挂载在el身上
> >
> > 如果是object则调用mountChildren取挂载children，在mountChildren中将children中的每一个传给patch进行回溯
> >
> > ![image-20220803212128246](images/image-20220803212128246.png)
> >
> > ![image-20220803212158804](images/image-20220803212158804.png)
>
> > ![image-20220803212212662](images/image-20220803212212662.png)
>
> 
>
> 总结
>
> 在mount的时候，会先调用render，render里面会调用patch，patch会根据vnode 的类型来做不同操作
>
> 发现是组件类型，会先初始化组件，初始化组件完成后，调用组件的render函数获取subTree，也就是组件内部的内容
>
> 之后就继续调用patch处理subTree
>
> 发现是element，进入挂载逻辑时，就是创建el，处理props，处理children（两种），添加到容器中。
>
> 获取subTree就好像拆箱的过程

###### 实现组件代理对象

> 实现直接this.msg和this.$el就可以获取相应的内容而不必写this.setup().msg
>
> 通过proxy代理对象实现
>
> 
>
> 在设置状态组件中进行proxy对象代理，将proxy挂载在instance身上
>
> ![image-20220803214015318](images/image-20220803214015318.png)
>
> 创建componentPublicInstance.js
>
> ![image-20220803214510132](images/image-20220803214510132.png)
>
> 将proxy和instance的this绑定在一起
>
> ![image-20220803214633649](images/image-20220803214633649.png)
>
> 为了方便获取到el，将el绑定在vnode身上
>
> ![image-20220803215024581](images/image-20220803215024581.png)
>
> 并在创建vnode时初始化一个el = null
>
> ![image-20220803215119771](images/image-20220803215119771.png)

###### 实现shapeflags(用代码可读性换代码性能)

> 一言以蔽之就是给vnode的四种状态element、stateful_component、text_children和array_children分别标识为0001、0010、0100、1000，然后patch之前都要createVNode，在createVNode中根据type初始化vnode身上的shapeFlag属性，然后在patch中根据vnode.shapeFlag来判断是string还是object来判断初始化元素还是初始化组件，在mountElement的处理children部分中根据vnode.shapeFlag来判断是string还是array来判断是直接挂载string还是mountChildren来回溯patch
>
> shared/src/ShapeFlags.js
>
> > ![image-20220803233706321](images/image-20220803233706321.png)
>
> vnode.js修改
>
> > ![image-20220803233800881](images/image-20220803233800881.png)
>
> render.js修改
>
> > ![image-20220803233836384](images/image-20220803233836384.png)
> >
> > ![image-20220803233857896](images/image-20220803233857896.png)

###### 实现注册事件功能

> vue中的注册事件例如onClick都是作为props进行处理的，所以实现注册事件要在mountElement中实现
>
> 在mountElement中的处理props部分，因为vue的注册事件有起名规范，根据规范注册事件都是on开头然后第三个字母为大写，因此使用正则来匹配这样的规范，匹配成功则进行事件注册，不成功则设置属性setAttribute
>
> 在addEventListener注册事件时要注意event名字的问题，又根据vue注册事件的起名规范是除了开头的on后其他字母都为小写则为js原生注册事件的事件名，因此使用slice(2).toLowerCase()来实现
>
> render.js中修改
>
> ![image-20220804120154456](images/image-20220804120154456.png)

###### props功能的实现

> 1、在setup中接收props
>
> 2、在render中通过this访问到props里key的value(==this.xxx，这个xxx是props里面的key，然后获取到对应的value==)
>
> 3、props必须是一个shallowReadonly
>
> 
>
> > 在setup中接收到props，需要在setupComponent中调用initProps
> >
> > ![image-20220804124201156](images/image-20220804124201156.png)
> >
> > initProps则是componentProps.js中的方法(==|| {}是为了让props不存在时为空==)
> >
> > ![image-20220804124233294](images/image-20220804124233294.png)
> >
> > 这样就将vnode中的props绑定在了instance身上，在此之前还需要在创建instance的时候初始化一个props属性
> >
> > ![image-20220804125456689](images/image-20220804125456689.png)
> >
> > 那么在调用setup的时候将instance身上的props传入setup就可以实现功能1在setup中接收props
> >
> > ![image-20220804125622145](images/image-20220804125622145.png)
>
> > 想要实现直接this.xxx获取到props中的值可以像实现组件代理对象中的一样，用proxy代理劫持然后get来实现
> >
> > 只需要在PublicInstanceProxyHandlers中判断key是不是setupState中的时候再判断key是不是props中的即可
> >
> > ![image-20220804125837072](images/image-20220804125837072.png)
> >
> > 其中hasOwn是shared中的公共函数，用于判断key是不是val身上的属性
> >
> > ![image-20220804130554592](images/image-20220804130554592.png)
>
> > 实现props是shallowReadonly只需要在将instance.props传给setup之前将其shallowReadonly一下
> >
> > ![image-20220804131458545](images/image-20220804131458545.png)
> >
> > 此时会出现一个问题因为props可能不是一个object然后在createReactiveObject时报错，修改方案是添加条件判断
> >
> > ![image-20220804131607488](images/image-20220804131607488.png)

###### emit功能的实现

> emit的使用用法如下
>
> https://juejin.cn/post/7026343868773875720
>
> emit是setup中的第二个参数context内的属性，第一个参数是props，在子组件中写好emit和要发送的自定义事件名称，那么在父组件中的dom节点上找到对应的事件名称并触发(==此处对应的名称也符合规范即on加首字母大写的事件名==)，如果除了事件还想传其他参数即要在父组件的注册事件上写对应形参
>
> 子组件Foo.js
>
> ![image-20220804144558569](images/image-20220804144558569.png)
>
> 父组件App.js，子组件Foo h函数的props也就是第二个参数内的onAdd和onAddFoo就是负组件中对应的自定义注册事件
>
> ![image-20220804144639718](images/image-20220804144639718.png)
>
> 在setup调用时增加参数emit，在创建instance时初始化emit
>
> ![image-20220804145055334](images/image-20220804145055334.png)
>
> ![image-20220804145148271](images/image-20220804145148271.png)
>
> 判断如果props
>
> ![image-20220804145201566](images/image-20220804145201566.png)
>
> 将emit传过来的事件名经过加工变成on开头+首字母大写的事件名然后在props里匹配，匹配到了就执行，其中...args是除了事件名之外穿的参数，在这里作为形参传递
>
> ![image-20220804150446632](images/image-20220804150446632.png)

###### slots功能的实现

> 普通插槽(this.$slots访问)，传入多个插槽(数组包裹)，具名插槽(根据名字来确定渲染的插槽以及渲染的位置)，作用域插槽(插槽中使用了子组件的render内的数据)
>
> 
>
> componentPublicInstance.js中的publicPropertiesMap中增加$slots
>
> ![image-20220804201237009](images/image-20220804201237009.png)
>
> component.js中的createComponentInstance中创建instance时初始化一个slots，并在setupComponent内initSlots
>
> ![image-20220804201402975](images/image-20220804201402975.png)
>
> 创建componentSlots.js，在initSlots中将children挂载在instance.slots身上
>
> ![image-20220804201811318](images/image-20220804201811318.png)
>
> 至此实现了最普通的插槽，用this.$slots访问并渲染出来了父组件传过来的插槽
>
> 接下来实现传入多个插槽，用数组包裹，此时this.$slots得出来的是数组不能渲染
>
> 
>
> 创建renderSlots.js(记得导入导出)
>
> ![image-20220804202332575](images/image-20220804202332575.png)
>
> Foo中重构
>
> ![image-20220804202416528](images/image-20220804202416528.png)
>
> 但是此时数组能渲染单个插槽却不行了，此处做一下支持
>
> 修改componentSlots.js
>
> ![image-20220804202622030](images/image-20220804202622030.png)
>
> 
>
> 实现具名插槽，能指定渲染插槽的名字和位置，那么这里就不再用数组包裹了，而是用对象包裹
>
> ![image-20220804202925902](images/image-20220804202925902.png)
>
> 给renderSlots传入第二个参数也就是名字key
>
> ![image-20220804203049910](images/image-20220804203049910.png)
>
> renderSlots.js更改
>
> ![image-20220804203121190](images/image-20220804203121190.png)
>
> componentSlots.js添加重构
>
> ![image-20220804203441551](images/image-20220804203441551.png)
>
> 至此具名插槽实现
>
> 实现作用域插槽，插槽变成函数形式，函数获取参数用{}解构一下
>
> ![image-20220804204000709](images/image-20220804204000709.png)
>
> renderSlots中接收第三个参数
>
> ![image-20220804204110742](images/image-20220804204110742.png)
>
> ![image-20220804204123045](images/image-20220804204123045.png)
>
> 初始化slots的时候也传入第三个参数，并且ShapeFlags添加SLOT_CHILDREN类型并在这里进行判断
>
> ![image-20220804204234302](images/image-20220804204234302.png)
>
> ![image-20220804204331069](images/image-20220804204331069.png)
>
> ![image-20220804204414623](images/image-20220804204414623.png)

###### Fragment和Text类型节点的实现

> 前面插槽一次渲染多个时会在这多个插槽外面加一层div，这里使用Fragment实现没有这一层div
>
> vnode.js中添加
>
> ![image-20220804210821749](images/image-20220804210821749.png)
>
> renderSlots.js中更改
>
> ![image-20220804210847558](images/image-20220804210847558.png)
>
> render中的patch更改
>
> ![image-20220804210915445](images/image-20220804210915445.png)
>
> 实际上就是渲染所有的children
>
> Text类似
>
> vnode.js
>
> ![image-20220804212702077](images/image-20220804212702077.png)
>
> ![image-20220804212708892](images/image-20220804212708892.png)
>
> render.js
>
> ![image-20220804212730981](images/image-20220804212730981.png)
>
> ![image-20220804212741263](images/image-20220804212741263.png)

###### getCurrentInstance功能的实现

> 这个api就是获取到当前的实例对象
>
> 在component.js中创建全局变量currentInstance，然后在setup执行的时候将instance赋给全局变量currentInstance，因为getCurrentInstance这个api是setup中使用的
>
> 
>
> 我们可以在 `setup` 中通过 `getCurrentInstance()` 来获取当前的组件实例
>
> 首先，我们需要在 `components` 中导出一个函数 `getCurrentInstance()`
>
> ```js
> // 将 currentInstance 作为一个全局变量
> let currentInstance
> 
> export function getCurrentInstance() {
>   return currentInstance
> }
> ```
>
> 在调用组件 `setup` 函数的时候将 setupInstance 赋值
>
> ```js
> function setupStatefulComponent(instance) {
>   if (setup) {
>     // 赋值
>     setCurrentInstance(instance)
>     const setupResult = setup(shallowReadonly(instance.props), {
>       emit: instance.emit,
>     })
>     // 重置
>     setCurrentInstance(null)
>     handleSetupResult(instance, setupResult)
>   }
> }
> ```
>
> 现在我们就已经实现了 `getCurrentInstance` 了。

###### provide和inject

> 实现 provide 和 inject
>
> 在本小节中我们将去实现 provide 和 inject
>
> 1. 例子
>
> 我们先看看 provide 和 inject 的一个例子
>
> ```js
> import { createApp, h, provide, inject } from '../../lib/mini-vue.esm.js'
> 
> const Provider = {
>   render() {
>     return h('div', {}, [h('div', {}, 'Provider'), h(Consumer)])
>   },
>   setup() {
>     // 在上层 provide
>     provide('foo', 'foo')
>   },
> }
> 
> const Consumer = {
>   render() {
>     return h('div', {}, 'Consumer: ' + `inject foo: ${this.foo}`)
>   },
>   setup() {
>     return {
>       // 在下层 inject
>       foo: inject('foo'),
>     }
>   },
> }
> 
> createApp(Provider).mount('#app')
> ```
>
> 2. 最简版实现
>
> 首先，我们需要创建一个 `apiInjecet.ts`
>
> ```js
> import { getCurrentInstance } from './component'
> 
> export function provide(key, value) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     currentInstance.providers[key] = value
>   }
> }
> export function inject(key) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     return currentInstance.providers[key]
>   }
> }
> ```
>
> 然后我们需要在 `instance` 上面挂载一下，注意我们在 `inject` 的时候需要获得 parent，所以我们还需要挂载一个 parent，而 parent 则是在 `createComponentInstance` 时进行挂载，我们需要一层一层的向下传递
>
> ```js
> import { getCurrentInstance } from './component'
> 
> export function provide(key, value) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     currentInstance.providers[key] = value
>   }
> }
> export function inject(key) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     const { parent } = currentInstance
>     return parent.providers[key]
>   }
> }
> export function createComponentInstance(vnode, parent) {
>   // 这里返回一个 component 结构的数据
>   const component = {
>     vnode,
>     type: vnode.type,
>     setupState: {},
>     props: {},
>     emit: () => {},
>     slots: {},
>     providers: {},
>     // 挂载 parent
>     parent,
>   }
>   component.emit = emit.bind(null, component) as any
>   return component
> }
> 
> // 然后再一层一层的传递
> 
> // 直到最上层
> // render.ts
> 
> function setupRenderEffect(instance, vnode, container) {
>   const subTree = instance.render.call(instance.proxy)
>   // 这里的第三个参数，就是 parent
>   patch(subTree, container, instance)
>   vnode.el = subTree.el
> }
> ```
>
> 现在我们就已经可以实现 `provide`、`inject` 的最简版的逻辑了。
>
> 3. 多层传递
>
> 但是目前我们的代码是存在问题的，例如我们知道 provide 是跨层级传递的，假设我们再生产者和消费者之间再加一层，那么代码就会出现问题。
>
> 3.1 例子
>
> ```js
> import { createApp, h, provide, inject } from '../../lib/mini-vue.esm.js'
> 
> const Provider = {
>   render() {
>     return h('div', {}, [h('div', {}, 'Provider'), h(Provider2)])
>   },
>   setup() {
>     provide('foo', 'foo')
>   },
> }
> 
> // 再 Provider 和 Provider2 加一层，那么最下层就获取不到了
> const Provider2 = {
>   render() {
>     return h('div', {}, [h('div', {}, 'Provider2'), h(Consumer)])
>   },
>   setup() {},
> }
> 
> const Consumer = {
>   render() {
>     return h('div', {}, 'Consumer: ' + `inject foo: ${this.foo}`)
>   },
>   setup() {
>     return {
>       foo: inject('foo'),
>     }
>   },
> }
> 
> createApp(Provider).mount('#app')
> ```
>
> 3.2 实现
>
> 那么这种该如何实现呢？其实我们就可以在初始化 instance 实例的时候：
>
> ```js
> export function createComponentInstance(vnode, parent) {
>   const component = {
>     vnode,
>     type: vnode.type,
>     setupState: {},
>     props: {},
>     emit: () => {},
>     slots: {},
>     // 把初始化的 provides 默认指向父级的 provides
>     providers: parent ? parent.providers : {},
>     parent,
>   }
>   component.emit = emit.bind(null, component) as any
>   return component
> }
> ```
>
> 这样我们跨层级的部分也已经解决了。
>
> 但是现在我们还存在一个问题，由于我们直接用的是直接覆盖，上面我们将 `instance.providers` 修改为 `parent.provides`，但是我们在 `provides` 中直接将自己的 provides 更改了，==因为是引用类型数据==，导致自己的 `parent.provides` 也被修改了。
>
> 例如下面的应用场景：
>
> ```js
> // 修改了自己的 provides.foo 也间接修改了 parent.provides.foo
> provide('foo', 'foo2')
> // 这时候取父亲的 provides.foo 发现就会被修改了
> const foo = inject('foo')
> ```
>
> 那么如何解决呢？
>
> ```js
> // apiInject.ts
> 
> export function provide(key, value) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     // 在这里不要直接设置
>     currentInstance.providers[key] = value
>   }
> }
> ```
>
> 我们其实可以通过原型链的方式巧妙的进行设置：
>
> ```js
> if (currentInstance) {
>     let provides = currentInstance.provides
>     if (currentInstance.parent) {
>         const parentProvides = currentInstance.parent.provides
>         // 如果自己的 provides 和 parent.provides，那么就证明是初始化阶段
>         if (provides === parentProvides) {
>             // 此时将 provides 的原型链设置为 parent.provides
>             // 这样我们在设置的时候就不会五绕道 parent.provides
>             // 在读取的时候因为原型链的特性，我们也能读取到 parent.provides
>             provides = currentInstance.provides = Object.create(parentProvides)
>         }
>     }
>     provides[key] = value
> }
> ```
>
> 这样设置后我们就可以
>
> 4. inject 的默认值
>
> 4.1 例子
>
> 我们可以在 inject 时设置一个默认值，默认值可能时一个函数或者一个原始值
>
> ```js
> const baseFoo = inject('baseFoo', 'base')
> const baseFoo = inject('baseBar', （）=> 'bar')
> ```
>
>  4.2 实现
>
> ```js
> export function inject(key, defaultValue) {
>   const currentInstance = getCurrentInstance()
>   if (currentInstance) {
>     const { parent } = currentInstance
>     // 如果 key 存在于 parent.provides
>     if (key in parent.provides) {
>       return parent.provides[key]
>     } else if (defaultValue) {
>       // 如果不存在，同时 defaultValue 存在
>       // 判断类型，同时执行或返回
>       if (typeof defaultValue === 'function') return defaultValue()
>       return defaultValue
>     }
>   }
> }
> ```
>
> 这样默认值的操作也已经可以了。

###### 实现customRenderer

> 

###### 初始化element更新流程

> 初始化 element 更新流程
>
> 在本小节中，我们将会初始化 element 的更新逻辑
>
> 1. 例子
>
> ```js
> export default {
>   setup() {
>     const counter = ref(1)
>     function inc() {
>       counter.value += 1
>     }
>     return {
>       counter,
>       inc,
>     }
>   },
>   render() {
>     return h('div', {}, [
>       h('div', {}, '' + this.counter),
>       h('button', { onClick: this.inc }, 'inc'),
>     ])
>   },
> }
> ```
>
> 首先我们需要解决模板使用 ref 的问题，因为目前 counter 是一个 ref，所以我们需要在实际执行 setup 的时候给这个返回的对象再包一层 proxyRefs
>
> ```js
> function setupStatefulComponent(instance) {
>     // 在这里给最终 setup 运行后的结果再套一层 proxyRefs
>     const setupResult = proxyRefs(
>         setup(shallowReadonly(instance.props), {
>             emit: instance.emit,
>         })
>     )
> }
> ```
>
> 2. 初始化更新逻辑
>
> 2.1 更新重新渲染视图
>
> 我们从例子中可以看出，`counter` 的更新并没有触发视图的更新，这是因为我们没有收集视图这个依赖，从而更改时也不会触发视图这个依赖。
>
> 那么我们最终是从哪里开始渲染视图的呢？
>
> ```js
> // render.ts
> 
> function setupRenderEffect(instance, vnode, container) {
>     // 就是在这个函数中
>     const subTree = instance.render.call(instance.proxy)
>     patch(subTree, container, instance)
>     vnode.el = subTree.el
> }
> ```
>
> 我们需要给这一层包一个 effect，现在我们就已经实现更新重新渲染视图了
>
> ```js
> function setupRenderEffect(instance, vnode, container) {
>     // 包一层 effect，让执行的时候去保存依赖
>     // 并在值更新的时候重新渲染视图
>     effect(() => {
>         const subTree = instance.render.call(instance.proxy)
>         patch(subTree, container, instance)
>         vnode.el = subTree.el
>     })
> }
> ```
>
> 2.2 获取上一个节点
>
> 但是目前我们的实现是非常有问题的，因为我们更新只会无脑的重新新加视图，而不是再对比视图，所以我们需要对视图进行判断
>
> 首先，我们需要给组件实例新增加一个状态，叫做 `isMounted`，用来记录该组件是否已经被渲染过，默认值是 `false`
>
> ```js
> function setupRenderEffect(instance, vnode, container) {
>     effect(() => {
>         // 根据 instance.isMounted 状态进行判断
>         if (instance.isMounted) {
>             // update 逻辑，获取上一个 subTree
>             const subTree = instance.render.call(instance.proxy)
>             vnode.el = subTree.el
>             // 获取上一个 subTree
>             const preSubTree = instance.subTree
>             // 然后将自己赋给 subTree
>             instance.subTree = subTree
>             console.log({ subTree, preSubTree })
>         } else {
>             // init 逻辑，存 subTree
>             const subTree = (instance.subTree = instance.render.call(
>                 instance.proxy
>             ))
>             patch(subTree, container, instance)
>             vnode.el = subTree.el
>             instance.isMounted = true
>         }
>     })
> }
> ```
>
> 借此我们就可以获取到旧的 subTree 了。
>
> 2.3 patch 新旧
>
> 然后我们在 `patch` 的时候只做了 init 逻辑，所以可以这样：
>
> ```js
> // 加入参数 n1,
> // n1 ==> preSubTree
> // n2 ==> currentSubTree
> function patch(n1, n2, container, parentInstance) {
>     const { type, shapeFlags } = n2
>     switch (type) {
>         case Fragment:
>             processFragment(n1, n2, container, parentInstance)
>             break
>         case TextNode:
>             processTextNode(n2, container)
>             break
>         default:
>             if (shapeFlags & ShapeFlags.ELEMENT) {
>                 processElement(n1, n2, container, parentInstance)
>             } else if (shapeFlags & ShapeFlags.STATEFUL_COMPONENT) {
>                 processComponent(n2, container, parentInstance)
>             }
>             break
>     }
> }
> ```
>
> 将上下游更改，同时我们触发渲染逻辑的时候
>
> ```js
> function setupRenderEffect(instance, vnode, container) {
>     effect(() => {
>         if (instance.isMounted) {
>             const subTree = instance.render.call(instance.proxy)
>             vnode.el = subTree.el
>             const preSubTree = instance.subTree
>             instance.subTree = subTree
>             // update 逻辑，preSubTree 传递
>             patch(preSubTree, subTree, container, instance)
>         } else {
>             const subTree = (instance.subTree = instance.render.call(
>                 instance.proxy
>             ))
>             // init 逻辑，preSubTree 传递 null
>             patch(null, subTree, container, instance)
>             vnode.el = subTree.el
>             instance.isMounted = true
>         }
>     })
> }
> ```
>
> 然后我们在处理 element 的时候
>
> ```js
> function processElement(n1, n2, container, parentInstance) {
>     // n1 存在，update 逻辑
>     if (n1) {
>         patchElement(n1, n2, container)
>     } else {
>         // 不存在 init 逻辑
>         mountElement(n1, n2, container, parentInstance)
>     }
> }
> 
> function patchElement(n1, n2, container) {
>     // 此函数用于对比 props 和 children
> }
> ```
>
> 3. 总结
>
> 最后呢，我们发现更新逻辑的核心就在于，将渲染视图部分放入 `effect` 中，这就导致了如果我们的数据是响应式的数据（例如 `ref`、`reactive`），那么这个响应式的数据更新的时候，就会重新触发视图。
>
> 然后我们保存一份上一个 `subTree`，与现在的 `subTree` 进行对比，从而实现新旧对比。
>
> 这就是更新的核心逻辑。

###### 更新props

> 更新 props
>
> 在本小节中，我们将会实现更新 props 的逻辑
>
> 1. 例子
>
> 首先，我们可以写一个例子，来看看更新 props 都会考虑到哪些情况：
>
> ```js
> const props = ref({
>   foo: 'foo',
>   bar: 'bar',
> })
> function patchProp1() {
>   // 逻辑1: old !== new
>   props.value.foo = 'new-foo'
> }
> function patchProp2() {
>   // 逻辑2: new === undefined || null, remove new
>   props.value.bar = undefined
> }
> function patchProp3() {
>   // 逻辑3: old 存在，new 不存在，remove new
>   props.value = {
>     bar: 'bar',
>   }
> }
> ```
>
> 2. 实现
>
> 在上一小节中我们已经在 `render.ts` 中创建了 `patchElement` 用于所有的更新处理。接下来，我们需要在这个函数中加入对 props 的更新逻辑的处理。
>
> ```js
> function patchElement(n1, n2, container) {
>   const oldProps = n1.props || {}
>   const newProps = n2.props || {}
>   patchProps(oldProps, newProps)
> }
> ```
>
> 然后我们在 `patchProps` 中加入多种情况下对于 props 的处理
>
> ```js
> function patchProps(oldProps, newProps) {
>   // 情况1: old !== new 这个走更新的逻辑
>   // 情况2: old 存在，new !== undefined，这个走删除的逻辑
>   // 情况3: old 存在，new 不存在，这个也走删除的逻辑
> }
> ```
>
> 2.1 情况 1
>
> ```js
> function patchElement(n1, n2, container) {
>   const oldProps = n1.props || {}
>   const newProps = n2.props || {}
>   // 这里需要传递 el，我们需要考虑一点，到这一层的时候
>   // n2.el 是 undefined，所以我们需要把 n1.el 赋给 n2.el
>   // 这是因为在下次 patch 的时候 n2 === n1, 此刻的新节点变成旧节点，el 就生效了
>   const el = (n2.el = n1.el)
>   patchProps(el, oldProps, newProps)
> }
> 
> function patchProps(el, oldProps, newProps) {
>   // 如果 oldProps === newProps 那就不需要对比了
>   if (oldProps === newProps) return
>   // 情况1: old !== new 这个走更新的逻辑
>   for (const propKey of Reflect.ownKeys(newProps)) {
>     const oldProp = oldProps[propKey]
>     const newProp = newProps[propKey]
>     // 新旧属性进行对比，如果不相等
>     if (oldProp !== newProp) {
>       // 直接更新属性，这里 hostPatchProp 的时候我们又传入了一个新属性，也就是 oldPropValue
>       // 在 `runtime-dom/index.ts/patchProp` 不要忘记添加这个参数
>       // 这个是为了让用户自己也能主动处理新旧 prop 差异
>       hostPatchProp(el, propKey, newProp, oldProp)
>     }
>   }
> }
> ```
>
> 2.2 情况2
>
> 在情况2 中，`newProp` 是 undefined 或者 null，由于我们处理的逻辑已经抽离到了 `runtime-dom/index` 中，所以这里我们需要去修改那里的代码。
>
> ```js
> function patchProp(el, prop, val, oldVal) {
>   if (isOn(prop)) {
>     const event = prop.slice(2).toLowerCase()
>     el.addEventListener(event, val)
>   } else {
>     console.log({ prop, val, oldVal })
> 		// 情况2: 如果 newVal === undefine || null 的时候，就删除
>     if (val === undefined || null) el.removeAttribute(prop)
>     else el.setAttribute(prop, val)
>   }
> }
> ```
>
> 2.3 情况3
>
> 在情况 3 中，我们去单独循环 newProps 肯定是不对的了，我们还要再去循环一遍 oldProps
>
> ```js
> function patchProps(el, oldProps, newProps) {
>   if (oldProps === newProps) return
>   for (const propKey of Reflect.ownKeys(newProps)) {
>     const oldProp = oldProps[propKey]
>     const newProp = newProps[propKey]
>     if (oldProp !== newProp) {
>       hostPatchProp(el, propKey, newProp, oldProp)
>     }
>   }
>   // 情况3: old 存在，new 不存在，这个也走删除的逻辑
>   // 在情况 3 中我们还要再次去循环一遍 oldProps
>   for (const propKey of Reflect.ownKeys(oldProps)) {
>     // 如果当前的 key 在 newProps 中没有
>     if (!(propKey in oldProps)) {
>       // 就去删除，第三个参数传 undefined 相当于走了情况 2，就调用了删除
>       hostPatchProp(el, propKey, undefined, oldProps[propKey])
>     }
>   }
> }
> ```
>
> 接下来我们还可以进行优化一下：
>
> ```js
> // shared/index
> 
> // 创建一个常量
> export const EMPTY_OBJ = {}
> // render.ts
> 
> function patchElement(n1, n2, container) {
>   // 将 {} ==> EMPTY_OBJ
>   const oldProps = n1.props || EMPTY_OBJ
>   const newProps = n2.props || EMPTY_OBJ
>   const el = (n2.el = n1.el)
>   patchProps(el, oldProps, newProps)
> }
> 
> 
> // patchProps 逻辑中
> // 在循环 oldProps 之前进行判断
> // 如果 oldProps 是空的就不要进行循环了
> if (oldProps !== EMPTY_OBJ) {
>   for (const propKey of Reflect.ownKeys(oldProps)) {
>     if (!(propKey in newProps)) {
>       hostPatchProp(el, propKey, undefined, oldProps[propKey])
>     }
>   }
> }
> ```
>
> 最后，这三种情况我们就都已经支持了。

###### 更新children(==text => array text => newText array => text==)

> 更新 children（一）
>
> 在本小节中，我们将会实现 `children` 更新第一部分逻辑，即
>
> - `text` => `array`
> - `text` => `newText`
> - `array` => `text`
>
> 而最后一部分的逻辑：
>
> - `array` => `array` 由于涉及到 diff 算法，将会在后面篇章中重点讲解
>
> 1. 例子
>
> 查看 [例子](https://github.com/zx-projects/mini-vue/blob/main/example/patchChildren/App.js)
>
> 2. 实现
>
> 2.1 `array` => `text`
>
> 还记得我们是在哪里进行处理更新逻辑的呢？
>
> ```js
> // render.ts
> 
> function patchElement(n1, n2, container) {
>     const oldProps = n1.props || EMPTY_OBJ
>     const newProps = n2.props || EMPTY_OBJ
>     const el = (n2.el = n1.el)
>     patchProps(el, oldProps, newProps)
>     patchChildren(n1, n2)
> }
> ```
>
> 下面我们处理过了 props 还要处理 children
>
> ```js
> function patchChildren(n1, n2) {
>     const prevShapeFlag = n1.shapeFlags
>     const shapeFlag = n2.shapeFlags
>     // 情况1：array => text
>     // 对新的 shapeFlag 进行判断
>     // 如果是文本
>     if (shapeFlag & ShapeFlags.TEXT_CHILDREN) {
>         if (prevShapeFlag & ShapeFlags.ARRAY_CHILDREN) {
>             // 如果老的 shapeFlag 是 array_children，需要做两件事
>             // 1. 清空原有 children
>             unmountChildren(n1.children)
>             // 2. 挂载文本 children
>             hostSetElementText(n2.el, n2.children)
>         } 
>     }
> }
> ```
>
> 如果要从 array => text，需要做两件事情：
>
> - 清空原有 `array_children`
> - 挂载文本 `text_children`
>
> 清空原有 array_children
>
> ```js
> function unmountChildren(children) {
>     for (let i = 0; i < children.length; i++) {
>         // 遍历 children，同时执行 remove 逻辑
>         // 由于这里涉及到元素渲染的实际操作，所以我们要抽离出去作为一个API
>         hostRemove(children[i].el)
>     }
> }
> // runtime-dom/index
> function remove(child) {
>   // 获取到父节点，【parentNode 是 DOM API】
>   const parentElement = child.parentNode
>   if (parentElement) {
>     // 父节点存在，就从父节点中删除这个子节点
>     parentElement.remove(child)
>   }
> }
> ```
>
> 挂载 text_children
>
> 由于挂载 textChildren 也涉及到了视图渲染，所以需要
>
> ```js
> // runtime-dom/index
> function setElementText(el, text) {
>   el.textContent = text
> }
> ```
>
> 现在我们就已经实现了 `text` => `array` 了。
>
> 2.2 `text` => `newText`
>
> 1. 实现
>
> ```js
> // 情况2：text -> newText
> function patchChildren(n1, n2) {
>     const prevShapeFlag = n1.shapeFlags
>     const shapeFlag = n2.shapeFlags
>     const c1 = n1.children
>     const c2 = n2.children
>     // new 是 text，进来
>     if (shapeFlag & ShapeFlags.TEXT_CHILDREN) {
>         if (prevShapeFlag & ShapeFlags.ARRAY_CHILDREN) {
>             unmountChildren(n1.children)
>             hostSetElementText(n2.el, n2.children)
>         } else {
>             // new 不是 array，进来
>             if (c1 !== c2) {
>                 // 新旧 text 进行判断，如果不相等，再进行更新
>                 hostSetElementText(n2.el, c2)
>             }
>         }
>     }
> }
> ```
>
> 2. 优化既有逻辑
>
> 上面那一段代码中，我们发现，`hostSetElementText` 出现了两次，根据判断条件的优化，我们可以这样：
>
> ```js
> if (shapeFlag & ShapeFlags.TEXT_CHILDREN) {
>     if (prevShapeFlag & ShapeFlags.ARRAY_CHILDREN) {
>         // 如果老的 shapeFlag 是 array_children，需要做两件事
>         // 1. 清空原有 children
>         unmountChildren(n1.children)
>         // 2. 挂载文本 children
>     }
>     // 将设置新的文本节点模块放在一起，这样代码量就减少了
>     // 逻辑也更清晰了一点
>     if (c1 !== c2) {
>         hostSetElementText(n2.el, c2)
>     }
> }
> ```
>
> 至此，我们也已经实现了 `text` => `newText` 的逻辑
>
> 2.3 `text` => `array`
>
> ```js
> if (shapeFlag & ShapeFlags.TEXT_CHILDREN) {
>    // 处理 new 是 text_chilren
> } else {
>     // 处理 new 是 array
>     if (prevShapeFlag & ShapeFlags.TEXT_CHILDREN) {
>         // 如果是 text -> array
>         // 1. 清空 text
>         hostSetElementText(n1.el, '')
>         // 2. mountChildren
>         // 这里注意传递的 container, parentInstance 都可以再上游传递过来
>         mountChildren(c2, container, parentInstance)
>     }
> }
> ```
>
> 至此，我们的 3 种更新情况就已经实现了。