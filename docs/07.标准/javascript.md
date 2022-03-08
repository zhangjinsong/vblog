---
title: javascript
date: 2022-02-19 01:58:30
permalink: /pages/d27974/
categories:
  - 标准
tags:
  - javascript
---
# JS 规范

## 命名

1. **变量**  
  命名方式：小驼峰  
  命名规范：前缀名词  
  命名建议：语义化

```javascript
// bad
let setCount = 10;
let getTitle = "loginTable";

// good
let maxCount = 10;
let tableTitle = "loginTable";
```

2. **常量**  
  常量全大写，用下划线连接

```javascript
const MAX_COUNT = 10;
```

3. **函数**  
  命名方式：小驼峰式命名法。  
  命名规范：前缀应当为动词。  
  命名建议：语义化

| 动词 | 含义                           | 返回值                                                  |
| ---- | ------------------------------ | ------------------------------------------------------- |
| can  | 判断是否可执行某个动作（权限） | 函数返回一个布尔值。 true: 可执行， false: 不可执行     |
| has  | 判断是否含有某个词             | 函数返回一个布尔值。 true: 含有此值， false: 不含有此值 |
| is   | 判断是否为某个词               | 函数返回一个布尔值。 true: 为此值， false: 不为此值     |
| get  | 获取某个词                     | 函数返回一个非布尔值                                    |
| set  | 设置某个词                     | 无返回值，返回是否设置或返回链式对象                    |
| load | 加载某些数据                   | 无返回值或返回是否加载完成的结果                        |

```javascript
// 是否可阅读
function canRead() {
  return true;
}
// 获取名称
function getName() {
  return this.name;
}
```

4. **类 / 构造函数**  
  命名方式：大驼峰式命名法（每个单词首字母大写），首字母大写  
  命名规范：前缀为名称。  
  命名建议：语义化

```javascript
class Person {
  public name: string;
  constructor(name) {
    this.name = name;
  }
}
const person = new Person('mevyn');
```

5. **类的成员**

- 类的成员包含：  
  - 公共属性和方法：跟变量和函数的命名一样。  
  - 私有属性和方法：前缀为\_(下划线)，后面跟公共属性和方法一样的命名方式。

```javascript
class Person {
  private _name: string;
  constructor() { }

  // 公共方法
  getName() {
    return this._name;
  }

  // 公共方法
  setName(name) {
    this._name = name;
  }
}
const person = new Person();
person.setName('mervyn');
person.getName(); // ->mervyn
```

- jquery 对象
  - jquery 对象必须以 `$` 开头命名

```javascript
let $body = $("body");
```

## 模块

- 模块应该以 ! 开始。这样确保了当一个不好的模块忘记包含最后的分号时，在合并代码到生产环境后不会产生错误。详细说明

- 文件应该以驼峰式命名，并放在同名的文件夹里，且与导出的名字一致。

- 增加一个名为 noConflict() 的方法来设置导出的模块为前一个版本并返回它。

- 永远在模块顶部声明 'use strict';。

```javascript
// fancyInput/fancyInput.js

!(function(global) {
  "use strict";

  var previousFancyInput = global.FancyInput;

  function FancyInput(options) {
    this.options = options || {};
  }

  FancyInput.noConflict = function noConflict() {
    global.FancyInput = previousFancyInput;
    return FancyInput;
  };

  global.FancyInput = FancyInput;
})(this);
```

- 构造函数 \* 给对象原型分配方法，而不是使用一个新对象覆盖原型。覆盖原型将导致继承出现问题：重设原型将覆盖原有原型！

```javascript
function Jedi() {
  console.log("new jedi");
}

// bad
Jedi.prototype = {
  fight: function fight() {
    console.log("fighting");
  },

  block: function block() {
    console.log("blocking");
  }
};

// good
Jedi.prototype.fight = function fight() {
  console.log("fighting");
};

Jedi.prototype.block = function block() {
  console.log("blocking");
};
```

方法可以返回 this 来实现方法链式使用。

```javascript
// bad
Jedi.prototype.jump = function jump() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function setHeight(height) {
  this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
Jedi.prototype.jump = function jump() {
  this.jumping = true;
  return this;
};

Jedi.prototype.setHeight = function setHeight(height) {
  this.height = height;
  return this;
};

var luke = new Jedi();

luke.jump().setHeight(20);
```

## 缩进

- 使用 `2` 个空格做为一个缩进层级，不允许使用 `4` 个空格 或 `tab` 字符

- `switch` 下的 `case` 和 `default` 必须增加一个缩进层级

```javascript
switch (variable) {
  case 1:
    break;
  default:
    break;
}
```

- 对象以缩进的形式书写，不要写在一行

```javascript
// better
let obj = {
  a: 1,
  b: 2
};

// bad
let obj = { a: 1, b: 2 };
```

## 分号

任何语句结尾都需要加分号 `;`

```javascript
do {
  x++;
} while (x < 10);
```

## 空格

- 二元运算符两侧必须有一个空格，一元运算符与操作对象之间不允许有空格

- 用作代码块起始的左花括号 `{` 前必须有一个空格

- `if / else / for / while / function / switch / do / try / catch / finally` 等关键字后，必须有一个空格

- 在非三目运算符中，`:` 之后必须有空格，之前不允许有空格

- `,` 和 `;` 之前不允许有空格，之后必须有空格

- 函数名和 `(` 之间不允许有空格

- 单行注释 `//` 后需要空格（若单行注释和代码同行，则 `//` 前也需要）

- 行尾不得有多余的空格

```javascript
let len = !arr.length;
if (len) {
  Demo(1, 2);
}

// 函数
function Demo(a, b) {
  let obj = {
    // 对象
    a: 1
  };
}
```

## 空行

- 代码块注释前与代码块后保留一个空行

```javascript
let a = 1;

// 注释
if (a == 2) {
  return;
}

a = 2;
```

## 换行

- 每个语句必须另起一行

- 变量赋值后需要换行

- 左大括号 `{` 后需要换行，右大括号 `}` 前需要换行

```javascript
let a,
  b,
  c = true;
if (c) {
  return;
}
```

## 注释

- 单行注释使用 `//`，多行注释使用 `/* */`

- 缩进与下一行代码保持一致

- 文档 / 接口注释使用以下写法

```javascript
/**
 * 文档描述
 * @author 作者
 * @date 创建时间
 * @update 更新者 更新时间
 */

/**
 * 接口描述
 * @param {String} title - 弹窗标题内容
 * @param {String} cancelBtnText = '默认值' - 取消按钮文本
 * @param {object} obj - 参数 obj 为一个对象
 * @param {String} obj.str - 参数 obj 的 str 属性
 */
function (title, cancelBtnText, obj) {
}
```

## 引号

最外层统一使用单引号 `''`

```javascript
let str = '<div id="test"></div>';
```

## 变量声明

- 变量在使用前必须通过 `var / let / const` 定义

- 不要使用未声明的变量，也不要先使用后声明

```javascript
let name = "MyName";
```

## 对象

对象属性名不需要加引号，有特殊字符除外

```javascript
let obj = {
  name: "test",
  age: 20,
  "max-weight": 60
};
```

## 大括号

`if / else / for / while / do / switch / try / catch` 等关键字后必须有大括号（即使代码块的内容只有一行）

```javascript
if (true) {
  return;
}
```

## undefined

- 永远不要直接使用 undefined 进行变量判断

- 使用 typeof 和字符串 `'undefined'` 对变量进行判断

```javascript
// good
if (typeof person === "undefined") {
  return;
}

// bad
if (person === undefined) {
  return;
}
```

## 其他

- 用 `===`, `!==` 代替 `==`, `!=`

- debugger 不要出现在生产环境的代码里

- 不要在循环内部声明函数

- 不允许有空的代码块

- 注释的大段代码 不需要的删除；如果需要请注释说明

