---
title: ES6 map reduce
date: 2018/2/24 17:03:00
tags:
- ES6 
- JavaScript
categories: JavaScript
---

### 题目一
------
不要使用JavaScript内置的parseInt()函数，利用map和reduce操作实现一个string2int()函数：

思路：把一个字符串13579先变成Array——[1, 3, 5, 7, 9]，再利用reduce()写出一个把字符串转换为Number的函数。<!-- more -->
```
'use strict'

function string2int(s) {
  return s.split('').map(function (x) {
    return x * 1
  }).reduce(function (x, y) {
    if (x === 0) {
      return 0
    } else {
      return x * 10 + y
    }
  })
}

// 测试:
if (string2int('0') === 0 && string2int('12345') === 12345 && string2int('12300') === 12300) {
  if (string2int.toString().indexOf('parseInt') !== -1) {
    console.log('请勿使用parseInt()!')
  } else if (string2int.toString().indexOf('Number') !== -1) {
    console.log('请勿使用Number()!')
  } else {
    console.log('测试通过!')
  }
} else {
  console.log('测试失败!')
}
```
### 题目二
------
请把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']。
思路：使用map遍历数组，将元素首字母大写，其余小写
```
'use strict'

function normalize(arr) {
  return arr.map(function (x) {
    return x.substring(0, 1).toUpperCase() + x.substring(1).toLowerCase()
  })
}

// 测试:
if (normalize(['adam', 'LISA', 'barT']).toString() === ['Adam', 'Lisa', 'Bart'].toString()) {
  console.log('测试通过!')
} else {
  console.log('测试失败!')
}
```

