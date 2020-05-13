### Vue 组件通信

#### 1、prop，事件
父 -> 子

父组件：
```
<parent>
	<child v-bind:msg="test msg"></child>
</parent>
```

子组件接收：
```
<child v-bind:msg="test msg"></child>
Vue.component('child', {
  props: ['msg', 'myMessage'...]  
  // 或者
  props: {
	// 检测类型
    msg: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})
```

子 -> 父

父组件：

```
Vue.component('parent', {
	template: `
		<child @sendMsg="getMsg"></child>
	`,
	methods: () => {
		getMsg(msg) {
			
		}
	}
})
```

子组件： 

```
Vue.component('child', {
	methods: () => {
		sendMsg() {
			this.$emit('getMsg', 'msg');
		}
	}
})
```

#### 2、非父子组件通信

1、创建公共实例

```
// 新建一个文件
const bus=New Vue();

// 发送组件中导入并发送事件
bus.$emit('事件名'， 参数)；

// 其他组件监听
bus.$on('事件名'，接受的参数)
```

2.vuex

#### 3、访问实例

1.访问根组件

```
// 获取根组件的数据
this.$root.foo

// 写入根组件的数据
this.$root.foo = 2

// 访问根组件的计算属性
this.$root.bar

// 调用根组件的方法
this.$root.baz()
```

2.访问父组件实例

子组件中： 

```
this.$parent.getMap();
```

##### 深层嵌套

父组件：

```
Vue.component('parent', {
	template: `
		<child></child>
	`,
	provide: function () {
	  return {
	    getMap: this.getMap
	  }
	}
})

```

然后在任何后代组件里，我们都可以使用 inject 选项来接收指定的我们想要添加在这个实例上的属性：

```
Vue.component('child', {
	inject: ['getMap']
})
```

3.访问子组件实例或子元素

父组件：

```
Vue.component('parent', {
	template: `
		<child ref=“childInstance”></child>
	`,
	methods: () => {
		getMsg(msg) {
			this.$ref.childInstance.click()
		}
	},
	provide: function () {
	  return {
	    getMap: this.getMap
	  }
	}
})

```

子组件：

```
Vue.component('child', {
	template: `
		<button ref="btn"></button>
	`,
	prov
	methods: () => {
		onClick() {
			this.$ref.btn.click()
		}
	},
})
```




