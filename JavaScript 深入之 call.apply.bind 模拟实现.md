#### 1.模拟 call ####
```
Function.prototype.myCall = function (obj) {
    obj = obj ? Object(obj) : window;
    obj.fn = this;
    let args = [...arguments].slice(1);
    const result = obj.fn(...args);
    delete obj.fn;
    return result;
};
```
#### 2.模拟 apply ####
```
Function.prototype.myApply = function(obj, args) {
    obj = obj ? Object(obj) : window;
    obj.fn = this;
    let result;
    if (!args) {
        result = obj.fn();
    } else {
        result = obj.fn(...args);
    }
    delete obj.fn;
    return result;
};
```

#### 3. 模拟 bind ####
```
Function.prototype.myBind = function(obj) {
    obj = obj ? Object(obj) : window;
    let fn = this;
    let args = Array.prototype.slice.myCall(arguments, 1);
    return function() {
        args = args.concat(Array.prototype.slice.myCall(arguments));
        return fn.myApply(obj, args);
    };
};
```

#### 4.模拟 bind （完整版）####
当 bind 返回的函数作为构造函数的时候，bind 时指定的 this 值会失效

```
Function.prototype.myBindNew = function(obj) {
    if (typeof this !== 'function') {
        throw new Error('Function.prototype.bind - what is trying to be bound is not callable');
    }
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);
    var fNOP = function() {};
    var fBound = function() {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : obj, args.concat(bindArgs));
    };
    // 方案1：修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值
    // fBound.prototype = this.prototype;
    // 方案1缺陷：直接修改 fBound.prototype 的时候，会直接修改绑定函数的 prototype
    // 方案2：指定一个空函数 fNOP 中转
    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
};
```
#### 5.测试 ####
```
let o = { value: 'Hellow World!' };
function f(v, j, k, i) {
    return `${this.value} ${v}-${j}-${k}-${i}`;
}
console.log(`call: ${f.call(o, 1, 2, 3)}`);
console.log(`myCall: ${f.myCall(o, 1, 2, 3)}`);
// call: Hellow World! 1-2-3-undefined
// myCall: Hellow World! 1-2-3-undefined
console.log('---------------');
console.log(`apply: ${f.apply(o, [1, 2, 3])}`);
console.log(`myApply: ${f.myApply(o, [1, 2, 3])}`);
// apply: Hellow World! 1-2-3-undefined
// myApply: Hellow World! 1-2-3-undefined
console.log('---------------');
let fn = f.bind(o, 1, 2, 3);
console.log(`bind: ${fn(4)}`);
let fn2 = f.myBind(o, 1, 2, 3);
console.log(`myBind: ${fn2(4)}`);
// bind: Hellow World! 1-2-3-4
// myBind: Hellow World! 1-2-3-4
let fn3 = f.myBindNew(o, 1, 2, 3);
let objNew = new fn3(4);
console.log(`myBindNew: ${fn3(5)}`);
// myBindNew: Hellow World! 1-2-3-5
```
