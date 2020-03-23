```
var count = 1;
var container = document.getElementById('container');


function getUserAction(event) {
  container.innerHTML = count++;
  console.log(this);
  console.log(event);
};

function debounce(func, wait, immediate) {

  let timeout, result;

  return function (e) {
    let context = this;
    let args = arguments;
    if (timeout) clearTimeout(timeout);
    if (immediate) {
      // 如果已经执行过，不再执行
      const callNow = !timeout;
      timeout = setTimeout(() => {
        timeout = null;
      }, wait)
      if (callNow) result = func.apply(this, args)
    } else {
      // 箭头函数的this定义：箭头函数的this是在定义函数时绑定的，不是在执行过程中绑定的。简单的说，函数在定义时，this就继承了定义函数的对象。
      timeout = setTimeout(() => {
        func.apply(this, args)
      }, wait);
    }
  }
}

container.onmousemove = debounce.call(container, getUserAction, 1000, false);
console.log('----');
```
