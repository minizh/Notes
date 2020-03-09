#### 一句话介绍 new: ####
> new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一

构造函数
```
function Otaku(name, age) {
    this.name = name;
    this.age = age;
    this.habit = 'Games';
    // return {
    //     name: name,
    //     habit: 'Games',
    // };
}

Otaku.prototype.strength = 60;

Otaku.prototype.sayYourName = function() {
    console.log('I am ' + this.name);
};
```

#### 构造函数有返回值的效果实现 ####

特殊场景：假如构造函数有返回值，举个例子：

```
function Otaku (name, age) {
    this.strength = 60;
    this.age = age;

    return {
        name: name,
        habit: 'Games'
    }
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // undefined
console.log(person.age) // undefined
```


在这个例子中，构造函数返回了一个对象，在实例 person 中只能访问返回的对象中的属性。

而且还要注意一点，在这里我们是返回了一个对象，假如我们只是返回一个基本类型的值呢？

再举个例子：

```
function Otaku (name, age) {
    this.strength = 60;
    this.age = age;

    return 'handsome boy';
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // undefined
console.log(person.habit) // undefined
console.log(person.strength) // 60
console.log(person.age) // 18
```

结果完全颠倒过来，这次尽管有返回值，但是相当于没有返回值进行处理。

所以我们还需要判断返回的值是不是一个对象，如果是一个对象，我们就返回这个对象，如果没有，我们该返回什么就返回什么。

#### 最终版 ####

```
/**
 * 实现步骤
 * 1、创建一个空对象
 * 2、从 arguments 中获取构造函数
 * 3、使空对象的原型指向构造函数的原型对象
 * 4、将 this 指向当前对象
 */
function objectFactory() {
    var obj = new Object(),
        Constructor = [].shift.call(arguments);
    obj.__proto__ = Constructor.prototype;
    const ret = Constructor.apply(obj, arguments);
    // 判断构造函数是否有返回值，如果有且返回的是个对象，则返回改对象，实例只能访问该对象的属性，否则返回实例对象
    return typeof ret === 'object' ? ret : obj;
}

var person = objectFactory(Otaku, 'Kevin', '18');

console.log(person.name); // Kevin
console.log(person.habit); // Games
console.log(person.strength); // 60
console.log(person.age); // undefined

person.sayYourName(); // I am Kevin

```
