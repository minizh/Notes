## js 实用小技巧

### 1.将函数的实际参数转换成数组的方法

```

方法一： var args = Array.prototype.slice.call(arguments);

方法二： var args = [].slice.call(arguments, 0);

方法三：

var args = []; 
for (var i = 1; i < arguments.length; i++) { 
    args.push(arguments[i]);
}
```

最后，附个转成数组的通用函数

```
var toArray = function(s){
    try{
        return Array.prototype.slice.call(s);
    } catch(e){
        var arr = [];
        for(var i = 0,len = s.length; i < len; i++){
            //arr.push(s[i]);
               arr[i] = s[i];  //据说这样比push快
        }
         return arr;
    }
}
```

### 2.判断对象类型

https://www.cnblogs.com/joyco773/p/6099715.html

```
Object.prototype.toString.call(arr) === "[Object Array]"
```

### 3.Object.assign(target, source1, source2…) 及 扩展运算符 ‘…’ 实现深拷贝

```
let oldObj = { a: 1, b: 1, c: { n: 3 } };
let newObj1 = Object.assign({}, oldObj);
let newObj2 = JSON.parse(JSON.stringify(oldObj));
let newObj3 = {...{m:1},...{k:2}};
oldObj.c.n = 0;
let multiObj = JSON.parse(JSON.stringify(Object.assign({}, oldObj, newObj2)));
console.log(oldObj, newObj1, newObj2, multiObj, newObj3)
```
