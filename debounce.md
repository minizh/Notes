#### Test code 

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Debounce</title>
    <style>
      #container {
        width: 100%;
        height: 200px;
        line-height: 200px;
        text-align: center;
        color: #fff;
        background-color: #444;
        font-size: 30px;
      }
    </style>
  </head>
  <body>
    <div id="container"></div>
    <script>
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
    </script>
  </body>
</html>

```
