#  一、基本使用
提示：以下所有文件都新建在src目录下

## 1. 定义路由
**1. 新建components/lazyImportComponent.tsx**

```jsx
import { Suspense, LazyExoticComponent } from 'react';

const LazyImportComponent = (props: { lazyChildren: LazyExoticComponent<() => JSX.Element> }) => {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <props.lazyChildren />
    </Suspense>
  );
};

export default LazyImportComponent;
```

对使用懒加载的路由组件，使用Suspense包裹，用于异步加载组件时显示过渡UI

**2. 新建router/routes.tsx**

```jsx
import { lazy } from 'react';

import LazyImportComponent from '@/components/lazyImportComponent';

const routes = [
  {
    path: '/login',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/login'))} />,
  },
  {
    path: '/',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
  },
];

export default routes;
```

使用lazy延迟加载组件的代码（路由懒加载）

## 2. 创建路由实例
新建src/router/index.tsx

```jsx
import { createBrowserRouter } from 'react-router-dom';

import routes from './routes';

//可传第二个参数，配置base路径，例如{ basename: "/app"}
const router = createBrowserRouter(routes);

export default router;
```

## 3. 管理路由
修改main.tsx

```jsx
import { RouterProvider } from 'react-router-dom';

import ReactDOM from 'react-dom/client';

import router from './router';

const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);

root.render(<RouterProvider router={router} />);
```

RouterProvider：提供路由配置并管理应用的导航逻辑

# 二、嵌套路由
如果要实现登录之后左侧菜单栏不变，右侧随路由的切换变化显示的内容，需使用嵌套路由

## 1. 定义路由配置文件
修改router/routes.tsx

```jsx
import { lazy } from 'react';

import LazyImportComponent from '@/components/lazyImportComponent';

const routes = [
  {
    path: '/login',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/login'))} />,
  },
  {
    path: '/',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    children: [
      {
        path: '/user',
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/user'))} />,
      },
      {
        path: '/manage',
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/manage'))} />,
      },
    ],
  },
];

export default routes;
```

## 2. 配置嵌套路由路口
修改layout.tsx

```jsx
import { Outlet } from 'react-router-dom';

export default function Layout() {
  return (
    <div className="main">
      菜单栏
      <Outlet />
    </div>
  );
}
```

Outlet组件呈现匹配的子路由，类似vue的router-view组件

# 三、索引路由
索引路由的内容渲染在父路由的Outlet中，类似于默认子路由。

在上述嵌套路由中，如果直接访问路由"/"，则没有匹配到任何子路由，页面只会显示“菜单栏”。可新增索引路由，在父路由没有匹配到任何子路由的情况下，显示默认的子路由内容

修改router/routes.tsx

```jsx
const routes = [
  //...
  {
    path: '/',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    children: [
      {
        index: true,
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/home'))} />,
      },
      //...
    ],
  },
];
```

# 四、重定向
索引路由可以用来路由重定向，当访问“/”使，重定向到“/user”

修改router/routes.tsx

```jsx
import { Navigate } from "react-router-dom";

const routes = [
  //...
  {
    path: '/',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    children: [
      {
        index: true,
        element: <Navigate to={'/user'} replace={true} />,
      },
      //...
    ],
  },
];
```

replace={true}：进行重定向

# 五、404页面
修改router/routes.tsx

```jsx
const routes = [
  //...
  {
    path: '*',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/notFound'))} />,
  },
]
```

404路由放在路由配置表的最后

# 六、声明式、编程式导航
## 1. 声明式导航
```html
<NavLink to="/manage">manage</NavLink>
```

## 2. 编程式导航
```jsx
import { useNavigate } from 'react-router-dom';

export default function User() {
  const navigation = useNavigate();
  return (
    <div>
      user
      <button onClick={() => navigation('/manage')}>manage</button>
    </div>
  );
}
```

# 七、路由传参
## 1. search传参
**1. 编程式传参**

```javascript
<NavLink to={{ pathname: '/file', search: '?sort=name&id=2' }}>file</NavLink>
```

**2. 命令式传参**

```jsx
<button onClick={() => navigation({ pathname: '/file', search: '?sort=name&id=2' })}>file</button>
```

**3. 页面接参**

```jsx
import { useSearchParams } from 'react-router-dom';

export default function File() {
  const [searchParams] = useSearchParams();
  return (
    <div>
      获取id：{searchParams.get('id')}
      获取sort：{searchParams.get('sort')}
    </div>
  );
}
```

## 2. state传参
**1. 编程式传参**

```javascript
<NavLink to="/file" state={{ id: 1 }}> file </NavLink>
```

**2. 命令式传参**

```javascript
<button onClick={() => navigation("/file", { state: { id: "2" } })}> file </button>
```

**3. 页面接参**

```javascript
import { useLocation } from 'react-router-dom';

export default function File() {
  const { state } = useLocation();
  return <div>{state.id}</div>;
}
```

特点：参数不会显示在路径上

## 3. 动态路由匹配
**1. 定义动态路由**

修改router/routes.tsx

```tsx
const routes = [
  //... 
  {
    path: '/',  
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    children: [
      //... 
      //新增
      {
        path: '/file/:id?',
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/file'))} />,
      },
    ],
  },
];
```

**2. 编程式传参**

```tsx
<NavLink to="/file/123">file</NavLink>
```

**3. 命令式传参**

```tsx
<button onClick={() => navigation('/file/123')}>file</button>
```

**4. 页面接参**

```tsx
import { useParams } from 'react-router-dom';
export default function File() {
  const { id } = useParams();
  return <div>{id}</div>;
}
```

# 八、错误处理
在路由组件加载过程中发生错误时展示的元素

**1. 新建components/errorBoundary.tsx**

```tsx
import { useRouteError } from 'react-router-dom';

export default function ErrorBoundary() {
  const error = useRouteError();
  //错误信息，可用来错误上报
  console.log(error);
  return <>错误页面</>;
}
```

**2. 修改router/routes.tsx**

```tsx
//新增
import ErrorBoundary from '@/components/errorBoundary';

const routes = [
  //...
  {
    path: '/',
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    errorElement: <ErrorBoundary />, 
    children:[
      //...
    ]
  },
]
```

# 九、路由守卫
loader：在渲染路由组件之前执行一些操作，比如异步数据加载，权限检查。

## 1. 同步权限检查
判断localStorage中存储的token值。当不存在token时，在渲染路由组件之前进行重定向到登录页

**1. 定义loader**

新建router/loader.ts

```tsx
import { redirect } from 'react-router-dom';

export function protectedLoader() {
  if (!localStorage.getItem('token')) {
    return redirect('/login');
  }
  return null;
}
```

**2. 挂载loader**

修改router/routes.tsx

```tsx
//新增
import { protectedLoader } from './loader';

const routes = [
  //...
  {
    path: '/',
    loader: protectedLoader,
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    errorElement: <ErrorBoundary />,
    children:[
      //...
    ]
  },
]
```

## 2. 异步权限检查
通过接口判断当前路由是否有访问权限，没有，则在渲染路由组件之前重定向到登录页

**1. 定义loader**

修改router/loader.ts

```tsx
/* 模拟网络接口 */
function getToken(): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(Math.random());
    }, 2000);
  });
}

export async function tokenLoader() {
  const num = await getToken();
  //如果随机数大于0.5则重定向到登录页
  if (num > 0.5) {
    return redirect('/login');
  } else {
    return null;
  }
}
```

**2. 挂载loader**

修改router/routes.tsx

```jsx
import { tokenLoader } from './loader';

const routes = [
  //... 
  {
    path: '/',
    loader: protectedLoader,  
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    errorElement: <ErrorBoundary />,
    children: [
      //... 
      {
        path: '/manage',
        loader: tokenLoader ,     //新增
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/manage'))} />,
      },
    ],
  },
];
```

**3. 增加导航加载状态**

在loader完成之前，路由不会跳转，如果接口请求时间过长，会导致路由跳转卡顿明显。此时需增加导航加载状态效果

修改layout.tsx

```javascript
import { Outlet, useNavigation } from 'react-router-dom';

export default function Layout() {
  const navigation = useNavigation();
  return (
    <div className="main">
    菜单栏
  {navigation.state === 'loading' && <div>数据加载中</div>}
    <Outlet />
    </div>
   );
  }
```

## 3. 异步数据加载
在渲染路由组件时，调用接口获取数据。

**1. 定义loader**

修改router/loader.ts

```javascript
//新增
export interface User {
  id: number;
  name: string;
}

/* 模拟网络接口 */
function getUsers(): Promise<User[]> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve([
        { id: 1, name: 'zhangsan' },
        { id: 2, name: 'lisi' },
      ]);
    }, 2000);
  });
}

export function usersLoader() {
  const userPromise = getUsers();
  return defer({ data: userPromise });
}
```

defer 在 loader 内使用，表明这个 loader 需要展示 Loading 状态。

**2. 挂载loader**

修改router/routes.tsx

```jsx
//新增
import { usersLoader } from './loader';

const routes = [
  //... 
  {
    path: '/', 
    loader: protectedLoader, 
    element: <LazyImportComponent lazyChildren={lazy(() => import('@/layout'))} />,
    errorElement: <ErrorBoundary />,
    children: [
      //... 
      {
        path: '/user',
        loader: usersLoader,     //新增
        element: <LazyImportComponent lazyChildren={lazy(() => import('@/pages/user'))} />,
      },
    ],
  },
];
```

**3. 获取loader返回的数据**

```javascript
import { useLoaderData, Await, useAsyncValue } from 'react-router-dom';

import type { User } from '@/router/loader';

export default function User() {
  const { data } = useLoaderData() as { data: Promise<User[]> };

return (
  <Await resolve={data}>
  <List />
  </Await>
);
}

const List = () => {
  const data = useAsyncValue() as User[];
  return (
    <>
    {data.map((o) => (
    <div key={o.id}>{o.name}</div>
))}
  </>
);
};
```

Await：需要结合`<Suspense>`使用，Loading 状态展示在`<Suspense>` 的 fallback 中

useAsyncValue：用于从最近的`<Await>`祖先组件返回解析后的数据

# 十、组件内守卫
使用情景：预防用户在还未保存修改前突然离开。

```javascript
import { useState } from 'react';
import { unstable_usePrompt } from 'react-router-dom';

export default function File() {
  const [value, setValue] = useState('');

  unstable_usePrompt({
    message: '确定离开吗',
    when: ({ currentLocation, nextLocation }) => value !== '' && currentLocation.pathname !== nextLocation.pathname,
  });

  return <input onChange={(v) => setValue(v.target.value)} />;
}
```

# 十一、状态保存
在有些场景中，我们需要在浏览器刷新或关闭时保存数据状态。之前常监听浏览器的beforeunload事件，在React Router V6中，新增了 useBeforeUnload，用来将重要的应用程序状态保存在页面上

```javascript
import { useState, useCallback } from 'react';
import { useBeforeUnload } from 'react-router-dom';

export default function Manage() {
  const [count, setCount] = useState(localStorage.getItem('count') || '0');

  //在浏览器关闭或者刷新时，保存数据
  useBeforeUnload(
    useCallback(() => {
      localStorage.setItem('count', count);
    }, [count]),
  );

  return (
    <div>
    {count}
    <button
  onClick={() => {
    setCount((prev) => prev + '1');
  }}
    >
    按钮
    </button>
    </div>
  );
}
```

# 结尾
action：使用react-router-dom提供的Form表单，当表单提交时action会被触发。但目前绝大部情况下都使用axios或fetch提交表单，因此action实际使用场景并不多，这里不再详细阐述

  


 

