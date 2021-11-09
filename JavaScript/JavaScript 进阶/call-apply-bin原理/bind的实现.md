## 1、bin 的实现原理

1. 封装时需要注意的点:

    >1. 参数：与 call() 一样 
    >2. 返回值: 是一个绑定的函数

2. 第一版

    ```js
    let obj = {
          name: 'Tom'
        };

        function fn() {
          console.log(this.name)
        }

        fn.bind(obj)() // Tom
        // ------------------------------------
        /**
        * 需求: 把 fn 的 this 指向 obj 
        * 根据默认绑定 this 法则 , myBind 函数中的 this 就是 fn 这个函数
        * 因为 call() 与 bind() 参数一致,所以这里可以调用 call(),实现改变 this 指向
        * 也可以使用 apply(),但是第二个参数需要解构
        * **/
        Function.prototype.myBind = function (context) {
          const fn = this
          return function func() {
            fn.call(context)
          }
        }

        fn.myBind(obj)() // Tom
    ```

3. 参数问题

    ```js
    let obj = {
      name: 'Tom'
    };

    function fn(a, b, c, d) {
      console.log(this.name)
      console.log(a + b + c + d)
    }
    /**
     * 需求: 把 fn 的 this 指向 obj 
     * 根据默认绑定 this 法则 , myBind 函数中的 this 就是 fn 这个函数
     * 因为 call() 与 bind() 参数一致,所以这里可以调用 call(),实现改变 this 指向
     * 也可以使用 apply(),但是第二个参数需要解构
     * **/
    Function.prototype.myBind = function (context, ...args) {
      const fn = this;
      // 容错
      args = args ? args : [];
      return function func() {
        // arguments : 返回的函数执行时的参数
        fn.call(context, ...args, ...arguments)
      }
    }
    fn.myBind(obj, 1, 2, 3)(4) // Tom  10

    ```

4. 还有一种情况需要考虑: 返回的函数是可以作为构造函数的

```js
    var value = 2;
    var foo = { value: 1};

    function bar(name, age) {
      this.habit = 'shopping';
      console.log(this.value);
      console.log(name);
      console.log(age);
    }
    //  在 bar 函数的 prototype (原型对象) 上添加属性 : frien = "kevin"
    bar.prototype.friend = 'kevin';

    //  bindFoo 是 bind 返回的函数 => 记住此时还没有执行
    var bindFoo = bar.bind(foo, 'daisy');

    //  根据 bindFoo() 函数实例化一个对象;然后隐式执行
    //  因为 new 操作符改变 this 指向; 当前 this 指向是 obj
    var obj = new bindFoo('18');// undefined   daisy   18

    //  habit 和 friend 属性是挂载在 bar 函数上的, obj 会继承 bar 的属性
    console.log(obj.habit);     // shopping
    console.log(obj.friend);    // kevin

// --------------------------实现原理------------------------------------

    Function.prototype.myBind = function (context, ...args) {
          const fn = this;
          // 容错
          args = args ? args : [];
          return function func() {
            // 判断当前函数(this) 是否在 func 这个构造函数上
            if (this instanceof func) {
              return new fn(...args, ...arguments);
            }
            // arguments : 返回的函数执行时的参数
            fn.call(context, ...args, ...arguments)
          }
        }
    //  bindFoo 是 bar.myBind 返回的函数 => 记住此时还没有执行
    let bindFoo = bar.myBind(foo,'daisy')
    //  根据 bindFoo() 函数实例化一个对象;然后隐式执行
    //  因为 new 操作符改变 this 指向; 当前 this 指向是 obj
    let obj = new bindFoo('18')// undefined   daisy   18
// --------------------------------详解----------------------------------

// 第三版
Function.prototype.myBind = function (context,...args) {
    const fn = this;
    // 容错
    args = args ? args : [];

    var func = function () {
        // 当作为构造函数时，this 指向实例，fn 指向绑定函数，因为下面一句 `func.prototype = this.prototype;`，已经修改了 func.prototype 为 绑定函数的 prototype，此时结果为 true，当结果为 true 的时候，this 指向实例。
        // 当作为普通函数时，this 指向 window，fn 指向绑定函数，此时结果为 false，当结果为 false 的时候，this 指向绑定的 context。
        fn.call(this instanceof func ? this : context, ...args,...arguments);
    }
    // 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承函数的原型中的值
    func.prototype = this.prototype;
    return func;
}


```