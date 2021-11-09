## String

### 1、转义字符

1. 除了普通的可打印字符以外，一些有特殊功能的字符可以通过转义字符的形式放入字符串中：

   | code                 | Output               |
   | -------------------- | -------------------- |
   | \0                   | 空字符               |
   | \\'                  | 单引号               |
   | \\"                  | 双引号               |
   | \\\                  | 反斜杠               |
   | \n                   | 换行                 |
   | \r                   | 回车                 |
   | \v                   | 垂直制表符           |
   | \t                   | 水平制表符           |
   | \b                   | 退格                 |
   | \f                   | 换页                 |
   | \uXXXX               | unicode 码           |
   | \u{X} ... \u{XXXXXX} | unicode codepoint    |
   | \xXX                 | Latin-1 字符(x 小写) |

### 2、 长字符串

1. 有时，你的代码可能含有很长的字符串。你可能想将这样的字符串写成多行，而不是让这一行无限延长或着被编辑器折叠。有两种方法可以做到这一点。

   ```js

   // 可以使用 + 运算符将多个字符串连接起来

   let longString =
   "This is a very long string which needs " +
   "to wrap across multiple lines because " +
   "otherwise my code is unreadable.";

   // 可以在每行末尾使用反斜杠字符（“\”），以指示字符串将在下一行继续。确保反斜杠后面没有空格或任何除换行符之外的字符或缩进; 否则反斜杠将不会工作

   let longString =
   "This is a very long string which needs \
   to wrap across multiple lines because \
   otherwise my code is unreadable.";

   // 字符串字面量也可以称为模板字面量

   `hello world` `hello! world!` `hello ${who}` escape `<a>${who}</a>`
   ```

### 3、字符串比较

1. JS 字符串在进行大于(小于)比较时，会根据第一个不同的字符的 ASCII 值码进行比较，情况分为多种：

2. 当数字跟字符串进行数学运算时(不光是>,<,=,还包括 ±\*/等运算)，会把字符串转换成数字。

3. 如果字符串是数字的形式，转换成数字后就直接根据数字大小进行比较。

4. 如果字符串不是不是数字形式，不管是’a’还是’100a’这样的形式，在转换时都转成了 NaN，而 NaN 跟数字比较大小时，只会返回 false。

   ```js
   var a = "a";
   var b = "b";
   if (a < b)
     // true
     print(a + " is less than " + b);
   else if (a > b) print(a + " is greater than " + b);
   else print(a + " and " + b + " are equal.");
   ```

### 4、基本字符串和字符串对象的区别

1. 字符串字面量 (通过单引号或双引号定义) 和 直接调用 String 方法(没有通过 new 生成字符串对象实例)的字符串都是基本字符串

2. JavaScript 会自动将基本字符串转换为字符串对象，只有将基本字符串转化为字符串对象之后才可以使用字符串对象的方法

   ```js
   var s_prim = "foo";
   var s_obj = new String(s_prim);

   console.log(typeof s_prim); // Logs "string"
   console.log(typeof s_obj); // Logs "object"
   ```

3. 当使用 eval 时，基本字符串和字符串对象也会产生不同的结果。eval 会将基本字符串作为源代码处理; 而字符串对象则被看作对象处理, 返回对象。

   ```js
   s1 = "2 + 2"; // creates a string primitive
   s2 = new String("2 + 2"); // creates a String object
   console.log(eval(s1)); // returns the number 4
   console.log(eval(s2)); // returns the string "2 + 2"
   ```

4. 利用 valueOf 方法，我们可以将字符串对象转换为其对应的基本字符串。

   ```js
   s1 = "2 + 2"; // creates a string primitive
   s2 = new String("2 + 2"); // creates a String object
   console.log(eval(s2.valueOf())); // returns the number 4
   ```

### 5、String 的属性

1. String 只有一个属性 length

2. String.prototype 可以为 String 对象增加新的属性

### 6、String 的方法

1. String.fromCharCode(num1[, ...[, numN]]) 通过一串 Unicode 创建字符串。

   ```js
   // 参数： num1, ..., numN 一系列 UTF-16 代码单元的数字。范围介于 0 到 65535（0xFFFF）之间。大于 0xFFFF 的数字将被截断。不进行有效性检查

   // 返回值： 该方法返回一个字符串，而不是一个  String 对象

   // 由于 fromCharCode() 是  String 的静态方法，所以应该像这样使用：String.fromCharCode()，而不是作为你创建的  String 对象的方法。

   // 使用 fromCharCode() 在 UTF-16 中，BMP 字符使用一个代码单元：

   String.fromCharCode(65, 66, 67); // 返回 "ABC"
   String.fromCharCode(0x2014); // 返回 "—"
   String.fromCharCode(0x12014); // 也是返回 "—"; 数字 1 被剔除并忽略
   String.fromCharCode(8212); // 也是返回 "—"; 8212 是 0x2014 的十进制表示
   ```

2. String.fromCodePoint(num1[, ...[, numN]]) 通过一串 码点 创建字符串

   ```js
   // 参数： num1, ..., numN 一串 Unicode 编码位置，即“代码点”

   // 返回值： 使用指定的 Unicode 编码位置创建的字符串

   // 异常： 如果传入无效的 Unicode 编码，将会抛出一个RangeError (例如： "RangeError: NaN is not a valid code point")。

   // 说明： 该方法返回一个字符串，而不是一个 String 对象。

   //  使用 fromCodePoint()

   String.fromCodePoint(42); // "*"
   String.fromCodePoint(65, 90); // "AZ"
   String.fromCodePoint(0x404); // "\u0404"
   String.fromCodePoint(0x2f804); // "\uD87E\uDC04"
   String.fromCodePoint(194564); // "\uD87E\uDC04"
   String.fromCodePoint(0x1d306, 0x61, 0x1d307); // "\uD834\uDF06a\uD834\uDF07"

   String.fromCodePoint("_"); // RangeError
   String.fromCodePoint(Infinity); // RangeError
   String.fromCodePoint(-1); // RangeError
   String.fromCodePoint(3.14); // RangeError
   String.fromCodePoint(3e-2); // RangeError
   String.fromCodePoint(NaN); // RangeError

   // String.fromCharCode() 方法不能单独获取在高代码点位上的字符
   // 另一方面，下列的示例中，可以返回 4 字节，也可以返回 2 字节的字符
   // (也就是说，它可以返回单独的字符，使用长度 2 代替 1!）
   console.log(String.fromCodePoint(0x2f804)); // or 194564 in decimal

   //  Polyfill ：String.fromCodePoint 方法是 ECMAScript2015（ES6）新增加的特性，所以一些老的浏览器可能还不支持。可以通过使用下面的 polyfill 代码来保证浏览器的支持

   if (!String.fromCodePoint)
     (function (stringFromCharCode) {
       var fromCodePoint = function (_) {
         var codeUnits = [],
           codeLen = 0,
           result = "";
         for (var index = 0, len = arguments.length; index !== len; ++index) {
           var codePoint = +arguments[index];
           // correctly handles all cases including `NaN`, `-Infinity`, `+Infinity`
           // The surrounding `!(...)` is required to correctly handle `NaN` cases
           // The (codePoint>>>0) === codePoint clause handles decimals and negatives
           if (!(codePoint < 0x10ffff && codePoint >>> 0 === codePoint))
             throw RangeError("Invalid code point: " + codePoint);
           if (codePoint <= 0xffff) {
             // BMP code point
             codeLen = codeUnits.push(codePoint);
           } else {
             // Astral code point; split in surrogate halves
             // https://mathiasbynens.be/notes/javascript-encoding#surrogate-formulae
             codePoint -= 0x10000;
             codeLen = codeUnits.push(
               (codePoint >> 10) + 0xd800, // highSurrogate
               (codePoint % 0x400) + 0xdc00 // lowSurrogate
             );
           }
           if (codeLen >= 0x3fff) {
             result += stringFromCharCode.apply(null, codeUnits);
             codeUnits.length = 0;
           }
         }
         return result + stringFromCharCode.apply(null, codeUnits);
       };
       try {
         // IE 8 only supports `Object.defineProperty` on DOM elements
         Object.defineProperty(String, "fromCodePoint", {
           value: fromCodePoint,
           configurable: true,
           writable: true,
         });
       } catch (e) {
         String.fromCodePoint = fromCodePoint;
       }
     })(String.fromCharCode);
   ```

3. String.raw(callSite, ...substitutions) 通过模板字符串创建字符串

   ```js
   String.raw(callSite, ...substitutions);

   String.raw`templateString`;
   // callSite：  一个模板字符串的“调用点对象”。类似{ raw: ['foo', 'bar', 'baz'] }

   // ...substitutions： 任意个可选的参数，表示任意个内插表达式对应的值。

   //  templateString： 模板字符串，可包含占位符（${...}）。

   // 返回值 ： 给定模板字符串的原始字符串。

   // 异常： 如果第一个参数没有传入一个格式正确的对象，则会抛出 TypeError 异常

   //  使用 String.raw()

   String.raw`Hi\n${2 + 3}!`;
   // 'Hi\\n5!'，Hi 后面的字符不是换行符，\ 和 n 是两个不同的字符

   String.raw`Hi\u000A!`;
   // "Hi\\u000A!"，同上，这里得到的会是 \、u、0、0、0、A 6个字符，
   // 任何类型的转义形式都会失效，保留原样输出，不信你试试.length

   let name = "Bob";
   String.raw`Hi\n${name}!`;
   // "Hi\nBob!"，内插表达式还可以正常运行

   // 正常情况下，你也许不需要将 String.raw() 当作函数调用。
   // 但是为了模拟 `t${0}e${1}s${2}t` 你可以这样做:
   String.raw({ raw: "test" }, 0, 1, 2); // 't0e1s2t'
   // 注意这个测试, 传入一个 string, 和一个类似数组的对象
   // 下面这个函数和 `foo${2 + 3}bar${'Java' + 'Script'}baz` 是相等的.
   String.raw(
     {
       raw: ["foo", "bar", "baz"],
     },
     2 + 3,
     "Java" + "Script"
   ); // 'foo5barJavaScriptbaz'
   ```

4. String.prototype.charAt(index) 从一个字符串中返回指定的字符

   ```js
   // 参数： index 一个介于0 和字符串长度减1之间的整数。 (0~length-1)

   // 例子：输出字符串中不同位置的字符如果没有提供索引，charAt() 将使用0。

   var anyString = "Brave new world";
   anyString.charAt(0); // 'B'
   anyString.charAt(1); // 'r'
   anyString.charAt(2); // 'a'
   anyString.charAt(3); // 'v'
   anyString.charAt(4); // 'e'
   anyString.charAt(999); // ''
   ```

5. String.prototype.charCodeAt(index) 返回 0 到 65535 之间的整数，表示给定索引处的 UTF-16 代码单元

   ```js
   // 参数: index 一个大于等于 0，小于字符串长度的整数。如果不是一个数值，则默认为 0。

   // 返回值： 指定 index 处字符的 UTF-16 代码单元值的一个数字；如果 index 超出范围，charCodeAt() 返回 NaN

   // 使用 charCodeAt()

   "ABC".charCodeAt(0); // returns 65:"A"

   "ABC".charCodeAt(1); // returns 66:"B"

   "ABC".charCodeAt(2); // returns 67:"C"

   "ABC".charCodeAt(3); // returns NaN
   ```

6. String.prototype.codePointAt(pos) 返回 一个 Unicode 编码点值的非负整数

   ```js
   // 参数： pos  这个字符串中需要转码的元素的位置。

   // 返回值： 返回值是在字符串中的给定索引的编码单元体现的数字，如果在索引处没找到元素则返回 undefined 。

   // 使用 codePointAt()

   "ABC".codePointAt(1); // 66
   "\uD800\uDC00".codePointAt(0); // 65536

   "XYZ".codePointAt(42); // undefined
   ```

7. String.prototype.concat(str2, [, ...strN]) 将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回

   ```js
   // 参数： str2 [, ...strN] 需要连接到 str 的字符串。

   // 返回值： 一个新的字符串，包含参数所提供的连接字符串。

   // 性能： 强烈建议使用赋值操作符（+, +=）代替 concat 方法。

   // 示例： 下面的例子演示如何将多个字符串与原字符串合并为一个新字符串

   let hello = "Hello, ";
   console.log(hello.concat("Kevin", ". Have a nice day."));
   // Hello, Kevin. Have a nice day.

   let greetList = ["Hello", " ", "Venkat", "!"];
   "".concat(...greetList); // "Hello Venkat!"

   "".concat({}); // [object Object]
   "".concat([]); // ""
   "".concat(null); // "null"
   "".concat(true); // "true"
   "".concat(4, 5); // "45"
   ```

8. String.prototype.endsWith(searchString[, length]) 用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 true 或 false

   ```js
   // searchString ： 要搜索的子字符串。

   // length：  可选，作为 str 的长度。默认值为 str.length。

   // 返回值： 如果传入的子字符串在搜索字符串的末尾则返回true；否则将返回 false。

   // 示例：

   var str = "To be, or not to be, that is the question.";

   str.endsWith("question."); // true
   str.endsWith("to be"); // false
   str.endsWith("to be", 19); // true

   // 兼容补丁 ：这个方法已经加入到 ECMAScript 6 标准当中，但是可能还没有在所有的  JavaScript 实现中可用。你可以通过如下的代码片段扩展 String.prototype.endsWith() 实现兼容

   if (!String.prototype.endsWith) {
     String.prototype.endsWith = function (search, this_len) {
       if (this_len === undefined || this_len > this.length) {
         this_len = this.length;
       }
       return this.substring(this_len - search.length, this_len) === search;
     };
   }
   ```

9. String.prototype.includes(searchString[, position]) 用于判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false

   ```js
   // searchString： 要在此字符串中搜索的字符串。

   // position ： 可选，从当前字符串的哪个索引位置开始搜寻子字符串，默认值为 0。

   // 返回值： 如果当前字符串包含被搜寻的字符串，就返回 true；否则返回 false。

   // 描述： 这个方法可以帮你判断一个字符串是否包含另外一个字符串。

   // --------------------------------------------------------------------------------------------------------------------------

   // 区分大小写： includes() 方法是区分大小写的。例如，下面的表达式会返回 false ：

   "Blue Whale".includes("blue"); // returns false

   // --------------------------------------------------------------------------------------------------------------------------

   // 兼容补丁：这个方法已经被加入到 ECMAScript 6 标准中，但未必在所有的 JavaScript 实现中都可以使用。然而，你可以轻松地 兼容 这个方法

   if (!String.prototype.includes) {
     String.prototype.includes = function (search, start) {
       "use strict";
       if (typeof start !== "number") {
         start = 0;
       }

       if (start + search.length > this.length) {
         return false;
       } else {
         return this.indexOf(search, start) !== -1;
       }
     };
   }

   // --------------------------------------------------------------------------------------------------------------------------

   // 示例：

   var str = "To be, or not to be, that is the question.";
   str.includes("To be"); // true
   str.includes("question"); // true
   str.includes("nonexistent"); // false
   str.includes("To be", 1); // false
   str.includes("TO BE"); // false
   ```

10. String.prototype.indexOf(searchValue [, fromIndex]) 返回调用它的 String 对象中第一次出现的指定值的索引，从 fromIndex 处进行搜索。如果未找到该值，则返回 -1

    ```js
    // searchValue： 要被查找的字符串值。

    "undefined".indexOf(); // 0   如果没有提供确切地提供字符串，searchValue 会被强制设置为 "undefined"

    // --------------------------------------------------------------------------------------------------------------------------

    // fromIndex：  可选，数字表示开始查找的位置。可以是任意整数，默认值为 0。

    "hello world".indexOf("o", -5); //  4   如果 fromIndex 的值小于 0，或者大于 str.length ，那么查找分别从 0 和str.length 开始
    "hello world".indexOf("o", 11); // -1

    // 返回值: 查找的字符串 searchValue 的第一次出现的索引，如果没有找到，则返回 -1。

    // --------------------------------------------------------------------------------------------------------------------------

    // 若被查找的字符串 searchValue 是一个空字符串，将会产生“奇怪”的结果

    "hello world".indexOf(""); // 返回 0
    "hello world".indexOf("", 0); // 返回 0
    "hello world".indexOf("", 3); // 返回 3
    "hello world".indexOf("", 8); // 返回 8

    // --------------------------------------------------------------------------------------------------------------------------

    // 如果 fromIndex 值大于等于字符串的长度，将会直接返回字符串的长度

    "hello world".indexOf("", 11); // 返回 11
    "hello world".indexOf("", 13); // 返回 11
    "hello world".indexOf("", 22); // 返回 11

    // --------------------------------------------------------------------------------------------------------------------------

    // 描述: 字符串中的字符被从左向右索引。第一个字符的索引（index）是 0，变量名为 stringName 的字符串的最后一个字符的索引是 stringName.length - 1

    "Blue Whale".indexOf("Blue"); // 返回 0
    "Blue Whale".indexOf("Blute"); // 返回 -1
    "Blue Whale".indexOf("Whale", 0); // 返回 5
    "Blue Whale".indexOf("Whale", 5); // 返回 5
    "Blue Whale".indexOf("", -1); // 返回 0
    "Blue Whale".indexOf("", 9); // 返回 9
    "Blue Whale".indexOf("", 10); // 返回 10
    "Blue Whale".indexOf("", 11); // 返回 10

    // --------------------------------------------------------------------------------------------------------------------------

    // indexOf 方法是区分大小写的。例如，下面的表达式将返回 -1：

    "Blue Whale".indexOf("blue"); // 返回 -1

    // --------------------------------------------------------------------------------------------------------------------------

    // 示例:下例使用 indexOf() 和 lastIndexOf() 方法定位字符串中 "Brave new world" 的值。

    var anyString = "Brave new world";

    console.log(
      "The index of the first w from the beginning is " + anyString.indexOf("w")
    );
    // logs 8
    console.log(
      "The index of the first w from the end is " + anyString.lastIndexOf("w")
    );
    // logs 10

    console.log(
      "The index of 'new' from the beginning is " + anyString.indexOf("new")
    );
    // logs 6
    console.log(
      "The index of 'new' from the end is " + anyString.lastIndexOf("new")
    );
    // logs 6

    // --------------------------------------------------------------------------------------------------------------------------

    // indexOf 和区分大小写

    var myString = "brie, pepper jack, cheddar";
    var myCapString = "Brie, Pepper Jack, Cheddar";

    console.log(
      'myString.indexOf("cheddar") is ' + myString.indexOf("cheddar")
    );
    // logs 19
    console.log(
      'myCapString.indexOf("cheddar") is ' + myCapString.indexOf("cheddar")
    );
    // logs -1
    ```

11. String.prototype.lastIndexOf(searchValue[, fromIndex])

    ```js
    // searchValue: 一个字符串，表示被查找的值。如果searchValue是空字符串，则返回fromIndex。

    // fromIndex: 可选待匹配字符串searchValue的开头一位字符从 str的第fromIndex位开始向左回向查找。fromIndex默认值是 +Infinity

    // --------------------------------------------------------------------------------------------------------------------------

    //  fromIndex >= str.length ，则会搜索整个字符串。如果 fromIndex < 0 ，则等同于 fromIndex == 0。

    // --------------------------------------------------------------------------------------------------------------------------

    // 返回值:  返回指定值最后一次出现的索引(该索引仍是以从左至右0开始记数的)，如果没找到则返回-1。

    // --------------------------------------------------------------------------------------------------------------------------

    // 描述: 字符串中的字符被从左向右索引。首字符的索引（index）是 0，最后一个字符的索引是 stringName.length - 1。

    "canal".lastIndexOf("a"); // returns 3 （没有指明fromIndex则从末尾l处开始反向检索到的第一个a出现在l的后面，即index为3的位置）
    "canal".lastIndexOf("a", 2); // returns 1（指明fromIndex为2则从n处反向向回检索到其后面就是a，即index为1的位置）
    "canal".lastIndexOf("a", 0); // returns -1(指明fromIndex为0则从c处向左回向检索a发现没有，故返回-1)
    "canal".lastIndexOf("x"); // returns -1
    "canal".lastIndexOf("c", -5); // returns 0（指明fromIndex为-5则视同0，从c处向左回向查找发现自己就是，故返回0）
    "canal".lastIndexOf("c", 0); // returns 0（指明fromIndex为0则从c处向左回向查找c发现自己就是，故返回自己的索引0）
    "canal".lastIndexOf(""); // returns 5
    "canal".lastIndexOf("", 2); // returns 2

    // --------------------------------------------------------------------------------------------------------------------------

    //lastIndexOf 方法区分大小写。例如，下面的表达式返回 -1：

    "Blue Whale, Killer Whale".lastIndexOf("blue"); // returns -1

    // --------------------------------------------------------------------------------------------------------------------------

    // 示例: 下例使用 indexOf 和 lastIndexOf 方法来定位字符串 "Brave new world" 中的值。

    var anyString = "Brave new world";

    console.log(
      "The index of the first w from the beginning is " + anyString.indexOf("w")
    );
    // Displays 8
    console.log(
      "The index of the first w from the end is " + anyString.lastIndexOf("w")
    );
    // Displays 10

    console.log(
      "The index of 'new' from the beginning is " + anyString.indexOf("new")
    );
    // Displays 6
    console.log(
      "The index of 'new' from the end is " + anyString.lastIndexOf("new")
    );
    // Displays 6
    ```

12. String.prototype.localeCompare(compareString[, locales[, options]]) 返回一个数字来指示一个参考字符串是否在排序顺序前面或之后或与给定字符串相同

    ```js
    // compareString： 用来比较的字符串

    // --------------------------------------------------------------------------------------------------------------------------

    // locales： 可选。 用来表示一种或多种语言或区域的一个符合 BCP 47 标准的字符串或一个字符串数组。 locales参数的一般形式与解释

    console.log("ä".localeCompare("z", "de")); // a negative value: in German, ä sorts with a
    console.log("ä".localeCompare("z", "sv")); // a positive value: in Swedish, ä sorts after z

    // --------------------------------------------------------------------------------------------------------------------------

    // options： 可选。

    // in German, ä has a as the base letter
    console.log("ä".localeCompare("a", "de", { sensitivity: "base" })); // 0

    // in Swedish, ä and a are separate base letters
    console.log("ä".localeCompare("a", "sv", { sensitivity: "base" })); // a positive value

    // 返回值：  如果引用字符存在于比较字符之前则为负数; 如果引用字符存在于比较字符之后则为正数; 相等的时候返回 0 .
    ```

13. String.prototype.match(regexp)

    ```js
    // 参数： regexp 一个正则表达式对象。如果传入一个非正则表达式对象，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp

    // 如果你没有给出任何参数并直接使用match() 方法 ，你将会得到一 个包含空字符串的 Array ：[""]

    // 返回值 ： 如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组

    // 如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（Array）。 在这种情况下，返回的项目将具有如下所述的其他属性

    // --------------------------------------------------------------------------------------------------------------------------

    // 匹配的结果包含如下所述的附加特性：

    groups; // 一个捕获组数组 或 undefined（如果没有定义命名捕获组）。

    index; // 匹配的结果的开始位置

    input; // 搜索的字符串.

    // 一个Array，其内容取决于global（g）标志的存在与否，如果未找到匹配则为null。

    // --------------------------------------------------------------------------------------------------------------------------

    // 描述： 如果正则表达式不包含 g 标志，str.match() 将返回与 RegExp.exec(). 相同的结果。

    // 示例： 在下例中，使用 match 查找 "Chapter" 紧跟着 1 个或多个数值字符，再紧跟着一个小数点和数值字符 0 次或多次。正则表达式包含 i 标志，因此大小写会被忽略

    var str = "For more information, see Chapter 3.4.5.1";
    var re = /see (chapter \d+(\.\d)*)/i;
    var found = str.match(re);

    console.log(found);

    // logs [ 'see Chapter 3.4.5.1',
    //        'Chapter 3.4.5.1',
    //        '.1',
    //        index: 22,
    //        input: 'For more information, see Chapter 3.4.5.1' ]

    // 'see Chapter 3.4.5.1' 是整个匹配。
    // 'Chapter 3.4.5.1' 被'(chapter \d+(\.\d)*)'捕获。
    // '.1' 是被'(\.\d)'捕获的最后一个值。
    // 'index' 属性(22) 是整个匹配从零开始的索引。
    // 'input' 属性是被解析的原始字符串。

    // --------------------------------------------------------------------------------------------------------------------------

    // 例子：match 使用全局（global）和忽略大小写（ignore case）标志

    var str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    var regexp = /[A-E]/gi;
    var matches_array = str.match(regexp);

    console.log(matches_array);
    // ['A', 'B', 'C', 'D', 'E', 'a', 'b', 'c', 'd', 'e']

    // --------------------------------------------------------------------------------------------------------------------------

    // 使用match()，不传参数

    var str = "Nothing will come of nothing.";

    str.match(); // returns [""]

    // --------------------------------------------------------------------------------------------------------------------------

    // 一个非正则表达式对象作为参数

    var str1 =
        "NaN means not a number. Infinity contains -Infinity and +Infinity in JavaScript.",
      str2 =
        "My grandfather is 65 years old and My grandmother is 63 years old.",
      str3 = "The contract was declared null and void.";
    str1.match("number"); // "number" 是字符串。返回["number"]
    str1.match(NaN); // NaN的类型是number。返回["NaN"]
    str1.match(Infinity); // Infinity的类型是number。返回["Infinity"]
    str1.match(+Infinity); // 返回["Infinity"]
    str1.match(-Infinity); // 返回["-Infinity"]
    str2.match(65); // 返回["65"]
    str2.match(+65); // 有正号的number。返回["65"]
    str3.match(null); // 返回["null"]
    ```

14. String.prototype.matchAll(regexp) 返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器

    ```js
    // 参数： regexp 一个正则表达式对象。如果传入一个非正则表达式对象，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp

    // 返回值： 一个迭代器（不可重用，结果耗尽需要再次调用方法，获取一个新的迭代器）。

    // --------------------------------------------------------------------------------------------------------------------------

    // 例子： Regexp.exec() 和 matchAll()

    // 在 matchAll 出现之前，通过在循环中调用 regexp.exec() 来获取所有匹配项信息

    const regexp = RegExp("foo[a-z]*", "g");
    const str = "table football, foosball";
    let match;

    while ((match = regexp.exec(str)) !== null) {
      console.log(
        `Found ${match[0]} start=${match.index} end=${regexp.lastIndex}.`
      );
      // expected output: "Found football start=6 end=14."
      // expected output: "Found foosball start=16 end=24."
    }
    // --------------------------------------------------------------------------------------------------------------------------

    // 如果使用 matchAll ，就可以不必使用 while 循环加 exec 方式（且正则表达式需使用 /g 标志）。使用 matchAll 会得到一个迭代器的返回值，配合 for...of, array spread, 或者 Array.from() 可以更方便实现功能

    const regexp = RegExp("foo[a-z]*", "g");
    const str = "table football, foosball";
    const matches = str.matchAll(regexp);

    for (const match of matches) {
      console.log(
        `Found ${match[0]} start=${match.index} end=${
          match.index + match[0].length
        }.`
      );
    }
    // expected output: "Found football start=6 end=14."
    // expected output: "Found foosball start=16 end=24."

    // matches iterator is exhausted after the for..of iteration
    // Call matchAll again to create a new iterator
    Array.from(str.matchAll(regexp), (m) => m[0]);
    // Array [ "football", "foosball" ]

    // --------------------------------------------------------------------------------------------------------------------------

    // 如果没有 /g 标志，matchAll 会抛出异常。

    const regexp = RegExp("[a-c]", "");
    const str = "abc";
    Array.from(str.matchAll(regexp), (m) => m[0]);
    // TypeError: String.prototype.matchAll called with a non-global RegExp argument

    // --------------------------------------------------------------------------------------------------------------------------

    // matchAll 内部做了一个 regexp 的复制，所以不像 regexp.exec, lastIndex 在字符串扫描时不会改变。

    const regexp = RegExp("[a-c]", "g");
    regexp.lastIndex = 1;
    const str = "abc";
    Array.from(str.matchAll(regexp), (m) => `${regexp.lastIndex} ${m[0]}`);
    // Array [ "1 b", "1 c" ]

    // --------------------------------------------------------------------------------------------------------------------------

    // 捕获组的更佳途径

    // matchAll 的另外一个亮点是更好地获取捕获组。因为当使用 match() 和 /g 标志方式获取匹配信息时，捕获组会被忽略

    var regexp = /t(e)(st(\d?))/g;
    var str = "test1test2";

    str.match(regexp);
    // Array ['test1', 'test2']

    // 使用 matchAll 可以通过如下方式获取分组捕获:

    let array = [...str.matchAll(regexp)];

    array[0];
    // ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', length: 4]
    array[1];
    // ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', length: 4]
    ```

15. String.prototype.normalize([form]) 会按照指定的一种 Unicode 正规形式将当前字符串正规化

    ```js
    // 参数： form 可选，四种 Unicode 正规形式（Unicode Normalization Form）"NFC"、"NFD"、"NFKC"，或 "NFKD" 其中的一个, 默认值为 "NFC"。

    // 返回值： 含有给定字符串的 Unicode 规范化形式的字符串

    // 可能出现的异常： 如果给 form 传入了上述四个字符串以外的参数，则会抛出 RangeError 异常

    // 描述： Unicode 为每个字符分配一个唯一的数值，称为代码点。例如，代码点为"A"U+0041。但是，有时多个代码点或代码点序列可以表示同一个抽象字符——"ñ"例如，字符可以由以下任一种表示：

    let string1 = "\u00F1";
    let string2 = "\u006E\u0303";

    console.log(string1); //  ñ
    console.log(string2); //  ñ

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------------

    // 但是，由于代码点不同，字符串比较不会将它们视为相等。而且由于每个版本的代码点数量不同，它们甚至有不同的长度。

    let string1 = "\u00F1"; // ñ
    let string2 = "\u006E\u0303"; // ñ

    console.log(string1 === string2); // false
    console.log(string1.length); // 1
    console.log(string2.length); // 2

    //  该normalize()方法通过将字符串转换为表示相同字符的所有代码点序列通用的规范化形式来帮助解决此问题。有两种主要的规范化形式，一种基于规范等价，另一种基于兼容性。

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------------

    // 示例：使用 normalize()

    // Initial string

    // U+1E9B: LATIN SMALL LETTER LONG S WITH DOT ABOVE
    // U+0323: COMBINING DOT BELOW
    var str = "\u1E9B\u0323";

    // Canonically-composed form (NFC)

    // U+1E9B: LATIN SMALL LETTER LONG S WITH DOT ABOVE
    // U+0323: COMBINING DOT BELOW
    str.normalize("NFC"); // "\u1E9B\u0323"
    str.normalize(); // same as above

    // Canonically-decomposed form (NFD)

    // U+017F: LATIN SMALL LETTER LONG S
    // U+0323: COMBINING DOT BELOW
    // U+0307: COMBINING DOT ABOVE
    str.normalize("NFD"); // "\u017F\u0323\u0307"

    // Compatibly-composed (NFKC)

    // U+1E69: LATIN SMALL LETTER S WITH DOT BELOW AND DOT ABOVE
    str.normalize("NFKC"); // "\u1E69"

    // Compatibly-decomposed (NFKD)

    // U+0073: LATIN SMALL LETTER S
    // U+0323: COMBINING DOT BELOW
    // U+0307: COMBINING DOT ABOVE
    str.normalize("NFKD"); // "\u0073\u0323\u0307"

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------------

    // 规范等价归一化

    // 在 Unicode 中，如果两个代码点序列表示相同的抽象字符，则它们具有规范等价性，并且应始终具有相同的视觉外观和行为（例如，它们应始终以相同的方式排序）

    let string1 = "\u00F1"; // ñ
    let string2 = "\u006E\u0303"; // ñ

    string1 = string1.normalize("NFD");
    string2 = string2.normalize("NFD");

    console.log(string1 === string2); // true
    console.log(string1.length); // 2
    console.log(string2.length); // 2

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------------

    // 组合和分解形式

    // 请注意，下面的规范化形式的长度"NFD"是2。那是因为"NFD"为您提供了规范形式的分解版本，其中单个代码点被拆分为多个组合代码点。的分解规范形式"ñ"是"\u006E\u0303"

    // 您可以指定"NFC"获取组合规范形式，其中多个代码点在可能的情况下替换为单个代码点。的组合规范形式"ñ"是"\u00F1"：

    let string1 = "\u00F1"; // ñ
    let string2 = "\u006E\u0303"; // ñ

    string1 = string1.normalize("NFC");
    string2 = string2.normalize("NFC");

    console.log(string1 === string2); // true
    console.log(string1.length); // 1
    console.log(string2.length); // 1
    console.log(string2.codePointAt(0).toString(16)); // f1
    ```

16. String.prototype.padEnd(targetLength [, padString]) 用一个字符串填充动态字符串（如果需要的话则填充填充），返回填充后达到指定长度的字符串。从当前字符串的开始（方法）开始填充

    ```js
    // 参数： targetLength 当前字符串需要填充到目标长度。如果这个数量还小于当前字符串的长度，则返回当前字符串长度。

    // padString：  任选，填充字符串。如果太长，使填充物后的字符串长度超过目标长度，则只保留最左边的部分，其他部分会截断。此参数的字符串是“ ”（U+020 ）。

    // 返回值： 在原字符串填充填充形成的填充字符串直到目标长度所的新字符串。

    // 示例

    "abc".padEnd(10); // "abc       "
    "abc".padEnd(10, "foo"); // "abcfoofoof"
    "abc".padEnd(6, "123456"); // "abc123"
    "abc".padEnd(1); // "abc"

    //  填充物:  如果原生环境不支持该方法，在其他代码之前先运行下面的代码，将创建String.prototype.padEnd()方法。

    // https://github.com/uxitten/polyfill/blob/master/string.polyfill.js
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd
    if (!String.prototype.padEnd) {
      String.prototype.padEnd = function padEnd(targetLength, padString) {
        targetLength = targetLength >> 0; //floor if number or convert non-number to 0;
        padString = String(typeof padString !== "undefined" ? padString : "");
        if (this.length > targetLength) {
          return String(this);
        } else {
          targetLength = targetLength - this.length;
          if (targetLength > padString.length) {
            padString += padString.repeat(targetLength / padString.length); //append to original to ensure we are longer than needed
          }
          return String(this) + padString.slice(0, targetLength);
        }
      };
    }
    ```

17. String.prototype.padStart(targetLength [, padString]) 方法用另一个字符串填充当前字符串（如果需要的话，会重复多次），以便产生的字符串达到定的长度。从当前字符串的起始开始填充

    ```js
    // 参数：  targetLength，当前字符串需要填充到目标长度。如果这个数量还小于当前字符串的长度，则返回当前字符串长度。

    // padString：  任选，填充字符串。如果字符串太长，使填充物后的字符串长度超过目标长度，则只保留最左侧的部分，其他部分会被截断。此参数的默认值为“”（U+0020） 。

    // 返回值： 在原字符串开头填充形成的填充字符串直到目标长度所的新字符串。

    // 示例：

    "abc".padStart(10); // "       abc"
    "abc".padStart(10, "foo"); // "foofoofabc"
    "abc".padStart(6, "123465"); // "123abc"
    "abc".padStart(8, "0"); // "00000abc"
    "abc".padStart(1); // "abc"

    // 填充物： 如果原生环境不支持该方法，在其他代码之前先运行下面的代码，将创建String.prototype.padStart()方法。

    // https://github.com/uxitten/polyfill/blob/master/string.polyfill.js
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart
    if (!String.prototype.padStart) {
      String.prototype.padStart = function padStart(targetLength, padString) {
        targetLength = targetLength >> 0; //floor if number or convert non-number to 0;
        padString = String(typeof padString !== "undefined" ? padString : " ");
        if (this.length > targetLength) {
          return String(this);
        } else {
          targetLength = targetLength - this.length;
          if (targetLength > padString.length) {
            padString += padString.repeat(targetLength / padString.length); //append to original to ensure we are longer than needed
          }
          return padString.slice(0, targetLength) + String(this);
        }
      };
    }
    ```

18. String.prototype.repeat(count) 构造并返回一个新的字符串，该字符串包含被连接的指定数量的字符串的副本

    ```js
    // 参数： count ，介于 0 和 +Infinity 之间的整数。表示在新构造的字符串中重复了多少遍原字符串。

    // 返回值： 包含指定字符串的指定数量副本的新字符串。

    // 异常： RangeError: 重复次数不能为负数。 RangeError: 重复次数必须小于 infinity，且长度不会大于最长的字符串

    // 兼容补丁(Polyfill)： 此方法已添加到 ECMAScript 2015 规范中，并且可能尚未在所有 JavaScript 实现中可用。然而，你可以使用以下代码段对 String.prototype.repeat() 进行填充

    if (!String.prototype.repeat) {
      String.prototype.repeat = function (count) {
        "use strict";
        if (this == null) {
          throw new TypeError("can't convert " + this + " to object");
        }
        var str = "" + this;
        count = +count;
        if (count != count) {
          count = 0;
        }
        if (count < 0) {
          throw new RangeError("repeat count must be non-negative");
        }
        if (count == Infinity) {
          throw new RangeError("repeat count must be less than infinity");
        }
        count = Math.floor(count);
        if (str.length == 0 || count == 0) {
          return "";
        }
        // 确保 count 是一个 31 位的整数。这样我们就可以使用如下优化的算法。
        // 当前（2014年8月），绝大多数浏览器都不能支持 1 << 28 长的字符串，所以：
        if (str.length * count >= 1 << 28) {
          throw new RangeError(
            "repeat count must not overflow maximum string size"
          );
        }
        var rpt = "";
        for (;;) {
          if ((count & 1) == 1) {
            rpt += str;
          }
          count >>>= 1;
          if (count == 0) {
            break;
          }
          str += str;
        }
        return rpt;
      };
    }

    // 示例：

    "abc".repeat(-1); // RangeError: repeat count must be positive and less than inifinity
    "abc".repeat(0); // ""
    "abc".repeat(1); // "abc"
    "abc".repeat(2); // "abcabc"
    "abc".repeat(3.5); // "abcabcabc" 参数count将会被自动转换成整数.
    "abc"
      .repeat(1 / 0)(
        // RangeError: repeat count must be positive and less than inifinity

        { toString: () => "abc", repeat: String.prototype.repeat }
      )
      .repeat(2);
    //"abcabc",repeat是一个通用方法,也就是它的调用者可以不是一个字符串对象.
    ```

19. String.prototype.replace(regexp|substr, newSubStr|function)

    返回一个由替换值（replacement）替换部分或所有的模式（pattern）匹配项后的新字符串。模式可以是一个字符串或者一个正则表达式，替换值可以是一个字符串或者一个每次匹配都要调用的回调函数。如果 pattern 是字符串，则仅替换第一个匹配项。

    且原字符串不会改变。

    参数 ：

    regexp (pattern)，一个 RegExp 对象或者其字面量。该正则所匹配的内容会被第二个参数的返回值替换掉。

    substr (pattern)，一个将被 newSubStr 替换的 字符串。其被视为一整个字符串，而不是一个正则表达式。仅第一个匹配项会被替换。

    newSubStr (replacement)，用于替换掉第一个参数在原字符串中的匹配部分的字符串。该字符串中可以内插一些特殊的变量名。参考下面的使用字符串作为参数。

    function (replacement)，一个用来创建新子字符串的函数，该函数的返回值将替换掉第一个参数匹配到的结果。参考下面的指定一个函数作为参数。

    返回值： 一个部分或全部匹配由替代模式所取代的新的字符串。

    描述： 该方法并不改变调用它的字符串本身，而只是返回一个新的替换后的字符串。在进行全局的搜索替换时，正则表达式需包含 g 标志。

    使用字符串作为参数

    | 变量名  | 代表的值                                                                                                                                                                                                                             |
    | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | $$      | 插入一个 "$"。                                                                                                                                                                                                                       |
    | $&      | 插入匹配的子串。                                                                                                                                                                                                                     |
    | $`      | 插入当前匹配的子串左边的内容。                                                                                                                                                                                                       |
    | $'      | 插入当前匹配的子串右边的内容。                                                                                                                                                                                                       |
    | $n      | 假如第一个参数是 RegExp 对象，并且 n 是个小于 100 的非负整数，那么插入第 n 个括号匹配的字符串。提示：索引是从 1 开始。如果不存在第 n 个分组，那么将会把匹配到到内容替换为字面量。比如不存在第 3 个分组，就会用“$3”替换匹配到的内容。 |
    | $<Name> | 这里 Name 是一个分组名称。如果在正则表达式中并不存在分组（或者没有匹配），这个变量将被处理为空字符串。只有在支持命名分组捕获的浏览器中才能使用。                                                                                     |

    指定一个函数作为参数

    你可以指定一个函数作为第二个参数。在这种情况下，当匹配执行后，该函数就会执行。 函数的返回值作为替换字符串。 (注意：上面提到的特殊替换参数在这里不能被使用。) 另外要注意的是，如果第一个参数是正则表达式，并且其为全局匹配模式，那么这个方法将被多次调用，每次匹配都会被调用。

    下面是该函数的参数：

    | 变量名            | 代表的值                                                                                                             |
    | ----------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
    | match             | 匹配的子串。（对应于上述的$&。）                                                                                     |
    | p1,p2, ...        | 假如 replace()方法的第一个参数是一个 RegExp 对象，则代表第 n 个括号匹配的字符串。（对应于                            | 上述的$1，$2 等。）例如，如果是用 /(\a+)(\b+)/ 这个来匹配，p1 就是匹配的 \a+，p2 就是匹配的 \b+。 |
    | offset            | 匹配到的子字符串在原字符串中的偏移量。（比如，如果原字符串是 'abcd'，匹配到的子字符串是 'bc'，那么这个参数将会是 1） |
    | string            | 被匹配的原字符串。                                                                                                   |
    | NamedCaptureGroup | 命名捕获组匹配的对象                                                                                                 |

    精确的参数个数依赖于 replace() 的第一个参数是否是一个正则表达式（RegExp）对象，以及这个正则表达式中指定了多少个括号子串，如果这个正则表达式里使用了命名捕获， 还会添加一个命名捕获的对象)

    下面的例子将会使 newString 变成 'abc - 12345 - #$\*%'：

    ```js
    function replacer(match, p1, p2, p3, offset, string) {
      // p1 is nondigits, p2 digits, and p3 non-alphanumerics
      return [p1, p2, p3].join(" - ");
    }
    var newString = "abc12345#$*%".replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
    console.log(newString); // abc - 12345 - #$*%
    ```

    示例 :在 replace() 中使用正则表达式

    ```js
    // 在下面的例子中，replace() 中使用了正则表达式及忽略大小写标示

    var str = "Twas the night before Xmas...";
    var newstr = str.replace(/xmas/i, "Christmas");
    console.log(newstr); // Twas the night before Christmas...
    ```

    在 replace() 中使用 global 和 ignore 选项

    ```js
    // 下面的例子中,正则表达式包含有全局替换(g)和忽略大小写(i)的选项,这使得replace方法用'oranges'替换掉了所有出现的"apples".

    var re = /apples/gi;
    var str = "Apples are round, and apples are juicy.";
    var newstr = str.replace(re, "oranges");

    // oranges are round, and oranges are juicy.
    console.log(newstr);
    ```

    交换字符串中的两个单词

    ```js
    // 下面的例子演示了如何交换一个字符串中两个单词的位置，这个脚本使用$1 和 $2 代替替换文本。

    var re = /(\w+)\s(\w+)/;
    var str = "John Smith";
    var newstr = str.replace(re, "$2, $1");
    // Smith, John
    console.log(newstr);
    ```

    使用行内函数来修改匹配到的字符。

    ```js
    // 在这个例子中，所有出现的大写字母转换为小写，并且在匹配位置前加一个连字符。重要的是，在返回一个替换了的字符串前，在匹配元素前进行添加操作是必要的。

    // 在返回前，替换函数允许匹配片段作为参数，并且将它和连字符进行连接作为新的片段。

    function styleHyphenFormat(propertyName) {
      function upperToHyphenLower(match) {
        return "-" + match.toLowerCase();
      }
      return propertyName.replace(/[A-Z]/g, upperToHyphenLower);
    }
    ```

20. String.prototype.replaceAll(regexp|substr, newSubstr|function)
    返回一个新字符串，新字符串所有满足 pattern 的部分都已被 replacement 替换。pattern 可以是一个字符串或一个 RegExp， replacement 可以是一个字符串或一个在每次匹配被调用的函数。

    且原始字符串保持不变。

    ```js
    // 参数: regexp （图案）,一个 RegExp 对象或文字与全球标志。匹配项被替换为 newSubstr 或 指定的返回值 function。没有全局 ("g") 标志的 RegExp 将抛出 TypeError：“replaceAll 必须使用全局 RegExp 调用”。

    // substr: AString 将被替换为 newSubstr。它被视为文字字符串，不会被解释为正则表达式。

    // newSubstr （替代品）, 该 String 替换由指定指定的字符串 regexp 或 substr 参数。支持多种特殊替换模式；请参阅下面的“将字符串指定为参数”部分。

    // function （替代品）,要调用的函数来创建新的子字符串，用于替换给定 regexpor 的匹配项 substr。提供给此函数的参数在下面的“将函数指定为参数”部分中进行了描述。

    //返回值: 一个新字符串，模式的所有匹配项都被替换项替换。

    //描述: 这种方法不会改变调用 String 对象。它只是返回一个新字符串。

    // 例子：使用 replaceAll

    "aabbcc".replaceAll("b", "."); // 'aa..cc'
    ```

21. String.prototype.search(regexp)

    search()方法执行正则表达式和 String 对象之间的一个搜索匹配。

    ```js
    // 参数:  regexp，一个正则表达式（regular expression）对象

    // 如果一个非正则表达式对象regexp，对象使用new RegExp(regexp)隐式地将其转换为正则表达式。

    // 返回值： 如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引；否则，返回-1。

    // 描述： 当你想要知道字符串中是否存在某种模式（模式）时可使用search()，特定时 则表达式的test()方法。当要了解更多匹配信息时，可使用match()（但更慢一些），该方法类似于正则表达式的exec()方法。

    // 示例： 例子：使用 search()，下面的例子中用两个不同的正则表达式对同一个字符串执行搜索匹配，得到一个成功匹配（正数返回值）和一个失败匹配（-1）

    var str = "hey JudE";
    var re = /[A-Z]/g;
    var re2 = /[.]/g;
    console.log(str.search(re)); // returns 4, which is the index of the first capital letter "J"
    console.log(str.search(re2)); // returns -1 cannot find '.' dot punctuation
    ```

22. String.prototype.slice(beginIndex[, endIndex])

    slice() 方法取出某个特定的字符串，并返回一个新的字符串，且不会改变原字符串。

    ```js
    // 参数： beginIndex，从该索引（以0为基数）处开始提取原字符串中的字符如果值为负数，会被当做， strLength + beginIndex 看待，的这里strLength的英文字符串的长度（例如，如果beginIndex 的英文-3则看作是：strLength - 3）

    // endIndex： 可选，在该索引（以0为基数）处结束提取字符串。如果省略该参数，slice()会一直提取到字符串末尾。如果该参数为负数，则被看作是strLength + endIndex的，这里的strLength就是字符串的长度（例如，如果 endIndex 是 -3，则是，strLength - 3）。

    // 返回值： 返回一个从原字符串中提取出来的新字符串

    // 描述：slice() 从一个字符串中提取字符串并返回新字符串。在一个字符串中的改变不会影响另一个字符串。也就是说，slice 不会修改原字符串（只会返回一个包含了原字符串中部分字符的新字符串）。

    // slice() 提取的新字符串包括beginIndex但不包括endIndex。下面有两个例子。

    // 例1：str.slice(1, 4) 提取第二个字符到第四个字符（被提取字符的索引值（索引），具体为1、2和3）。

    // 例2：str.slice(2, -1) 提取第三个字符到倒数第一个字符。

    // 例子： 使用 slice() 创建一个新的字符串，下面的例子使用 slice()创建了一个新的字符串。

    var str1 = "The morning is upon us.", // str1 的长度 length 是 23。
      str2 = str1.slice(1, 8),
      str3 = str1.slice(4, -2),
      str4 = str1.slice(12),
      str5 = str1.slice(30);
    console.log(str2); // 输出：he morn
    console.log(str3); // 输出：morning is upon u
    console.log(str4); // 输出：is upon us.
    console.log(str5); // 输出：""
    //  -----------------------------------------

    // 给 slice() 负值指数

    var str = "The morning is upon us.";
    str.slice(-3); // 返回 'us.'
    str.slice(-3, -1); // 返回 'us'
    str.slice(0, -1); // 返回 'The morning is upon us'
    ```

23. String.prototype.split([separator[, limit]])

    split() 方法使用指定的分隔符字符串将一个 String 对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。

    ```js
    // separator ：指定表示每个拆分应发生的点的字符串，separator 可以是一个字符串或正则表达式。 如果纯文本分隔符包含多个字符，则必须找到整个字符串来表示分割点。如果在str中省略或不出现分隔符，则返回的数组包含一个由整个字符串组成的元素。如果分隔符为空字符串，则将str原字符串中每个字符的数组形式返回。

    // limit： 一个整数，限定返回的分割片段数量。当提供此参数时，split 方法会在指定分隔符的每次出现时分割该字符串，但在限制条目已放入数组时停止。如果在达到指定限制之前达到字符串的末尾，它可能仍然包含少于限制的条目。新数组中不返回剩下的文本。

    // 返回值： 返回源字符串以分隔符出现位置分隔而成的一个 Array

    // 描述 ： 找到分隔符后，将其从字符串中删除，并将子字符串的数组返回。如果没有找到或者省略了分隔符，则该数组包含一个由整个字符串组成的元素。如果分隔符为空字符串，则将str转换为字符数组。如果分隔符出现在字符串的开始或结尾，或两者都分开，分别以空字符串开头，结尾或两者开始和结束。因此，如果字符串仅由一个分隔符实例组成，则该数组由两个空字符串组成。

    // 如果分隔符是包含捕获括号的正则表达式，则每次分隔符匹配时，捕获括号的结果（包括任何未定义的结果）将被拼接到输出数组中。但是，并不是所有浏览器都支持此功能。

    // 示例： 使用 split()，下例定义了一个函数：根据指定的分隔符将一个字符串分割成一个字符串数组。分隔字符串后，该函数依次输出原始字符串信息，被使用的分隔符，返回数组元素的个数，以及返回数组中所有的元素。

    function splitString(stringToSplit, separator) {
    var arrayOfStrings = stringToSplit.split(separator);

    console.log('The original string is: "' + stringToSplit + '"');
    console.log('The separator is: "' + separator + '"');
    console.log("The array has " + arrayOfStrings.length + " elements: ");

    for (var i = 0; i < arrayOfStrings.length; i++)
        console.log(arrayOfStrings[i] + " / ");
    }

    var tempestString = "Oh brave new world that has such people in it.";
    var monthString = "Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec";

    var space = " ";
    var comma = ",";

    splitString(tempestString, space);
    splitString(tempestString);
    splitString(monthString, comma);

    // 上例输出下面结果：

    The original string is: "Oh brave new world that has such people in it."
    The separator is: " "
    The array has 10 elements: Oh / brave / new / world / that / has / such / people / in / it. /

    The original string is: "Oh brave new world that has such people in it."
    The separator is: "undefined"
    The array has 1 elements: Oh brave new world that has such people in it. /

    The original string is: "Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec"
    The separator is: ","
    The array has 12 elements: Jan / Feb / Mar / Apr / May / Jun / Jul / Aug / Sep / Oct / Nov / Dec /

    // --------------------------------------------------------------------------------------------------

    // 移出字符串中的空格： 下例中，split() 方法会查找“0 或多个空白符接着的分号，再接着 0 或多个空白符”模式的字符串，找到后，就将空白符从字符串中移除，nameList 是 split 的返回数组。

    var names = "Harry Trump ;Fred Barney; Helen Rigby ; Bill Abel ;Chris Hand ";

    console.log(names);

    var re = /\s*(?:;|$)\s*/;
    var nameList = names.split(re);

    console.log(nameList);

    // 上例输出两行，第一行输出原始字符串，第二行输出结果数组。

    Harry Trump ;Fred Barney; Helen Rigby ; Bill Abel ;Chris Hand
    [ "Harry Trump", "Fred Barney", "Helen Rigby", "Bill Abel", "Chris Hand", "" ]


    // --------------------------------------------------------------------------------------------------

    // 限制返回值中分割元素数量

    var myString = "Hello World. How are you doing?";
    var splits = myString.split(" ", 3);

    console.log(splits);
    // 上例输出：

    ["Hello", "World.", "How"]

    //  ------------------------------------------------------------------------------------------------------

    // 靠正则来分割使结果中包含分隔块

    // 如果 separator 包含捕获括号（capturing parentheses），则其匹配结果将会包含在返回的数组中

    var myString = "Hello 1 word. Sentence number 2.";
    var splits = myString.split(/(\d)/);

    console.log(splits);
    Copy to Clipboard
    // 上例输出：

    [ "Hello ", "1", " word. Sentence number ", "2", "." ]

    // --------------------------------------------------------------------

    // 使用一个数组来作为分隔符

    const myString = 'this|is|a|Test';
    const splits = myString.split(['|']);

    console.log(splits); //["this", "is", "a", "Test"]

    const myString = 'ca,bc,a,bca,bca,bc';

    const splits = myString.split(['a','b']);
    // myString.split(['a','b']) is same as myString.split(String(['a','b']))

    console.log(splits);  //["c", "c,", "c", "c", "c"]

    ```

24. String.prototype.startsWith(searchString[, position])

    startsWith() 方法用来判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回 true 或 false。

    ```js
    // 参数： searchString，要搜索的子字符串。

    // position 可选，在 str 中搜索 searchString 的开始位置，默认值为 0。

    // 返回值： 如果在字符串的开头找到了给定的字符则返回true；否则返回false。

    // 描述： 这个方法能够让你确定一个字符串是否以另一个字符串开头。这个方法区分大小写。

    // 兼容：

    if (!String.prototype.startsWith) {
      Object.defineProperty(String.prototype, "startsWith", {
        value: function (search, pos) {
          pos = !pos || pos < 0 ? 0 : +pos;
          return this.substring(pos, pos + search.length) === search;
        },
      });
    }

    // 示例： 使用 startsWith()

    var str = "To be, or not to be, that is the question.";

    alert(str.startsWith("To be")); // true
    alert(str.startsWith("not to be")); // false
    alert(str.startsWith("not to be", 10)); // true
    ```

25. String.prototype.substring(indexStart[, indexEnd])

    substring() 方法返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。

    描述： substring 提取从 indexStart 到 indexEnd（不包括）之间的字符。特别地：

    > 1. 如果 indexStart 等于 indexEnd，substring 返回一个空字符串。
    > 2. 如果省略 indexEnd，substring 提取字符一直到字符串末尾。
    > 3. 如果任一参数小于 0 或为 NaN，则被当作 0。
    > 4. 如果任一参数大于 stringName.length，则被当作 stringName.length。
    > 5. 如果 indexStart 大于 indexEnd，则 substring 的执行效果就像两个参数调换了一样。见下面的例子。

    ```js
    // 参数： indexStart，需要截取的第一个字符的索引，该索引位置的字符作为返回的字符串的首字母。

    // indexEnd： 可选，一个 0 到字符串长度之间的整数，以该数字为索引的字符不包含在截取的字符串内。

    // 返回值： 包含给定字符串的指定部分的新字符串。

    // 示例: 使用 substring,下例使用 substring 输出字符串 "Mozilla" 中的字符：

    var anyString = "Mozilla";

    // 输出 "Moz"
    console.log(anyString.substring(0, 3));
    console.log(anyString.substring(3, 0));
    console.log(anyString.substring(3, -3));
    console.log(anyString.substring(3, NaN));
    console.log(anyString.substring(-2, 3));
    console.log(anyString.substring(NaN, 3));

    // 输出 "lla"
    console.log(anyString.substring(4, 7));
    console.log(anyString.substring(7, 4));

    // 输出 ""
    console.log(anyString.substring(4, 4));

    // 输出 "Mozill"
    console.log(anyString.substring(0, 6));

    // 输出 "Mozilla"
    console.log(anyString.substring(0, 7));
    console.log(anyString.substring(0, 10));

    // -------------------------------------------------------------------------------------

    // 运用 length 属性来使用 substring()

    // 下面一个例子运用了    String.length 属性去获取指定字符串的倒数元素。显然这个办法更容易记住，因为你不再像上面那个例子那样去记住起始位置和最终位置。

    // Displays 'illa' the last 4 characters
    var anyString = "Mozilla";
    var anyString4 = anyString.substring(anyString.length - 4);
    console.log(anyString4);

    // Displays 'zilla' the last 5 characters
    var anyString = "Mozilla";
    var anyString5 = anyString.substring(anyString.length - 5);
    console.log(anyString5);

    // --------------------------------------------------------------------------------------

    // 例子： 下例替换了一个字符串中的子字符串。可以替换单个字符和子字符串。该例结尾调用的函数将 "Brave New World" 变成了 "Brave New Web"。

    function replaceString(oldS, newS, fullS) {
      // Replaces oldS with newS in the string fullS
      for (var i = 0; i < fullS.length; i++) {
        if (fullS.substring(i, i + oldS.length) == oldS) {
          fullS =
            fullS.substring(0, i) +
            newS +
            fullS.substring(i + oldS.length, fullS.length);
        }
      }
      return fullS;
    }

    replaceString("World", "Web", "Brave New World");

    // 需要注意的是，如果 oldS 是 newS 的子字符串将会导致死循环。例如，尝试把 "Web" 替换成 "OtherWorld"。一个更好的方法如下：

    function replaceString(oldS, newS, fullS) {
      return fullS.split(oldS).join(newS);
    }
    ```

26. String.prototype.toLocaleLowerCase()

    toLocaleLowerCase()方法根据任何指定区域语言环境设置的大小写映射，返回调用字符串被转换为小写的格式。

    > 1. str.toLocaleLowerCase()
    > 2. str.toLocaleLowerCase(locale)
    > 3. str.toLocaleLowerCase([locale, locale, ...])

    ```js
    // 参数:  locale 可选,参数 locale 指明要转换成小写格式的特定语言区域。 如果以一个数组 Array形式给出多个locales,  最合适的地区将被选出来应用,默认的locale是主机环境的当前区域(locale)设置

    // 返回值： 根据任何特定于语言环境的案例映射规则将被调用字串转换成小写格式的一个新字符串。

    // 异常： 如果locale 参数不是有效的语言标签，则会抛出（“无效的语言标签：xx_yy”）；如果数组元素不是字符串类型，则会抛出（“locales 参数中的无效元素”）

    // 例子： 使用toLocaleLowerCase()

    "ALPHABET".toLocaleLowerCase(); // 'alphabet'

    "\u0130".toLocaleLowerCase("tr") === "i"; // true
    "\u0130".toLocaleLowerCase("en-US") === "i"; // false

    let locales = ["tr", "TR", "tr-TR", "tr-u-co-search", "tr-x-turkish"];
    "\u0130".toLocaleLowerCase(locales) === "i"; // true
    ```

27. String.prototype.toLocaleUpperCase()

    toLocaleUpperCase() 方法根据当地主机语言环境把字符串转换为大写格式，并返回转换后的字符串。

    > 1. str.toLocaleUpperCase()
    > 2. str.toLocaleUpperCase(locale)
    > 3. str.toLocaleUpperCase([locale, locale, ...])

    ```js
    // 参数： locale 任选

    // 返回值： 根据任何特定于语言环境的大小写映射，表示转换为大写的调用字符串的新字符串。

    // 异常： 与toLocaleLowerCase() 相同

    // 例子： 使用 toLocaleUpperCase()

    "alphabet".toLocaleUpperCase(); // 'ALPHABET'

    "Gesäß".toLocaleUpperCase(); // 'GESÄSS'

    "i\u0307".toLocaleUpperCase("lt-LT"); // 'I'

    let locales = ["lt", "LT", "lt-LT", "lt-u-co-phonebk", "lt-x-lietuva"];
    "i\u0307".toLocaleUpperCase(locales); // 'I'
    ```

28. String.prototype.toLowerCase()

    toLowerCase() 调用该方法的字符串值转为小写形式，并返回

    > 1. str.toLowerCase()

    ```js
    // 返回值： 一个新的字符串，表示转换为小写的调用字符串。

    // 描述： toLowerCase 求调用该方法的字符串值转为小写形式，并返回。toLowerCase 不会影响字符串的值。

    // 示例： 使用 toLowerCase()

    console.log('中文简体 zh-CN || zh-Hans'.toLowerCase());
    // 中文简体 zh-cn || zh-hans

    ​console.log( "ALPHABET".toLowerCase() );
    // "alphabet"
    ```

29. String.prototype.toString()

    toString() 方法返回指定对象的字符串形式

    ```js
    // 返回值: 一个表示调用对象的字符串。

    // 描述： String覆盖对象了Object对象的 toString方法;并没有继承Object.toString()

    // 示例 ： 下例输出一个字符串对象（String object）的字符串值：

    var x = new String("Hello world");

    alert(x.toString()); // 输出 "Hello world"
    ```

30. String.prototype.toUpperCase()

    toUpperCase() 将调用该方法的字符串转为大写形式并返回（如果调用该方法的值不是字符串类型会被强制转换）

    ```js
    // 返回值： 一个新的字符串，表示转换为大写的调用字符串。

    // 异常：  在null或undefined类型上调用，例如：String.prototype.toUpperCase.call(undefined)

    // 描述： toUpperCase() 返回转为大写形式的字符串。此方法不会影响原字符串设置的值，因为JavaScript中字符串的值是不可改变的

    // 示例：

    console.log("alphabet".toUpperCase()); // 'ALPHABET'

    //--------------------------------------------------------------------------------------------------------------------------

    // 将非字符串类型的 this （上下文）转为字符串；此方法适合任何非类型字符串类型的值转为字符串，当你将其背景 this值设置为非字符串类型

    const a = String.prototype.toUpperCase.call({
      toString: function toString() {
        return "abcdef";
      },
    });

    const b = String.prototype.toUpperCase.call(true);

    // 输出 'ABCDEF TRUE'。
    console.log(a, b);
    ```

31. String.prototype.trim()

    trim() 方法会从一个字符串的终端删除空白字符。在这个上下文中的空白字符是所有的空白字符（空格、制表符、无中断空格等）以及所有行终止符字符（如 LF，CR 等）

    ```js
    // 返回值： 一个代表调用字符串端删除空白的新字符串。

    // 描述： trim() 方法返回一个从两头删除空白字符的字符串，不影响原字符串修饰。

    // 例子： 使用 trim()；下面的例子中将显示小写的字符串'foo'：

    var orig = "   foo  ";
    console.log(orig.trim()); // 'foo'

    // 另一个 .trim() 例子，只从一边删除

    var orig = "foo    ";
    console.log(orig.trim()); // 'foo'

    // --------------------------------------------------------------------------------

    // 兼容： 如果trim()不存在，可以在所有代码前执行以下代码

    if (!String.prototype.trim) {
      String.prototype.trim = function () {
        return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, "");
      };
    }
    ```

32. String.prototype.trimEnd()

    trimEnd() 方法从一个字符串的末端移除空白字符。trimRight() 是这个方法的别名。

    ```js
    // 返回值： 一个新字符串，表示从调用字串的末（右）端除去空白。

    // 描述: trimEnd() / trimRight()方法移除原字符串右端的连续空白符并返回，trimEnd() / trimRight()方法并不会直接修改原字符串本身。

    // 示例： 使用trimEnd()； 下面的例子输出了小写的字符串"   foo":

    var str = "   foo  ";

    alert(str.length); // 8

    str = str.trimRight(); // 或写成str = str.trimEnd();
    console.log(str.length); // 6
    console.log(str); // '   foo'
    ```

33. String.prototype.trimStart()

    trimStart() 方法从字符串的开头删除空格。trimLeft() 是此方法的别名。

    ```js
    // 返回值： 一个新字符串，表示从其开头（左端）除去空格的调用字符串。

    // 描述： trimStart() / trimLeft() 方法移除原字符串左端的连续空白符并返回一个新字符串，并不会直接修改原字符串本身。

    // 示例： 使用 trimStart()；下面的例子输出了小写的字符串 "foo  "：

    var str = "   foo  ";

    console.log(str.length); // 8

    str = str.trimStart(); // 等同于 str = str.trimLeft();
    console.log(str.length); // 5
    console.log(str); // "foo  "
    ```

34. String.prototype.valueOf()

    valueOf() 方法返回 String 对象的原始值

    ```js
    // 返回值: 表示给定String对象的原始值的字符串。

    // 描述： 该valueOf()方法String将String对象的原始值作为字符串数据类型返回。该值相当于String.prototype.toString()。

    // 示例

    var x = new String("Hello world");
    console.log(x.valueOf()); // Displays 'Hello world'
    ```
