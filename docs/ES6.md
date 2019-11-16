# 2019-10-1看完》》》CSS3

let 和 const 命令

## let 命令

```
[let声明变量不在window里](https://stackoverflow.com/questions/55030498/why-dont-const-and-let-statements-get-defined-on-the-window-object) 
const constVar = 'some string';
let letVar = 'some string';
var varVar = 'some string';

(function() {
  console.log(window.constVar); // prints undefined
  console.log(window.letVar); // prints undefined
  console.log(window.varVar); // prints 'some string'
})();
```

`for`循环的计数器，就很合适使用`let`命令。

```javascript
for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined
```

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。


### ES6 声明变量的六种方法

ES5 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，后面章节还会提到，另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法。

## 顶层对象的属性

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。

```javascript
window.a = 1;
a // 1

a = 2;
window.a // 2
```

上面代码中，顶层对象的属性赋值与全局变量的赋值，是同一件事。

顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，`window`对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。

ES6 为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。

```javascript
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

上面代码中，全局变量`a`由`var`命令声明，所以它是顶层对象的属性；全局变量`b`由`let`命令声明，所以它不是顶层对象的属性，返回`undefined`。

## globalThis 对象

JavaScript 语言存在一个顶层对象，它提供全局环境（即全局作用域），所有代码都是在这个环境中运行。但是，顶层对象在各种实现里面是不统一的。

- 浏览器里面，顶层对象是`window`，但 Node 和 Web Worker 没有`window`。
- 浏览器和 Web Worker 里面，`self`也指向顶层对象，但是 Node 没有`self`。
- Node 里面，顶层对象是`global`，但其他环境都不支持。

同一段代码为了能够在各种环境，都能取到顶层对象，现在一般是使用`this`变量，但是有局限性。

- 全局环境中，`this`会返回顶层对象。但是，Node 模块和 ES6 模块中，`this`返回的是当前模块。
- 函数里面的`this`，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this`会指向顶层对象。但是，严格模式下，这时`this`会返回`undefined`。
- 不管是严格模式，还是普通模式，`new Function('return this')()`，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全策略），那么`eval`、`new Function`这些方法都可能无法使用。

综上所述，很难找到一种方法，可以在所有情况下，都取到顶层对象。下面是两种勉强可以使用的方法。

```javascript
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

现在有一个[提案](https://github.com/tc39/proposal-global)，在语言标准的层面，引入`globalThis`作为顶层对象。也就是说，任何环境下，`globalThis`都是存在的，都可以从它拿到顶层对象，指向全局环境下的`this`。

垫片库[`global-this`](https://github.com/ungap/global-this)模拟了这个提案，可以在所有环境拿到`globalThis`。

扩展运算符取代`apply`方法的一个实际的例子，应用`Math.max`方法，简化求出一个数组最大元素的写法。

```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

**（1）复制数组**
扩展运算符提供了复制数组的简便写法。

```javascript
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```


**（2）合并数组**
```javascript
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

不过，这两种方法都是浅拷贝，使用的时候需要注意。

**（3）与解构赋值结合**
```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5
```
数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

## 数组实例的 entries()，keys() 和 values()

ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

如果不使用`for...of`循环，可以手动调用遍历器对象的`next`方法，进行遍历。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

`indexOf`方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于`-1`，表达起来不够直观。二是，它内部使用严格相等运算符（`===`）进行判断，这会导致对`NaN`的误判。

```javascript
[NaN].includes(NaN)
// true
```

- `filter()` 返回满足条件的新数组（没有满足条件，返回 [])
- `some()` 只要有满足条件的返回 true （否则 false)
- `every()` 所以元素满足条件则返回 true （否则 false)

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

```javascript
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

```
/*
async函数对 Generator 函数的改进
（1）内置执行器。
（2）更好的语义。
（3）更广的适用性。
（4）返回值是 Promise。
*/

async function f() {
    return 'hello world';
}
  
f().then(v => console.log(v))
// "hello world"

// 错误处理
// 统一放在try...catch结构中
async function main() {
    try {
      const val1 = await firstStep();
      const val2 = await secondStep(val1);
      const val3 = await thirdStep(val1, val2);
  
      console.log('Final: ', val3);
    }
    catch (err) {
      console.error(err);
    }
  }

// 建议一、把await命令放在try...catch代码块中。  
// 建议二、多个await命令后面的异步操作，建议同时触发  await Promise.all

// async 原理
async function fn(args) {
    // ...
  }
  
  // 等同于
  
  function fn(args) {
    return spawn(function* () {
      // ...
    });
  }
```

```javascript

// ES6 的类，完全可以看作构造函数的另一种写法。
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true



// class 继承  extends

class A {
    static hello() {
      console.log('hello world');
    }
  }
  
  class B extends A {
  }
  
  B.hello()  // hello world

```


## 柯里化

柯里化（currying）指的是将一个多参数的函数拆分成一系列函数，每个拆分后的函数都只接受一个参数（unary）。

```javascript
function add (a, b) {
  return a + b;
}

add(1, 1) // 2
```

上面代码中，函数`add`接受两个参数`a`和`b`。

柯里化就是将上面的函数拆分成两个函数，每个函数都只接受一个参数。

```javascript
function add (a) {
  return function (b) {
    return a + b;
  }
}
// 或者采用箭头函数写法
const add = x => y => x + y;

const f = add(1);
f(1) // 2
```

上面代码中，函数`add`只接受一个参数`a`，返回一个函数`f`。函数`f`也只接受一个参数`b`。


注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

```javascript
// 报错
function f(a, ...b, c) {
  // ...
}
```

ES6 允许使用“箭头”（`=>`）定义函数。

```javascript
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。


箭头函数有几个使用注意点。

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

上面四点中，第一点尤其值得注意。`this`对象的指向是可变的，但是在箭头函数中，它是固定的。



```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```
如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致`this`总是指向函数定义生效时所在的对象（本例是`{id: 42}`），所以输出的是`42`。



```javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}

```

上面代码中，`cat.jumps()`方法是一个箭头函数，这是错误的。调用`cat.jumps()`时，如果是普通函数，该方法内部的`this`指向`cat`；如果写成上面那样的箭头函数，使得`this`指向全局对象，因此不会得到预期结果。这是因为对象不构成单独的作用域，导致`jumps`箭头函数定义时的作用域就是全局作用域。

```
var x=11;
var obj={
  x:22,
  say:function(){
    console.log(this.x)
  }
}
obj.say();
//console.log输出的是22
```

```
var x=11;
var objFather={
  x:33,
  obj:{
    x:22,
    say:()=>{
    console.log(this.x);
    }
  }
}

objFather.obj.say();  // 11
```

对象嵌套没有作用域增加

```html
<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>
```

上面代码中，`<script>`标签打开`defer`或`async`属性，脚本就会异步加载。渲染引擎遇到这一行命令，就会开始下载外部脚本，但不会等它下载和执行，而是直接执行后面的命令。

`defer`与`async`的区别是：`defer`要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；`async`一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，`defer`是“渲染完再执行”，`async`是“下载完就执行”。另外，如果有多个`defer`脚本，会按照它们在页面出现的顺序加载，而多个`async`脚本是不能保证加载顺序的。

建议使用 `defer`

### 内部变量

ES6 模块应该是通用的，同一个模块不用修改，就可以用在浏览器环境和服务器环境。为了达到这个目标，Node 规定 ES6 模块之中不能使用 CommonJS 模块的特有的一些内部变量。

首先，就是`this`关键字。ES6 模块之中，顶层的`this`指向`undefined`；CommonJS 模块的顶层`this`指向当前模块，这是两者的一个重大差异。

其次，以下这些顶层变量在 ES6 模块之中都是不存在的。

- `arguments`
- `require`
- `module`
- `exports`
- `__filename`
- `__dirname`


`import`命令具有提升效果，会提升到整个模块的头部，首先执行。

```javascript
foo();

import { foo } from 'my_module';
```


`import()`加载模块成功以后，这个模块会作为一个对象，当作`then`方法的参数。因此，可以使用对象解构赋值的语法，获取输出接口。

```javascript
import('./myModule.js')
.then(({export1, export2}) => {
  // ...·
});
```

```javascript
Promise.all([
  import('./module1.js'),
  import('./module2.js'),
  import('./module3.js'),
])
.then(([module1, module2, module3]) => {
   ···
});
```

ES6 在`Number`对象上，新提供了`Number.isFinite()`和`Number.isNaN()`两个方法。

```javascript
Number.isFinite(15); // true
Number.isNaN('true' / 'true') // true
```
`Number.isFinite()`对于非数值一律返回`false`, `Number.isNaN()`只有对于`NaN`才返回`true`

ES6 将全局方法`parseInt()`和`parseFloat()`，移植到`Number`对象上面，行为完全保持不变。
```javascript
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

## Math 对象的扩展

`Math.trunc`方法用于去除一个数的小数部分，返回整数部分。
`Math.sign`方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。


```javascript
Math.trunc(4.9) // 4
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
```
与严格比较运算符（===）的行为基本一致。

```javascript
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
```

不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。

## Object.assign()
注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

`Object.assign`方法实行的是浅拷贝


### 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

**（1）for...in**

`for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

**（2）Object.keys(obj)**

`Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

**（3）Object.getOwnPropertyNames(obj)**

`Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

**（4）Object.getOwnPropertySymbols(obj)**

`Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名。

**（5）Reflect.ownKeys(obj)**

`Reflect.ownKeys`返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。

```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

上面代码中，`Reflect.ownKeys`方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性`2`和`10`，其次是字符串属性`b`和`a`，最后是 Symbol 属性。

### 解构赋值

对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```
注意，解构赋值的拷贝是浅拷贝

```javascript
// Promise构造函数接受 1个 函数作为参数
const promise = new Promise((resolve, reject) =>{

    if( /* 异步操作成功 */ 'success') {
        resolve(value);
    }else{
        reject(error);
    }
})
// Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数
// then方法可以接受两个回调函数作为参数, 第2个是可选参数
promise.then(function(value) {}, function(error) {});

// Promise 新建后就会立即执行。

// 用Promise对象实现的 Ajax 操作的例子

const getJSON = function(url) {
    const promise = new Promise(function(resolve, reject){
      const handler = function() {
        if (this.readyState !== 4) {
          return;
        }
        if (this.status === 200) {
          resolve(this.response);
        } else {
          reject(new Error(this.statusText));
        }
      };
      const client = new XMLHttpRequest();
      client.open("GET", url);
      client.onreadystatechange = handler;
      client.responseType = "json";
      client.setRequestHeader("Accept", "application/json");
      client.send();
  
    });
  
    return promise;
  };
  
  getJSON("/posts.json").then(function(json) {
    console.log('Contents: ' + json);
  }, function(error) {
    console.error('出错了', error);
  });


//   调用resolve或reject并不会终结 Promise 的参数函数的执行。
new Promise((resolve, reject) => {
    resolve(1);
    console.log(2);
}).then((r) => {
    console.log(r);
});
// 2
// 1

// 链式调用  第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
getJSON("/posts.json").then(function(json) {
    return json.post;
  }).then(function(post) {
    // ...
  });


// ****不建议在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），建议使用catch方法。
// Reject 和 catch方法都存在，谁在前面-先调用谁。

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // 处理前面所有Promise产生的错误
  });

const promise = new Promise(function(resolve, reject) {
    resolve('ok');
    throw new Error('test');  // 状态已经改变，不会执行
});
promise
.then(function(value) { console.log(value) })
.catch(function(error) { console.log(error) });  // throw 不执行，此处没有错误捕获
  // ok


  someAsyncThing().then(function() {
    return someOtherAsyncThing();
  }).catch(function(error) {
    console.log('oh no', error);
    // 下面一行会报错，因为y没有声明
    y + 2;
  }).catch(function(error) {
    console.log('carry on', error);
  });
  // oh no [ReferenceError: x is not defined]
  // carry on [ReferenceError: y is not defined]

  promise
  .then(result => {})
  .catch(error => {})
  .finally(() => {});


//   finally 实现
  Promise.prototype.finally = function (callback) {
    let P = this.constructor;
    return this.then(
      value  => P.resolve(callback()).then(() => value),
      reason => P.resolve(callback()).then(() => { throw reason })
    );
  };

//   Promise.all()
const p = Promise.all([p1, p2, p3]); // 都成功，则返回值组成一个数组返回。有一个失败，失败实例传给回调函数。

// 如果p2没有自己的catch方法，就会调用Promise.all()的catch方法
const p1 = new Promise((resolve, reject) => {
    resolve('hello');
  })
  .then(result => result);
  
  const p2 = new Promise((resolve, reject) => {
    throw new Error('报错了');
  })
  .then(result => result);
  
  Promise.all([p1, p2])
  .then(result => console.log(result))
  .catch(e => console.log(e));
  // Error: 报错了

//   Promise.race()  
const p = Promise.race([p1, p2, p3]);  // 一旦迭代器中的某个promise解决或拒绝，返回的 promise就会解决或拒绝。

// 如果 5 秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数。
const p = Promise.race([
    fetch('/resource-that-may-take-a-while'),
    new Promise(function (resolve, reject) {
      setTimeout(() => reject(new Error('request timeout')), 5000)
    })
  ]);
  
  p
  .then(console.log)
  .catch(console.error);


var promise1 = new Promise(function(resolve, reject) {
  setTimeout(resolve, 1000, 'one');
});

var promise2 = new Promise(function(resolve, reject) {
  setTimeout(resolve, 500, 'two');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
// Both resolve, but promise2 is faster
}).catch(function(error) {
console.log(error);
});
//'two'


// setTimeout 从第三个参数开始，为回调函数的执行参数
setTimeout(function (a,b){
    console.log(a,b);
},10,200,400);
//200 400
```


```javascript
/*
console.log(typeof NaN);  //number
console.log(typeof NAN);  //undefined
*/


/*
//forEach 和 map
let arr = [1,2,3,3,3];
let foreachArr = arr.forEach((x,index) => arr[index]=2*x);  // undefined   [2, 4, 6, 6, 6] arr的值被修改
let mapArr = arr.map((x,index) => arr[index]=2*x);  // [2, 4, 6, 6, 6]   [2, 4, 6, 6, 6]  arr的值被修改
console.log(arr, mapArr);
// forEach 无返回值 ， map有返回值（新数组占内存）

*/


// set和map的用法  1、去重。2、属性和方法
// set 不重复值集合
/*
const s = new Set();
[1,1,2,2,3,3].forEach(x=>s.add(x));   // add 是 set的方法
for(let i of s){
    console.log(i);
}
console.log(s, 's');

let arr = [1,2,3,3,3];
let str = 'abbbbc';
// 去除 数组、字符串 的重复成员（不会发生类型转换
let newArr = [...new Set(arr)];
let newStr = [...new Set(str)].join('');

console.log(newArr, newStr);

// 2、属性和方法  size  操作方法（4个，用于操作数据）和遍历方法（4个，

s.add(1) //添加某个值，返回 Set 结构本身
s.delete(1) //删除某个值，返回一个布尔值，表示删除是否成功。
s.has(1) //返回一个布尔值，表示该值是否为Set的成员。
s.clear() //清除所有成员，没有返回值。

s.add(1).add(2).add(2).add(3);
s.size // 2
s.has(1) // true
s.has(4) // false

s.delete(2);
s.has(2) // false

console.log(s, '遍历');
for (let item of s.keys()) {
    console.log(item);   // 1 3
}
for (let item of s.values()) {
    console.log(item);   // 1,3
}
for (let item of s.entries()) {
    console.log(item);   // [1, 1]   [3, 3]
}

s.forEach((index, item) => console.log(item));

数组的map和filter方法间接使用 Set
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}

// 并集（Union）、交集（Intersect）和差集（Difference）
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}

new Set([1,2,{},{},2,3,NaN,NaN])   //Set(5) {1, 2, {}, {}, 3, NaN}   空对象是不相等的，NaN是相等的

*/

// map 键值对集合  是和内存地址绑定的。
const map = new Map();
map.set('aa','aa');
map.get('aa');
console.log(map);

const m1 = new Map([['name','caiyi' ],['title','author']]);
// 实际上  上面等价下面
// 除了数组，只要有键值对的都可以作为map的参数
const items = [
    ['name', '张三'],
    ['title', 'Author']
  ];
  
const m = new Map();
items.forEach(([key, value]) => m.set(key,value));

console.log(m.size);
console.log(m.has('name'));
console.log(m.get('name'));


m.set(1, 'aaa').set(1, 'bbb');
m.get(1) // "bbb"    覆盖前一次的值

new Map().get('asfddfsasadf')
// undefined    未知的键 =》 undefined

const map2 = new Map();

map2.set(['a'], 555);
console.log(map2.get(['a'])); // undefined  内存地址是不一样的


// map属性和遍历方法
// size
// set
// get
// has
// delete  成功 true  失败 false
// clear  没有返回值

// 遍历和set差不多  类似数组和对象
// keys  
// values 
// entries
// forEach


// 同样 有weakMap 典型场合就是 DOM 节点作为键名。
let myElement = document.getElementById('logo');
let myWeakmap = new WeakMap();

myWeakmap.set(myElement, {timesClicked: 0});

myElement.addEventListener('click', function() {
  let logoData = myWeakmap.get(myElement);
  logoData.timesClicked++;
}, false);

```





