# let和const命令

## let
let：声明的变量，只在let命令所在的代码块内有效。

for循环的计数器，就很合适使用let命令。

```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

上面代码中，变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变，而循环内被赋给数组a的函数内部的console.log(i)，里面的i指向的就是全局的i。也就是说，所有数组a的成员里面的i，指向的都是同一个i，导致运行时输出的是最后一轮的i的值，也就是 10。

如果使用let，声明的变量仅在块级作用域内有效，最后输出的是 6。

另外，for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

上面代码正确运行，输出了 3 次abc。这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。

#### let的特点
<strong>一、不存在变量提升</strong>

let声明的变量一定要在声明后使用，否则会报错。

<strong>暂时性死区</strong>

只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

```
typeof x; // ReferenceError
let x;

//如果一个变量根本没有被声明，使用typeof反而不会报错
typeof undeclared_variable // "undefined"
```

比较隐蔽的“死区”

上面代码中，调用bar函数之所以报错（某些实现可能不报错），是因为参数x默认值等于另一个参数y，而此时y还没有声明，属于“死区”。
```
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

```
    var x = x;
    console.log(x); //undefined
```

<strong>三、不允许重复声明</strong>

let不允许在相同作用域内，重复声明同一个变量。

<strong>块级作用域</strong>

注意！！！考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句

```
// 块级作用域内部的函数声明语句，建议不要使用
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 块级作用域内部，优先使用函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

还有一个需要注意的地方。ES6 的块级作用域必须有大括号，如果没有大括号，JavaScript 引擎就认为不存在块级作用域。

## const
const声明一个只读的常量。一旦声明，常量的值就不能改变。
一旦声明，必须马上初始化。

#### 特点

<strong>块级作用域</strong>
<strong>暂时性"死区"</strong>
<strong>不可重复声明</strong>

```
    const foo = {};

    // 给变量添加属性
    foo.prop = 123;
    foo.name = "变量A";
    
    console.log(foo);
    
    // 给他另外赋值就会报错
    // foo = {}
```

常量foo储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

冻结对象(也可以冻结属性)
```
    // 冻结对象
    const foo = Object.freeze({});

    // 常规模式时，下面一行不起作用；
    // 严格模式时，该行会报错
    foo.prop = 123;
```

#### ES6声明变量的6种方法
var

function

let

const

import

class


## 顶层对象的属性
顶层对象，在浏览器环境中，指的是window对象，
在node中指的是global对象。

顶层对象的属性与全局变量挂钩。顶层对象的属性是到处可以读写的，
不利于模块化编程。

ES6开始，var命令和function命令声明的变量是顶层对象的属性，
let命令，const命令，class命令声明的全局变量，不属于顶层对象的属性。

```$xslt
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

取顶层对象的方法

```$xslt
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

es2020中引入 globalthis作为顶层对象，任何环境都可以通过他拿到顶层对象




>https://es6.ruanyifeng.com/#docs/let








