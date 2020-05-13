#### 1、如何优化SPA应用的首屏加载速度慢的问题？

1. 将公用的JS库通过script标签外部引入，减小 app.bundel 的大小，让浏览器并行下载资源文件，提高下载速度；

2. 在配置 路由时，页面和组件使用懒加载的方式引入，进一步缩小 app.bundel 的体积，在调用某个组件时再加载对应的js文件；

3. 加一个首屏loading图，提升用户体验；

#### 2、谈谈你对MVVM开发模式的理解

MVVM分为Model、View、ViewModel三者。

- Model：代表数据模型，数据和业务逻辑都在Model层中定义；

- View：代表UI视图，负责数据的展示；

- ViewModel：负责监听Model中数据的改变并且控制视图的更新，处理用户交互操作；

Model和View并无直接关联，而是通过ViewModel来进行联系的，Model和ViewModel之间有着双向数据绑定的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步。

这种模式实现了Model和View的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作dom。


#### 3、简述Vue的响应式原理

1. 当一个Vue实例创建时，vue会遍历data选项的属性，  

2. 用 Object.defineProperty 将它们转为getter/setter并且在内部追踪相关依赖， 
 
3. 在属性被访问和修改时通知变化，

4. 每个组件实例都有相应的watcher程序实例，它会在组件渲染的过程中把属性记录为依赖， 
 
5. 之后当依赖项的setter被调用时，会通知watcher重新计算，从而致使它关联的组件得以更新。

#### 4、解释单向数据流和双向数据绑定

> 单向数据流： 顾名思义，数据流是单向的。数据流动方向可以跟踪，流动单一，追查问题的时候可以更快捷。缺点就是写起来不太方便。要使 UI 发生变更就必须创建各种 action 来维护对应的 state。

> 双向数据绑定：数据之间是相通的，将数据变更的操作隐藏在框架内部。优点是在表单交互较多的场景下，会简化大量与业务无关的代码。缺点就是无法追踪局部状态的变化，增加了出错时 debug 的难度。

#### 6、对 MVC、MVVM 的理解

**MVC**

> 特点：

1. View 传送指令到 Controller；

2. Controller 完成业务逻辑后，要求 Model 改变状态；

3. Model 将新的数据发送到 View，用户得到反馈。

所有通信都是单向的。

**MVVM**

> 特点：

1. 各部分之间的通信，都是双向的；

2. 采用双向绑定： View 的变动，自动反映在 ViewModel，反之亦然。

#### 7、Vue 生命周期的理解

Vue 实例有一个完整的生命周期，生命周期也就是指一个实例从开始创建到销毁的这个过程。

**`1. beforeCreate()`**：在实例创建之间执行，数据未加载状态。

**`2. created()`**：在实例创建、数据加载后，能初始化数据，DOM 渲染之前执行。

**`3. beforeMount()`**：虚拟 DOM 已创建完成，在数据渲染前最后一次更改数据。

**`4. mounted()`**：页面、数据渲染完成，真实 DOM 挂载完成。

**`5. beforeUpadate()`**：重新渲染之前触发。

**`6. updated()`**：数据已经更改完成，DOM 也重新 render 完成，更改数据会陷入死循环。

**`7. beforeDestory()`**：前者是销毁前执行（实例仍然完全可用）。

**`8. destoryed()`**：销毁后执行。

#### 8、组件通信

**父组件向子组件通信**

子组件通过 props 属性，绑定父组件数据，实现双方通信。

**子组件向父组件通信**

将父组件的事件在子组件中通过 $emit 触发。

**非父子组件、兄弟组件之间的数据传递**

````
/*新建一个Vue实例作为中央事件总嫌*/
let event = new Vue();

/*监听事件*/
event.$on('eventName', (val) => { 
	//......do something
});

/*触发事件*/
event.$emit('eventName', 'this is a message.')
````

#### 5、 Vue 如何去除 URL 中的 “#”

vue-router 默认使用 hash 模式，所以在路由加载的时候，项目中的 URL 会自带 “#”。如果不想使用 “#”， 可以使用 vue-router 的另一种模式 history：

```
new Router({
	mode: 'history',
	routes: [ ]
})
```

需要**`注意`**的是，当我们启用 history 模式的时候，由于我们的项目是一个单页面应用，所以在路由跳转的时候，就会出现访问不到静态资源而出现 “404” 的情况，这时候就需要服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 “index.html” 页面。

#### 9、$route 和 $router 的区别

$router 为 VueRouter 实例，想要导航到不同 URL，则使用 $router.push 方法。

$route 为当前 router 跳转对象里面可以获取 name 、 path 、 query 、 params 等。

#### 10、vue-router 使用params与query传参有什么区别

vue-router 可以通过 params 与 query 进行传参

```
// 传递
this.$router.push({path: './xxx', params: {xx:xxx}})
this.$router.push({path: './xxx', query: {xx:xxx}})

// 接收
this.$route.params

this.$route.query
```

>- params 是路由的一部分,必须要有。query 是拼接在 url 后面的参数，没有也没关系

>- params 不设置的时候，刷新页面或者返回参数会丢，query 则不会有这个问题

#### 11. NextTick 是做什么的

$nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM。

> 具体可参考官方文档：深入响应式原理(https://cn.vuejs.org/v2/guide/reactivity.html)。

#### 12. Vue 组件 data 为什么必须是函数

因为 JS 本身的特性带来的，如果 data 是一个对象，那么由于对象本身属于引用类型，当我们修改其中的一个属性时，会影响到所有 Vue 实例的数据。如果将 data 作为一个函数返回一个对象，那么每一个实例的 data 属性都是独立的，不会相互影响了。

#### 13. 计算属性 computed 和事件 methods 有什么区别

我们可以将同一函数定义为一个 method 或者一个计算属性。对于最终的结果，两种方式是相同的。

不同点：

- computed：计算属性是基于它们的依赖进行缓存的，只有在它的相关依赖发生改变时才会重新求值。

- method：只要发生重新渲染， method 调用总会执行该函数。

#### 14. 对比 jQuery ，Vue 有什么不同

jQuery 专注视图层，通过操作 DOM 去实现页面的一些逻辑渲染； Vue 专注于数据层，通过数据的双向绑定，最终表现在 DOM 层面，减少了 DOM 操作。

Vue 使用了组件化思想，使得项目子集职责清晰，提高了开发效率，方便重复利用，便于协同开发。

#### 15. Vue 中 key 的作用

key 的特殊属性主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用 key，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。

> 有相同父元素的子元素必须有独特的 key。重复的 key 会造成渲染错误。

例如：不要将数组下标作为 key 值

假如数组中有三个节点a，b，c，如果使用下标作为 dom 的 key 值。
```
[a,b,c] //0:a,1:b,2:c
```
这时删除 b
```
[a,c] //0:a,1:c
```
原来 key 值为1对应的节点为 b，但是现在变成了 c。这时 key 值就失去了存在的意义，渲染就会有问题，所有为 key 值绑定一个稳定值才是正确的做法。

具体参考官方API(https://cn.vuejs.org/v2/api/#key)。

#### 16. Vue 的核心是什么

数据驱动、组件系统。

#### 17. Vue 等单页面应用的优缺点

> 优点

- 良好的交互体验

- 良好的前后端工作分离模式

- 减轻服务器压力

> 缺点

- SEO 难度较高

- 前进、后退管理

- 初次加载耗时多

#### 18. Vue 数组更改注意事项

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

1. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue

> 原因：对于数组，vue 初始化的时候是无法设置好所有属性（索引），并非通过 defineProperty， 而是通过**监听**能够直接修改数组本身的方法（例如：push, pop, shift, splice 等）来实现响应式的。

2. 当你修改数组的长度时，例如：vm.items.length = newLength

> 原因：js 数组的 length 命名访问器属性是不可配置的（configurable = false），也就无法设置 getter 或者 setter

举个例子：
```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

>为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：

```
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：

```
vm.$set(vm.items, indexOfItem, newValue)
```

>为了解决第二类问题，你可以使用 splice：
```
vm.items.splice(newLength)
```

#### 19. Vue 对象更改检测注意事项

受现代 JavaScript 的限制 (而且 Object.observe 也已经被废弃)，Vue 不能检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 getter/setter 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的。例如：
```
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```

>对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性。例如，对于：
```
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
```
你可以添加一个新的 age 属性到嵌套的 userProfile 对象：
```
Vue.set(vm.userProfile, 'age', 27)
```
你还可以使用 vm.$set 实例方法，它只是全局 Vue.set 的别名：
```
vm.$set(vm.userProfile, 'age', 27)
```
>有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：
```
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
你应该这样做：
```
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

#### 20. delete 和 Vue.delete 删除数组的区别

delete 只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变。

Vue.delete 直接删除了数组 改变了数组的键值。

#### 21.计算属性 computed 与 侦听器 watch

>**computed**： 当你有一些数据需要随着其它数据变动而变动时使用

`我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 getter！如果你不希望有缓存，请用方法来替代`

>**watch**： 当需要在数据变化时执行异步或开销较大的操作时使用

`使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。因为 computed 返回状态是同步的，直接 return。`
