# javascript-spec

## 分号

**1**、 在

```
[
(
+
-
/
`
*
```

前面加分号

[JavaScript 语句后应该加分号么？](https://www.zhihu.com/question/20298345)


## 变量命名

不使用类型前缀命名

```js
/*
 * bad
 */
var eBtn = document.getElementById('btn') // element
var aQueue = ['1','2'] // array
var sName = 'nimo' // string
var iCount = 2 // integer
var oUser = {name: 'nimo'} // object
var flValue = 1.223123 // float
var reName = /nimo/ // regexp
var fnCall = function () {/*...*/} // function
```

**在必要时通过后缀标示或通过单词含义去表达类型**

> 让维护者通过单词就能知道类型是什么

```js
/*
 * array
 */
var id = 'a123'
var idArray = ['a123', 'b123']
// 尽量避免使用 var id= 'a123,b123' 需要获取数据时通过 idArray.join(',') 获取

// queue 已经能够表明这是一个数组
var queue = ['1', '2']

/*
 * element
 */
var btn = document.getElementById('btn') // btn 一般都是 element不需要做标示

/*
 * object
 */
var userData = {name: 'nimo'}
var actionDict = {'delete': '删除', 'edit': '编辑'}
var cacheMap = {}; cacheMap[url] = {/*...*/}

/*
 * number
 */
var count = 1 // count 肯定是数字类型
var age = 10 // iAge 是多此一举
var price = 10.5 // 可以不命名成 flPrice

/*
 * function
 */

// 看到动词就知道是个函数
var changeName = function(){}
var getData = function () {}
var setData = function () {}
```

## 注释

函数注释使用 JSDoc 风格，其他注释随意。

### 原则

只在必要的地方注释，尽量通过变量名和语句替代注释。例如：

```js
// bad
if (age < 18) {
    console.log('Close your eyes')
}
// good
var isKid = age < 18
if (isKid) {
    console.log('Close your eyes')
}
```

必要时直接通过文档的方式写出程序流程和设计方式

### @param

描述: 记录传递给一个函数的参数


#### 概述

`@param` 标签提供了对某个函数的参数的各项说明，包括参数名、参数数据类型、描述等。

`@param` 标签要求您指定要描述参数的名称。您还可以包含参数的数据类型，使用大括号括起来，和参数的描述。

参数类型可以是一个内置的JavaScript类型，如String或Object。

如果您提供的描述，在描述之前插入一个连字符，可以使注释更具可读性。请务必在连字符后加一个空格。

#### 例子

##### 名称, 类型, 和说明

下面的示例演示如何在 `@param` 标签中包含名称，类型，和说明。

只注释变量名称,例如：

```js
/**
 * @param somebody
 */
function sayHello(somebody) {
    alert('Hello ' + somebody);
}
```

注释变量名 和 变量类型 ,例如：

```js
/**
 * @param {string} somebody
 */
function sayHello(somebody) {
    alert('Hello ' + somebody);
}
```

注释变量名 、 变量类型 和 变量说明 ,例如：


```js
/**
 * @param {string} somebody - Somebody's name.
 */
function sayHello(somebody) {
    alert('Hello ' + somebody);
}
```

##### 变量是一个对象，带属性

如果参数是一个对象，有特定的属性，您可以通过 `@param` 标签提供额外的属性。例如，假如 `employee` 参数有 `name` 和 `department` 属性，您可以使用`.`标识属性的方式描述。


```jsx
/**
 * Assign the project to an employee.
 * @param {object} employee - The employee who is responsible for the project.
 * @param {string} employee.name - The name of the employee.
 * @param {string} employee.department - The employee's department.
 */
Project.prototype.assign = function(employee) {
    // ...
};
```

同样，可以联想到如果假如 `employee` 参数是一个数组，这个数组中包含 `name` 和 `department` 元素，那么可以这么描述。

```js
/**
 * Assign the project to a list of employees.
 * @param {object[]} employees - The employees who are responsible for the project.
 * @param {string} employees[].name - The name of an employee.
 * @param {string} employees[].department - The employee's department.
 */
Project.prototype.assign = function(employees) {
    // ...
};
```

##### 可选参数和默认值

下面的例子说明如何描述一个参数是可选的，并且具有默认值。

一个可选参数（ 使用Google Closure Compiler 语法）：

```js
/**
 * @param {string=} somebody - Somebody's name.
 */
function sayHello(somebody) {
    if (!somebody) {
        somebody = 'John Doe';
    }
    alert('Hello ' + somebody);
}
```

**不要将默认参数值写在注释中，因为这样要维护两份默认值。如果漏掉了任何一个，文档就是错误的**
```js
/** bad! bad! bad! bad! bad! bad! bad! bad! bad! bad! bad!
 * @param {string} [somebody=John Doe] - Somebody's name.
 * bad! bad! bad! bad! bad! bad! bad! bad! bad! bad! bad! */
```

##### 多个类型参数

```js
/**
 * @param {(string|string[])} [somebody] - Somebody's name, or an array of names.
 */
function sayHello(somebody) {
    if (!somebody) {
        somebody = 'John Doe';
    } else if (Array.isArray(somebody)) {
        somebody = somebody.join(', ');
    }
    alert('Hello ' + somebody);
}
```

##### 允许任何类型

```js
/**
 * @param {any} somebody - Whatever you want.
 */
function sayHello(somebody) {
    console.log('Hello ' + JSON.Stringify(somebody));
}
```


##### 可重复使用的参数

```js
/**
 * Returns the sum of all numbers passed to the function.
 * @param {...number} num - A positive or negative number.
 */
function sum(num) {
    var i = 0, n = arguments.length, t = 0;
    for (; i &lt; n; i++) {
        t += arguments[i];
    }
    return t;
}
```

##### 回调函数

如果参数接受一个回调函数，您可以使用 `@callback` 标签来定义一个回调类型，然后回调类型包含到 `@param` 标签中。

```js
/**
 * This callback type is called `requestCallback` and is displayed as a global symbol.
 *
 * @callback requestCallback
 * @param {number} responseCode
 * @param {string} responseMessage
 */

/**
 * Does something asynchronously and executes the callback on completion.
 * @param {requestCallback} cb - The callback that handles the response.
 */
function doSomethingAsynchronously(cb) {
    // code
};
```

### @returns

描述: 记录一个函数的返回值

`@returns` 标签描述一个函数的返回值。语法和 `@param` 类似。


```js
/**
 * Returns the sum of a and b
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
function sum(a, b) {
    return a + b;
}
```

```js
/**
 * Returns the sum of a and b
 * @param {number} a
 * @param {number} b
 * @returns {number} Sum of a and b
 */
function sum(a, b) {
    return a + b;
}
```

```js
/**
 * Returns the sum of a and b
 * @param {number} a
 * @param {number} b
 * @param {boolean} retArr If set to true, the function will return an array
 * @returns {number|array} Sum of a and b or an array that contains a, b and the sum of a and b.
 */
function sum(a, b, retArr) {
    if (retArr) {
        return [a, b, a + b];
    }
    return a + b;
}
```

### @example

描述: 提供一个如何使用描述项的例子。

提供一个如何使用描述项的例子。

```js
/**
 * Solves equations of the form a * x = b
 * @example
 * globalNS.method1(5, 10);
 * // 2
 * @example
 * globalNS.method(5, 15);
 * // 3
 * @returns {number} Returns the value of x for the equation.
 */
globalNS.method1 = function (a, b) {
    return b / a;
};
```
