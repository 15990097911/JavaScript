### 1、 call 改变 this 的函数实现原理

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
       * 2. 返回值： 要知道我们要返回的是什么
       * **/
      Function.prototype.myCall = function (context, ...args) {
        // context : this需要指向的对象
        // args: 因为不知道有多少个参数，所以这里使用扩展运算符 可以使用 arguments 代替 // [1, 2, 3, 4]

        // 先做一个容错处理
        context = context || window;
        args = args ? args : [];

        // 给context新增一个独一无二的属性以免覆盖原有属性
        const key = Symbol();
        context[key] = this;
        //通过隐式绑定的方式调用函数
        const result = context[key](...args);
        //删除添加的属性
        delete context[key];
        //返回函数调用的返回值
        return result;
      };

      obj2.getName.myCall(obj1, 1, 2, 3, 4);
      // tom
      // test.html:16 Arguments(4) [1, 2, 3, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
      // test.html:17 10

```