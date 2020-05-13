>**背景**：对于 Vue 这类渐进式前端开发框架，为了构建 SPA（单页面应用），需要引入前端路由系统，这也就是 Vue-Router 存在的意义。

#### 1.vue-router 的两种模式的区别
#### 为什么要有 hash 和 history

>前端路由的核心，就在于 —— 改变视图的同时不会向后端发出请求。

为了达到这一目的，浏览器当前提供了以下两种支持：

**hash** —— 即地址栏 URL 中的 # 符号（此 hash 不是密码学里的散列运算）。

比如这个 URL：http://www.abc.com/#/hello，hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

**history** —— 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）

这两个方法应用于浏览器的历史记录栈，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。

因此可以说，hash 模式和 history 模式都属于浏览器自身的特性，Vue-Router 只是利用了这两个特性（通过调用浏览器提供的接口）来实现前端路由。


#### 2.vue 路由传参的几种基本方式

方法一：

```
// 直接调用$router.push 实现携带参数的跳转:
this.$router.push({
  path: `/describe/${id}`,
})

// 对应路由配置如下：
{
 path: '/describe/:id',
 name: 'Describe',
 component: Describe
}

// 在子组件中可以使用来获取传递的参数值。
this.$route.params.id
```

方法二：

```
// 父组件中：通过路由属性中的name来确定匹配的路由，通过params来传递参数。
this.$router.push({
   	name: 'Describe',
  	params: {
    	id: id
  	}
})

// 对应路由配置: 注意这里不能使用:/id来传递参数了，因为父组件中，已经使用params来携带参数了。
{
	path: '/describe',
 	name: 'Describe',
 	component: Describe
}

// 子组件中: 这样来获取参数
this.$route.params.id
```

方法三：

```
//父组件：使用path来匹配路由，然后通过query来传递参数
//这种情况下 query传递的参数会显示在url后面?id=？
this.$router.push({
    path: '/describe',
    query: {
    	id: id
    }
})

//对应路由配置：
{
 	path: '/describe',
 	name: 'Describe',
 	component: Describe
}

// 对应子组件: 这样来获取参数
this.$route.query.id
```

#### params 传参和 query 传参的差别：

1、用法上的

　　query 要用 path 来引入，params 要用 name 来引入，接收参数都是类似的，分别是 this.$route.query.name 和 this.$route.params.name。

注意接收参数的时候，已经是 $route（当前导航对象） 而不是 $router（路由实例） ！！

2、展示上的

　　query 更加类似于我们 ajax 中 get 传参，params 则类似于 post，说的再简单一点，前者在浏览器地址栏中显示参数，后者则不显示。

**注意：params 传参在页面刷新后，params 中的参数会丢失**


#### 3.vue 监听路由变化的三种方式

方法一：通过 watch
```
// 监听,当路由发生变化的时候执行
watch:{
  $route(to,from){
    console.log(to.path);
  }
}
```

方法二：key 是用来阻止“复用”的。

Vue 为你提供了一种方式来声明“这两个元素是完全独立的——不要复用它们”。只需添加一个具有唯一值的 key 属性即可(Vue文档原话)

```
<router-view :key="key"></router-view>
 
computed: {
  key() {
    return this.$route.name !== undefined? this.$route.name +new Date(): this.$route +new Date()
  }
}
```
使用 computed 属性和 Date() 可以保证每一次的 key 都是不同的，这样就可以如愿刷新数据了。

方法三：通过 vue-router 的钩子函数 beforeRouteEnter  beforeRouteUpdate  beforeRouteLeave

```
beforeRouteEnter (to, from, next) {
  // 在渲染该组件的对应路由被 confirm 前调用
  // 不！能！获取组件实例 `this`
  // 因为当钩子执行前，组件实例还没被创建
},
beforeRouteUpdate (to, from, next) {
  // 在当前路由改变，但是该组件被复用时调用
  // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
  // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
  // 可以访问组件实例 `this`
},
beforeRouteLeave (to, from, next) {
  // 导航离开该组件的对应路由时调用
  // 可以访问组件实例 `this`
},
```


#### 4.vue 手动刷新页面的四种方式

1、this.$router.go(0)

2、location.reload()

3、跳转空白页再跳回原页面

在需要页面刷新的地方写上：this.$router.push('/emptyPage')，跳转到一个空白页。在 emptyPage.vue 里 beforeRouteEnter 钩子里控制页面跳转，从而达到刷新的效果
```
beforeRouteEnter (to, from, next) {
      next(vm => {
        vm.$router.replace(from.path)
      })
}
```

这种画面虽不会一闪，但是能看见路由快速变化。

4、控制 <router-view> 的显示隐藏

默认 <router-view v-if="isRouterAlive" /> isRouterAlive 肯定是true，在需要刷新的时候把这个值设为 false，接着再重新设为 true：
```
this.isRouterAlive = false
this.$nextTick(function () {
  this.isRouterAlive = true
})
```
这种方法从画面上是看不出破绽的。也可以搭配 provide、inject 使用。

首先在我们的根组件APP.vue里面,写入刷新方法，路由初始状态是显示的

```
<template>
  <div id="app">
        <router-view v-if="isRouterAlive"></router-view>
  </div>
</template>
 
<script>
export default {
  name: 'App',
  provide (){
     return {
       reload:this.reload
     }
  },
  data(){
    return {
       isRouterAlive:true
    }
  },
  methods:{
    reload (){
       this.isRouterAlive = false
       this.$nextTick(function(){
          this.isRouterAlive = true
       })
    }
  },
  components:{
  }
}
</script>
```

然后在需要刷新的页面引入依赖：inject: ['reload'],

在需要执行的地方直接调用方法即可：this.reload()。
