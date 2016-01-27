# Javascript语言核心－object

## 一、js对象

### 1. 什么是js对象？

**对象可以看作是属性的无序集合，每个属性都是一个名/值对。**

js对象除了有自身的属性，每个对象还拥有三个相关的对象特性：*原型（prototype）*，*类（class）*，*扩展标记（extensible flag）*。

1.原型（prototype）

对象的原型属性是用来继承属性的。

- 通过对象字面量创建的对象使用Object.prototype作为它们的原型。
- 通过new 创建的对象使用构造函数的prototype属性作为它们的原型。
- 通过Object.create()创建的对象使用第一个参数（可以为null）作为它们的原型。

要想检测一个对象是否是另一个对象的原型（或处于原型链中），使用isPrototypeOf()方法。

2.类（class）

对象的类属性是一个字符串，用以表示对象的类型信息。通过默认的toString()方法获得的字符串[object class]判断。

3.扩展标记（extensible flag）

对象的可扩展性用以表示是否可以给对象添加新属性。

- 通过Object.esExtensible()查询对象的可扩展性。
- 通过Object.preventExtensions()将对象转换为不可扩展的。（操作不可逆）

js对象分为三类：

- *内置对象*是有ECMAScript规范定义的对象或类。例如，数组、函数、日期和正则表达式。
- *宿主对象*是由js解释器所嵌入的宿主环境（通常为web浏览器）定义的。例如HTMLElement对象
- *自定义对象*是运行中的js代码创建的对象。


### 2. 对象的属性

**每个属性都是一个名/值对。**

js对象的每个属性还拥有一些与之相关的值，称为“属性特性”：

- 可写，表明是否可以设置该属性的值
- 可枚举，表明是否可以通过for/in循环返回该属性
- 可配置，表明是否可以删除或修改该属性。

对象的属性的基础操作：*创建*、*设置*、*查找*、*删除*、*检测*和*枚举*。

js对象的属性分为两类：

- *自有属性*是直接在对象中定义的属性。
- *继承属性*是在对象的原型对象中定义的属性。


## 二、创建对象的常用方法

### 1. 对象直接量

```javascript
var empty = {};
var obj = {x: 1, y: 2};
```

### 2. new + 构造函数

```javascript
var empty = new Object();
var obj = new Object({x: 1, y: 2});
```

### 3. Object.create()

ES5定义的方法，用来创建一个对象，第一个参数是这个对象的原型，第二个参数可选，用来对对象的属性进一步描述。

```javascript
var empty = Object.create(Object.prototype);
```

## 三、对象的属性

### 1.属性的常用操作

#### 1) 属性的查询和设置

可以通过点(.)或方括号([])运算符来获取属性的值。

```javascript
var obj = {};
obj.x = 1;    // 通过点(.)
obj['y'] = 2; // 通过方括号([])

// 当属性名为变量，或含空格等特殊字符时需要用数组写法（方括号）
obj['like this!'] = 3;

var name = "iidear";
obj[name] = 4;
```

#### 2) 属性的继承

1. 如果要查询对象o的属性x，如果o中不存在x，那么将会继续在o的原型对象中查询属性x，直至找到一个原型是null的对象为止。

2. 如果给对象o的属性x复制，如果o中已经有属性x，那么这个赋值操作只改变这个已有属性x的值。如果o中不存在属性x，
那么赋值操作给o添加一个新属性x（不管o的原型链中是否有属性x）。

3. 如果o继承来一个只读属性x，那么不允许给o的属性x赋值。

#### 3) 删除属性

delete 运算符可以删除对象的属性。它的操作数应当是一个属性访问表达式。

```javascript
delete obj.x;
delete obj['y'];
```

#### 4) 检测属性

判断某个属性是否存在于某个对象中。可以通过in运算符、hasOwnProperty()和propertyIsEnumerable()检测。

- in运算符，如果对象的自由属性或继承属性中包含这个属性则返回true。

- 对象的hasOwnProperty()方法用来检测给定的名字是否是对象的自由属性。对于继承属性会返回false。

- propertyIsEnumerable()只有检测到是自由属性且这个属性的可枚举性为true时它才返回true。

- 直接通过“!==”判断一个属性是否是undefined。

#### 5) 枚举属性

遍历对象的属性。

- for/in循环

```javascript
for (p in o) {
    if (!o.hasOwnProperty(p)) continue; // 跳过继承的属性
}
for (p in o) {
    if (typeof o[p] === 'function') continue; // 跳过方法
}
```

- ES5中，Object.keys()返回一个数组，这个数组由对象中可枚举的自有属性的名称组成。

- ES5中，Object.getOwnPropertyNames()返回对象的所有自有属性的名称。

#### 6) 存取器属性（访问器属性）getter和setter

在ES5中，属性值可以用一个或两个方法替代，即getter和setter。由getter和setter定义的属性称做“存取器属性”（accessor property）。

当程序查询存取器属性的值时，js调用getter方法。这个方法的返回值就是属性存取表达式的值。

当程序设置一个存取器属性的值时，js调用setter方法，将赋值表达式右侧的值当作参数传入setter。

```javascript
var obj = {
    _age: '',
    get age() { return this._age; },
    set age(value) { this._age = value; }
};
```

#### 7) 属性的特性

对于数据属性，它的4个特性为它的值（value）、可写性（writable）、可枚举性（enumerable）、可配置性（configurable）。

对于存取器属性，它不具有值特性和可写性，它的可写性是由setter方法是否存在决定。所以它的4个特性为读（set）、取（get）、可枚举行和可配置性。

```javascript
var obj = {};
// 添加一个不可枚举
Object.defineProperty(obj, 'x', {
    value: 1, writable: true, enumerable: false, configurable: true
})
```



## 四、序列化对象

序列化对象是指将对象的状态转换为字符串，也可将字符串还原为对象。

通过JSON.stringify()和JSON.parse()来序列化和还原js对象。

## 五、对象方法
toString()、toLocaleString()、valueOf()
