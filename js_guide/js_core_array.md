# js数组

## 一. 什么是js数组？

数组是值的有序集合。js数组是js对象的特殊形式，数组索引和碰巧是整数的属性名差不多，数组会有length属性。数组的实现经过了优化，使得其通过索引访问数组元素比访问对象属性要快很多。

### 1. 创建数组

```
// 字面量法
var empty = [];
var arr = [1, 2, 'a', {}];

// 通过构造函数
var empty = new Array();
var arr1 = new Array(10); // 创建一个length为10的数组
var arr2 = new Array(1, 2, 'a', {});
```

### 2. 数组元素的读写

使用[]操作符来访问数组中的一个元素。

```
var a = [], b;
a[1] = 'abc';
b = a[1]; // b = 'abc';
```

可以用负数或非整数来索引数组。此时会被当作常规的对象属性，length属性不变。
```
var a = [];
a['str'] = 'thats ok!'; // a.length === 0;
a['10'] = 'that will change length'; // a.length === 11;
```

## 二、数组方法

### 1. join()
```
Array.prototype.join = function (link) {
    var ret = '';
    link = typeof link === 'undefined' ? ',' : String(link);
    
    for (var i = 0, len = this.length - 1; i < len; i++) {
        ret += this[i];
        ret += link;
    };
    ret += this[this.length - 1];
    return ret;
};

var a = [1, 2, 3];
a.join(); // => "1,2,3"
a.join(''); // => "123"
a.join('-'); // => "1-2-3"
```

### 2. reverse()
```
var a = [1, 2, 3];
a.reverse(); // => a变为[3, 2, 1]
```

### 3. sort()
```
var a = [2, 1, 3];
a.sort(); // a变为 [1, 2, 3]
a.sort(function(a, b){
    return b - a;
}); // a变为[3, 2, 1]
```

### 4. concat()

### 5. slice()

### 6. splice()

### 7. push()和pop()

### 8. unshift()和shift()

### 9. toString()和toLocaleString()

### 10. forEach()

### 11.map()

### 12. filter()

### 13. every()和some()

### 14. reduce()和reduceRight()

### 15. indexOf()和lastIndexOf()
