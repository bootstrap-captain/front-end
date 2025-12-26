# 受控组件

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
