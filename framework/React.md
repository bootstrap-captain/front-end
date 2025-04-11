# 基本入门

## Vite-Cli

```bash
npm create vite@latest
# 依次输入  项目名， React, TypeScript+SWC
```

![image-20250410202623678](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250410202623678.png)

## 入口文件

### index.html

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8"/>
    <link rel="icon" type="image/svg+xml" href="/vite.svg"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Vite + React + TS</title>
</head>
<body>
<!--主容器：只会放一个组件-->
<div id="root"></div>
<!--引入main.tsx-->
<script type="module" src="/src/main.tsx"></script>
</body>
</html>
```

### main.tsx

- 入口文件，将APP组件渲染到页面

```js
import {StrictMode} from 'react'
import {createRoot} from 'react-dom/client'
import App from './App.tsx'

/*获取页面节点,并转换为虚拟DOM*/
createRoot(document.getElementById('root')!).render(
    /*将组件渲染到目标结点的页面上
   * 使用React严格语法，比如react过时的api*/
    <StrictMode>
        {/*引入App组件*/}
        <App/>
    </StrictMode>,
)
```

### App.tsx

- 导出APP组件
- 一般不会在这里直接写组件，而是利用App.tsx来统一导入其他的组件

```js
export default function App() {
    return (
        <div>Hello World</div>
    )
}
```

## 开发规范

```bash
# 具体的模块，建在src/components/xxx中
- tsx：React的模块组件代码              
- ts：ts功能代码 
- css：样式代码

# 导出的模块，可以在App.tsx中统一配置
```

### Food.tsx

```tsx
import {sayHello} from "./sayHello.ts";
import './food.css'

function Food() {
    return (
        <>
            <div className='header'>我是食物</div>
            <button onClick={sayHello}>点我</button>
        </>

    );
}

export default Food;
```

### food.css

```css
.header {
    height: 200px;
    width: 500px;
    background-color: gold;
}
```

### Food.ts

```ts
export function sayHello() {
    console.log("food");
}
```

### APP.tsx

```tsx
import Food from "./component/food/Food.tsx";

function App() {
    return (
        <>
            <Food/>
        </>
    )
}

export default App
```

## TSX语法

- React的语法，TSX最终也会被翻译为js

### 基本规则

```tsx
import {sayHello} from "./sayHello.ts";
import './food.css'
import Cat from "../cat/Cat.tsx";

function Food() {
    const name: string = 'erick';

    /*1. 虚拟DOM, 必须有一个根标签，且根标签内的标签必须闭合，因此用<div>包裹起来
    * 2. 绑定样式时，不要用class, 要使用className,避免和ES6中的class关键字引发冲突
    * 3. 使用内联样式时，用style={{key:value}}方式
    * 4. 使用js表达式时，使用{}来获取
    * 5. 标签首字母
                 如果是小写开头，比如<div>，则将该标签转换为html中同名元素。若html中无该标签同名元素，则报错
                 如果是大写开头，比如<Nike/>，则就去渲染对应的组件，若组件没定义，则报错*/
    return (
        <>
            <div className='header'>我是食物</div>
            <div style={{background: "blue"}}>我是第二个</div>
            <button onClick={sayHello}>点我</button>
            {name}
            <Cat/>
        </>
    );
}

export default Food;
```

### 数组/对象

```tsx
function Cat() {
    /*数组，React会自动遍历*/
    const cat: string[] = ['狸花', '美短', '大橘'];
    /*对象：报错： Objects are not valid as a React child */
    const people = {name: 'erick', age: 12};
    return (
        <>
            <div>{cat}</div>
            {/*<div>{people}</div>*/}
        </>

    );
}

export default Cat;
```

### js表达式

- jsx中，标签中内容需要引入js表达式，则需要用{}来使用

```bash
# js表达式： 可以用一个变量来接收的js代码
- 可以产生一个值，则可以放在{}中
         1. a
         2. a+b
         3. demo()                - 调用函数
         4. arr.map()
         5. function test(){}     - 定义一个函数
         
# js代码： 不产生值的
         1. if
         2. for
         3. switch
```

```jsx
function Cat() {
    const cat: string[] = ['狸花', '美短', '大橘'];
    return (
        /*li中必须指定key，从而让DOM在比较时可以使用，可以用index*/
        <div>
            {cat.map((item, index) => (
                <li key={index}>
                    {item}
                </li>
            ))}
        </div>

    );
}

export default Cat;
```

## Fragment

- tsx中，组件返回值都必须用一个闭合标签来包裹，因此多了很多不需要的div

### 闭合标签

```tsx
const Cat = () => {
    return (
        <div>
            <div>哈哈</div>
            <div>嘿嘿</div>
        </div>
    )
}

export default Cat;
```

### 空标签

- 空标签不能指定任何属性，会不在加对应的标签

```tsx
const Cat = () => {
    return (
        <>
            <div>哈哈</div>
            <div>嘿嘿</div>
        </>
    )
}

export default Cat;
```

### Fragment

- 如果不想嵌套不必要的div，则可以使用React提供的Fragment
- React在解析时，会自动把Fragment去掉
- 相比空标签，Fragment可以指定key，只能指定key，可以参与唯一标识的遍历

```tsx
import {Fragment} from "react/jsx-runtime";

const Cat = () => {
    return (
        <Fragment key={0}>
            <div>哈哈</div>
            <div>嘿嘿</div>
        </Fragment>
    )
}

export default Cat;
```

## 虚拟DOM

- 本质是Object类型的对象(一般对象)
- 虚拟DOM轻，真实DOM比较重。虚拟DOM是React内部使用，不需真实DOM那么多的属性
- 虚拟DOM最终会被React转换为真实DOM，呈现在页面上

```js
import {StrictMode} from 'react'
import {createRoot} from 'react-dom/client'
import App from './App.tsx'

const root = createRoot(document.getElementById('root')!);

root.render(
    <StrictMode>
        <App/>
    </StrictMode>,
);

console.log("Virtual DOM", root);
console.log("Virtual DOM Type", typeof root); // object

const realDom = document.getElementById('root');
console.log('Real Dom', realDom);
```

## 开发工具

- 在谷歌浏览器中加入，后面可以在Component中，查看每个组件的具体的属性，组件树之间的关系

![image-20250410215846410](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250410215846410.png)

![image-20250410215904086](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250410215904086.png)

# 函数组件

## 基本写法

### 普通函数

```tsx
/*  渲染组件到页面
   * 1. React解析函数组件标签，找到了Father组件，调用该函数
   * 2. 将返回的该组件的虚拟DOM转换为真实DOM,随后呈现在页面
   * 
   * a：必须要有返回值，值就是该组件的虚拟dom
   * b：首字母必须大写，否则就会当作html标签来渲染*/
export default function Father() {
    return (
        <div>Father</div>
    );
}
```

### 箭头函数

```tsx
export const Father = () => {
    return (
        <div>Father</div>
    );
};
```

## useState

- 当前组件的一些状态，属性
- 属性发生改变时，就会触发该函数的重新调用，导致组件重新render

```bash
# 属性改变： 浅比较，发生变化后，才会重新render当前组件
- 基本数据类型：比较值
- 引用数据类型：比较引用值
```

### 基本数据

```tsx
import {useState} from "react";

export default function Father() {
    console.log("Father Render");

    /*解构赋值：参数一：state的名字，参数二：对应的set方法*/
    const [name, setName] = useState<string>("erick");

    /*改变方式一：依赖原来数据*/
    const changeNameFirst = () => {
        setName((prevName: string) => {
            return prevName + '~'
        });
    }

    /*改变方式二：传入新值*/
    const changeNameSecond = () => {
        setName("lucy");
    }

    return (
        <div>
            <div>{name}</div>
            <button onClick={changeNameFirst}>改名一</button>
            <button onClick={changeNameSecond}>改名二</button>
        </div>
    );
}
```

### 引用数据

```tsx
import {BaseSyntheticEvent, useState} from "react";

export default function Father() {
    console.log("Father Render");
    const [student, setStudent] = useState({} as Student);

    const studentChange = (type: string) => {
        return (event: BaseSyntheticEvent) => {
            setStudent({
                /*原对象解构赋值*/
                ...student,
                /*新属性覆盖*/
                [type]: event.target.value
            })
        }
    }

    return (
        <div>
            {/*每次键盘事件后，页面都会重新渲染*/}
            <div>{student.name}=={student.email}=={student.phone}</div>
            姓名：<input onChange={studentChange('name')}/>
            邮箱：<input onChange={studentChange('email')}/>
            电话：<input onChange={studentChange('phone')}/>
        </div>
    );
}

export type Student = {
    name: string;
    email: string;
    phone: string;
}
```

### 更新方式

- 方法调用是同步的，更新操作是异步的

```bash
1. state更新操作按钮
2. state发生改变
3. 页面重新render
```

```tsx
import {useState} from "react";

export default function Father() {
    console.log("Father Render");

    const [count, setCount] = useState<number>(0);

    const incr = () => {
        setCount((preCount: number) => {
            return preCount + 1;
        });
        /*异步更新：原来是0，这里依然是0*/
        console.log(count);
    }
    
    return (
        <div>
            {/*进行到return时，已经异步更新完毕*/}
            <div>{count}</div>
            <button onClick={incr}>加一</button>
        </div>
    );
}
```

## props

- 父子组件之间通信，父组件向子组件传递属性，方法
- 属性状态和对应的方法：保存在父组件中，传递给子组件，允许子组件调用，从而修改属性

### 属性/方法

- 普通函数

```tsx
import {BaseSyntheticEvent} from "react";

/*属性定义，函数声明*/
type SonProps = {
    age: number
    username?: string,
    say: () => void;
    /*普通函数*/
    work: (city: string, year: number) => string;
    /*函数柯里化*/
    hardWork: (city: string, year: number) => (event: BaseSyntheticEvent) => void;
}

export default function Son(props: SonProps) {
    const {age, username, say, work, hardWork} = props
    return (
        <>
            <h2>{age} == {username}</h2>
            <button onClick={say}>无参-无返回值</button>
            <button onClick={() => {
                return work('北京', 2025)
            }}>有参-有返回值
            </button>
            <button onClick={hardWork('南京', 2024)}>柯里化</button>
        </>
    );
}
```

- 箭头函数

```tsx
import {BaseSyntheticEvent, FC} from "react";

interface SonProps {
    age: number;
    username?: string;
    say: () => void;
    work: (city: string, year: number) => string;
    hardWork: (city: string, year: number) => (event: BaseSyntheticEvent) => void;
}

export const Son: FC<SonProps> = (props) => {
    const {age, username, say, work, hardWork} = props;

    return (
        <>
            <h2>{age}==={username}</h2>
            <button onClick={say}>无参数</button>

            <button onClick={() => {
                return work('北京', 2025);
            }}>有参数普通写法
            </button>

            <button onClick={
                hardWork("南京", 2024)
            }>柯里化写法
            </button>
        </>
    );
}
```

```tsx
import {BaseSyntheticEvent, useState} from "react";
import Son from "./Son.tsx";

export default function Father() {
    console.log("Father Render");
    const [age, setAge] = useState<number>(0);
    const [username, setUsername] = useState<string>("lucy");

    const firstMethod = () => {
        setAge(age + 1);
        setUsername((prevState) => {
            return prevState + '~';
        })
        console.log("first method");
    }

    /*普通写法*/
    const secondMethod = (city: string, year: number): string => {
        console.log("second method", city, year);
        return `${city}, ${year}`;
    }

    /*函数柯里化写法*/
    const thirdMethod = (city: string, year: number) => {
        return (event: BaseSyntheticEvent) => {
            console.log(event)
            console.log("third method", city, year);
        }
    }

    return (
        <div>
            <div>
                <Son age={age} username={username} say={firstMethod} work={secondMethod} hardWork={thirdMethod}/>
            </div>
        </div>
    );
}
```

### children

- 组件标签中，嵌套的内容，默认不会渲染，这个数据会被封装到props中
- 可以嵌套html标签或者其他组件

```tsx
import Son from "./Son.tsx";
import Cat from "./Cat.tsx";

export default function Father() {
    return (
        <div>
            <div>
                {/*嵌套html*/}
                <Son><h2>你好</h2></Son>
                {/*嵌套组件*/}
                <Son><Cat/></Son>
            </div>
        </div>
    );
}
```

```tsx
import {ReactNode} from "react";

interface SonProps {
    /*组件插槽中的数据，html或者其他组件*/
    children: ReactNode;
}

export default function Son(props: SonProps) {
    const {children} = props;

    return (
        <>
            <div>子组件</div>
            {/*渲染位置*/}
            {children}
        </>
    );
}
```

### renderProps

- 子组件用到父组件，顶层组件的方法和属性

```tsx
interface SonProps {
    /*从父组件获取*/
    name: string;
    /*从顶层组件获取*/
    address: string;
}

export default function Son(props: SonProps) {
    const {name, address} = props;

    return (
        <>
            <div>子组件</div>
            <h2>{name}</h2>
            <h3>{address}</h3>
        </>
    );
}
```

```tsx
import {ReactNode, useState} from "react";

interface FatherProps {
    fatherRender: (address: string) => ReactNode;
}

export default function Father(props: FatherProps) {
    const [address] = useState<string>('北京');
    const {fatherRender} = props;

    return (
        <div>
            <div>父组件开始</div>
            {fatherRender(address)}
            <div>父组件结束</div>
        </div>
    );
}
```

```tsx
import Father from "./components/Father.tsx";
import {useState} from "react";
import Son from "./components/Son.tsx";

export default function App() {
    const [name] = useState<string>("lucy");

    return (
        <Father fatherRender={(address) => {
            return <Son name={name} address={address}/>
        }}/>
    )
}
```

## useRef

- 维护组件的属性，状态，ref变化不会导致页面的重复渲染，ref也有缓存，不会因为函数的重复调用而消失
- 如果这种属性不被页面渲染所需要，可以使用ref，比如查询条件

```tsx
import {useRef, useState} from "react";

export default function App() {
    const counter = useRef<number>(0);
    console.log('render App', counter.current);

    const [age, setAge] = useState<number>(18);

    return (
        <>
            <div>{age}</div>
            <button onClick={() => {
                setAge((prevState) => {
                    return prevState + 1;
                });
            }}>改变state
            </button>

            <button onClick={() => {
                counter.current += 1;
            }}>改变counter
            </button>
        </>
    )
}
```

# 性能优化

## re-render

```bash
# state： 浅比较
- 基本数据类型：值
- 引用数据类型：引用值
```

## 父子组件

### 默认情况

- 渲染父组件，遇到子组件就会渲染子组件，不管子组件有没有用到父组件的状态(DFS)
- 父组件一旦重新render，就会触发所有子组件的重新render

```tsx
import Son from "./Son.tsx";
import {useState} from "react";

export default function Father() {
    console.log('父组件render');
    const [name, setName] = useState<string>('shuzhan');

    return (
        <div>
            <div>{name}</div>
            <button onClick={() => setName((prevState) => {
                return prevState + '~';
            })}>改变姓名
            </button>
            <Son/>
        </div>
    );
}
```

```tsx
export default function Son() {
    console.log('子组件render')
    return (
        <div>子组件</div>
    );
}
```

### memo-函数组件

- 对函数组件的整体缓存
- 封装子组件，只有子组件存在props，并且props发生变化时，父组件的re-render才会触发子组件的re-render
- 浅比较

```tsx
import Son from "./Son.tsx";
import {useState} from "react";

export default function Father() {
    console.log('父组件render');
    const [name, setName] = useState<string>('shuzhan');

    return (
        <div>
            <div>{name}</div>
            <button onClick={() => setName((prevState) => {
                return prevState;
            })}>改变姓名
            </button>
            <Son name="shuzhan"/>
        </div>
    );
}
```

```tsx
import {memo} from "react";

type SonProps = {
    name: string,
}

/*memo进行整个函数组件的缓存*/
export default memo(function Son(props: SonProps) {
    console.log('子组件render', props);
    return (
        <div>子组件</div>
    );
});
```

## useMemo-函数调用

- 缓存函数计算结果
- 只要count或者age发生变化，页面就会重新渲染，就会每次都去调用handleCountChange函数
- handleCountChange只和count有关，希望只有count发生变化时，才去重新调用该函数

```bash
# 缺点：不要大量使用，
- 缓存值会占用内存，过度使用可能导致性能下降

# 场景
- 适用于计算成本高（如复杂运算、大数组处理）
- 需要稳定对象引用的场景，比如在上面memo函数中，比较props中的引用数据类型时候
```

```tsx
import {useMemo, useState} from "react";

export default function App() {
    const [count, setCount] = useState<number>(0);
    const [age, setAge] = useState<number>(10);

    /*参数一：回调函数
     * 参数二：监控项目
     * 1. 组件挂载完毕后，第一次执行
     * 2. count变化时，才去调用该函数
     * 3. age变化时候，对该函数结果执行了缓存，不会再次调用 */
    const finalCount = useMemo(() => {
        return handleCountChange(count);
    }, [count]);

    return (
        <>
            <div>
                <button onClick={() => setCount(count + 1)}>count加1</button>
                <button onClick={() => setAge(age + 1)}>age加1</button>
                <div>{finalCount}</div>
            </div>
        </>
    )
}

/*一个函数调用*/
function handleCountChange(count: number): number {
    console.log("executing count");
    return count * 10;
}
```

## useEffect

- 在组件的生命周期内，进行一些操作，整个过程，没有发生任何的用户操作事件

```bash
# 场景
- 发送ajax请求
- 手动更改真实DOM
- 设置订阅，启动定时器

# 可以把useEffect Hook看成如下三个函数的组合
- componentDidMount:  组件加载完毕后
- componentDidUpdate: 组件更新后
- componentWillMount: 组件卸载前
```

### 空监听state

- 依赖项数据为空数组：不监测任何属性，只会在组件整个渲染完毕后，执行一次

```tsx
import {useEffect, useState} from "react";

export default function App() {
    console.log('render');

    const [data, setData] = useState<string>('');

    /*1. 检测空数组
    * 2. 在hook中不修改state属性*/
    useEffect(() => {
        console.log("副作用hook");
    }, [])
    return (
        <>
            <div>{data}</div>
            <button onClick={() => {
                setData(previous => previous + '~')
            }}>修改数据
            </button>
        </>
    )
}
```

### 指定监听state

- 页面初始化渲染完毕后调用一次钩子，state改变时继续调用钩子

```tsx
import {useEffect, useState} from "react";

export default function App() {

    const [data, setData] = useState<number>(1);

    console.log('render', data);

    /*1. 检测state属性-data
    * 2. 在hook中修改state属性*/
    useEffect(() => {
        console.log("副作用hook");
    }, [data]);
    return (
        <>
            <div>{data}</div>
            <button onClick={() => {
                setData(previous => previous + 1)
            }}>修改数据
            </button>
        </>
    )
}
```

### 监听所有state

- 如果不指定监听的state，则默认会监听当前组件的所有state

```tsx
import {useEffect, useState} from "react";

export default function App() {

    const [data, setData] = useState<number>(1);

    console.log('render', data);

    /*1. 不指定监听的state*/
    useEffect(() => {
        console.log("副作用hook");
    });
    return (
        <>
            <div>{data}</div>
            <button onClick={() => {
                setData(previous => previous + 1)
            }}>修改数据
            </button>
        </>
    )
}
```

### 死循环

- 不要在副作用中，监听某个state，同时改变某个state，否则就会触发页面的无限渲染

```tsx
import {useEffect, useState} from "react";

export default function App() {

    const [data, setData] = useState<number>(1);

    console.log('render', data);

    /*1. 不指定监听的state*/
    useEffect(() => {
        setData((prev) => prev + 1);
    }, [data]);
    return (
        <>
            <div>{data}</div>
        </>
    )
}
```

# 受控组件

## 高阶函数

### 1. 定义

```bash
# 高阶函数：    如果一个函数满足下面两条任意一个
- A函数，接收的参数是一个函数，       那么A就是高阶函数
- A函数，调用的返回值依然是一个函数，  那么A就是高阶函数
       #    Promise  setTimeout, arr.map

# 函数柯里化
- 通过函数调用继续返回函数的方式，实现多次接受参数，最后统一处理的函数编码方式
```

### 2. 非柯里化

```ts
function sum(a, b, c) {
    return a + b + c;
}

let sum1 = sum(1, 3, 5);
console.log(sum1);
```

### 3. 柯里化

```ts
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

## 函数回调

- 如果在回调方法中，需要传递除了event之外的参数，可以考虑柯里化和非柯里化两种方式

### 1. 调用方式

```tsx
export default function App() {

    const work = (): string => {
        console.log("App work");
        return "Hello World!";
    }

    /*内容： 该函数的返回值*/

    /*1. 不带(), 函数不会执行，该变量的返回值就是一个函数*/
    console.log(work);
    /*2. 带()，该函数执行后，返回值：Hello World!*/
    console.log(work());

    return (
        <>

        </>
    )
}
```

```tsx
export default function App() {

    const work = (): string => {
        console.log("App work");
        return "Hello World!";
    }

    return (
        <>
            {/*1. 不带()，该变量的返回值是一个函数，一开始不会执行，事件触发后，会调用该函数*/}
            邮箱：<input onBlur={work} type="text" placeholder="邮箱"/>

            {/*2. 带(), 该变量的返回值是一个string, 一上来就会执行该方法，并将返回值作为回调函数*/}
            姓名：<input onBlur={work()} type="text" placeholder="姓名"/>
        </>
    )
}
```

### 2. 非柯里化

```tsx
import {BaseSyntheticEvent} from "react";

export default function App() {

    const work = (keyName: string, event: BaseSyntheticEvent) => {
        console.log(keyName);
        console.log(event.target.value);
    }


    return (
        <>
            {/*用函数包装，再去调用*/}
            邮箱：<input onBlur={(event: BaseSyntheticEvent) => {
            work('email', event);
        }} type="text" placeholder="邮箱"/>
        </>
    )
}
```

### 3. 柯里化

```tsx
import {BaseSyntheticEvent} from "react";

export default function App() {

    const work = (keyName: string) => {
        console.log(keyName);
        return (event: BaseSyntheticEvent) => {
            console.log(event.target.value);
        }
    }

    return (
        <>
            {/*用函数包装，再去调用
             1. 一上来就调用一次
             2. 后续每次点击，就会调用其内层函数
             3. dataType可以自己传递，但是event是React帮忙传递的*/}
            邮箱：<input onBlur={work('email')} type="text" placeholder="邮箱"/>
        </>
    )
}
```

## 非受控组件

- 收集表单中的数据：页面内所有输入类的DOM，现用现取

```tsx
import {BaseSyntheticEvent, useRef} from "react";

export default function App() {

    const nameRef = useRef<HTMLInputElement>({} as HTMLInputElement);
    const addressRef = useRef<HTMLInputElement>({} as HTMLInputElement);
    const emailRef = useRef<HTMLInputElement>({} as HTMLInputElement);

    /*用的时候再去取*/
    const savePeople = (event: BaseSyntheticEvent) => {
        console.log(event);
        event.preventDefault();
        alert(nameRef.current.value + addressRef.current.value + emailRef.current.value);
    }

    return (
        <div>
            <form action={""} onSubmit={savePeople}>
                用户名：<input type={"text"} ref={nameRef}/>
                地址：<input type={"text"} ref={addressRef}/>
                邮箱：<input type={"text"} ref={emailRef}/>
                <button>提交</button>
            </form>
        </div>
    )
}
```

##  受控组件

- 每次改变输入框的值，就将其值放入state中

```tsx
import {BaseSyntheticEvent, useState} from "react";

export interface People {
    name: string;
    address: string;
    email: string;
}

export default function App() {
    console.log('render');

    const [people, setPeople] = useState<People>({} as People);

    const savePeople = (keyName: string) => {
        return (event: BaseSyntheticEvent) => {
            setPeople({
                ...people,
                [keyName]: event.target.value,
            })
        }
    }

    const log = (event: BaseSyntheticEvent) => {
        event.preventDefault();
        console.log(people);
    }

    return (
        <div>
            <form action={""} onSubmit={log}>
                用户名：<input type={"text"} onBlur={savePeople('name')}/>
                地址：<input type={"text"} onBlur={savePeople('address')}/>
                邮箱：<input type={"text"} onBlur={savePeople('email')}/>
                <button>提交</button>
            </form>
        </div>
    )
}
```

# React-Router

```bash
# SPA: Single Page Application: 单页面Web应用
- 整个页面只有一个完整的页面
- 点击页面中的链接，不会刷新页面，只会做页面的局部更新
- 数据都需要通过Ajax请求获取，并在前端异步呈现
- 单页面，多组件，通过点击，路由跳转到指定组件
```

## 基本介绍

- react的一个插件库，专门用来实现一个SPA应用，基于React的项目基本都会用到该插件
- [官网](https://reactrouter.com/home)

```bash
# react-router-dom
- 为web打造

# react-router-native
- 为react原生应用开发的

# react-router-any
- 全部都能用，通用型强，api能复杂点
```

```bash
npm i react-router-dom@7.5.0
```

## 路由规则

- 只能在url中进行跳转，没有页面上的link可以点
- 匹配成功后，不再向下遍历匹配

### serviceRouter.tsx

- element：都是路由组件，懒加载，只有点击触发了某个路由，才会去调用对应的函数式路由组件

```tsx
import {createBrowserRouter, createRoutesFromElements, Route} from "react-router-dom";
import Welcome from "../pages/welcome/Welcome.tsx";
import ErickError from "../pages/error/ErickError.tsx";
import Home from "../pages/home/Home.tsx";
import Good from "../pages/good/Good.tsx";
import Cart from "../pages/cart/Cart.tsx";
import Admin from "../pages/admin/Admin.tsx";
import Customer from "../pages/admin/customer/Customer.tsx";
import Guest from "../pages/admin/guest/Guest.tsx";

export const router = createBrowserRouter(createRoutesFromElements(
    /*顶层路由: 需要在Welcome中设置Outlet
      element: 配置的组件都是懒加载的*/
    <Route path={'/'} element={<Welcome/>} errorElement={<ErickError/>}>
        {/*二级路由*/}
        <Route path={'home'} element={<Home/>}/>
        <Route path={'good'} element={<Good/>}/>
        <Route path={'cart'} element={<Cart/>}/>
        {/*三级路由: 需要在Admin中设置Outlet*/}
        <Route path={'admin'} element={<Admin/>}>
            <Route path={'customer'} element={<Customer/>}/>
            <Route path={'guest'} element={<Guest/>}/>
        </Route>
    </Route>
));
```

### App.tsx

```tsx
import {RouterProvider} from "react-router-dom";
import {router} from "./routes/serviceRouter.tsx";

export default function App() {
    return (
        <RouterProvider router={router}></RouterProvider>
    )
}
```

### Welcome.tsx

```tsx
import {Outlet} from "react-router-dom";

export default function Welcome() {
    return (
        <>
            <div>我是欢迎页面</div>
            {/*声明下一级别的路由的渲染的地方*/}
            <Outlet/>
        </>
    );
}
```

![image-20250411161553671](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250411161553671.png)

## 页面点击

- 路由页面中，定义一些页面元素链接
- 可以定义在父路由页面中，可以识别到当前的路由，子路由，父路由
- 点击后，浏览器url进行跳转，从而触发之前定义的路由规则，装填不同的组件

### Welcome.tsx

```tsx
import {createBrowserRouter, createRoutesFromElements, Route} from "react-router-dom";
import Welcome from "../pages/welcome/Welcome.tsx";
import ErickError from "../pages/error/ErickError.tsx";
import Home from "../pages/home/Home.tsx";
import Good from "../pages/good/Good.tsx";
import Cart from "../pages/cart/Cart.tsx";
import Admin from "../pages/admin/Admin.tsx";
import Customer from "../pages/admin/customer/Customer.tsx";
import Guest from "../pages/admin/guest/Guest.tsx";

export const router = createBrowserRouter(createRoutesFromElements(
    /*顶层路由: 需要在Welcome中设置Outlet*/
    <Route path={'/'} element={<Welcome/>} errorElement={<ErickError/>}>
        {/*二级路由*/}
        <Route path={'home'} element={<Home/>}/>
        <Route path={'good'} element={<Good/>}/>
        <Route path={'cart'} element={<Cart/>}/>
        {/*三级路由: 需要在Admin中设置Outlet*/}
        <Route path={'admin'} element={<Admin/>}>
            <Route path={'customer'} element={<Customer/>}/>
            <Route path={'guest'} element={<Guest/>}/>
        </Route>
    </Route>
));
```

### Admin.tsx

```tsx
import {NavLink, Outlet} from "react-router-dom";

export default function Admin() {
    return (
        <>
            <div>用户管理</div>
            {/*全路径*/}
            <NavLink to={'/admin/customer'}>普通用户</NavLink><br/>
            {/*子路由名字*/}
            <NavLink to={'guest'}>游客模式</NavLink><br/>
            <Outlet/>
        </>
    );
}
```

## 编程式路由

- 当触发某个函数的时候，在ts的代码中，让浏览器中的url发生变化，从而再根据路由规则显示某些组件

```tsx
import {NavigateFunction, useNavigate} from "react-router-dom";

export default function Guest() {

    /*钩子函数*/
    const navigate: NavigateFunction = useNavigate();

    const handleGoToCart = () => {
        /*全路径*/
        navigate('/cart', {
            replace: false,
            state: {}
        });
    }

    return (
        <>
            <div>游客模式</div>
            <button onClick={handleGoToCart}>回到Cart</button>
        </>

    );
}
```

## 跳转模式

### push

- 默认方式

```bash
# 默认push，使用压栈操作，在浏览器页面
- 回退或者前进，可以进行

http://localhost:3000/home             # 3
http://localhost:3000/about/detail     # 2     # 当前元素
http://localhost:3000/about            # 1
```

### replace

```bash
# 在Navlink中开启 
 <NavLink replace={true}
 
# 替换操作， 如果下一个是替换模式，则会替换掉栈顶元素
http://localhost:3000/about/detail     # 2     
http://localhost:3000/about            # 1
```

## 传参

- 从一个路由跳转到其他路由时，可以携带数据传递到目标路由
- params参数，search参数，state参数

### 路由规则

- 在浏览器中直接输入对应的链接，即可
- 必须匹配好，不然后面的页面中的点击就会不生效

```tsx
import {createBrowserRouter, createRoutesFromElements, Route} from "react-router-dom";
import Welcome from "../pages/welcome/Welcome.tsx";
import ErickError from "../pages/error/ErickError.tsx";
import Home from "../pages/home/Home.tsx";
import Good from "../pages/good/Good.tsx";
import Cart from "../pages/cart/Cart.tsx";

export const router = createBrowserRouter(createRoutesFromElements(
    <Route path={'/'} element={<Welcome/>} errorElement={<ErickError/>}>

        {/*1. 既可以传参数，也可以不传参
           2. 如果没有外层的包裹，则只能使用带参数的url访问
           */}

        {/*1. params传递参数：
        页面链接： http://localhost:5173/home/123/erick_params*/}
        <Route path={'home'} element={<Home/>}>
            <Route path={':id/:type'} element={<Home/>}/>
        </Route>

        {/*2. search传递参数
        页面链接: http://localhost:5173/good?id=999&type=erick_search*/}
        <Route path={'good'} element={<Good/>}>
            <Route path={'good'} element={<Good/>}></Route>
        </Route>

        {/*3. state传参数： 不能通过页面链接的方式
        页面链接：http://localhost:5173/cart*/}
        <Route path={'cart'} element={<Cart/>}/>
    </Route>
));
```

### 页面点击

```tsx
import {NavLink, Outlet} from "react-router-dom";

export default function Welcome() {
    const params = {
        id: '123',
        type: 'erick_params',
    }

    const search = {
        id: '999',
        type: 'erick_search',
    }

    const state = {
        id: '666',
        type: 'erick_state',
    }

    return (
        <>
            <div>我是欢迎页面</div>
            {/*1. param 传参数*/}
            <NavLink to={`/home/${params.id}/${params.type}`}>Home页面</NavLink><br/>

            {/*2. search传参*/}
            <NavLink to={`/good?id=${search.id}&type=${search.type}`}>Good页面</NavLink><br/>

            {/*3. state传参数*/}
            <NavLink to={'/cart'} state={{
                id: state.id,
                type: state.type,
            }}>Cart页面</NavLink><br/>
            <Outlet/>
        </>
    );
}
```

### 数据接收

- 目标路由页面，接收传递的参数

```tsx
import {useParams} from "react-router-dom";

export default function Home() {
    /*useParams钩子函数*/
    const params = useParams();
    const {id, type} = params;
    return (
        <div>我是家{id} ===== {type}</div>
    );
}
```

```tsx
import {useLocation, useSearchParams} from "react-router-dom";

export default function Good() {
    /*方式一：searchParams*/
    const searchParams = useSearchParams();
    const [search] = searchParams;

    /*方式二：从location中获取：
    结果：   ?id=999&type=erick_search*/
    const location = useLocation();
    console.log(location.search);

    return (
        <>
            <div>{search.get('id')} ===== {search.get('type')}</div>
        </>
    )
}
```

```tsx
import {useLocation} from "react-router-dom";

export default function Cart() {
    /*钩子函数*/
    const location = useLocation();

    return (
        <div>购物车{location.state.id} === {location.state.type}</div>
    );
}
```

# 状态管理

## Context

- 多层父子组件之间通信的一种方式
- 开发中一般不用context，一般都用它来封装react插件
- Props传递：传递数据可以使用props，但是层级多了后，必须一层层传递props，比较麻烦
- 而且props传递时，层级多时，如果从顶层组件到底层组件，中间组件并不想获取到这个数据

### 定义

```ts
import {createContext} from "react";

export interface FoodContextConfig {
    name: string;
    version: string;
    appId: string;
    work: () => void;
}

/*定义context*/
export const FoodContext = createContext<FoodContextConfig>({} as FoodContextConfig);
```

### 顶层组件

- 包裹了Father组件，则Father组件及Father组件的子组件系列，都可以访问到顶组件的数据

```tsx
import {FoodContext} from "./component/context/FoodContext.ts";
import Father from "./component/cat/Father.tsx";

export default function App() {

    return (
        <div>
            <FoodContext.Provider value={{
                name: '顶部',
                version: '1.0.0',
                appId: 'random',
                work: () => {
                    console.log("hello")
                }
            }}>
                {/*子组件*/}
                <Father/>
            </FoodContext.Provider>
        </div>
    )
}
```

### 中间组件

```tsx
import FirstSon from "./FirstSon.tsx";
import SecondSon from "./SecondSon.tsx";

export default function Father() {

    /*当前组件用不到对应的属性，就什么也不做*/
    return (
        <>
            <FirstSon/>
            <SecondSon/>
        </>
    )
}
```

### 消费组件

- 消费组件可以通过顶层组件提供的方法，来对其中的内容进行修改

```tsx
import {useContext} from "react";
import {FoodContext, FoodContextConfig} from "../context/FoodContext.ts";

export default function FirstSon() {
    /*直接使用*/
    const {appId} = useContext<FoodContextConfig>(FoodContext);

    return (
        <div>first son{appId}</div>
    )
}
```

```tsx
import {FoodContext} from "../context/FoodContext";

export default function SecondSon() {
    console.log('SecondSon Render');
    return (
        <div>
            {/*复杂逻辑用这种*/}
            <FoodContext.Consumer>
                {context => (<div>{context.name}</div>)}
            </ FoodContext.Consumer>

        </div>
    );
}
```
