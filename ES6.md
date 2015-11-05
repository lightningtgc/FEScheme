### 规范大纲

### 新特性列表


## 注意点

#### rest语法`...`如果不是原目标（或兼容）的格式，将会转为空。

```js
let aa = 9;
let bb = [...aa]; // []
let bb = {...aa}; // {}

let aa = { 'aa': 11 };
let bb = [...aa]; // []
let bb = {...aa}; // { "aa": 11 }

let aa = [3, 6, 9];
let bb = [...aa]; // [3, 6, 9]
let bb = {...aa}; // {"0":3, "1":6, "2":9} 转为以index为key的对象

```

### 资料

[ES6测试题](http://perfectionkills.com/javascript-quiz-es6/)
