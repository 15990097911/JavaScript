## 1、bin 的实现原理

```js
      let obj1 = { name: "tom" };
      let obj2 = {
        name: "joy",
        getName(a, b, c, d) {
          console.log(this.name);
          console.log(arguments);
          console.log(a + b + c + d);
        },
      };
      // this： 谁调用指向谁
      // obj2.getName(); // joy

      /**
       * 封装时需要注意的点
       * 1. 参数：应为时不知道有多少个参数，但是最低应该会有一个
       * 2. 返回值：一个绑定好的的函数
       * 3. 与call 的区别：返回值不同
       * 4. 与apply 的区别： 参数不同，返回值也不同
       * **/
      Function.prototype.myBind = function (context, ...args) {
        // context : this需要指向的对象
        // args: 因为不知道有多少个参数，所以这里使用扩展运算符 可以使用 arguments 代替 // [1, 2, 3, 4]
        const fn = this;
        args = args ? args : [];
        return function newFn(...newFnArgs) {
          //  this instanceof newFn  检测 this 这个函数 在不在 newFn 这个函数上
          //  在的话直接调用 fn 函数
          if (this instanceof newFn) {
            return new fn(...args, ...newFnArgs);
          }
          return fn.apply(context, [...args, ...newFnArgs]);
        };
      };

       let  fun =  obj2.getName.myBind(obj1, 1, 2, 3, 4)();
      // tom
      // Arguments(4) [1, 2, 3, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
      // 10
```