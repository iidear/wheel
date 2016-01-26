# Javascript语言核心

## 数据类型。

js数据类型分为两类。

原始类型：number, string, boolean, null, undefined。

对象类型：对象是属性的集合，每个属性都由“名／值对”构成。js核心定义了对象类，数组类，函数类，日期类，正则类，错误类。

### 全局对象

全局对象的属性是全局定义的符号，js程序可以直接使用。当js解释器启动时（或者任何web浏览器加载新页面的时候），它将创建一个新的全局对象，并给它一组定义的初始属性：

- 全局属性，比如undefined, Infinity和NaN
- 全局函数，比如isNaN(), parseInt()和eval()
- 构造函数，比如Date(), RegExp(), String(), Object()和Array()
- 全局对象，比如Math和JSON

全局对象可通过下述方式获得：

```javascript
var global = (function(){
    return this || (1, eval)('this');
}());
```

### 数据类型检测

js是弱类型语言。检测数据类型是必要的。

主要通过typeof，Object.prototype.toString及其它hack方法。

#### number

```javascript
var isNumber = function (value) {
    return typeof value === 'number'; // 对NaN也返回true
}

// 改进
var isNumber = function (value) {
    return value === +value;
}
```

#### string

```javascript
var isString = function (value) {
    return typeof value === 'string';
}
```

#### boolean

```javascript
var isBoolean = function (value) {
    return typeof value === 'boolean';
}
```

#### undefined

```javascript
var isUndefined = function (value) {
    return typeof value === 'undefined';
}
```
#### object

```javascript
var isObject = function (value) {
    return value !== null && typeof value === 'object';
}

var isEmptyObject = function (obj) {
    var name;
    for (name in obj) {
        return false;
    }
    return true;
}
```

#### array

```javascript
var isArray = Array.isArray;

var isArraylike = function (obj) {
    var length = 'length' in obj && obj.length,
        type = typeof obj;

    if (type === 'function' || (obj != null && obj === obj.window)) {
        return false;
    }

    return type === 'array' || length === 0 ||
        typeof length === 'number' && length > 0 && (length - 1) in obj;
}
```

#### function

```javascript
var isFunction = function (obj) {
    return typeof obj === 'function';
}
```

#### date, regExp

```javascript
var isDate = function (obj) {
    return Object.prototype.toString.call(obj) === '[object Date]';
}

var isRegExp = function (obj) {
    return Object.prototype.toString.call(obj) === '[object RegExp]';
}
```


## 变量

在js中，变量使用关键字var声明。

### 变量作用域

js使用函数作用域：变量在声明它们的函数体以及这个函数体嵌套的任意函数体内都是有意义的。

声明提前（hoisting）：js函数里声明的所有变量（但不包括赋值操作）都被“提前”至函数体的顶部。

```javascript
function f() {
    console.log(x);
    var x = 'abc';
    console.log(x);
    var y = 'def';
}
```

等价于

```javascript
function f() {
    var x, y;
    console.log(x);
    x = 'abc';
    console.log(x);
    y = 'def';
}
```

### 作用域链

js是基于词法作用域的语言，也就是说函数在它被定义时的作用域中运行而不是在被执行时的作用域内运行。一个函数的作用域是它被定义的时候所处的"范围"，那么js里的函数作用域是在函数被定义的时候就确定了，所以它是静态的作用域，词法作用域又称为静态作用域。

作用域链：每一段js代码（全局代码或函数）都有一个与之关联的作用域链。

在定义一个函数时，会保存一个作用域链（外层函数的声明上下文对象链）。当调用这个函数时，它创建一个新的对象（声明上下文对象）来存储它的局部变量，并将这个对象添加至保存的那个作用域链上，同时创建一个新的更长的表示函数调用作用域的“链”。
