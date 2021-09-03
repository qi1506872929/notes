## let 声明变量

1. 变量不能重复声明
2. 块儿级作用域 全局, 函数, eval
   if else while for 里都算块级作用域
3. 不存在变量提升
4. 不影响作用域链

## const 定义常量

1. 一定要赋初始值，不允许重复声明
2. 一般常量使用大写(潜规则)
3. 常量的值不能修改
4. 块儿级作用域
5. 对于数组和对象的元素修改, 不算做对常量的修改, 不会报错

应用场景：声明对象类型使用 const，非对象类型声明选择 let

## 变量的解构赋值

ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值，这被称为解构赋值。

1. 数组的结构 `let [xiao, liu, zhao, song] = F4;`

2. 对象的解构 `let {name, age, xiaopin} = zhao;`

## 模板字符串

ES6 引入新的声明字符串的方式 『``』 '' ""

1. 声明 `let str = `我也是一个字符串哦!`;`

2. 内容中可以直接出现换行符

3. 变量拼接 `let out = `${lovest}是我心目中最搞笑的演员!!`;`

## 简化对象写法

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。

## 箭头函数

ES6 允许使用「箭头」（=>）定义函数。

1. this 是静态的. this 始终指向函数声明时所在作用域下的 this 的值

2. 不能作为构造实例化对象

3. 不能使用 arguments 变量

4. 箭头函数的简写
   1. 省略小括号, 当形参有且只有一个的时候
   2. 省略花括号, 当代码体只有一条语句的时候, 此时 return 必须省略,而且语句的执行结果就是函数的返回值
5. 总结
   1. 箭头函数适合与 this 无关的回调。定时器, 数组的方法回调
   2. 箭头函数不适合与 this 有关的回调. 事件回调, 对象的方法

## 函数参数默认值

ES6 允许给函数参数赋值初始值

1. 形参初始值 具有默认值的参数, 一般位置要靠后(潜规则)

2. 与解构赋值结合

## rest 参数

ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments
arguments 是类数组，args 是数组

```js
// rest 参数必须要放到参数最后
function fn(a, b, ...args) {
  console.log(a);
  console.log(b);
  console.log(args);
}
fn(1, 2, 3, 4, 5, 6);
```

## 扩展运算符

1. 『...』 扩展运算符能将『数组』转换为逗号分隔的『参数序列』

2. 应用：
   1. 数组的合并 等同于 concat
   2. 数组的克隆(浅克隆)
   3. 将伪数组转为真正的数组
   ```js
   const divs = document.querySelectorAll("div");
   const divArr = [...divs];
   console.log(divArr);
   ```

## symbol

1. ES6 引入了一种新的原始数据类型 Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，是一种类似于字符串的数据类型。

2. 特点

   1. Symbol 的值是唯一的，用来解决命名冲突的问题
   2. Symbol 值不能与其他数据进行运算
   3. Symbol 定义的对象属性不能使用 for…in 循环遍历，但是可以使用 Reflect.ownKeys 来获取对象的所有键名

3. 创建 Symbol
   let s2 = Symbol();
   let s2 = Symbol('尚硅谷');
   let s3 = Symbol('尚硅谷'); // s2 !== s3
   Symbol.for 创建
   let s4 = Symbol.for('尚硅谷');
   let s5 = Symbol.for('尚硅谷'); // s4 === s5

4. Symbol 内置值

   - Symbol.hasInstance
     当其他对象使用 instanceof 运算符，判断是否为该对象的实例时，会调用这个方法
   - Symbol.isConcatSpreadable
     对象的 Symbol.isConcatSpreadable 属性等于的是一个布尔值，表示该对象用于 Array.prototype.concat()时，是否可以展开。
   - Symbol.species
     创建衍生对象时，会使用该属性
   - Symbol.match
     当执行 str.match(myObject) 时，如果该属性存在，会调用它，返回该方法的返回值。
   - Symbol.replace
     当该对象被 str.replace(myObject)方法调用时，会返回该方法的返回值。
   - Symbol.search
     当该对象被 str.search (myObject)方法调用时，会返回该方法的返回值。
   - Symbol.split
     当该对象被 str.split(myObject)方法调用时，会返回该方法的返回值。
   - Symbol.iterator
     对象进行 for...of 循环时，会调用 Symboliterator 方法，返回该对象的默认遍历器
   - Symbol.toPrimitive
     该对象被转为原始类型的值时，会调用这方法，返回该对象对应的原始类型值。
   - Symbol. toStringTag
     在该对象上面调用 toString 方法时，回该方法的返回值
   - Symbol. unscopables
     该对象指定了使用 with 关键字时，哪些性会被 with 环境排除。

## 迭代器

遍历器（Iterator）就是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。

1. ES6 创造了一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费
2. 原生具备 iterator 接口的数据(可用 for of 遍历)
   a) Array
   b) Arguments
   c) Set
   d) Map
   e) String
   f) TypedArray
   g) NodeList
3. 工作原理
   a) 创建一个指针对象，指向当前数据结构的起始位置
   b) 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员
   c) 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
   d) 每调用 next 方法返回一个包含 value 和 done 属性的对象

注: 需要自定义遍历数据的时候，要想到迭代器。

## 生成器

生成器函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同

1. `*` 的位置没有限制(可以偏左或偏右)
2. 生成器函数返回的结果是迭代器对象，调用迭代器对象的 next 方法可以得到 yield 语句后的值
3. yield 相当于函数的暂停标记，也可以认为是函数的分隔符，每调用一次 next 方法，执行一段代码
4. next 方法可以传递实参，作为上一个 yield 语句的返回值

```js
function* gen(arg) {
  console.log(arg); // AAA
  let one = yield 111;
  console.log(one); // BBB
  let two = yield 222;
  console.log(two); // CCC
  let three = yield 333;
  console.log(three); // DDD
}

//执行获取迭代器对象
let iterator = gen("AAA");
console.log(iterator.next()); // {value: 111, done: false}
//next方法可以传入实参
console.log(iterator.next("BBB")); // {value: 222, done: false}
console.log(iterator.next("CCC")); // {value: 333, done: false}
console.log(iterator.next("DDD")); // {value: undefined, done: false}
```

## promise

Promise 是 ES6 引入的异步编程的新解决方案。语法上 Promise 是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

1. Promise 构造函数: Promise (excutor) {}
2. Promise.prototype.then 方法
3. Promise.prototype.catch 方法

## Set

ES6 提供了新的数据结构 Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历，集合的属性和方法：

1. size 返回集合的元素个数
2. add 增加一个新元素，返回当前集合
3. delete 删除元素，返回 boolean 值
4. has 检测集合中是否包含某个元素，返回 boolean 值
5. clear 清空集合，返回 undefined

```js
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1];
//1. 数组去重
let result = [...new Set(arr)];
console.log(result);
//2. 交集
let arr2 = [4, 5, 6, 5, 6];
let s2 = new Set(arr2); // 4 5 6
let result = [...new Set(arr)].filter((item) => s2.has(item));
console.log(result);
//3. 并集
let union = [...new Set([...arr, ...arr2])];
console.log(union);
//4. 差集
let diff = [...new Set(arr)].filter((item) => !new Set(arr2).has(item));
console.log(diff);
```

## Map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合。但是“键”
的范围不限于字符串，各种类型的值（包括对象）都可以当作键。Map 也实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历。Map 的属性和方法：

1. size 返回 Map 的元素个数
2. set 增加一个新元素，返回当前 Map
3. get 返回键名对象的键值
4. has 检测 Map 中是否包含某个元素，返回 boolean 值
5. clear 清空集合，返回 undefined

## class

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。基本上，ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

知识点：

1. class 声明类
2. constructor 定义构造函数初始化
3. extends 继承父类
4. super 调用父级构造方法
5. static 定义静态方法和属性
6. 父类方法可以重写

## 数值扩展

0. Number.EPSILON 是 JavaScript 表示的最小精度 EPSILON 属性的值接近于 2.2204460492503130808472633361816E-16

1. 二进制和八进制
   let b = 0b1010; // 二进制 10
   let o = 0o777; // 八进制 511
   let d = 100; // 十进制 100
   let x = 0xff; // 十六进制 255

2. Number.isFinite 检测一个数值是否为有限数

3. Number.isNaN 检测一个数值是否为 NaN

4. Number.parseInt Number.parseFloat 字符串转整数

5. Number.isInteger 判断一个数是否为整数

6. Math.trunc 将数字的小数部分抹掉

7. Math.sign 判断一个数到底为正数 负数 还是零

## 对象方法扩展

1. Object.is 比较两个值是否严格相等，与『===』行为基本一致，
   NaN === NaN 为 false，Object.is(NaN, NaN) 为 true
   Object.is(+0, -0) 为 false
   Object.is(+0, 0) 为 true
   Object.is(-0, 0) 为 false
2. Object.assign 对象的合并，将源对象的所有可枚举属性，复制到目标对象
3. Object.setPrototypeOf 设置原型对象 Object.getPrototypeof

## 模块化

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来。

1. 优点

   1. 防止命名冲突
   2. 代码复用
   3. 高维护性

2. 模块化规范产品
   ES6 之前的模块化规范有：

   1. CommonJS => NodeJS、Browserify
   2. AMD => requireJS
   3. CMD => seaJS

3. ES6 模块化语法
   模块功能主要由两个命令构成：export 和 import。
   - export 命令用于规定模块的对外接口
   - import 命令用于输入其他模块提供的功能

## es7 includes `**`

1. includes 方法用来检测数组中是否包含某个元素，返回布尔类型值

2. 在 ES7 中引入指数运算符 `**`，用来实现幂运算，功能与 Math.pow 结果相同

## es8

1. async await

   async 和 await 两种语法结合可以让异步代码像同步代码一样

   - async 函数
     1. async 函数的返回值为 promise 对象，
     2. promise 对象的结果由 async 函数执行的返回值决定
   - await 表达式
     1. await 必须写在 async 函数中
     2. await 右侧的表达式一般为 promise 对象
     3. await 返回的是 promise 成功的值
     4. await 的 promise 失败了, 就会抛出异常, 需要通过 try...catch 捕获处理

2. Object.values 和 Object.entries

   1. Object.keys() 获取对象所有的键
   2. Object.values() 获取对象所有的值
   3. Object.entries() 方法返回一个给定对象自身可遍历属性 `[key,value]` 的数组 可以用来创建 Map `const m = new Map(Object.entries(school));`

3. Object.getOwnPropertyDescriptors
   该方法返回指定对象所有自身属性的描述对象
   可以用来克隆对象 `Object.create(Object.getPrototypeOf(obj),Object.getOwnPropertyDescriptors(obj))`

## es9

1. Rest 参数与 spread 扩展运算符在 ES6 中已经引入，不过 ES6 中只针对于数组，在 ES9 中为对象提供了像数组一样的 rest 参数和扩展运算符

2. 正则扩展-命名捕获分组

```js
let str = '<a href="http://www.atguigu.com">尚硅谷</a>';
const reg = /<a href="(?<url>.*)">(?<text>.\*)<\/a>/;
const result = reg.exec(str);
console.log(result.groups.url);
console.log(result.groups.text);
```

3. 正则扩展-反向断言

```js
let str = "JS5211314你知道么555啦啦啦";
//正向断言
const reg = /\d+(?=啦)/;
const result = reg.exec(str);
//反向断言
const reg = /(?<=么)\d+/;
const result = reg.exec(str);
console.log(result);
```

4. 正则扩展-dotAll 模式
   正则表达式中点`.`匹配除回车外的任何单字符，标记『s』改变这种行为，允许行终止符出现

## es10

1. Object.fromEntries 是 Object.entries 的逆运算，将二维数组或 map 转换为对象

2. trimStart 和 trimEnd 指定清除字符串左侧或右侧的空白

3. Array.prototype.flat 与 flatMap

```js
//将多维数组转化为低维数组
const arr = [1, 2, 3, 4, [5, 6]];
const arr = [1, 2, 3, 4, [5, 6, [7, 8, 9]]];
//参数为深度 是一个数字
console.log(arr.flat(2));
// flatMap 是 flat 和 map 方法的合并，map 操作的同时将结果转化为低维数组
```

4. Symbol.prototype.description 获取 Symbol 的字符串描述

```js
let s = Symbol("尚硅谷");
console.log(s.description); // 尚硅谷
```

## es11

1. 私有属性

```js
class Person {
  name; // 公有属性
  #age; // 私有属性
  constructor(name, age) {
    // 构造方法
    this.name = name;
    this.#age = age;
  }
  intro() {
    console.log(this.name);
    console.log(this.#age);
  }
}
```

2. Promise.allSettled
   返回结果总是成功，值是所有 promise 对象的状态和值的数组(all 方法返回结果看传入的 promise 数组的结果，值是所有成功的值或是失败的值)

3. String.prototype.matchAll
   常用于数据的批量提取

   - `match`返回值是一个数组，如果没有任何匹配项则返回`null`；`matchAll`返回迭代器，要想查看其结果需要遍历迭代器。
   
   - `match`匹配到第一个匹配项后即停止匹配；`matchAll`会匹配出字符串中所有的匹配项。
   
4. 可选链操作符 `?.`
   `const dbHost = config && config.db && config.db.host;`
   `const dbHost = config?.db?.host;`

5. 动态 import

6. BigInt 大整形 用于大数值运算

7. globalThis 始终指向全局变量
