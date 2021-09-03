[toc]

# js 发展史

## js 的特点

- 解释性语言 ——（不需要编译成文件）跨平台
- 单线程
- ECMA 标注
- js 执行队列 —— 轮转时间片：类似吃饭

## 浏览器

- IE：Trident
- Firefox：Gecko
- Chrome：Webkit / Blink
- Safari：Webkit
- Opera：Presto / Blink

# js 入门

## js 引入

- 页面内嵌
- 外部引入

## js 基本语法

### 变量

- 单一 var

### 值类型 —— 数据类型

- 不可改变的原始值（栈 stack）—— Number、Boolean、String、underfined、null
- 引用值（堆 heap）—— array、Object、function、data、RegExp...

### js 运算符

1. 运算操作符 "+"
   1. 数学运算、字符串连接
   2. 任何数据类型加字符串都等于字符串
      
      > 赋值的顺序自右向左，计算的顺序自左向右
2. 比较运算符
   - 比较结果为 boolean 值
   - NaN 不等于任何数，包括自己
3. 逻辑运算符
   - && 判断数据是否存在
   - || 解决浏览器兼容
   - ! 将式子变为 boolean 值，然后取反

### 条件语句

- break：终止循环
- continue：终止本次循环进行下一次循环

### typeof() (区分类型)

- 有两种使用方法，括号或者空格
  
  > number、string、boolean、undefined、object(数组、对象、null)、function
- 当且仅当未定义的变量在 typeof(a)中时，控制台不报错，返回 undefined
  > typeof(typeof(a)) -> String
  > 封装 type 方法，返回所有参数类型

```js
// 封装type
function type(target) {
  var ret = typeof target;
  var template = {
    "[object Null]": "null",
    "[object Array]": "array",
    "[object Object]": "object",
    "[object Number]": "number - object",
    "[object Boolean]": "boolean - object",
    "[object String]": "string - object",
  };
  var str = Object.prototype.toString.call(target);
  if (ret == "object") {
    return template[str];
    // return str.slice(8, -1).toLowerCase()
  } else {
    return ret;
  }
}
```

### 类型转换

```js
tofixed(); // 保留有效数字
Math.ceil(); // 向上取整
Math.floor(); // 向下取整
Math.round(); // 四舍五入
// 浏览器可正常计算范围：前16位，后16位
```

#### 显式类型转换

1. Number(mix)
   - string(里面不是数字，如：'123a') -> NaN
   - undefined -> NaN
2. parseInt(string, radix) 整型 - 将目标转换为十进制
   - string('100px') -> 100
3. parseFloat(string) 浮点型
4. toString(radix)
   - null 和 undefined 不能用 toString，将十进制改为 radix 进制
5. String(mix)
6. Boolean()

#### 隐式类型转换

1. isNaN() 判断是否是 NaN - 调用 Number()
2. ++/-- +/-(一元正负) - 调用 Number()
3. `+` -> 连接字符串，boolean 会转换成 0,1 进行运算, null 变 0
4. -\*/% - 调用 Number()
5. && || !
   - && || -> 通过转换成 boolean 进行判断，返回的值依旧是式子的值
   - ! -> 转换成 boolean，然后取反
6. < > <= >=
   - 如果是字符串与数字比较，会将字符串变成数字然后比较，字符串之间比较是进行 asc 码的比较
7. == !=
   - undefined 和 null 与 0 比较都是 false
   - undefined == null
   - NaN == NaN -> false

#### 不发生类型转换

- !== ===

# js 函数

## 定义

1. 函数声明
   `function test() {}`
   - 如果打印 test，会得到函数体（弱数据类型语言不会输出地址
2. 函数表达式
   - 命名函数表达式
     `var test = function abc() { document.write('a'); }`
     `test.name = abc`
   - 匿名函数表达式 —— 函数表达式
     `var demo = function () { document.write('a'); }`
     `test.name = demo`

## 组成形式

1. 函数名称
2. 参数
   - 形参 func.length —— 形参长度
   - 实参 arguments —— 实参列表
     
     > 当形参与实参对应时，数据产生映射，一个变另一个跟着变。
3. return
   - 终止函数
   - 返回值

## 作用域

- 变量（变量作用域又称上下文）和函数生效（能被访问）的区域

  - 全局变量
  - 局部变量

- `[[scope]]`：每个 javascript 函数都是一个对象，对象中有些属性我们可以访问，但有些不可以，这些属性仅供 javascript 引擎存取，`[[scope]]`就是其中一个。`[[scope]]`指的就是我们所说的作用域,其中存储了运行期上下文的集合。

- 作用域链:`[[scope]]` 中所存储的执行期上下文对象的集合，这个集合呈链式链接，我们把这种链式链接叫做作用域链。

- 运行期上下文：当函数执行时,会创建一个称为执行期上下文的内部对象（AO）。一个执行期上下文定义了一个函数执行时的环境，函数每次执行时对应的执行上下文都是独一无二的，所以多次调用一个函数会导致创建多个执行上下文，当函数执行完毕，它所产生的执行上下文被销毁。

- 查找变量：从作用域链的顶端依次向下查找。

### 预编译

- js 运行三部曲：
  - 语法分析 - 扫描语法错误
  - 预编译
  - 解释执行

#### 预编译前奏

1. imply global 暗示全局变量：即任何变量，如果变量未经声明就赋值，此变量就位全局对象（window）所有。
2. 一切声明的全局变量，全是 window 的属性。（window 就是全局的域）

#### 预编译四部曲

- 预编译发生在函数执行的前一刻

1. 创建 AO 对象（Activation Object 执行期上下文）
2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined
3. 将实参值和形参统一
4. 在函数体里面找函数声明，值赋予函数体

## 递归

1. 找规律
2. 找出口

## 闭包

- 当内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域链不释放,造成内存泄露。

### 闭包的作用

- 实现公有变量 eg:函数累加器
- 可以做缓存(存储结构) eg:eater
- 可以实现封装，属性私有化 eg: Person();
- 模块化开发，防止污染全局变量

## 立即执行函数

- 定义：此类函数没有声明，在一次执行过后即释放。适合做初始化工作。
- 只有表达式才能被执行符号执行，能被执行符号执行的表达式，函数的名字会被自动忽略。

# js 对象

## 属性的增、删、改、查

- 增、改：调用 对象.属性 = ...
- 查：对象.属性
- 删：delete 对象.属性

## 对象的创建方法

1. 字面量 plainObject
   `var obj = {}`
2. 构造函数（大驼峰式命名规则）

- 系统自带 new Object();Array();Number();Boolean();String();Date();
- 自定义

> 构造函数内部原理 (new 关键字)
>
> - 在函数最前面隐式的加上 `this = { __proto__ : Person.prototype }` -> `this = Object.create(func.prototype)`
> - 执行 this.xxx = xxx
> - 隐式的返回 this

3. Object.create(原型)方法

## 包装类

- 可以进行对象的操作，但是进行运算后会变成基础类型，不再具有对象的特点。

- new String();
- new Boolean();
- new Number();

```js
// 包装类
var num = 4;
num.len = 3;
// new Number(4).len = 3;   delete
// new Number(4).len
console.log(num.len); // -> undefined
```

## 原型

1. 定义：原型是 function 对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。
2. 利用原型特点和概念，可以提取共有属性。
3. 对象如何查看原型 -> 隐式属性`_proto_`
4. 对象如何查看对象的构造函数 -> constructor

## 原型链

1. 如何构成原型链?
2. 原型链上属性的增删改查
   > 除了引用修改，操作不了原型的属性。
   > ++ 操作是将原型的属性值加一然后赋给自己，原型属性不变。
3. **绝大多数**对象最终都会继承自 Object.prototype
   `Object.create(null);` -> 没有原型，且手动赋给原型值不生效。
4. Object.create(原型);
   `document.write(); -> 调用 toString() 方法，将结果打印在页面上`

## call/apply

- 作用：改变 this 指向
- 区别：传参列表不同
  > call 需要把实参按照形参的个数传进去
  > apply 需要传一个 arguments

## 继承发展史

1. 传统形式 —-> 原型链
   
   > 过多的继承了没用的属性
2. 借用构造函数
   > 不能继承借用构造函数的原型
   > 每次构造函数都要多执行一个函数
3. 共享原型
   
   > 不能随便改动自己的原型
4. 圣杯模式

```js
var inherit = (function () {
  var F = function () {};
  return function (Target, Origin) {
    F.prototype = Origin.prototype;
    Target.prototype = new F();
    Target.prototype.constructer = Target;
    Target.prototype.uber = Origin.prototype;
  };
})();
Father.prototype.lastName = "Deng";
function Father() {}
function Son() {}
inherit(Son, Father);
var son = new Son();
var father = new Father();
```

## 对象的枚举

- for in 循环 -> 遍历对象
- hasOwnProperty() -> 判断属性是否是原型的
  
  > 会判断到 Object 上自己设置的属性，系统设置的不会判断
- in -> 判断该属性是否是对象的属性（原型上的也算该对象的）
- instanceof
  
  > A instanceof B -> A 对象是不是 B 构造函数构造出来的（官方文档） 看 A 对象的原型链上有没有 B 的原型

> 拓展
>
> 1. 如何实现链式调用模式（模仿 jquery）
>    在对象的函数上返回对象(this)
> 2. 属性表示方法
>    obj.prop -> 隐式转换为 obj['prop']
>    所以当属性是变量时要用 obj[name]
> 3. 区别数组和对象的三种方法
>    a. constructor
>    b. instanceof
>    c. Object.prototype.toString.call 方法

```js
var obj = {}
obj.constructor  // -> 返回Object
[].constructor  // -> 返回Array

obj instanceof Array  // -> false
[] instanceof Array  // -> true

Object.prototype.toString.call([]); //  [ "object Array"]
Object.prototype.toString.call({}); //  [ "object Object"]
```

> 4. 引用值比对的是地址
> 5. this
>    a. 函数预编译过程 this —> window
>    b. 全局作用域里 this —> window
>    c. call/apply 可以改变函数运行时 this 指向
>    a. obj.func(); func()里面的 this 指向 obj (谁调用指向谁)
> 6. arguments
>    a. arguments.callee ：指向自身引用
>    b. func.caller ：返回被调用的环境
> 7. 克隆
>    浅层克隆：克隆引用值时，新旧对象的引用地址相同，会相互影响。
>    深层克隆
> 8. 三目运算符
>    条件判断 ? 是 : 否 -> 并且会返回值
> 9. 不可配置的属性
>    全局上使用 var 操作所得的属性（window），叫做不可配置的属性,无法被 delete 操作。

# js 数组

## 数组定义

1. new Array(length/content); `var arr = new Array();`
2. 字面量 `var arr = [];`
   > 两种方法都一样，除了赋值长度
   > `var arr = new Array(10) --- 长度; var arr1 = [10] --- 内容;`

## 数组的读和写

1. arr[num] -> 不可以溢出读 结果 undefined
2. arr[num] = xxx; -> 可以溢出写

## 数组常用的方法

1. 改变原数组

- push() 添加 （从最后一位开始增加，可以同时增加多个）返回数组长度
- pop() 剪切 （从最后一位开始剪切，没有参数） 返回剪切的数
- shift() 剪切 （从第一位开始剪切，没有参数） 返回剪切的数
- unshift() 添加 （从第一位开始增加，可以同时增加多个） 返回数组长度
- sort() 排序，默认是按 asc 码排序
  > 自定义排序 -> 1. 必须写两个形参 2. 看返回值 ->
  > 返回值为负数，位置不变，返回值为正数，参数互换位置，为 0，不动
- reverse() 逆转，返回的是原数组
- splice(从第几位开始，截取多少的长度，在切口处添加新的数据(添加的参数可以是多个)) 返回截取的片段
  > 第一个参数可以是负数，-1 等于 this.length - 1
  > `参数 += (参数 > 0) ? 0 : this.length`

2. 不改变原数组

- concat() 拼接数组，返回一个新的数组，原数组不变。
  `arr.concat(arr1);`
- join() 数组返回字符串 ——> split() 字符串返回数组
  
  > 根据参数进行连接和拆分，join 是连接成串，split 是拆分成数组
- toString() 把数组连接成字符串，并返回该字符串
  `var arr = [1, 2, 3]; arr.toString(); -> "1,2,3"`
- slice() 截取字符串
  > 不写参数：截取整个数组
  > 一位参数：从该位开始截取，一直截取到最后(可以是负数)
  > 两位参数：从该位开始截取, 截取到该位

## 类数组

- 属性要为索引（数字）属性，**必须有 length 属性**，最好加上 push
  > 如果加上 splice: Array.prototype.splice，输出形式变成数组形式
  > 如果强行让类数组调用 push 方法，则会根据 length 属性值的位置进行属性的扩充

> 拓展
>
> - `[] + '' -> ''`
> - `[] + 1 -> '1'`
> - `{} + [] -> 0`
> - `[] + {} -> "[object Object]"`
> - `[] == [] -> false 引用值比较的是地址`

- 类数组转数组的方法
  > ... 运算符
  > Array.from
  > Array.prototype.slice.apply(arguments)

# es5 严格模式

- 浏览器是基于的 es3.0 + es5.0 的新增方法使用的
- es5.0 严格模式：那么 es3.0 和 es5.0 产生冲突的部分就是用 es5.0 否则会用 es3.0

## "use strict";

- 不再兼容 es3 的一些不规则语法。使用全新的 es5 规范。
- 两种用法:
  > 全局严格模式 -- 写在页面逻辑的最顶端
  > 局部函数内严格模式(推荐) -- 局部函数内的最顶端
- 就是一行字符串，不会对不兼容严格模式的浏览器产生影响。
- 不支持 with，arguments.callee，func.caller，变量赋值前必须声明，局部 this 必须被赋值(Person.call(null/undefined)赋值什么就是什么)，拒绝重复属性和参数。
  > with(Object)：可以修改作用域链的最顶端为参数对象，可以用于简化代码，但是由于过于强大，修改作用域链会消耗大量的效率，es5.0 就弃用了
  > eval('console.log("a")')：可以将字符串作为代码执行，但是它会改变作用域，在 es3.0 里都不要使用

# try catch

## try{} catch(e){} finally{}

- 在 try 里面的发生错误，不会执行错误后的 try 里面的代码
- catch 会抛出 error.name 和 error.message

## Error.name 的六种值对应的信息

1. EvalError: eval() 的使用与定义不一致
2. RangeError：数值越界
3. ReferenceError：非法或不能识别的引用数值
4. SyntaxError：发生语法解析错误 - 语义分析时发生错误
5. TypeError：操作数类型错误
6. URIError: URI 处理函数使用不当

# Dom (Document Object Model)

- DOM 定义了表示和修改文档所需的方法。DOM 对象即为宿主对象，由浏览器厂商定义，用来操作 html 和 xml 功能的一类对象的集合。也有人称 DOM 是对 HTML 以及 XML 的标准编程接口。
  
  > xml 能自定义标签，html 不能

## Dom 结构树

- document -> HTMLDocument.prototype -> Document.prototype (原型链)

## Dom 基本操作

### 对节点的增删改查

#### 查

1. 查看元素节点

- document 代表整个文档
- document.getElementByld() -> 元素 id 在 IE8 以下的浏览器，不区分 id 大小写，而且也返回匹配 name 属性的元素

- .getElementsByTagName() -> 标签名
- .getElementsByClassName() -> 类名 -> IE8 和 IE8 以下的 IE 版本中没有，可以多个 class 一起

- .getElementsByName(); -> 需注意，只有部分标签 name 可生效(表单，表单元素，img，iframe)
- .querySelector() -> css 选择器，在 IE7 和 IE7 以下的版本中没有
  
  > querySelector 和 querySelectorAll 不是实时的，就和照相一样
- .querySelectorAll() -> css 选择器，在 IE7 和 IE7 以下的版本中没有

2. 遍历节点树

- parentNode -> 父节点(最顶端的 parentNode 为#document);
- childNodes -> 子节点们
- firstChild -> 第一个子节点
- lastChild -> 最后一个子节点
- nextSibling -> 后一个兄弟节点
- previousSibling -> 前一个兄弟节点

3. 基于元素节点树的遍历

- parentElement -> 返回当前元素的父元素节点(最顶端的 parentElement 为 html);(IE 不兼容)

- children -> 只返回当前元素的元素子节点

- node.childElementCount === node.children.length 当前元素节点的子元素节点个数(IE 不兼容)
- firstElementChild -> 返回的是第一个元素节点(IE 不兼容)
- lastElementChild -> 返回的是最后一个元素节点(IE 不兼容)
- nextElementSibling / previousElementSibling -> 返回后一个/前一个兄弟元素节点(IE 不兼容)

#### 增

- document.createElement(); -> 创建元素节点
  `var div = document.createElement('div')`
- document.createTextNode(); -> 创建文本节点
  `var test = document.createTextNode('邓宝宝')`
- document.createComment(); -> 创建注释节点
  `var conment = document.createComment('this is comment')`
- document.createDocumentFragment(); -> 创建文档碎片节点

#### 插

- parentNode.appendChild(); -> 任何一个元素节点都有此方法
  `div.appendChild(text)`
  
  > 与 push 类似，在元素的尾部插入节点，如果是将页面上已有的节点插入另一个地方，会执行剪切操作，即原来的地方没有该节点。
- parentNode.insertBefore(a, b); -> insert a before b
  `div.insertBefore(strong, span) -> 在span标签前插入strong标签`
  > 同样是剪切操作
  > 封装 insertAfter 方法
  ```js
  Element.prototype.insertAfter = function (targetNode, afterNode) {
    var beforeNode = afterNode.nextElementSibling;
    if (beforeNode != null) {
      this.insertBefore(targetNode, beforeNode);
    } else {
      this.appendChild(targetNode);
    }
  };
  ```

#### 删

- parent.removeChild(); -> “谋杀”
  `div.removeChild(i) -> 返回 <i></i>，所以实际上是剪切`
- child.remove(); -> “自杀”
  `i.remove() -> 没有返回值`

#### 替换

- parent.replaceChild(new, origin); -> 用 new 的元素替换 origin 的元素
  `div.replaceChild(p, strong) -> 返回 <strong></strong>`

### Dom 结构树

```Dom 结构树
           - Document —— - HTMLDocument
                               - Text                   -  HTMLHeadElement
           - CharacterData ——                           -  HTMLBodyElement
 Node ——                       - Comment                -  HTMLTitleElement
                                                        -  HTMLParagraphElement
           - Element(文档中的元素) —— - HTMLElement ——   -  HTMLInputElement
                                                        -  HTMLTableElement
           - Attr                                       -  ...etc.
```

> document -> HTMLDocument.prototype -> Document.prototype (原型链)

### 基于结构树的方法

1. getElementById 方法定义在 Document.prototype 上，即 Element 节点上不能使用。
2. getElementsByName 方法定义在 HTMLDocument.prototype 上，即非 html 中的 document 不能使用(xml,document,Element)
3. getElementsByTagName 方法定义在 Document.prototype 和 Element.prototype 上
4. HTMLDocument.prototype 定义了一些常用的属性，body，head，分别指代 HTML 文档中的<body><head>标签。
5. Document.prototype 上定义了 documentElement 属性，指代文档的根元素，在 HTML 文档中,他总是指代<html>元素
6. getElementsByClassName、querySelectorAll、querySelector 在 Document.prototype，Element.prototype 类中均有定义

> document.head -> head 标签
> document.body -> body 标签
> document.documentElement -> html 标签
> getElementsByTagName、getElementsByClassName 可以被 div 等标签调用

### 节点的类型

- 元素节点 —— 1
- 属性节点 —— 2 （节点本身包括的属性，调用 attributes 查看）
- 文本节点 —— 3
- 注释节点 —— 8
- document —— 9
- DocumentFragment —— 11

> 后面的数字表示节点调用 nodeType 返回的值

### 节点的四个属性

- nodeName
  
  > 元素的标签名，以大写形式表示，只读
- nodeValue
  
  > Text 节点或 Comment 节点的文本内容,可读写
- nodeType
  
  > 该节点的类型,只读
- attributes
  
  > Element 节点的属性集合

### 节点的一个方法

- Node.hasChildNodes(); -> 返回布尔值

### Element 节点属性

1. Element 节点的属性

- innerHTML -> 改变元素里面的 html 内容(可以是标签、属性，并且会生效)
  `div.innerHTML // 取操作`
  `div.innerHTML = '123' // 会覆盖之前的内容`
  `div.innerHTML += '456' // 追加内容`
- innerText(火狐不兼容) / textContent(老版本 IE 不好使)
  `div.innerText // 取操作`
  `div.innerText = '123' // 会覆盖其他的内容（包括元素）`

2. Element 节点的方法

- ele.setAttribute(); -> 添加属性
  `div.setAttribute('class', 'demo')`
- ele.getAttribute(); -> 查询属性
  `div.getAttribute('class') -> 'demo'`

## Date 对象

- W3CSchool API 文档
- 纪元时间：1970 - 01 -01 （计算机零点时间）
- Date() 返回当日的日期和时间。
- getFullYear() 从 Date 对象以四位数字返回年份。
- getMonth() 从 Date 对象返回月份 (0 ~ 11)。
- getDate() 从 Date 对象返回一个月中的某一天 (1 ~ 31)。
- getDay() 从 Date 对象返回一周中的某一天 (0 ~ 6)。
- getHours() 返回 Date 对象的小时 (0 ~ 23)。
- getMinutes() 返回 Date 对象的分钟 (0 ~ 59)。
- getSeconds() 返回 Date 对象的秒数 (0 ~ 59)。
- getTime() 返回 1970 年 1 月 1 日至今的毫秒数。
- parse() 返回 1970 年 1 月 1 日午夜到指定日期（字符串）的毫秒数。
- setTime() 以毫秒设置 Date 对象
- toString() 把 Date 对象转换为字符串。
- toTimeString() 把 Date 对象的时间部分转换为字符串。
- toDateString() 把 Date 对象的日期部分转换为字符串。
- 时间戳：记录下的某个时刻

## js 定时器

- setInterval(); -> 返回值为一个数，作为唯一表示
  
  > 这个函数是不准的
- setTimeout(); -> 返回值为一个数，作为唯一表示
  
  > setInterval() 和 setTimeout() 的唯一表示会依次增加，从 1 开始
- clearInterval();
  
  > 参数是 setInterval() 的唯一标识，可以清除掉该定时器。
- clearTimeout();
- 全局对象 window 上的方法，内部函数 this 指向 window
  > 注意：setInterval("func()",1000);
  > setInterval("console.log('a')",1000); 也能执行，一般不这么写
  > 封装函数，打印当前是何年何月何日何时，几分几秒

```js
Date.prototype.myDay = function () {
  var date = new Date();
  return (
    "现在是" +
    date.getFullYear() +
    "年" +
    (date.getMonth() + 1) +
    "月" +
    date.getDate() +
    "日" +
    date.getHours() +
    "时" +
    date.getMinutes() +
    "分" +
    date.getSeconds() +
    "秒"
  );
};
var date = new Date();
console.log(date.myDay());
```

## Dom 基本操作

### 查看滚动条的滚动距离

- window.pageXOffset/pageYOffset -> IE 及 IE8 以下不兼容
- document.body.scollLeft/scollTop 或 document.documentElement.scollLeft/scollTop
  
  > 兼容性比较混乱，总是取两个值相加，因为不可能存在两个同时有值。
- 封装兼容性方法，求滚动条滚动距离 getScrollOffset() -> tools

```js
// 滚动条滚动距离
function getScrollOffset() {
  if (window.pageXOffset) {
    return {
      x: window.pageXOffset,
      y: widnow.pageYOffset,
    };
  } else {
    return {
      x: document.body.scrollLeft + document.documentElement.scrollLeft,
      y: document.body.scrollTop + document.documentElement.scrollTop,
    };
  }
}
```

### 查看视口的尺寸

- window.innerWidth/innerHeight -> IE 及 IE8 以下不兼容
- document.documentElement.clientWidth/clientHeight
  
  > 标准模式下，任意浏览器都兼容
- document.body.clientWidth/clientHeight
  
  > 适用于怪异模式下的浏览器
- 封装兼容性方法，返回浏览器视口尺寸 getViewportOffset()
  > 浏览器有两种渲染模式：标准模式 和 怪异/混杂模式 -> <!DOCTYPE html> 存在为标准模式，否则为怪异模式
  > 怪异/混杂模式一经启动，会使用之前版本的语法，试图兼容之前页面的语法

```js
// 返回浏览器视口尺寸
function getViewportOffset() {
  if (window.innerWidth) {
    return {
      w: window.innerWidth,
      h: window.innerHeight,
    };
  } else {
    if (document.compatMode === "BackCompat") {
      return {
        w: document.body.clientWidth,
        h: document.body.clientHeight,
      };
    } else {
      return {
        w: document.documentElement.clientWidth,
        h: document.documentElement.clientHeight,
      };
    }
  }
}
```

### 查看元素的几何尺寸

- domEle.getBoundingClientRect(); -> 兼容性很好
  > 该方法返回一个对象，对象里面有 left,top,right,bottom 等属性。left 和 top 代表该元素左上角的 X 和 Y 坐标，right 和 bottom 代表元素右下角的 X 和 Y 坐标。
  > height 和 width 属性老版本 IE 并未实现
  > 返回的结果并不是“实时的”

### 查看元素的尺寸

- dom.offsetWidth, dom.offsetHeight

### 查看元素的位置

- dom.offsetLeft, dom.offsetTop
  
  > 对于无定位父级的元素，返回相对文档的坐标。对于有定位父级的元素，返回相对于最近的有定位的父级的坐标。
- dom.offsetParent
  > 返回最近的有定位的父级，如无，返回 body，body.offsetParent 返回 null
  > eg：求元素相对于文档的坐标 getElementPosition

### 让滚动条滚动

- window 上有三个方法：scroll(),scrollTo() scrollBy();
  > 三个方法功能类似，用法都是将 x，y 坐标传入，前两个用法一模一样。实现让滚动轮滚动到当前位置。
  > 区别：scrollBy() 会在之前的数据基础之上做累加。

## 脚本化 CSS

### 读写元素 css 属性

1. dom.style.prop -> 只能查询到行间样式

- 可读写行间样式，没有兼容性问题，碰到 float 这样的保留字属性，前面应加 css
  
  > eg：float -> cssFloat
- 复合属性必须拆解，组合单词变成小驼峰式写法
- 写入的值必须是字符串格式

2. 查询计算样式

- window.getComputedStyle(ele, null); -> 当前元素所展示的 css 的显示值（包括默认值）
  `window.getComputedStyle(div, null).prop`
  
  > 第二个参数一般是 null，可以用来选中该元素的伪元素
- 计算样式只读
- 返回的计算样式的值都是绝对值，没有相对单位
- IE8 及 IE8 以下不兼容
  - 查询样式
    > ele.currentStyle
    > 计算样式只读
    > 返回的计算样式的值不是经过转换的绝对值，原值是什么返回什么
    > IE 独有的属性
- 封装兼容性方法 getStyle(elem, prop);

```js
// 查询样式
function getStyle(elem, prop) {
  if (window.getComputedStyle) {
    return window.getComputedStyle(elem, null)[prop];
  } else {
    return elem.currentStyle[prop];
  }
}
```

## 事件

> 事件本身存在，可以通过绑定函数，当事件发生时触发。事件是交互体验的核心功能

### 如何绑定事件处理函数

1. ele.onXXX = function (event){} （又叫句柄的方式）
   > 兼容性很好，但是一个元素的同一个事件上只能绑定一个处理程序
   > 基本等同于写在 HTML 行间上
2. obj.addEventListener(type(事件类型), fn(处理函数), false);
   > false ：冒泡, true ：捕获
   > IE9 以下不兼容，可以为一个事件绑定多个处理程序
3. obj.attachEvent('on' + type, fn);
   
   > IE 独有，一个事件同样可以绑定多个处理程序

### 事件处理程序的运行环境

1. ele.onXXX = function (event){}
   
   > 程序 this 指向 dom 元素本身
2. obj.addEventListener(type, fn, false);
   
   > 程序 this 指向 dom 元素本身
3. obj.attachEvent('on’+ type, fn);
   > 程序 this 指向 window
   > 封装兼容性的 addEvent(elem, type, handle);方法

```js
// 给一个 dom 对象添加该事件类型的处理函数
function addEvent(elem, type, handle) {
  if (elem.addEventListener) {
    elem.addEventListener(type, handle, false);
  } else if (elem.attachEvent) {
    elem.attachEvent("on" + type, function () {
      handle.call(elem);
    });
  } else {
    elem["on" + type] = handle;
  }
}
```

### 解除事件处理程序

- ele.onclick = false/""/null;
- ele.removeEventListener(type, fn, false);
- ele.detachEvent('on'+ type, fn);
  > 注：若绑定匿名函数，则无法解除
  > 封装兼容性的 removeEvent(elem, type, handle);方法

```js
// 删除该 dom 对象事件类型的处理函数
function removeEvent(elem, type, handle) {
  if (elem.removeEventListener) {
    elem.removeEventListener(type, handle, false);
  } else if (elem.detachEvent) {
    elem.detachEvent("on" + type, function () {
      handle.call(elem);
    });
  } else {
    elem["on" + type] = null;
  }
}
```

### 事件处理模型 —— 事件冒泡、捕获

- 一个对象的一个事件类型上面绑定的一个处理函数只能遵循一个处理模型

1. 事件冒泡：
   
   > 结构上 (非视觉上) 嵌套关系的元素，会存在事件冒泡的功能，即同一事件，自子元素冒泡向父元素。(自底向上)
2. 事件捕获：
   > 结构上 (非视觉上) 嵌套关系的元素，会存在事件捕获的功能，即同一事件，自父元素捕获至子元素 (事件源元素) 。(自顶向下)
   > IE 没有捕获事件
3. 触发顺序，先捕获，后冒泡
   
   > 一个对象的一个事件类型上面绑定两个处理函数，让两个函数分别遵循事件冒泡和事件捕获
4. focus，blur，change，submit，reset，select 等事件不冒泡

### 取消冒泡和阻止默认事件

- 取消冒泡
  > W3C 标准 event.stopPropagation();但不支持 ie9 以下版本
  > IE 独有 event.cancelBubble = true;
  > 封装取消冒泡的函数 stopBubble(event)

```js
// 取消冒泡
function stopBubble(event) {
  if (event.stopPropagation) {
    event.stopPropagation();
  } else {
    event.cancelBubble = true;
  }
}
```

- 阻止默认事件 - 默认事件 —— 表单提交，a 标签跳转，右键菜单等
  > return false; 以对象属性的方式注册的事件才生效（句柄）
  > event.preventDefault(); W3C 标注，IE9 以下不兼容
  > event.returnValue = false; 兼容 IE
  > 封装阻止默认事件的函数 cancelHandler(event);

```js
// 阻止默认事件
function cancelHandler(event) {
  if (event.preventDefault) {
    event.preventDefault();
  } else {
    event.returnvalue = false;
  }
}
```

### 事件对象 —— 发生的是什么事

- event || window.event 用于 IE

#### 事件源对象 —— 是谁发生的事件

- event.target -> 火狐只有这个
- event.srcElement -> IE 只有这个
- 这两 chrome 都有

#### 事件委托

- 利用事件冒泡，和事件源对象进行处理
- 优点：
  1. 性能 不需要循环所有的元素一个个绑定事件
  2. 灵活 当有新的子元素时不需要重新绑定事件

```js
// 要求给每个li绑定点击事件，点击后显示li里的文本信息
var ul = document.getElementsByTagName("ul")[0];
ul.onclick = function (e) {
  var event = e || window.event;
  var tar = event.target || event.srcElement;
  if (tar.tagName.toLowerCase() === "li") alert(tar.innerText);
};
```

### 事件分类

#### 鼠标事件

1. click（单击）、dblclick（双击）、mousedown（鼠标按下）、mousemove（鼠标移动）、mouseup（鼠标松开）、contextmenu（右键菜单）、mouseover、mouseout、mouseenter（鼠标进入）、mouseleave（鼠标移开）
   > click = mousedown + mouseup
   > 双击事件的触发顺序：mousedown → mouseup → click → mousedown → mouseup → click → dblclick
   > mouseover = mouseenter，mouseout = mouseleave
2. 用 button 来区分鼠标的按键，0（左键）/1/2（右键）
3. DOM3 标准规定：click 事件只能监听左键，只能通过 mousedown 和 mouseup 来判断鼠标右键
4. 如何解决 mousedown 和 click 的冲突

```js
var firstTime = 0;
var lastTime = 0;
var key = false;
document.onmousedown = function () {
  firstTime = new Date().getTime();
};
document.onmouseup = function () {
  lastTime = new Date().getTime();
  if (lastTime - firstTime > 300) {
    key = true;
  }
};
document.onclick = function () {
  if (key) {
    console.log("click");
  }
};
```

#### 键盘事件

1. keydown keyup keypress
2. keydown > keypress > keyup
3. keydown 和 keypress 的区别
   > keydown 可以响应任意键盘按键，keypress 只响应字符类键盘按键
   > keypress 返回 ASCII 码，可以转换成相应字符
   > 操作类按键只能用 keydown
   > 监测字符类按键，并且区分大小写用 keypress
   > String.fromCharCode(e.charCode) -> 显示输入的字符

#### 文本操作事件

1. input、change、focus、blur

- input：输入或删除都会触发事件
- change：聚焦和失去焦点时比对，内容相同不触发，不同就会触发。
- focus：聚焦时触发
- blur：失去焦点时触发

#### 窗体操作类（window 上的事件）

1. scroll load

- scroll：滚动条滚动时触发事件
- load：等页面全部就绪后触发（页面自动完成的工作已全部就绪）
  
  > 一般不用的原因：效率最低、没有意义

## json

- JSON 是一种传输数据的格式(以对象为样板，本质上就是对象，但用途有区别，对象就是本地用的，json 是用来传输的)
- JSON.parse(); string — > json
- JSON.stringify(); json — > string
  
  > 与函数和构造函数有区别一样，JSON 和对象的区别，JSON 的属性要加引号

## 异步加载 JS

1. js 加载的缺点：

- 加载工具方法没必要阻塞文档，过度 js 加载会影响页面效率，一旦网速不好，那么整个网站将等待 js 加载而不进行后续渲染等工作。
- 有些工具方法需要按需加载，用到再加载，不用不加载。

2. javascript 异步加载的三种方案
   1. defer 异步加载，但要等到 dom 文档全部解析完才会被执行。只有 IE 能用，也可以将代码写到内部。
   2. async 异步加载，加载完就执行，async 只能加载外部脚本，不能把 js 写在 script 标签里。
      
      > 1.2 执行时也不阻塞页面
   3. 创建 script，插入到 DOM 中，加载完毕后 callBack,
      > 异步加载 script 标签方法 (tools.js)，但是函数执行传参会出现问题，第二个参数在传入时会报错函数未定义，解决方法：
      > (1) 传参使用匿名函数，函数内部执行体写上参数函数并执行
      > (2) 传入参数函数执行体的字符串("test()")，使用 eval()执行
      > (3) 在传入的 js 文件里定义一个对象，将参数函数的函数体定义为对象的属性，然后传入该参数函数的字符串("test()")，在加载函数里面进行执行 obj[callback]()

```js
// 异步加载 script 标签
function loadScript(url, callback) {
  var script = document.createElement(" script ");
  script.type = "text/javascript";
  if (script.readyState) {
    script.onreadystatechange = function () {
      // Ie
      if (script.readyState == "complete" || script.readyState == "loaded") {
        callback();
      }
    };
  } else {
    script.onload = function () {
      //Safari chrome firefox opera
      callback();
    };
  }
  script.src = url;
  document.head.appendChild(script);
}
```

### 浏览器渲染页面

1.  domTree -> 深度优先原则
    > dom 节点的解析，如果需要下载资源会异步进行。比如一个 img 标签，dom 会直接挂到树上，然后继续向下解析，同时异步下载 img 资源。
    > 深度优先原则：就是一条枝干走到头，再进行下一条枝干。
    > 广度优先原则：一个层面一个层面的搜索。
2.  cssTree -> 深度优先原则，和 dom 节点对应
    > domTree 生成后会等待 cssTree 的生成，然后拼接在一起，生成 randerTree (渲染树)，然后渲染引擎绘制页面。
    > reflow (重排，效率最低)
    > (1) dom 节点的删除、增加
    > (2) dom 节点的宽高变化，位置变化，display: none -> block
    > (3) offsetWidth,offsetLeft
    > repaint (重绘，浪费的效率较少) 比如修改颜色，背景等

## js 加载时间线

1. 创建 Document 对象，开始解析 web 页面。解析 HTML 元素和他们的文本内容后添加 Element 对象和 Text 节点到文档中。这个阶段 document.readyState = 'loading'。
2. 遇到 link 外部 css，创建线程加载，并继续解析文档。
3. 遇到 script 外部 js，并且没有设置 async、defer，浏览器加载，并阻塞，等待 js 加载完成并执行该脚本，然后继续解析文档。
4. 遇到 script 外部 js，并且设置有 async、defer，浏览器创建线程加载，并继续解析文档。
   
   > 对于 async 属性的脚本，脚本加载完成后立即执行。(异步禁止使用 document.write())
5. 遇到 img 等，先正常解析 dom 结构，然后浏览器异步加载 src，并继续解析文档。
6. 当文档解析完成，document.readyState = 'interactive'。
7. 文档解析完成后，所有设置有 defer 的脚本会按照顺序执行。(注意与 async 的不同，但同样禁止使用 document.write());
8. document 对象触发 DOMContentLoaded 事件，这也标志着程序执行从同步脚本执行阶段，转化为事件驱动阶段。
9. 当所有 async 的脚本加载完成并执行、img 等加载完成后，document.readyState = 'complete'， window 对象触发 load 事件。
10. 从此，以异步响应方式处理用户输入、网络事件等。

## 正则表达式 (RegExp)

### 课前补充

1. 转义字符 \

- \n —— 换行 \r —— 行结束 \t —— table（tab 缩进）

2. 多行字符串
3. 字符串换行符 \n

### RegExp

> 作用：匹配特殊字符或有特殊搭配原则的字符的最佳选择

1. 两种创建方式
   `var reg = /abc/m`
   `var reg = new RegExp("abc","m");`
2. 修饰符

- i：忽略大小写
- m：多行匹配
- g：全局匹配

3. 方括号

- [abc] 查找方括号之间的任何字符
- [^abc] 查找任何不再方括号之间的字符
- [0-9]
- [a-z]
- [A-Z]
- [A-z]
- (red|blue|green) 查找任何指定的选项

4. 元字符 - 拥有特殊含义的字符

- \w === [0-9A-z_]
- \W === [^\w]
- \d === [0-9]
- \D === [^\d]
- \s === [\t\n\r\v\f]
- \S === [^\s]
- \b === 单词边界
- \B === 非单词边界
  `var reg = /\bcde\B/g; var str = "abc cdefgh"; str.match(reg)`
- . === [^\r\n] 查找单个字符，除了换行和行结束符

5. 量词

- n+ -> {1, }
- `n* -> {0, }`
- n? -> {0, 1}
  
  > 正则表达式符合贪婪匹配原则，能多匹配就会多匹配。如果加?就会变成非贪婪匹配。如果是??或者\*?，就会变成空匹配，将匹配到的用空串代替
- n{x} -> 匹配包含 X 个 n 的序列的字符串
- n{x, y} -> 匹配包含 X 至 Y 个 n 的序列的字符串
- n{x, } -> 匹配包含至少 X 个 n 的序列的字符串
- ^n 匹配任何开头为 n 的字符串
- n$ 匹配任何结尾为 n 的字符串
- ?= -> 正向预查 正向断言 (后面跟着谁)
  `var str = 'abaaa'; var reg = /a(?=b)/g; var reg = /a(!=b)/g;`
- != -> (后面跟着的不是谁)

> eg：检验一个字符串首尾是否含有数字 -> /^\d|\d$/g

6. 属性

- global -> 是否含有 g
- ignoreCase -> 是否含有 i
- multiline -> 是否含有 m
- source -> 正则表达式的源文本。
- lastIndex -> 一个整数，标示着开始下一次匹配的字符位置。(游标)

7. 方法

- exec() -> 检索字符串中指定的值。返回找到的值，并确定其位置。
- test() -> 检索字符串中指定的值。返回 true 或 false。
  > 如何选出四个一样的字符组成的字符串，如：'aaaa'
  > /(\w)\1\1\1/g 此时的小括号叫子表达式，\1 代表子表达式的值
  > 如果要是'aabb'呢 -> /(\w)\1(\w)\2/g
  > 如果括号嵌套，\1 表示最外面的括号，\2 才表示里面的第一个括号

```js
var reg = /(\w)\1(\w)\2/g;
var str = "aabb";
reg.exec(str); // ["aabb", "a", "b", index: 0, input: "aabb", groups: undefined]
```

8. 支持正则表达式的 String 对象的方法

- search() -> 检索与正则表达式相匹配的游标值。（位置，匹配不到返回 -1）
- match() -> 找到一个或多个正则表达式的匹配。
- replace() -> 替换与正则表达式匹配的子串。

```js
// 'aabb' -> 'bbaa'
var reg = /(\w)\1(\w)\2/g;
var str = "aabb";
console.log(str.replace(reg, "$2$2$1$1")); // 和下面返回结果一样
console.log(
  str.replace(reg, function ($, $1, $2) {
    // 三个参数分别表示正则表达式匹配的结果，第一个子表达式的内容，第二个子表达式的内容
    return $2 + $2 + $1 + $1;
  })
);
// 将 'the-first-name' -> 'theFirstName'
var reg = /-(\w)/g;
var str = "the-first-name";
console.log(
  str.replace(reg, function ($, $1) {
    return $1.toUpperCase();
  })
);

var str = "100000000000"; // -> '100,000,000' 的格式
var reg = /(?=\B(\d{3})+$)/g; // 从结尾开始找，每三个数之前的且不是单词边界的空
console.log(str.replace(reg, ","));
```

- split() -> 把字符串分割为字符串数组。
