# JavaScript

## 1 JS 的数据类型有哪些？

**记忆题**

- 8 种，ES6 之前有 6 种，新增 2 种
- 三种基本类型、两个空值、对象、两个新增类型
- 三基：数字（number）、字符串（string）、布尔（boolean）
- 两空：null 和 undefined，通常约定用 null 表示空对象
- 对象（object）
- 新增:
  - 大整数（bigint），由于 JS 数字类型 number 是以双精度浮点数存储，当数字位数较多时无法支持有效位数整数的准确性，因此增加了 bigint（如： `12345678n` ）
  - 符号（symbol）
- **提了就零分的答案**：数组、函数、日期。这些是类，不是数据类型。

## 2 原型链是什么？

**大概念题**

- 先把大概念分割成小概念，抽象化为具体（举例）。

### 2.1 是什么？

#### 2.1.1 什么是原型？

1. 哦，原型链涉及到的概念挺多的，我举例说明一下吧。
2. 假设我们声明一个普通对象 `x = {}` ，这个 `x` 会有一个隐藏属性 `__proto__` ，这个属性会全等于 `Object.prototype` ，即

```javascript
// JavaScript
x.__proto__ === Object.prototype;
```

3. 此时，`x` 的隐藏属性全等于 `x` 构造函数的 `prototype` 属性，我们说 `x` 的原型是 `Object.prototype` 或者说 `Object.prototype` 是 `x` 的原型。

#### 2.1.2 什么是原型链？

1. 接下来，我来说原型链，还是举例说明吧。
2. 假设我们声明一个数组对象 `a = []` ，这个 `a` 会有一个隐藏属性 `__proto__` ，这个属性会全等于 `Array.prototype` ，即

```javascript
// JavaScript
a.__proto__ === Array.prototype;
```

3. 此时，`a` 的原型是 `Array.prototype` ，与 `x` 不同的是 `Array.prototype` 也有一个隐藏属性 `__proto__` ，这个属性会全等于 `Object.prototype`
4. 这样一来，`a` 就有两层原型：
   1. `a` 的原型是 `Array.prototype`
   2. `a` 的原型的原型是 `Object.prototype`
5. 形成了一个链条，这就是原型链

### 2.2 怎么做？

```javascript
// JavaScript
// 创建原型链
// 不推荐的写法
x.__proto__ = 原型;

// ES5的写法 让 x.__proto__ = 构造函数.prototype
const x = new 构造函数();

// ES6的写法
const x = Object.create(原型);
```

### 2.3 解决了什么问题？

- 在没有 `Class` 的情况下实现继承。

1. 以 `a` 的原型的原型是 `Object.prototype` 为例
2. `a` 是 `Array` 的实例，`a` 拥有 `Array.prototype` 里的属性
3. `Array` 继承了 `Object`
4. `a` 是 `Object` 的间接实例，`a` 拥有 `Object.prototype` 里的属性
5. 如此，`a` 既有 `Array.prototype` 的属性，又有 `Object.prototype` 的属性

### 2.4 优缺点及怎么解决缺点

- 优点：
  - 简单、优雅
- 缺点：
  - 与 `Class` 相比，不支持私有属性
  - 写法复杂
- 怎么解决缺点：
  - 使用 `Class`，井号开头声明私有属性（如： `#scope` ），但 `Class` 是 ES6 引入，不支持旧版 IE

## 3 这段代码中的 this 是什么？

- `this` 是 `call()` 的第一个输入参数
- `call()` 的第一个输入参数默认为 `undefined`
- 浏览器发现这个参数为 `undefined` 会将其修改为 `window`
- Node.js 发现这个参数为 `undefined` 会将其修改为 `global` 或保持 `undefined` （不同版本效果不同）
- 在函数体第一行添加 `use strict;` 开启严格模式，禁止宿主环境修改 `this`

```
f(p) ==> f.call( undefined, p)
o.c.f(p) ==> f.call( o.c, p)
```

```javascript
// JavaScript
var length = 4;

function callback() {
  console.log(this.length); // 问：打印出什么？
}

const obj = {
  length: 5,
  method(callback) {
    callback();
    // 等价于
    // callback.call(undefined)
  },
};

obj.method(callback, 1, 2);
```

```javascript
// JavaScript
const arr = [
  function () {
    console.log(this);
  },
  2,
];

arr[0]();
// 等价于
// arr.0.call(arr)
```

## 4 JS 的 new 做了什么？

**记忆题**

- 博客总结
- 参考文章 [JS 的 new 到底是干什么的？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/23987456)

1. 创建临时对象（新对象）
2. 绑定原型
3. 指定 this = 临时对象
4. 执行构造函数
5. 返回临时对象

## 5 JS 的立即执行函数是什么？

**概念题**

1. 是什么：声明一个匿名函数，然后立即执行它。这种做法就是立即执行函数。立即执行函数不是指这个函数，而是指这种做法。
2. 怎么做：

```javascript
// JavaScript
(function () {
  console.log("立即执行函数");
})();

void (function () {
  console.log("立即执行函数");
})();

var x = (function () {
  console.log("立即执行函数");
})();
```

3. 解决了什么问题：为了创建一个局部变量。在 ES6 之前，只能通过这种方法来创建局部作用域。
4. 优点：兼容性好，甚至支持 ES3。
5. 缺点：语法比较丑。
6. 怎么解决缺点：使用 ES6 的 let + 块语句。

```javascript
// JavaScript
{
  let a = 1;
  console.log(a);
}
```

## 6 JS 的闭包是什么？怎么用？

**概念题**

1. 是什么：
   1. 闭包是 JS 的一种语法特性。一种语言可以选择支持闭包或者不支持。 JS 的所有函数都支持闭包。（口述代码）
   2. 闭包 = 函数 + 自由变量
2. 怎么做：把一个函数和自由变量放到非全局环境中，就是闭包。

```javascript
// JavaScript
{
  let count;
  function add() {
    count += 1;
  }
}
```

```javascript
// JavaScript
var api = (function () {
  let lives = 3;
  return {
    getLives: () => lives,
    die: () => {
      lives -= 1;
    },
  };
})();
```

3. 解决了什么问题：
   1. 避免污染全局环境
   2. 提供对局部变量的间接访问
   3. 维持变量，使其不被垃圾回收
4. 优点：简单，好用。
5. 缺点：闭包使用不当可能造成内存泄露。
6. 怎么解决缺点：IE 不用、少用、慎用。

## 7 如何实现类？

- ES5 没有 class

### 7.1 方法一：使用原型（写 TS 会比较困难）

```javascript
// JavaScript
function Dog(name) {
  this.name = name;
  this.legsNumber = 4;
}

Dog.prototype.bark = function () {
  console.log(this.name);
  console.log(this.legsNumber);
};

var dogA = new Dog("a");
dogA.bark();
```

### 7.2 方法二：使用 class

```javascript
// JavaScript
class Dog {
  constructor(name) {
    this.name = name;
    this.legsNumber = 4;
  }

  bark() {
    console.log(this.name);
    console.log(this.legsNumber);
  }
}

const dogA = new Dog("a");
dogA.bark();
```

## 8 JS 如何实现继承？

### 8.1 方法一：使用原型链

```javascript
// JavaScript
function Animal(legsNumber) {
  this.legsNumber = legsNumber;
}

Animal.prototype.eat = function () {
  console.log("吃嘛嘛香！");
};

function Dog(name) {
  this.name = name;
  Animal.call(this, 4);
}

// 修改原型链
// 不能用这句，因为不同浏览器原型属性名可能不为 __proto__
// Dog.prototype.__proto__ = Animal.prototype;
// 这句也不行，因为 ES5 没有 Object.create()
// Dog.prototype = Object.create(Animal.prototype);
// 这句还不行，虽然用了 new 语句，但是有问题
// Dog.prototype = new Animal();
// new 语句会：
// 1. 创建临时对象
// 2. 绑定原型
// 3. 指定 this = 临时对象
// 4. 执行构造函数
// 5. 返回临时对象
// 我们不需要执行构造函数，因此需要创建一个中间函数充当空的构造函数
var EmptyAnimal = function () {};
EmptyAnimal.prototype = Animal.prototype;
Dog.prototype = new EmptyAnimal();

Dog.prototype.bark = function () {
  console.log(this.name);
  console.log(this.legsNumber);
};

var dogA = new Dog("a");
dogA.eat();
```

### 8.2 方法二：使用 class

```javascript
// JavaScript
class Animal {
  constructor(legsNumber) {
    this.legsNumber = legsNumber;
  }

  eat() {
    console.log("吃嘛嘛香！");
  }
}

class Dog extends Animal {
  constructor(name) {
    super(4);
    this.name = name;
  }

  bark() {
    console.log(this.name);
    console.log(this.legsNumber);
  }
}

const dogA = new Dog("a");
dog.eat();
```

## 9 手写节流、防抖

**记忆题**

- 博客总结
- 使用场景：按钮点击、用户拖拽

```javascript
// JavaScript
// 节流（技能冷却中）
const flash = (distance) => {
  console.log(`闪现${distance}`);
};

const throttle = (fn, time, ...args) => {
  let timer = null;

  return () => {
    if (timer) {
      return;
    }
    fn.call(undefined, ...args);
    timer = setTimeout(() => {
      clearTimeout(timer);
    }, time);
  };
};

throttle(flash, 2 * 1000, 999);
throttle(flash, 2 * 1000, 99);
throttle(flash, 2 * 1000, 9);
```

```javascript
// JavaScript
// 防抖（回城被打断）
const tp = () => {
  console.log("回城成功");
};

const debounce = (fn, time, ...args) => {
  let timer = null;

  return () => {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.call(undefined, ...args);
      clearTimeout(timer);
    }, time);
  };
};

debounce(tp, 3 * 1000);
```

## 10 手写发布订阅

**记忆题**

- 博客总结

```javascript
// JavaScript
const eventHub = {
  map: new Map(),
  // 监听事件
  on: (name, fn) => {
    // 入队
    eventHub.map.set(name, eventHub.map.get(name) || []);
    eventHub.map.get(name).push(fn);
  },
  // 取消监听事件
  off: (name, fn) => {
    // alias，别名，简写读操作
    const queue = eventHub.map.get(name);
    // 短路求值，当某个条件满足就不再执行后边逻辑，少写个if-else
    if (!queue) {
      return;
    }
    const index = queue.indexOf(fn);
    if (index < 0) {
      return;
    }
    queue.splice(index, 1);
  },
  // 触发监听事件
  emit: (name, data) => {
    const queue = eventHub.map.get(name);
    if (!queue) {
      return;
    }
    queue.forEach.call(undefined, (fn) => fn(data));
  },
};

eventHub.on("click", console.log);
eventHub.on("click", console.error);

setTimeout(() => {
  eventHub.emit("click", "hi");
}, 3000);
```

## 11 手写 AJAX

**记忆题**

- 博客总结

```javascript
// JavaScript
const request = new XMLHttpRequest();
request.open("GET", "/xxx");
request.onreadystatechange = () => {
  if (request.readyState === 4) {
    // 304 内容没有更改
    if (
      (request.status >= 200 && request.status < 300) ||
      request.status === 304
    ) {
      console.log("success");
    } else {
      console.log("fail");
    }
  }
};
// 请求体 GET 方法写了没用
request.send("{'name':'woozyzzz'}");
```

## 12 手写简化版 Promise

**记忆题**

- 博客总结
- Promise/A+ 规范

```javascript
// JavaScript
class Promise2 {
  #status = "pending";

  constructor(fn) {
    this.queue = [];

    const resolve = (data) => {
      this.#status = "fullfilled";
      const f1f2 = this.queue.shift();
      if (!f1f2) {
        return;
      }
      const [f1] = f1f2;
      if (!f1) {
        return;
      }
      const x = f1.call(undefined, data);
      if (x instanceof Promise2) {
        x.then(
          (data) => {
            // 调用下一个f1
            resolve(data);
          },
          (reason) => {
            // 调用下一个f2
            reject(reason);
          }
        );
      } else {
        // 调用下一个f1
        resolve(x);
      }
    };
    const reject = (reason) => {
      this.#status = "rejected";
      const f1f2 = this.queue.shift();
      if (!f1f2) {
        return;
      }
      const [_, f2] = f1f2;
      if (!f2) {
        return;
      }
      const x = f2.call(undefined, reason);
      if (x instanceof Promise2) {
        x.then(
          (data) => {
            // 调用下一个f1
            resolve(data);
          },
          (reason) => {
            // 调用下一个f2
            reject(reason);
          }
        );
      } else {
        // 调用下一个f1
        resolve(x);
      }
    };

    fn.call(undefined, resolve, reject);
  }

  then(fn1, fn2) {
    this.queue.push([fn1, fn2]);
    // 存疑
    // return new Promise2();
  }
}

const p = new Promise2(function (resolve, reject) {
  setTimeout(() => {
    // resolve("hi");
    reject("error");
  }, 3000);
});
p.then(
  (data) => {
    console.log(data);
  },
  (reason) => {
    console.error(reason);
  }
);
```

## 13 手写 Promise.all

**记忆题**

- 博客总结
- 要点
  1. 要在 Promise 上写而不是在原型上
  2. all 的参数（Promise 数组）和返回值（新的 Promise 对象）
  3. 用数组记录结果
  4. 只要有一个 reject 就整体 reject

```javascript
// JavaScript
Promise.all2 = function (list) {
  const results = [];
  let count = 0;
  return new Promise((resolve, reject) => {
    list.forEach((promise, index) => {
      promise.then(
        (data) => {
          results[index] = data;
          count += 1;
          if (count >= list.length) {
            resolve(results);
          }
        },
        (reason) => {
          reject(reason);
        }
      );
    });
  });
};
```

## 14 手写深拷贝

**记忆题**

- 博客总结

### 14.1 方法一：用 JSON

```javascript
// JavaScript
const data = JSON.parse(JSON.stringify(response));
```

- 要点是指出该方法的缺点：
  - 不支持 Date、正则、undefined、函数等数据
  - 不支持引用（环状结构）
  - 必须说出自己还会方法二

### 14.2 方法二：用递归

- 要点：
  - 递归
  - 判断类型 函数、数组、日期、正则
  - 检查环 `a.self = a`
  - 不拷贝原型上的数据

```javascript
// JavaScript
const deepClone = (a, cache) => {
  if (!cache) {
    cache = new Map();
  }
  const cache = new Map();
  // 不考虑 iframe\DOM等，如果考虑，那么 instanceof Object 不能判断 a 是对象
  if (a instanceof Object) {
    if (cache.has(a)) {
      return cache.get(a);
    }
    let result;
    if (a instanceof Function) {
      // 函数有 prototype 就不是箭头函数
      if (a.prototype) {
        result = function () {
          return a.apply(this, arguments);
        };
      } else {
        result = () => a.apply(undefined, arguments);
      }
    } else if (a instanceof Array) {
      result = [];
    } else if (a instanceof Date) {
      // 日期减0是时间戳
      result = new Date(a - 0);
    } else if (a instanceof RegExp) {
      result = new RegExp(a.source, a.flags);
    } else {
      result = {};
    }

    cache.set(a, result);

    for (let key in a) {
      // 继承的属性不需要深拷贝
      if (a.hasOwnProperty(key)) {
        result[key] = deepClone(a[key], cache);
      }
    }
    return result;
  } else {
    return a;
  }
};
```

## 15 手写数组去重

**记忆题**

- 博客总结

```javascript
// JavaScript
// 使用 Set，通常禁用
var a1 = [1, 2, 2, 3, 3, 3];

var uniq = function (arr) {
  return Array.from(new Set(arr));
  // return [...new Set(arr)];
};

uniq(a1);
```

```javascript
// JavaScript
// 计数排序变形
var a1 = [1, 2, 2, 3, 3, 3];

var uniq = function (arr) {
  var result = [];
  for (let i of a1) {
    if (result.includes(i)) {
      continue;
    }
    result.push(i);
  }
  return result;
};

uniq(a1);
```

```javascript
// JavaScript
// Map
var a1 = [1, 2, 2, 3, 3, 3];

var uniq = function (arr) {
  var map = new Map();
  for (let i = 0; i < a.length; i++) {
    let number = a[i];
    if (number === undefined) {
      return;
    }
    if (map.has(number)) {
      continue;
    }
    map.set(number, true);
  }
  return [...map.keys()];
};

uniq(a1);
```
