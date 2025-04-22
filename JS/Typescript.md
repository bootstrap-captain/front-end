# 简介

- 微软开发，基于JS的一个扩展语言。包含了JS的所有内容，是JS的超集
- TS需要编译为JS，然后交给浏览器或者其他JS运行环境执行

```bash
1. 下载node.js
2. 安装typescript:    npm i -g typescript        # ts解析器

# 查看版本：5.8.3
tsc -v
tsc --version
```

- 在项目根目录下，执行tsc -init，就会生成一个tsconfig.json
- 进入项目目录，执行tsc或tsc -w(热部署)，就会编译所有的ts文件为js文件

![image-20250413125614015](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250413125614015.png)

```bash
{
  "compilerOptions": {
    /*模块化规范: commonJs， ES6*/
    "module": "ES6",
    /*编译后的js的版本*/
    "target": "ES6",
    /*"lib": []   ts中引入的js的内置的库，如DOM，在写ts代码时，会有提示
                  一般不写，会有默认值
                  一旦写上，就会替换 */

    /*编译后的 js 文件的存放位置, 会自动在当前文件夹创建dist文件夹*/
    "outDir": "./dist",
    /*是否编译js文件: 默认false, 涉及到文件的移动 文件从 src 移动到 dist中*/
    "allowJs": false,
    /*是否用ts的规范去检测js文件: 默认false*/
    "checkJs": false,
    /*是否在js文件中包含注释， 默认false*/
    "removeComments": false,
    /*当ts有语法错误时，不会生成js*/
    "noEmitOnError": true,
    /*是否开启js的严格模式： 严格模式性能更好， 会在生成的js文件中，带上 "use strict" 的头*/
    "alwaysStrict": true,
    /*不允许隐式的any类型，也就是隐式的不定义类型*/
    "noImplicitAny": true,
    /*严格检查空值*/
    "strictNullChecks": true,
    /*所有严格检查的总开关*/
    "strict": true
  },
  /*哪些文件不需要编译
   默认值：["node_modules", "bower_components","jspm_packages"]*/
  "exclude": [
  ],
  /*哪些文件需要实时编译
        两个*： 任意目录
        一个*： 任意文件
   */
  "include": [
    "./src/**/*"
  ]
}
```

# 基本语法

## 变量类型

### js类型

```bash
# JS中的内置构造函数： Number, String, Boolean等，用于创建对应对象的包装对象
- 日常开发中很少使用

# TS进行类型声明的时候，通常都是用小写的number，string， boolean
```

```ts
let a: number = 19;
let b: boolean = false;
let c: string = 'erick';
```

```ts
let str1: string;
let str2: String;

str1 = 'erick';

// 报错 str1 = new String('adf');

/*可以赋值为基本数据类型string和包装类型*/
str2 = 'afaf';
str2 = new String('adfa');
```

### any

- 关闭了ts的语法检查
- any的变量可以赋值给其他任何变量，哪怕转换失败也不会报错

```ts
/* 1. 显示声明为any的变量， 可以对该变量任何属性值*/
let a: any;
a = true;
a = 'nice';

/*2. any变量，可以给任何其他变量，会将其他变量也变成any
*    2.1 编译不报错
*    2.2 运行不报错
* */
let b = a;
b = true;
b = 'a';
```

### unknown

- 任意类型，但是是一个类型安全的any
- unknown 只能自己变化类型，不能赋值给任何其他显示声明的类型(编译错误)

```ts
let a: unknown;
a = 1;
a = 'haha';

/*2. unknown 赋值给其他显示声明的，会编译错误
* 哪怕上面的a，已经是string*/
let b: string;
//b = a;
```

- 如果要将unknown  赋值给其他的，可以通过下面三种方式

```ts
let a: unknown = 'erick';
let b: number;


if (typeof a === "number") {
    b = a;
}

/* 强制转换: 解决了编译问题，但是最终b='erick*/
b = a as number;

b = <number>a;
```

### never

- 任何值都不是，不能有值，undefined, null, '',0都不行
- 不会有返回值结果，用于专门来抛出异常的方法，不要去限制变量

```ts
// 不要去限制变量
let a: never;
// 编译错误 a = 'adf';

function say(flag: boolean): void {
    if (flag) {
        sayHi();
    } else {
        console.log('hi');
    }
}

/*异常的处理*/
function sayHi(): never {
    throw new Error("failure");
}

say(true);
```

### void

- 函数返回值

```ts
// 默认返回值：undefined
function fn01(): void {

}

function fn02(): void {
    return;
}

function fn03(): void {
    return undefined;
}
```

### 数组

- js中默认数组中元素可以存放不同类型数据

```ts
/*声明数组变量的两种方式*/
let arr1: string[] = [];
let arr2: Array<number> = [];

arr1.push("erick");
arr2.push(2);
// arr1.push(true); 报错
```

### tuple

- 元组:  固定长度的数组

```ts
let arr: [string, number, boolean?];
/*元组在赋值时候，个数和类型必须严格和声明时候一样*/
arr = ['df', 12];
console.log(arr);
```

### enum

- 数字枚举

```ts
/*定义一个枚举*/
enum Brand {
    APPLE,
    XIAOMI,
    HUAWEI,
    OPPO
}

/*1.赋值*/
let phone: Brand = Brand.APPLE;
console.log(phone); // 0
console.log(phone.valueOf()); // 0

let res: string = Brand[phone]; // APPLE
console.log(res);
```

- 字符串枚举

```ts
/*定义一个枚举*/
enum Brand {
    APPLE = 'iphone-15',
    XIAOMI = 'xiaomi-14',
    HUAWEI = 'huiwei-12',
    OPPO = 'oppo-10'
}

let phone: Brand = Brand.APPLE;
console.log(phone); // iphone-15
```

- 常量枚举
- 只需要在enum前加关键字const，生成的js代码会小很多，提升js运行时性能

```ts
/*定义一个枚举*/
const enum Brand {
    APPLE = 'iphone-15',
    XIAOMI = 'xiaomi-14',
    HUAWEI = 'huiwei-12',
    OPPO = 'oppo-10'
}

let phone: Brand = Brand.APPLE;
console.log(phone); // iphone-15
```

### 联合类型

```ts
/*将入参：限定为某几种参数*/
function say(info: string | number | boolean) {
    console.log(info)
}

say('hello');
say(1);
say(true);
```

### 字面量

```ts
/*定义一个枚举类型的字面量
*  1. 后续使用该值时，只能从这里定义的去选 */
let fruit: 'apple' | 'banana' | boolean;

fruit = 'banana';
console.log(fruit);

fruit = "banana";
console.log(fruit);

fruit = false;
console.log(fruit);

// fruit = "water";  就会编译报错
```

```ts
export type Student = {
    // 如果字段类型是枚举性质的，则可以
    address: 'beijing' | 'nanjing' | 'shenzhen',
}

let firstStudent: Student = {
    address: 'beijing',
}

let secondStudent: Student = {
    address: 'nanjing',
}

/*就会报错*/
/*
let third:Student = {
    address: 'adf',
}*/
```

### type

- 为一个类型起别名

```ts
/*1. 联合类型*/
type Fruit = string | number | boolean;

let a: Fruit;
a = 'daf';

/*2. 字面量类型*/
type Gender = 'boy' | 'female' | 'male';
let b: Gender;
b = 'female';
```

```ts
type Fruit = {
    name?: string;
    address: string;
}

let fruit: Fruit = {
    name: 'apple',
    address: 'xian'
}
```

### 对象

```ts
let person: {
    name: string,
    address: string,
    age?: number,/*可选属性*/
    [fieldName: string]: unknown;/*多余属性*/
}

person = {
    name: 'erick',
    address: 'beijing',
    email: 'erick@gmail.com',
}

console.log(person);
```

### 函数

```ts
/*定义一个函数的结构，定义函数参数类型，返回值类型*/
let say: (address: string, age: number) => string;

/*实际声明函数的实现*/
say = function (address: string, age: number) {
    return address + age;
}
```

## 高阶函数

- A函数，如果形参是函数类型，或者返回值是函数类型，则A就是高阶函数

### 自定义

```js
function check(address: string) {
    console.log(address);

    return function work(name: string) {
        console.log(`${address}, ${name}`);
    }
}

/*调用*/
check('北京')('erick');
```

### 柯里化

```js
/*因为调用参数时，不一定一下子都能拿到所有参数*/
function sum(a) {
    console.log(`a=${a}`);

    return (b) => {
        console.log(`b=${b}`)

        return (c) => {
            console.log(`c=${c}`)
            return a + b + c;
        }
    }
}

/*a=1
 b=3
 c=5
*/
sum(1)(3)(5);
```

# 面向对象

## 属性简写

```ts
class ErickService {
    name: string;
    address: String;

    public constructor(name: string, address: string) {
        this.name = name;
        this.address = address;
    }
}

let erickService = new ErickService('shuzhan', 'beijing');
console.log(erickService.address);
console.log(erickService.name);
```

```ts
class ErickService {
    /*简写形式：需要一定的修饰符号配合：这里的public，就是该类上对应属性的修饰符号*/
    public constructor(public name: string, public address: string) {
    }
}

let erickService = new ErickService('shuzhan', 'beijing');
console.log(erickService.address);
console.log(erickService.name);
```

## 修饰符

```bash
# public
- 类内部，子类，类外部访问

# protected
- 类内部，子类

# private 
- 类内部

# readyonly
- 只读属性，一旦初始化完成后，不能更改
```

```ts
class ErickService {
    public constructor(public name: string, public readonly address: string) {
    }
}

let erickService = new ErickService('shuzhan', 'beijing');
console.log(erickService.address);
console.log(erickService.name);
erickService.name = 'lisi';
// 编译错误
// erickService.address='127.0.0.1';
```

## Abstract

- 无法被实例化，定义类的结构和行为，可包含抽象方法和具体实现
- 为派生类提供一个基础结构，派生类必须实现其中的抽象方法
- 实现一些共有的方法

```ts
abstract class SearchService {
    /*1. 可以定义了一些公共属性*/
    protected constructor(public input: string, public output: string) {
    }

    public abstract callApi(): number;

    public search = () => {
        console.log('start');
        this.callApi();
        console.log('end');
    }
}

class HomeSearchService extends SearchService {
    /*初始化抽象类的变量
    * 1.建议加上 override， 表示是为了抽象类的构造期*/
    public constructor(public override input: string,
                       public override output: string,
                       public homeUrl: string) {
        super(input, output);
    }


    /*override: 可以不写，但是建议写*/
    override callApi(): number {
        console.log(this.homeUrl);
        return 1111;
    }
}

let homeSearchService = new HomeSearchService('in', 'out', '___');
homeSearchService.search();
```

## interface

- 接口只能定义格式，不能包含任何实现
- 接口多实现，接口可以继承一个接口

```ts
interface ISearchService {
    input: string,
    output: string

    callApi(): number;

}

class HomeSearchService implements ISearchService {
    /*上面定义好了对应的field*/
    public constructor(public input: string, public output: string,) {
    }

    callApi(): number {
        return 0;
    }
}

let homeSearchService: ISearchService = new HomeSearchService('in', 'out');
console.log(homeSearchService.input);
```

- 定义对应的的格式：比如返回的具体的数据

```ts
interface Result {
    input: string,
    output?: string
}
```

## 范型

### 作用范围

#### 方法

```ts
function say<T, R>(input: T, output: R): void {
    console.log(`${input} === ${output}`);
}

const check = <T, R>(input: T, output: R) => {
    console.log(`${input} === ${output}`);
}

say<string, string>('he', 'ha');
check<number, string>(12, 'he');
```

#### 类

```ts
class Erick<T, R, D> {
    say(input: T) {
        console.log(input);
    };

    check(first: R, second: D) {
        console.log(`${first} is ${second}`);
    }
}

let erick = new Erick<number, string, number>();
erick.say(123);
erick.check('he', 345);
```

### 类型限制

#### 默认类型

- 默认不给任何值，和T=any是一样的

```ts
function login<T>(cert: T): void {
    console.log(cert);
}

/*传递number*/
login<number>(123);

/*使用默认类型*/
login<string>('hello world!');
```

```ts
function login<T = any>(cert: T): void {
    console.log(cert);
}

/*传递number*/
login<number>(123);

/*使用默认类型*/
login<string>('hello world!');
```

#### 基类

- 该参数必须传递

```ts
interface BaseCert {
    username: string;
}

interface RSACert extends BaseCert {
    privateKey: string;
    publicKey: string;
}


function login<T extends BaseCert>(cert: T): void {
    console.log(cert);
}

const rsaCert: RSACert = {
    privateKey: '123',
    publicKey: '345',
    username: 'admin',
}

login(rsaCert);
```

#### 默认指定类型

- 给了一个默认类型，可以重新给其对应的类型

```ts
function login<T = string>(cert: T): void {
    console.log(cert);
}

/*传递number*/
login<number>(123);

/*使用默认类型*/
login('hello world!');
```



## 属性

### type

```ts
/*base entity*/
export type BaseUser = {
    id: string;
    email: string;
}

/*拓展： 类似继承*/
export type CustomerUser = BaseUser & {
    customerBrand: string;
}

const customer: CustomerUser = {
    id: '123',
    email: '23',
    customerBrand: '23'
}
```

### interface

```ts
/*base entity*/
export interface BaseUser {
    id: string;
    email: string;
}

/*拓展： 类似继承*/
export interface CustomerUser extends BaseUser {
    customerBrand: string;
}

const customer: CustomerUser = {
    id: '123',
    email: '23',
    customerBrand: '23'
}
```

# 常用技巧

## 非null断言

```ts
let a: string | null = null;

// ! : 跟在变量后面 非null断言
if (a!) {
    console.log(a);
}
```

