#### 1.出现时机：
跳转相同路径，两次或以上，控制台会报错:

vue-router 是先报了一个 Uncaught (in promise) 的错误(因为 push 没加回调)，然后再点击路由的时候才会触发 NavigationDuplicated 的错误(路由出现的错误，全局错误处理打印了出来)：

```
[NavigationDuplicated {_name: "NavigationDuplicated", name: "NavigationDuplicated"}]
```

#### 2.原因

Vue-router 在3.1之后把 $router.push() 方法改为了 Promise。所以假如没有回调函数，错误信息就会交给全局的路由错误处理，因此就会报上述的错误。

#### 3.解决方案

##### 方案1

固定vue-router版本到3.0.7以下。这个方案没什么说的，就是简单粗暴，没有任何理由。但是你能确保以后不升级vue-router吗？

##### 方案2

禁止全局路由错误处理打印，这个也是vue-router开发者给出的解决方案：

```
import Router from 'vue-router'

const originalPush = Router.prototype.push
Router.prototype.push = function push(location, onResolve, onReject) {
  if (onResolve || onReject) return originalPush.call(this, location, onResolve, onReject)
  return originalPush.call(this, location).catch(err => err)
}
```

把这段代码放在引入 vue-router 之后就行。

##### 方案3(高成本高收益)

vue-router 的开发者也给出了解决方法，你需要为每个 router.push 增加回调函数。
```
router.push('/location').catch(err => {err})
```

对于我们来说这个解决方案的成本可能很高，但是是值得的。在 vue-router 3.1 版本之前的 push 调用时不会返回任何信息，假如 push 之后路由出现了问题也不会有任何的错误信息。如果你使用方案1固定了 vue-router 的版本，那么以后的项目需要路由的回调时你根本无从下手。

##### 方案4

如果你使用了 Element-UI，并且方案2无法解决你的问题。那么你只能用方案1来固定你的 vue-router 版本了。这是因为 Element-UI 的 el-menu 在重复点击路由的时候报的错误（ Element 也出现该问题是因为 Element 也使用了 vue-router ），而且这个错误是 Element-UI 内部的路由问题，你无法通过方案2和3去解决。只能选择暂时不升级 Vue-router。

好消息是 Element-UI 在2.13.0版本中解决了这个问题。参考 https://github.com/ElemeFE/element/releases/tag/v2.13.0
