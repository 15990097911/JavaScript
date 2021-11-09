## Number

### Number 静态属性

#### 1、数值类型 Number 最大的数值和最小的正值

  1. 能表示的最大数值

      ```js
      /**
       *  1、Number.MAX_VALUE 的值接近于1.7976931348623157e+308，大于 Number.MAX_VALUE 的用 Infinity 表示
      *  2、 MAX_VALUE 是 Number 上的一个静态属性，可以直接 Number.MAX_VALUE 引用
      */
      Number.MAX_VALUE; // 1.7976931348623157e+308;
      Number.MAX_VALUE === Number.MAX_VALUE + 1; // true
      ```

  2.  能表示的最小正值

      ```js
      /**
       *  1、Number.MIN_VALUE 是最接近0的正值，而不是最小的负值
      *  2、Number.MIN_VALUE 的值约为5e-324，小于 MIN_VALUE 的正值会被转换为0
      *  3、MAX_VALUE 是 Number 上的一个静态属性，调用时，可直接使用 Number.MIN_VALUE
      */
      Number.MIN_VALUE; // 5e-324
      Number.MIN_VALUE + 1; // 1
      ```

  3. 能表示的最小负数

      ```js
      -Number.MAX_VALUE; // -1.7976931348623157e+308
      ```

  4. 能表示的最大负数

      ```js
      -MIN_VALUE; // -5e-324
      ```

#### 2、数值类型 Number 最大安全**整数**与最小安全**整数**

1. 最大安全整数

    ```js
    /**
     *  1、MAX_SAFE_INTEGER 是一个值为 9007199254740991的常量
    *  2、Javascript 的数字存储使用的是 IEEE 754 中规定的双精度浮点数，而这一数据类型能够安全存储 -(2^53 - 1) 到 2^53 - 1 之间的数值（包含边界值）
    *  3、安全存储的意思是指能够准确区分两个不相同的值
    *  4、MAX_SAFE_INTEGER 是 Number 上的一个静态属性，调用时，可直接使用 MAX_SAFE_INTEGER
    */
    Number.MAX_SAFE_INTEGER; // 9007199254740991
    Math.pow(2, 53) - 1; // 9007199254740991
    //  -------------------------------------------------------------------------
    //  下面这个例子这在数学上是错误的，但是在JS中，因为超出了最大安全整数而无法区分这两个值有什么不同
    Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; // true
    ```

2. 最小安全整数

    ```js
    /**
     *  1、MIN_SAFE_INTEGER 是一个值为 -9007199254740991的常量
    *  2、MIN_SAFE_INTEGER 是 Number 上的一个静态属性，调用时，可直接使用 MIN_SAFE_INTEGER
    *  3、其他与 MAX_SAFE_INTEGER 一致
    */
    Number.MIN_SAFE_INTEGER; // -9007199254740991
    -(Math.pow(2, 53) - 1); // -9007199254740991
    ```

#### 3、数之间的最小间隔 Number.EPSILON

1. 可以接受的最小误差范围

    ```js
    /**
     *  1、Number.EPSILON 属性表示 1 与Number可表示的大于 1 的最小的浮点数之间的差值。
    *  2、EPSILON 属性的值接近于 2.2204460492503130808472633361816E-16，或者 2^-52。
    *  3、Number.EPSILON 实际上是 JavaScript 能够表示的最小精度，误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了
    */

    0.1 + 0.2 === 0.3; // false
    0.1 + 0.2 - 0.3 < Number.EPSILON; // true
    ```

#### 4、特殊的“非数字”值 NAN

1. Number.NaN 表示“非数字”（Not-A-Number）,和 NaN 相同

2. 全局属性 NaN 的值表示不是一个数字

3. NaN 是一个全局对象的属性,NaN 属性的初始值就是 NaN，和 Number.NaN 的值一样

4. 在现代浏览器中（ES5 中）,NaN 属性是一个不可配置，不可写的属性。但在 ES3 中，这个属性的值是可以被更改的。

5. NaN 如果通过 == 、 != 、 === 、以及 !==与其他任何值比较都将不相等 -- 包括与其他 NAN 值进行比较

6. 判断一个值是否是 NaN

    ```js
    NaN === NaN; // false
    Number.NaN === NaN; // false
    isNaN(NaN); // true
    isNaN(Number.NaN); // true

    function valueIsNaN(v) {
      return v !== v;
    }
    valueIsNaN(1); // false
    valueIsNaN(NaN); // true
    valueIsNaN(Number.NaN); // true

    // 如果当前值是NaN，或者将其强制转换为数字后将是NaN，则前者将返回true。而后者仅当值当前为NaN时才为true
    isNaN("hello world"); // true
    Number.isNaN("hello world"); // false
    ```

### Number 静态方法

#### 1、Number.isNaN(value)

1. 判断传入的值是否为 NaN

#### 2、Number.isFinite(value)

1. 判断传入的值是否为有限数

2. 它检查给定值的类型是否为 Number，并且该数字既不是正数，也不是 Infinity 负数 Infinity，也不是 NaN。

3. 与全局 isFinite()函数相比，此方法不会先将参数转换为数字。这意味着只有 number 类型的值并且是有限返回值 true

    ```js
    Number.isFinite(Infinity); // false
    Number.isFinite(NaN); // false
    Number.isFinite(-Infinity); // false
    Number.isFinite(0); // true
    Number.isFinite(2e64); // true
    Number.isFinite("0"); // false, would've been true with
    // global isFinite('0')
    Number.isFinite(null); // false, would've been true with
    // global isFinite(null)
    // -------------------------
    if (Number.isFinite === undefined)
      Number.isFinite = function (value) {
        return typeof value === "number" && isFinite(value);
      };
    ```

    ```js
    // 局isFinite()函数确定传递的值是否为有限数。如果需要，首先将参数转换为数字。
    isFinite(Infinity); // false
    isFinite(NaN); // false
    isFinite(-Infinity); // false
    isFinite(0); // true
    isFinite(2e64); // true
    isFinite(910); // true
    isFinite(null); // true, would've been false with the
    // more robust Number.isFinite(null)
    isFinite("0"); // true, would've been false with the
    // more robust Number.isFinite("0")
    ```

#### 3、Number.isInteger(value)

1. 确定传递的值是否为整数

2. 如果目标值是整数，则返回 true，否则返回 false。如果值为 NaN、Infinity，则返回 false。该方法还将返回 true 可以表示为整数的浮点数。

    ```js
    Number.isInteger(0); // true
    Number.isInteger(1); // true
    Number.isInteger(-100000); // true
    Number.isInteger(99999999999999999999999); // true
    Number.isInteger(0.1); // false
    Number.isInteger(Math.PI); // false
    Number.isInteger(NaN); // false
    Number.isInteger(Infinity); // false
    Number.isInteger(-Infinity); // false
    Number.isInteger("10"); // false
    Number.isInteger(true); // false
    Number.isInteger(false); // false
    Number.isInteger([1]); // false
    Number.isInteger(5.0); // true
    Number.isInteger(5.000000000000001); // false
    // 这个小数的精度达到了小数点后16个十进制位，转成二进制位超过了53个二进制位，导致最后的那个1被丢弃了
    Number.isInteger(5.0000000000000001); // true
    ```

#### 4、Number.isSafeInteger(value)

1. 确定传递的值是否为安全整数（介于-(2^53 - 1)和之间的数字 2^53 - 1）。

2. 一个安全整数是一个整数

    ```js
    Number.isSafeInteger(3); // true
    Number.isSafeInteger(Math.pow(2, 53)); // false
    Number.isSafeInteger(Math.pow(2, 53) - 1); // true
    Number.isSafeInteger(NaN); // false
    Number.isSafeInteger(Infinity); // false
    Number.isSafeInteger("3"); // false
    Number.isSafeInteger(3.1); // false
    Number.isSafeInteger(3.0); // true
    ```

#### 5、Number.parseFloat(value)

1. 可以把一个字符串解析成浮点数。如果无法从参数解析数字，则返回 NaN。

2. 如果此参数不是字符串，则使用 ToString 抽象操作。忽略此参数中的前导空格

3. 返回值：从给定的 解析出的浮点数 string ，或者 NaN 当第一个非空白字符无法转换为数字时。

    ```js
    // 此方法与全局parseFloat()函数具有相同的功能：
    if (Number.parseFloat === undefined) {
      Number.parseFloat = parseFloat;
    }
    // 以下示例均返回3.14：
    parseFloat(3.14);
    parseFloat("3.14");
    parseFloat("  3.14  ");
    parseFloat("314e-2");
    parseFloat("0.0314E+2");
    parseFloat("3.14some non-digit characters");
    parseFloat({
      toString: function () {
        return "3.14";
      },
    });
    // ---------------------------------
    parseFloat("FF2"); // NAN
    parseFloat(900719925474099267n); //900719925474099300 丢失精度
    parseFloat("900719925474099267n"); // 900719925474099300 丢失精度
    ```

4. 全局对象 parseFloat()

    ```js
    // 1、如果parseFloat遇到除加号 (+)、减号(-)、数字( 0–9)、小数点(.) 或指数(e 或 E)以外的字符，则它会忽略该字符以及之后的所有字符，返回当前已经解析到的浮点数。

    // 2、如果参数字符串的第一个字符不能被解析成为数字,则 parseFloat 返回 NaN

    // 3、参数首位和末位的空白符会被忽略

    // 4、第二个小数点的出现也会使解析停止（在这之前的字符都会被解析）

    // 5、parseFloat 也可以解析并返回 Infinity

    // 6、parseFloat解析 BigInt 为 Numbers, 丢失精度。因为末位 n 字符被丢弃
    ```

#### 6、Number.parseInt(string, radix)

1. 依据指定基数 [ 参数 radix 的值]，把字符串 [ 参数 string 的值] 解析成整数

2. 如果 string 参数不是字符串，则使用 ToString() ,忽略 string 参数中的前导空格

3. radix 参数是可选的，介于 2 和之间的整数 36，例如指定 16 表示被解析值是十六进制数。请注意，10 不是默认值！

    ```js
    parseInt("123", 5); // 将'123'看作5进制数，返回十进制数38 => 1*5^2 + 2*5^1 + 3*5^0 = 38
    ```

4. 如果 radix 参数不是 Number 类型，则会强制转换为 Number 类型

5. 返回值：从给定字符串中解析的整数。如果基数小于 2 或 大于 36，且第一个非空白字符不能转换为数字，则返回 NaN

6. 和全局对象 parseInt() 一样。

    ```js
    // 如果输入的 string以 "0x"或 "0x"（一个0，后面是小写或大写的X）开头，那么radix被假定为16，字符串的其余部分被当做十六进制数去解析

    // 如果输入的 string以 "0"（0）开头， radix被假定为8（八进制）或10（十进制）。具体选择哪一个radix取决于实现

    // 如果输入的 string 以任何其他值开头， radix 是 10 (十进制)。

    // 因此，在使用 parseInt 时，一定要指定一个 radix

    // 如果第一个字符不能转换为数字，parseInt会返回 NaN。

    // 以下例子均返回15:
    parseInt("0xF", 16);
    parseInt("F", 16);
    parseInt("17", 8);
    parseInt(021, 8);
    parseInt("015", 10); // parseInt(015, 8); 返回 13
    parseInt(15.99, 10);
    parseInt("15,123", 10);
    parseInt("FXX123", 16);
    parseInt("1111", 2);
    parseInt("15 * 3", 10);
    parseInt("15e2", 10);
    parseInt("15px", 10);
    parseInt("12", 13);
    // 以下例子均返回 NaN:
    parseInt("Hello", 8); // 根本就不是数值
    parseInt("546", 2); // 除了“0、1”外，其它数字都不是有效二进制数字
    // 以下例子均返回 -15：
    parseInt("-F", 16);
    parseInt("-0F", 16);
    parseInt("-0XF", 16);
    parseInt(-15.1, 10);
    parseInt(" -17", 8);
    parseInt(" -15", 10);
    parseInt("-1111", 2);
    parseInt("-15e1", 10);
    parseInt("-12", 13);
    // 下例中全部返回 4:
    parseInt(4.7, 10);
    parseInt(4.7 * 1e22, 10); // 非常大的数值变成 4
    parseInt(0.00000000000434, 10); // 非常小的数值变成 4
    // 下面的例子返回 224
    parseInt("0e0", 16);
    // 而且由于必须兼容旧版的浏览器，所以永远都要明确给出radix参数的值
    // ------------------------------------------
    // 有时采用一个更严格的方法来解析整型值很有用。此时可以使用正则表达式：
    filterInt = function (value) {
      if (/^(\-|\+)?([0-9]+|Infinity)$/.test(value)) return Number(value);
      return NaN;
    };

    console.log(filterInt("421")); // 421
    console.log(filterInt("-421")); // -421
    console.log(filterInt("+421")); // 421
    console.log(filterInt("Infinity")); // Infinity
    console.log(filterInt("421e+0")); // NaN
    console.log(filterInt("421hop")); // NaN
    console.log(filterInt("hop1.61803398875")); // NaN
    console.log(filterInt("1.61803398875")); // NaN
    ```

### Number 实例

#### 1、使用 Number()

1. 下例使用 Number 作为函数来转换 Date 对象为数字值：

    ```js
    var d = new Date("December 17, 1995 03:24:00");
    print(Number(d)); //  "819199440000"
    ```

2. 转换数字字符串为数字

    ```js
    Number("123"); // 123
    Number("12.3"); // 12.3
    Number("12.00"); // 12
    Number("123e-1"); // 12.3
    Number(""); // 0
    Number(null); // 0
    Number("0x11"); // 17
    Number("0b11"); // 3
    Number("0o11"); // 9
    Number("foo"); // NaN
    Number("100a"); // NaN
    Number("-Infinity"); //-Infinity
    ```

#### 2、所有 Number 实例都继承自 Number.prototype

1. Number.prototype.toExponential() 以指数表示法返回该数值字符串表示形式

    ```js
    // 以指数表示法返回该数值字符串表示形式

    // 参数：可选。一个整数，用来指定小数点后有几位数字。默认情况下用尽可能多的位数来显示数字。

    // 返回值：一个用幂的形式 (科学记数法) 来表示Number 对象的字符串 ；小数点后以参数 提供的值来四舍五入；如果 参数 参数被忽略了，小数点后的将尽可能用最多的位数来表示该数值。

    // 示例
    var numObj = 77.1234;

    alert("numObj.toExponential() is " + numObj.toExponential()); //输出 7.71234e+1

    alert("numObj.toExponential(4) is " + numObj.toExponential(4)); //输出 7.7123e+1

    alert("numObj.toExponential(2) is " + numObj.toExponential(2)); //输出 7.71e+1

    alert("77.1234.toExponential() is " + (77.1234).toExponential()); //输出 7.71234e+1

    alert("77 .toExponential() is " + (77).toExponential()); //输出 7.7e+1

    // 对数值字面量使用 toExponential() 方法，且该数值没有小数点和指数时，应该在该数值与该方法之间隔开一个空格，以避免点号被解释为一个小数点。也可以使用两个点号调用该方法。

    // 如果 参数 太小或太大将会抛出引用错误 RangeError，介于0和 20（包括20）之间的值不会引起 RangeError，执行环境也可以支持更大或更小范围。

    // 如果该方法在一个非数值类型对象上调用，会抛出类型错误 TypeError
    ```

2. Number.prototype.toFixed() 使用定点表示法来格式化一个数值

    ```js
    // 参数:小数点后数字的个数；介于 0 到 20 （包括）之间，实现环境可能支持更大范围。如果忽略该参数，则默认为 0

    //返回值：使用定点表示法表示给定数字的字符串。

    // 抛出异常：如果参数太小或太大，0 到 20（包括）之间的值不会引起 RangeError；如果该方法在一个非Number类型的对象上调用则抛出TypeError

    // 如果数值大于 1e+21，该方法会简单调用 Number.prototype.toString()并返回一个指数记数法格式的字符串

    // --------------------------------------------------------------------------------------------------------------------------------

    // 使用 toFixed

    var numObj = 12345.6789;

    numObj.toFixed(); // 返回 "12346"：进行四舍六入五看情况，不包括小数部分
    numObj.toFixed(1); // 返回 "12345.7"：进行四舍六入五看情况

    numObj.toFixed(6); // 返回 "12345.678900"：用0填充

    (1.23e20).toFixed(2); // 返回 "123000000000000000000.00"

    (1.23e-10).toFixed(2); // 返回 "0.00"

    (2.34).toFixed(1); // 返回 "2.3"

    (2.35).toFixed(1); // 返回 '2.4'. Note it rounds up

    (2.55).toFixed(1) - // 返回 '2.5'. Note it rounds down - see warning above
      (2.34).toFixed(1); // 返回 -2.3 （由于操作符优先级，负数不会返回字符串）

    (-2.34).toFixed(1); // 返回 "-2.3" （若用括号提高优先级，则返回字符串）
    ```

3. Number.prototype.toLocaleString([locales [, options]) 返回这个数字在特定语言环境下的表示字符串

    ```js
    // locales： 可选。缩写语言代码

    // 要使用的编号系统。可能的值有: "arab", "arabext", "bali", "beng", "deva", "fullwide", "gujr", "guru", "hanidec"(中文十进制数字), "khmr", "knda", "laoo", "latn", "limb", "mlym", "mong", "mymr", "orya", "tamldec", "telu", "thai", "tibt"

    // options：可选，包含一些或所有的下面属性的类:“decimal” 用于纯数字格式；“currency” 用于货币格式；“percent” 用于百分比格式;“unit”用于单位格式

    // -----------------------------------------------------------------------------------

    // 在没有指定区域的基本使用时，返回使用默认的语言环境和默认选项格式化的字符串。

    var number = 3500;

    number.toLocaleString(); // Displays "3,500" if in U.S. English locale

    // locales 和 options 参数目前还不是所有浏览器都支持的

    // 可以依靠使用非法参数时规定抛出的 RangeError 异常

    function toLocaleStringSupportsLocales() {
      var number = 0;
      try {
        number.toLocaleString('i');
      } catch (e) {
        return e​.name === 'RangeError';
      }
      return false;
    }

    // -----------------------------------------------------------------------------------

    // 使用 locales

    var number = 123456.789;

    // 德国使用逗号作为小数分隔符，分位周期为千位
    console.log(number.toLocaleString('de-DE'));
    // → 123.456,789

    // 在大多数阿拉伯语国家使用阿拉伯语数字
    console.log(number.toLocaleString('ar-EG'));
    // → ١٢٣٤٥٦٫٧٨٩

    // 印度使用千位/拉克（十万）/克若尔（千万）分隔
    console.log(number.toLocaleString('en-IN'));
    // → 1,23,456.789

    // nu 扩展字段要求编号系统，e.g. 中文十进制
    console.log(number.toLocaleString('zh-Hans-CN-u-nu-hanidec'));
    // → 一二三,四五六.七八九

    // 当请求不支持的语言时，例如巴厘语，加入一个备用语言，比如印尼语
    console.log(number.toLocaleString(['ban', 'id']));
    // → 123.456,789

    // -----------------------------------------------------------------------------------

    // 使用 options

    var number = 123456.789;

    // 要求货币格式
    console.log(number.toLocaleString('de-DE', { style: 'currency', currency: 'EUR' }));
    // → 123.456,79 €

    // 日元不使用小数位
    console.log(number.toLocaleString('ja-JP', { style: 'currency', currency: 'JPY' }))
    // → ￥123,457

    // 限制三位有效数字
    console.log(number.toLocaleString('en-IN', { maximumSignificantDigits: 3 }));
    // → 1,23,000


    ```

4. Number.prototype.toPrecision(precision) 以指定的精度返回该数值对象的字符串

    ```js
    // 参数： precision  可选。一个用来指定有效数个数的整数。

    // 返回值： 以定点表示法或指数表示法表示的一个数值对象的字符串

    // 异常： precison 参数不在 1 和 100 （包括）之间

    // 示例

    var numObj = 5.123456;
    numObj.toPrecision(); //输出 5.123456
    numObj.toPrecision(5); //输出 5.1235
    numObj.toPrecision(2); //输出 5.1
    numObj.toPrecision(1); //输出 5
    // 注意：在某些情况下会以指数表示法返回
    (1234.5).toPrecision(2); // "1.2e+3"
    ```

5. Number.prototype.toString([radix]) 返回指定 Number 对象的字符串表示形式

    ```js
    // 参数： radix 指定要用于数字到字符串的转换的基数(从2到36)。如果未指定 radix 参数，则默认值为 10。

    // 异常： 如果 toString() 的 radix 参数不在 2 到 36 之间，将会抛出一个 RangeError。

    // 示例

    var count = 10;
    count.toString(); // 输出 '10'
    (17).toString(); // 输出 '17'
    (17.2).toString(); // 输出 '17.2'
    var x = 6;
    x.toString(2); // 输出 '110'
    (254).toString(16); // 输出 'fe'
    (-10).toString(2); // 输出 '-1010'
    (-0xff).toString(2); // 输出 '-11111111'
    ```

6. Number.prototype.valueOf() 返回一个被 Number 对象包装的原始值

    ```js
    //  返回值 ： 表示指定 Number 对象的原始值的数字

    // 描述 ： 该方法通常是由 JavaScript 引擎在内部隐式调用的，而不是由用户在代码中显式调用的。

    // 示例：

    var numObj = new Number(10);
    console.log(typeof numObj); // object
    var num = numObj.valueOf();
    console.log(num); // 10
    console.log(typeof num); // number
    ```
