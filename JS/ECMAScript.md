# 基本介绍

## 基本概念

- 浏览器分为两部分：渲染引擎和JS引擎

```bash
# 渲染引擎(浏览器内核)： chrome的blink
- 解析html和css

# js引擎： chrome的v8引擎
- js解释器，用来读取并处理网页的js代码，将js转换为二进制语言
- 逐行执行，如果某行报错，则下面不再执行
```

![image-20250412114558356](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250412114558356.png)

## 基本组成

```bash
# ECMAScript
- 规定了JS的编程语法和基础核心，是所有浏览器厂商共同遵循的一套JS语法工业标准

# DOM
- 文档对象模型, Document Object Model
- 通过DOM提供的接口可以对页面上的各种元素进行操作

# BOM
- 浏览器对象模型，Browser Object Model
- 与浏览器窗口进行互动的对象结构，如弹出框，跳转等
```

## 书写位置

- js的代码尽量写到文档末尾，因为html是从上到下加载

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--2，3在打开网页时就会执行-->
    <!--2. 内嵌样式-->
    <script>alert('内嵌')</script>

    <!--3. 外部引入-->
    <script src="erick.js"></script>
</head>
<body>

<!--1. 行内js-->
<button onclick="alert('行内')">+</button>

</body>
</html>
```

## 输入输出

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <script>
        /*可以向DOM中插入具体的标签*/
        document.write('hello');
        document.write('<h1>我是h1</h1>');

        <!--显示一个对话框，用来提示用户输入文字-->
        let value = prompt('输入东西到屏幕');

        console.log(value);

        alert('弹出东西到页面')
    </script>
</head>
<body>

</body>
</html>
```

# 数据类型

## 类型获取

```bash
# typeof
- 返回一个表示 变量原始类型 的字符串
- 检测基本数据类型（未声明变量、 undefined 、布尔、数字、字符串、 Symbol 、 BigInt ）和函数
```

```js
let address = 'erick';
/*变量的类型：string*/
console.log(typeof address);

function test() {
    console.log('hello');
}

/*function*/
console.log(typeof test);
```

## 常见类型

### number

```js
let age = 19;
let count = '12';
let name = 'erick';

// isNotANumber: 尝试将一个变量转换为number，返回值为boolean

// NAN和任何其他数字处理的结果，都是NAN
console.log(isNaN(age));   // false
console.log(isNaN(count))  // false
console.log(isNaN(name));  // true
console.log('eri' - 1);      // NAN
```

### string

- 模版字符串：变量拼接，自浮串格式

```js
// 可以包含变量
let age = 19
let name = 'erick'

console.log(`我是${name}, 今年${age}岁了`);

// 可以换行，保留格式
let str = `<ul>
<li>he</li>
<li>ha</li>
<li>j</li>
</ul>`

console.log(str)
```

### undefined

- 声明了变量，但是没有赋值，默认值就是undefined

```js
let address;
console.log('result',address);     // undefined
console.log(typeof address); // undefined
```

### null

- 声明了变量，但是赋值为null

```js
let address = null
console.log(address)          // null
console.log(typeof address)   // object，本身就是一个js的内置对象
```

### object

- 数组就是引用数据类型object

```js
let arr = [1,4,6,8]
console.log(typeof arr) // object
```

### Symbol

```js
let s = Symbol();

console.log(s, typeof s); // symbol对象

/*创建方式一：*/
let a1 = Symbol('erick');
let a2 = Symbol('erick');
console.log(a1 === a2); // false，两个属性值相同的标记

/*创建方式二：*/
let b1 = Symbol.for('erick');
let b2 = Symbol.for('erick');
console.log(b1 === b2);  // true

/*symbol不能与任何其他数据类型运算, 直接报错*/
console.log(b1 + 'erick');
```

## 数组

### 创建

```js
/*1. 创建数组*/
let arr01 = ['a', 'b', 'c']
let arr02 = [];
let arr03 = new Array(3);
```

### CRUD

```js
let arr01 = ['a', 'b', 'c']

arr01[1] = 'y';
arr01.push('d');                   // 尾插
arr01.unshift('z')           // 头插
let pop = arr01.pop();      // 尾删，返回删除的元素
let shift = arr01.shift();   // 头删， 返回删除的元素

arr01.splice(0,5);  // 起始索引，删除几个元素
```

### 遍历

```js
let arr = ['red', 'green', 'pink'];

/*for循环*/
for (let i = 0; i < arr.length; i++) {
    console.log(i);
    console.log(arr[i]);
}
```

```js
let arr = ['red', 'green', 'pink'];

/*foreach: 普通函数*/
arr.forEach(function (item, index) {
    console.log(item);
    console.log(index)
});

/*foreach：箭头函数*/
arr.forEach((item, index) => {
    console.log(item);
    console.log(index);
})
```

```js
// map

let arr = ['appLe', 'HAHA', 'HEHE'];
/*传递一个参数：就是当前元素内容*/
let newArr1 = arr.map((item) => {
    /*遍历并处理：不改变之前的数组，创建一个新的数组*/
    return item.toUpperCase();
});

/*参数一：当前元素内容
* 参数二：索引值*/
let newArr2 = arr.map((item, index) => {
    return item.toUpperCase() + index;
});
```

### 反转

```js
let arr = ['appLe', 'HAHA', 'HEHE'];
/*1. 反转数组，在原数组上操作*/
arr.reverse();
console.log(arr);
```

### 判断

```js
let arr = ['appLe', 'HAHA', 'HEHE'];
console.log(arr instanceof Array);
console.log(Array.isArray(arr));
```

### 拼接

```js
let arr = ['appLe', 'HAHA', 'HEHE'];

let str = arr.toString();
console.log(str); // appLe,HAHA,HEHE

let res = arr.join('-');
console.log(res); // appLe-HAHA-HEHE
```

### 删除指定索引

#### filter+箭头函数

```js
let arr = ['fruit', 'apple', 'lemon'];
let removed = 'apple';

let newArr = arr.filter((item) => {
    return item !== removed;
});

console.log(arr);    // 原数组不变
console.log(newArr); // 过滤后的数组
```

### 解构赋值

- 将数组的元素快速批量赋值给一些列变量的简洁语法

#### 基本使用

```js
let arr = [1, 4, 5, 7];
let [a, b, c, d] = arr; // 快速解构

console.log(`${a}, ${b}, ${c}, ${d}`);
```

#### 变量少

```js
/*变量少，数组元素多： 则从前向后解析*/
let arr = [1, 4, 5, 7,9];
let [a, b, c, d] = arr;

console.log(`${a}, ${b}, ${c}, ${d}`)
```

```js
/*利用剩余参数来解决： 真数组*/
let arr = [1, 4, 5, 7, 8];
let [a, b, ...other] = arr;

console.log(`${a}, ${b}, ${other}`)
```

#### 变量多

```js
/*变量多，数组元素少： 则后面的为undefined*/
let arr = [1, 4];
let [a, b, c, d] = arr;

console.log(`${a}, ${b}, ${c}, ${d}`)
```

```js
/*利用默认值，防止undefined*/
let arr = [1, 4];
let [a = -1, b = -1, c = -1, d = -1] = arr;

console.log(`${a}, ${b}, ${c},${d}`)
```

#### 占位

- 5不会被解析

```js
let arr = [1, 4, 5, 7];
let [a, b, , d] = arr; // 快速解构

console.log(`${a}, ${b}, ${d}`);
```

## 集合

### set

```js
let set = new Set();
set.add('a');
set.add('b');
set.add('c');
set.add('c');

set.delete('a');
set.has('b');
let keys = set.keys();

for (let v of set){
    console.log(v);
}
```

### map

```js
let map = new Map();
map.set('name', 'erick');
map.set({name: 'erick', age: 20}, 'people');

let name = map.get('name');

map.delete('name');

for (let v of map) {
    console.log(v); // k-v对
}
```



## 类型转换

### 转string

```js
let age = 10;

let age1 = age + '';
let age2 = String(age);
let age3 = age.toString();
```

### 转number

```js
let age1 = parseInt('18');        // 整数
let age2 = parseFloat('19.12');    // 浮点型
let age3 = parseInt('18px');
let age4 = Number(20);

console.log(parseInt('18.91')); // 只取正数部分
```

## 运算符

### ===/==

- ==单纯比较值是否相等
- ===是比较值和类型是否相等

### 展开运算符

```js
// 1. 展开一个数组 1 2 3
let arr1 = [1, 2, 3];
console.log(...arr1);

// 2. 合并两个数组
let arr2 = [4, 5, 6];
let arr3 = [7, 8, 9];
const num = [...arr2, ...arr3]; // 合并两个数组
console.log(...num);

// 3.深克隆对象
let person = {name: 'erick', age: 20};
let person_1 = {...person};
person_1.name = 'jack';
console.log(person_1, person);

// 4.克隆同时进行修改
let dog = {name: 'mimi', age: 2};
let dog2 = {...dog, name: 'xiaohua'};
console.log(dog, dog2);
```

## 作用域

```bash
# 作用域链
- 底层的变量查找机制
- 在函数被执行时，优先查找当前函数作用域中查找变量
- 如果当前查找不到，则依次逐级查找父级作用域直到全局作用域
- 子可访问父，父不可访问子
```

### 函数作用域

- 在函数内部声明的变量，只能在函数内部使用，不能在外部使用

```js
function say() {
    /*无法在函数外部访问*/
    let name = 'erick';
    console.log(name)
}

say();
console.log(name);
```

### 块级作用域

```js
{
    let name = 'erick';

    function say() {
        console.log(name);
    }

    say();
}

console.log(name);
```

### 全局作用域

- 写在当前js文件最外层的变量，就是当前js的全局作用域

## 闭包

- 内层函数加外层函数的变量，一起构成了闭包
- 并返回内层函数

```js
function outer() {
    /*外层函数的变量：提升作用域，不会被垃圾回收*/
    let address = '西安';

    /*内层函数：数据私有，外部无法直接修改address*/
    function inner() {
        address += '~';
        console.log(address);
    }

    return inner;
}

let inner = outer(); /*返回值为内层函数*/
inner();/*函数调用*/
```

## dubug

### 浏览器

- 在浏览器中对应的source中，找到对应的js文件，在对面里面添加断点
- 重新刷新页面，就会进入到断点里面

![image-20250412124241374](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250412124241374.png)

### webstorm

- 在IDEA中，添加对应的断点，debug模式启动项目

# 函数

## 具名函数

- 调用可以在函数声明前或者声明后

```js
function getName(id) {
    return 'erick' + id;
}

let result = getName('111');
console.log(result);
```

## 匿名函数

### 函数表达式

- 将匿名函数赋值给一个变量，然后通过变量名()来进行调用
- 只能先声明函数，再调用，不能反

```js
/*赋值*/
let getSum = function (a, b) {
    return a + b;
}

/*调用*/
let result = getSum(2, 4);
console.log(result);
```

### 立即执行函数

-  防止变量污染，但是不能获取返回值

```js
/*有参无返回值*/
(function (food) {
    console.log(`eat ${food}`);
})
('apple'); // 必须用; 分开，第二个()其实就是调用
```

## 箭头函数

- 是对函数表达式进行的更简短的函数写法
- 并且不绑定this
- 不能使用arguments动态参数

### 基本写法

```js
/*函数表达式*/
let first = function (a, b) {
    return a + b;
}
/*箭头函数*/
let firstHook = (a, b) => {
    return a + b;
}

console.log(first(10, 20));
console.log(firstHook(10, 20));
```

### 省略语法

- 只有一个形参，可以省略小括号
- 函数体只有一行代码，可以省略大括号
- 函数体只有一个return时，可省略return关键字

```js
let first = address => address.toUpperCase();

let result = first('shanxi');
console.log(result);
```

## 函数参数

### 默认值

- 可以给形参默认值，位置在声明时候靠后
- 一旦给了默认值，该参数就是可传可不传

```js
let check = (age = 10, address = '西安') => {
    console.log(`age: ${age} address ${address}`);
    return age + address;
};

check(20);
```

### 动态参数

- 伪数组
- js内置函数arguments，只存在于普通函数中，箭头函数没有

```js
function sum() {
    /* [Arguments] { '0': 2, '1': 'erick', '2': 6 } */
    console.log(arguments)
}

sum(2, 'erick', 6);
```

### 剩余参数

- 真数组，可以使用数组的api
- 在所有函数中都有
- 优先使用剩余参数，而不是动态参数

```ts
function sum(a, b, ...other) {
    
}

sum(2, 'erick', 6);
```

## this指向

- 对于函数中的this，谁调用该函数，谁就是this

### 普通函数

```js
console.log(this);     // window 调用该js

function say() {
    console.log(this);  // windows 调用该函数
}

say();

let people = {
    name: 'erick',

    work: function () {
        console.log(this) // people
    }
}

people.work();
```

### 箭头函数

- 箭头函数不会创建自己的this，它只会从自己的作用域链的上一层沿用this

```js
let say = () => {
    console.log(this); // window,   作用域链上一层，就是window
}
say()

let people = {
    name: 'erick',

    say: () => {
        // 函数内部没this，向上一层找对象的this
        console.log(this) // window
    }
}

people.say();

let nancy = {
    name: 'erick',

    say: function () {
        let a = 10;
        /*嵌套一个箭头函数*/
        let work = () => {
            console.log(this) // obj
        }
        work()
    }
}

nancy.say();
```

# 面向对象

## 基本语法

### 创建

```js
/*1. 创建对象*/
let people = {
    username: 'erick',
    password: '123456',
}

let dog = new Object({
    name: 'dog',
    age: 32,
});

console.log(typeof people); // object
console.log(typeof dog); // object
```

### 属性

```js
let people = {
    username: 'erick',
}

/*属性有，则是修改*/
people.username = 'lucy';
/*属性没有，则是新增*/
people.password = '123456';
people.address = 'xian';
/*删除属性*/
delete people.address;
```

- 如果某些属性不是驼峰

```js
let people = {
    username: 'erick',
    'first-name': 'John',
};

console.log(people.username);
console.log(people['first-name']);
console.log(people['username']);
```

- 对象属性遍历

```js
let people = {
    username: 'erick',
    password: '123456',
};

for (let key in people) {
    console.log(key, people[key]);
}
```

### 解构赋值

```js
let people = {
    'first-name': 'John',
    address: 'xian',
    country: 'China',
    other: {
        gender: 'Male',
        id: '19',
    }
}

/*1. 部分解构，
* 2. 多级解构
* 3. 解构别名*/
let {address, 'first-name': firstName, other: {id}} = people;
console.log(address, firstName, id);
```

### 属性/方法简写

```js
/*全写*/
let name = 'erick';
let people = {
    name: name,
    say: function () {
        console.log('hello');
    }
}

/*简写*/
let size = 'big';
let dog = {
    size,
    say() {
        console.log('hello');
    }
}

people.say();
dog.say();
```

## Object

```js
let people = {
    username: 'erick',
    password: '123456'
}
/*1. keys()：所有的属性名的数组*/
let keys = Object.keys(people);
console.log(keys);

/*2. values(): 所有属性值的数组*/
let values = Object.values(people);
console.log(values);

/*3. assign()：将一个类中的所有属性赋值过去*/
let lucy = {
    email: 'erick@gmail.com',
};
Object.assign(lucy, people);
console.log(lucy);

/*4. is(): 基本数据类型：比较值
*          引用数据类型：比较引用*/
let result = Object.is('tom','tom');
console.log(result);
```



## 构造函数

- 也是一种创建对象的方式

### 实例属性/方法

- 公共的属性和方法，可以封装在构造函数中
- 不同的对象new出来后，所指向的方法都是单独的内存，不能复用

```js
function People(username, password) {
    this.username = username;
    this.password = password;

    this.say = () => {
        console.log(`Hello ${this.username} ${this.password}`);
    }
}

let lucy = new People('lucy', '123456');

let tom = new People('tom', '654321');

console.log(typeof lucy); // object
console.log(typeof tom);
```

### 静态属性/方法

- 只能通过类名调用，不能通过实例调用

```js
function People(username) {
    this.username = username;
    this.say = () => {
        console.log(`Hello ${this.username}`);
    }
}

/*静态方法*/
People.work = function () {
    console.log('work');
}

/*静态属性*/
People.password = 'password';

let people = new People('test');

console.log(people.username);
/*只能通过构造函数名来调用*/
console.log(People.password);
```

## 原型

### 原型对象

- 每个构造函数，都有一个prototype属性，指向另一个对象，称prototype为原型对象

```bash
# 公共属性： 写到构造函数中
# 公共方法： 写到原型对象中
```

```js
function People(username, password) {
    this.username = username;
    this.password = password;
}

People.prototype.work = function () {
    console.log('work');
}

let erick = new People('erick', '12');
let lucy = new People('lucy', '12');
console.log(erick.work === lucy.work); // 原型对象上
```

- 构造函数和原型对象中的this，都指向实例化的对象

```js
let constructorThis;
let prototypeThis;

function People(username, password) {
    this.username = username;
    this.password = password;
    constructorThis = this;
}

People.prototype.work = function () {
    console.log('work');
    prototypeThis = this;
}

let people = new People('lucy', '123456');
people.work();

console.log(constructorThis === prototypeThis);
```

- 扩展js本身的库，自定义函数

```js
let arr = [1, 2, 3];

Array.prototype.say = function () {
    console.log('hi');
}

arr.say();
```

# Class语法

## 基本语法

```js
class People {
    /*1.构造方法：接受参数*/
    constructor(email, address) {
        this.email = email;
        this.address = address;
    }

    /*静态方法和属性：只能通过类调用*/
    static region = 'cn';

    static getAddress() {
        console.log('address');
    }

    /*2. 方法：放在类的原型对象上，供实例使用*/
    say() {
        // lucy实例调用say，this就是lucy
        this.work();
    }

    work() {
        console.log('work');
    }
}

let lucy = new People('lucy', 'xian');
let tom = new People('tom', 'beijing');

console.log(lucy.say === tom.say); /*不同实例的方法是指向同一个地方*/
```

## 继承

```js
class People {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    say() {
        console.log('people')
    }

    work() {
        console.log('people work hard')
    }
}

class Boy extends People {
    constructor(name, age, sex, address) {
        super(name, age); // 对父类的属性进行初始化
        this.sex = sex;
        this.address = address;
    }

    /*自定义方法*/
    fish() {
        console.log('boy fishing')
    }

    /*方法重写
    * 方法名相同：就会认为是重写*/
    say() {
        console.log('boy dancing')
    }
}
```

# 模块化

## 块级作用域

### js文件之间

- 默认js文件间变量和函数都只能当前文件访问

![image-20250413145205412](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250413145205412.png)

### js文件+html

- 如果不同的js中有相同的变量名，在html中引用过时，会出现重名冲突
- 谁先引进，就用谁的数据

![image-20250413145438510](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250413145438510.png)

## ES6模块化

### EXPORT

#### 分别暴露

```js
/*变量*/
export let address = 'beijing';

/*具名函数*/
export function say(name) {
    console.log(`Hello ${name}`);
}

/*箭头函数*/
export const work = (brand) => {
    console.log(`Hello ${brand}`);
}

/*对象*/
export let people = {
    username: 'erick',
    password: '123456'
}
```

#### 统一暴露

- 先定义好变量，方法，对象，最后统一暴露

```js
let age = 'beijing';

function check(name) {
    console.log(`Hello ${name}`);
}

const hardWork = (brand) => {
    console.log(`Hello ${brand}`);
}

let person = {
    username: 'erick',
    password: '123456'
}

export {age, person, check, hardWork};
```

#### 默认暴露

- 一个js只能使用一个default

- 

```js
// 对象
export default {
    address: 'xian',

    sleep() {
        console.log('go to sleep')
    }
}
```

```js
// 变量
let result = 'final';
export default result;
```

```js
// 具名函数
export default function eat(food) {
    console.log(food);
}
```

```js
// 匿名函数
export default function () {
    console.log('hello');
}
```

```js
// 箭头函数
export default (kind) => {
    console.log(kind);
}
```

#### 混合使用

```js
export let name = 'erick';

let age = 12;
export {age};

export default {
    address: 'xian',

    sleep() {
        console.log('go to sleep')
    }
}
```

### IMPORT

- 可以在对应的html中导入

#### 通用方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="module">
        import * as first from './js/demo01.js';

        console.log(first.address);
    </script>
</head>
<body>
</body>
</html>
```

#### 解构赋值

- 必须和导出的变量名和函数名一样

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="module">
        import {age, default as second, name} from './js/demo01.js';

        console.log(name);
        console.log(age);
        second.sleep();
    </script>
</head>
<body>
</body>
</html>
```

#### default

- 直接导入default的变量，其他的不行

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="module">
        /*只能用于default的*/
        import fourth from './js/demo01.js';

        fourth.sleep();
    </script>
</head>
<body>
</body>
</html>
```

### 集成app.js

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="module" src="js/app.js">

    </script>
</head>
<body>
</body>
</html>
```

```js
import {age, default as obj, name} from './demo01.js';

console.log(name);
console.log(age);
obj.sleep();
```

