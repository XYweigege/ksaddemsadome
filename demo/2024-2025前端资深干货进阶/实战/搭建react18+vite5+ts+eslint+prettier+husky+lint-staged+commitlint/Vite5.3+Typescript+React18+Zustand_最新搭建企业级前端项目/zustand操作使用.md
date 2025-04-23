# 一、特点
1. 完整的 ts 支持
2. 相比于redux，@reduxjs/toolkit等库，API简单，易于上手
3. 基于发布订阅模式实现的响应式

# 二、安装
```javascript
pnpm i zustand 
pnpm i  immer
```

zustand：状态管理库

immer：以更方便的方式处理不可变状态

# 三、使用
## 1. 创建 store
新建store/user.ts

```javascript
import { produce } from 'immer';
import { create } from 'zustand';

interface UserInfo {
  name: string;
  age: number;
}

interface UserState {
  userInfo: UserInfo;
  token: string;
  updateUserInfo: (parmas: UserInfo) => void;
  updateAge: (params: number) => void;
  updateToken: (params: string) => void;
}

// 创建状态存储
const useUserStore = create<UserState>((set) => ({
  userInfo: {
    name: 'zhangsan',
    age: 23,
  },
  token: 'S1',
  //更新整个对象
  updateUserInfo: (userInfo) => set({ userInfo }), //合并userInfo
  //更新对象中某个属性
  updateAge: (age) =>
    set(
      produce((state) => {
        state.userInfo.age = age;
      }),
    ),
  //更新原始数据类型
  updateToken: (token) => set({ token }),
}));

export default useUserStore;
```

## 2. 使用 store
1. 创建user组件

新建pages/components/user.tsx

```javascript
import { useShallow } from 'zustand/react/shallow';

import useUserStore from '@/store/user';
const User = () => {
  const { userInfo, updateUserInfo, updateAge } = useUserStore(
    useShallow((state) => ({
      userInfo: state.userInfo,
      updateUserInfo: state.updateUserInfo,
      updateAge: state.updateAge,
    })),
  );

  return (
    <>
    <div>
    姓名：{userInfo.name} 年龄：{userInfo.age}
  </div>
  <button onClick={() => updateUserInfo({ name: 'lisi', age: 24 })}>更新用户</button>
  <button onClick={() => updateAge(userInfo.age + 1)}>更新年龄</button>
  </>
);
};

export default User;
```

这里使用`useShallow`，是为了防止修改userInfo时，整个userStore存储状态更新，从而导致token组件也进行不必要的更新

1. 创建token组件

新建pages/components/token.tsx

```javascript
import { useShallow } from 'zustand/react/shallow';

import useUserStore from '@/store/user';

const Token = () => {
  const { token, updateToken } = useUserStore(
    useShallow((state) => ({ token: state.token, updateToken: state.updateToken })),
  );

  return (
    <>
    <div>token：{token}</div>
  <button onClick={() => updateToken('23652')}>更新token</button>
  </>
);
};

export default Token;
```

在token组件中同样使用`useShallow`进行包裹，否则token值的更新，也同样会触发user组件不必要的更新

如果单纯的取一个token值，可简写为：

```javascript
const token = useUserStore((state) => state.token);
```

1. 创建展示页面

新建pages/info.tsx

```javascript
import Token from './components/token';
import User from './components/user';

const Info = () => {
  return (
    <div>
    <User />
    <Token />
    </div>
  );
};

export default Info;
```

## 3. 异步 action
React-redux的异步需要借助redux-thunk中间件，@reduxjs/toolkit也需借助extraReducers选项去处理

zustand可直接通过async，await来处理异步action

新建store/list.ts

```javascript
import { produce } from 'immer';
import { create } from 'zustand';

const getData = () => {
  return new Promise<number>((resolve) => {
    setTimeout(() => {
      resolve(Math.random() * 100);
    }, 200);
  });
};

interface ListState {
  list: number[];
  updateList: () => void;
}

const useListStore = create<ListState>((set) => ({
  list: [],
  updateList: async () => {
    try {
      const data = await getData();
      set(
        produce((state) => {
          state.list.push(data);
        }),
      );
    } catch {
      /* empty */
    }
  },
}));

export default useListStore;
```

# 四、数据持久化
## 1. 启用持久化
新建store/token.ts

```javascript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface TokenState {
  token: string;
  updateToken: (params: string) => void;
}

const useTokenStore = create<TokenState>()(
  persist(
    (set) => ({
      token: '',
      updateToken: (token) => set({ token }),
    }),
    {
      name: 'token', //存储的名称
    },
  ),
);

export default useTokenStore;
```

## 2. 修改存储位置
默认存储到localStorage，可以存储到sessionStorage

```javascript
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

interface TokenState {
  token: string;
  updateToken: (params: string) => void;
}

const useTokenStore = create<TokenState>()(
  persist(
    (set) => ({
      token: '',
      updateToken: (token) => set({ token }),
    }),
    {
      name: 'token', //存储的名称
      storage: createJSONStorage(() => sessionStorage), //存储到sessionStorage
    },
  ),
);

export default useTokenStore;
```

## 3. 自定义要持久化的字段
默认会将store中的所有字段都缓存，可以通过partialize指定要缓存的字段

```javascript
import { produce } from 'immer';
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';
interface TokenState {
  user: {
    name: string;
    token: string;
  };
  updateToken: (params: string) => void;
}

const useTokenStore = create<TokenState>()(
  persist(
    (set) => ({
      user: {
        name: '',
        token: '',
      },
      updateToken: (token) =>
        set(
          produce((state) => {
            state.user.token = token;
          }),
        ),
    }),
    {
      name: 'token', //存储的名称
      storage: createJSONStorage(() => sessionStorage), //存储到sessionStorage
      partialize: (state) => ({ token: state.user.token }), //值存储token字段，而非user
    },
  ),
);

export default useTokenStore;
```

  


 

