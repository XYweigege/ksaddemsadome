 

# 🥇🥇🥇一文带你打通微前端-qiankun/microapp/icestark/wujie全解析

[lyllovelemon](/user/4054654615034824/posts)

2023-12-04 7,698 阅读17分钟

关注

微前端的设计要素：路由隔离、js隔离、css隔离、预加载机制、通信机制、多微应用激活，如果不了解微前端请自行搜索学习

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0405f8ff12cf4c03bfdb054df022f90f~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1921&h=1101&s=160321&e=png&b=fffdfd)

一个完善的微前端框架需要考虑：

* 子应用的激活和卸载能力

页面从主应用切换到子应用，子应用切换到另一个子应用，框架需要正确加载资源、渲染页面、切换流畅

* 子应用独立运行能力

需要考虑到子应用运行后不污染主应用的css/js/window/location等对象

* 应用通信能力

需要设计主应用和子应用通信，子应用互相通信的能力

* 路由切换能力

接入子应用后，需要不影响浏览器正常的前进、后退、URL展示

本文带你打通主流微前端框架qiankun、wujie、micro app、icestark设计思路，加深对微前端的理解

# 方案对比

|     | qiankun | micro app | icestark | wujie |
| --- | --- | --- | --- | --- |
| 原理  | 基于single-spa | 基于web components，通过customElement结合自定义shadow DOM实现 | 路由劫持 | 基于web components 和iframe |
| 数据通信 | 发布订阅模式 | 绑定监听函数 | window.store通信或者基于Event Emitter通信 | props/window/Event Bus |
| js隔离 | snapshotSandbox/legacySandbox/proxySandbox(基于proxy的沙箱) | 基于proxy的沙箱机制 | 基于proxy的沙箱机制 | 基于iframe的沙箱机制 |
| 样式隔离 | Scoped CSS/Shadow DOM/web component | Scoped CSS | css Module（shadow DOM还在实验阶段） | Shadow DOM |
| 预加载 | √   | √   | √   | √   |
| 多实例激活 | √   | √   | ×   | √   |
| 支持ESM(vite) | ×   | √   | √   | √   |
| 浏览器兼容性 | 支持IE和主流浏览器(但是IE不支持proxy，只支持单例模式) | 支持IE11以上和主流浏览器(不支持web component的浏览器无法使用) | 支持IE11以上和主流浏览器 | 支持IE和主流浏览器 |
| 接入成本 | 较低  | 低   | 低   | 低   |
| 社区成熟度和活跃度 | 最高  | 较高  | 中等  | 较低  |

根据表格可得知

* 对浏览器兼容性要求高或者需要隔离的比较彻底，推荐使用wujie，
* 不需要支持vite且需要多实例激活，推荐使用qiankun
* icestark的优势是更加灵活，支持微模块和vite

实际在选择方案时，需要考虑多种因素，选择最适合团队的方案

# 基本使用

## qiankun

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d594e1531a64e6aa5406dc1595d8d61~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1119&h=1151&s=142424&e=png&b=f9fafb)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f6d6a5781fec4b38ae8b0117df396a39~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=285&h=111&s=3445&e=png&b=f9fafb)

* registerMircoApps为主动注册微应用
* loadMicroApp是手动加载微应用

微应用只需要导出三个生命周期钩子boostrap/mount/unmount即可

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f9514a4f83c4b85addb2091f153f78b~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1679&h=1033&s=254989&e=png&b=f8f9fa) webpack配置需要支持微前端

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/378ed8824a0749ffbe1b85db513dd2cf~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=955&h=531&s=65508&e=png&b=f9fafb)

## micro app

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/21ae954dfd3e41869ab4d6bd250f1dd5~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1195&h=891&s=81038&e=png&b=f4f7f9)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/085e7a2e188b4460a5bf8bcad9fd4e35~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=901&h=485&s=35385&e=png&b=f6f8fa)

## icestark

具体使用参考官方文档[icestark.gitee.io/docs/guide/…](https://link.juejin.cn?target=https%3A%2F%2Ficestark.gitee.io%2Fdocs%2Fguide%2Fuse-layout%2Fvue "https://icestark.gitee.io/docs/guide/use-layout/vue")

## wujie

具体起步参考[wujie-micro.github.io/doc/guide/s…](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fstart.html "https://wujie-micro.github.io/doc/guide/start.html")

# 沙箱隔离

沙箱隔离机制主要是为了避免全局变量污染和全局css样式污染，当微应用是二方接入或三方接入时，和主应用可能存在路由命名冲入、变量命名冲突、css命名冲突等问题，不解决这些问题可能会导致微前端页面样式错乱、甚至页面报错、渲染失败

## qiankun沙箱隔离

qiankun的css沙箱分为shadow DOM/Scoped CSS/Web Component

qiankun的js沙箱分为snapshotSandbox（快照沙箱）/proxySandbox(代理沙箱)/LegacySandbox

不管是哪种沙箱，都是在**mounted阶段激活，unmounted阶段卸载**

### snapshotSandbox

**基于diff实现的沙箱**，用于不支持proxy的低版本浏览器

实现原理：

1.  快照保存window对象(window对象的键值对以hashMap类型存储)
2.  微应用mount时，modifyPropsMap存在，将变更modifyPropsMap应用到全局window，没有则跳过该步骤；浅拷贝window键值对，用于卸载时恢复环境
3.  微应用unmount时，快照的键值对和window的键值对进行diff比较，diff结果用于恢复微应用环境

```typescript
export default class SnapshotSandbox implements SandBox {
  // proxy指向windowProxy
  proxy: WindowProxy;
  // 微应用名称(唯一标识)
  name: string;
  // 类型为沙箱类型
  type: SandBoxType;
  // 沙箱是否在运行中 
  sandboxRunning = true;

  private windowSnapshot!: Window;
  // 记录需要修改属性
  private modifyPropsMap: Record<any, any> = {};

  constructor(name: string) {
    this.name = name;
    this.proxy = window;
    // 类型为SnapShotSandbox
    this.type = SandBoxType.Snapshot;
  }

  active() {
    // 记录当前快照
    this.windowSnapshot = {} as Window;
    // 遍历window的key
    iter(window, (prop) => {
      // 将window的值保存在windowSnapshot上
      this.windowSnapshot[prop] = window[prop];
    });

    // 恢复之前的变更
    Object.keys(this.modifyPropsMap).forEach((p: any) => {
      window[p] = this.modifyPropsMap[p];
    });
    // 激活时沙箱运行状态为true
    this.sandboxRunning = true;
  }

  inactive() {
    // 清空修改过的属性
    this.modifyPropsMap = {};

    iter(window, (prop) => {
      if (window[prop] !== this.windowSnapshot[prop]) {
        // 记录变更，恢复环境
        this.modifyPropsMap[prop] = window[prop];
        window[prop] = this.windowSnapshot[prop];
      }
    });

    if (process.env.NODE_ENV === 'development') {
      console.info(`[qiankun:sandbox] ${this.name} origin window restore...`, Object.keys(this.modifyPropsMap));
    }
    // inactive沙箱运行状态为false
    this.sandboxRunning = false;
  }

  patchDocument(): void {}
}

```

### proxySandbox

snapshotSandbox和legacySandBox都是单例模式下的沙箱(即同时只激活一个微应用)，proxySandbox解决的是**多个微应用同时激活**时的沙箱隔离问题

实现原理：

1.  将主应用window上的原生属性拷贝出来(比如location,document等)，放在一个单独的对象fakeWindow上
2.  每个微应用维护自己的window，当在微应用中读写原生属性时优先修改window，不是原生属性的读写的是fakeWindow

```kotlin
export default class ProxySandbox implements SandBox {
  /** window 值变更记录 */
  private updatedValueSet = new Set<PropertyKey>();
  // document对象
  private document = document;
  name: string;
  type: SandBoxType;
  proxy: WindowProxy;
  // 记录沙箱是否在运行
  sandboxRunning = true;
  latestSetProp: PropertyKey | null = null;

  active() {
    // 当前沙箱没有在运行则激活的沙箱数量+1
    if (!this.sandboxRunning) activeSandboxCount++;
    // 将当前微应用的沙箱是否运行变量设置为true
    this.sandboxRunning = true;
  }

  inactive() {
    if (process.env.NODE_ENV === 'development') {
      ...
    }
    // 如果是测试环境或者激活的沙箱数量为0
    if (inTest || --activeSandboxCount === 0) {
      // 将全局value重置 Object.keys(this.globalWhitelistPrevDescriptor).forEach((p) => {
        const descriptor = this.globalWhitelistPrevDescriptor[p];
        // descriptor存在通过Object.defineProperty修改属性
        if (descriptor) {
          Object.defineProperty(this.globalContext, p, descriptor);
        } else {
          // descriptor不存在通过delete删除属性
          delete this.globalContext[p];
        }
      });
    }
    // 将沙箱是否运行变量设置为false
    this.sandboxRunning = false;
  }

  public patchDocument(doc: Document) {
    this.document = doc;
  }

  // 全局白名单描述符（记录了修改之前的属性）
  globalWhitelistPrevDescriptor: { [p in (typeof globalVariableWhiteList)[number]]: PropertyDescriptor | undefined } =
    {};
  globalContext: typeof window;

  constructor(name: string, globalContext = window, opts?: { speedy: boolean }) {
    // 微应用name(唯一标识) 
    this.name = name;
    // 全局上下文，参数默认值是window
    this.globalContext = globalContext;
    // 沙箱类型为proxySandbox
    this.type = SandBoxType.Proxy;
    const { updatedValueSet } = this;
    const { speedy } = opts || {};
    
    // 创建fakeWindow
    const { fakeWindow, propertiesWithGetter } = createFakeWindow(globalContext, !!speedy);

    const descriptorTargetMap = new Map<PropertyKey, SymbolTarget>();
    // fakeWindow用proxy拦截
    const proxy = new Proxy(fakeWindow, {
      set: (target: FakeWindow, p: PropertyKey, value: any): boolean => {
        if (this.sandboxRunning) {
          this.registerRunningApp(name, proxy);

          // 同步属性到 globalContext
          if (typeof p === 'string' && globalVariableWhiteList.indexOf(p) !== -1) {
            this.globalWhitelistPrevDescriptor[p] = Object.getOwnPropertyDescriptor(globalContext, p);
            // @ts-ignore
            globalContext[p] = value;
          } else {
            // 保存 description while the property existed in globalContext before
            if (!target.hasOwnProperty(p) && globalContext.hasOwnProperty(p)) {
              const descriptor = Object.getOwnPropertyDescriptor(globalContext, p);
              const { writable, configurable, enumerable, set } = descriptor!;
              // 只有writable属性可以被覆盖
              // here we ignored accessor descriptor of globalContext as it makes no sense to trigger its logic(处理解决沙箱逃逸问题)
              // we force to set value by data descriptor
              if (writable || set) {
                Object.defineProperty(target, p, { configurable, enumerable, writable: true, value });
              }
            } else {
              target[p] = value;
            }
          }

          updatedValueSet.add(p);

          this.latestSetProp = p;

          return true;
        }

        ...

        // 在 严格模式 下，Proxy 的 handler.set 返回 false 会抛出 TypeError，在沙箱卸载的情况下应该忽略错误
        return true;
      },

      get: (target: FakeWindow, p: PropertyKey): any => {
        this.registerRunningApp(name, proxy);

        if (p === Symbol.unscopables) return unscopables;
        // 避免使用window.window或window.self触发沙箱逃逸
        if (p === 'window' || p === 'self') {
          return proxy;
        }

        // 劫持用 globalThis关键词的globalWindow
        if (p === 'globalThis' || (inTest && p === mockGlobalThis)) {
          return proxy;
        }

        if (p === 'top' || p === 'parent' || (inTest && (p === mockTop || p === mockSafariTop))) {
          // 如果主应用在iframe上下文中, 允许沙箱逃逸
          if (globalContext === globalContext.parent) {
            return proxy;
          }
          return (globalContext as any)[p];
        }

        // proxy.hasOwnProperty会先触发getter, 它的值即globalContext.hasOwnProperty
        if (p === 'hasOwnProperty') {
          return hasOwnProperty;
        }

        if (p === 'document') {
          return this.document;
        }

        if (p === 'eval') {
          return eval;
        }

        if (p === 'string' && globalVariableWhiteList.indexOf(p) !== -1) {
          // @ts-ignore
          return globalContext[p];
        }

        const actualTarget = propertiesWithGetter.has(p) ? globalContext : p in target ? target : globalContext;
        const value = actualTarget[p];

        // frozen的值直接返回
        if (isPropertyFrozen(actualTarget, p)) {
          return value;
        }

        // 非原生属性直接返回避免重新绑定this
        if (!isNativeGlobalProp(p as string) && !useNativeWindowForBindingsProps.has(p)) {
          return value;
        }

        /* Some dom api must be bound to native window, otherwise it would cause exception like 'TypeError: Failed to execute 'fetch' on 'Window': Illegal invocation'
           See this code:
             const proxy = new Proxy(window, {});
             // in nest sandbox fetch will be bind to proxy rather than window in master
             const proxyFetch = fetch.bind(proxy);
             proxyFetch('https://qiankun.com');
        */
        const boundTarget = useNativeWindowForBindingsProps.get(p) ? nativeGlobal : globalContext;
        return rebindTarget2Fn(boundTarget, value);
      },

      has(target: FakeWindow, p: string | number | symbol): boolean {
        // 判断cachedGlobalObjects上有没有p属性
        return p in cachedGlobalObjects || p in target || p in globalContext;
      },

      getOwnPropertyDescriptor(target: FakeWindow, p: string | number | symbol): PropertyDescriptor | undefined {
        /*
         as the descriptor of top/self/window/mockTop in raw window are configurable but not in proxy target, we need to get it from target to avoid TypeError
         see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/getOwnPropertyDescriptor
         > A property cannot be reported as non-configurable, if it does not exist as an own property of the target object or if it exists as a configurable own property of the target object.
         */
        if (target.hasOwnProperty(p)) {
          const descriptor = Object.getOwnPropertyDescriptor(target, p);
          descriptorTargetMap.set(p, 'target');
          return descriptor;
        }

        if (globalContext.hasOwnProperty(p)) {
          const descriptor = Object.getOwnPropertyDescriptor(globalContext, p);
          descriptorTargetMap.set(p, 'globalContext');
          // A property cannot be reported as non-configurable, if it does not exist as an own property of the target object
          if (descriptor && !descriptor.configurable) {
            descriptor.configurable = true;
          }
          return descriptor;
        }

        return undefined;
      },

      // trap to support iterator with sandbox
      ownKeys(target: FakeWindow): ArrayLike<string | symbol> {
        return uniq(Reflect.ownKeys(globalContext).concat(Reflect.ownKeys(target)));
      },

      defineProperty: (target: Window, p: PropertyKey, attributes: PropertyDescriptor): boolean => {
        const from = descriptorTargetMap.get(p);
        /*
         Descriptor must be defined to native window while it comes from native window via Object.getOwnPropertyDescriptor(window, p),
         otherwise it would cause a TypeError with illegal invocation.
         */
        switch (from) {
          case 'globalContext':
            return Reflect.defineProperty(globalContext, p, attributes);
          default:
            return Reflect.defineProperty(target, p, attributes);
        }
      },

      deleteProperty: (target: FakeWindow, p: string | number | symbol): boolean => {
        // 根据name注册运行的app
        this.registerRunningApp(name, proxy);
        if (target.hasOwnProperty(p)) {
          // FakeWindow上有p属性就删除
          delete target[p];
          // 同时在Set中删除
          updatedValueSet.delete(p);

          return true;
        }

        return true;
      },

      // 确保在微应用中`window instanceof Window`返回true
      getPrototypeOf() {
        return Reflect.getPrototypeOf(globalContext);
      },
    });

    this.proxy = proxy;

    activeSandboxCount++;

    function hasOwnProperty(this: any, key: PropertyKey): boolean {
      // 等价于hasOwnProperty.call(obj, key)
      if (this !== proxy && this !== null && typeof this === 'object') {
        return Object.prototype.hasOwnProperty.call(this, key);
      }

      return fakeWindow.hasOwnProperty(key) || globalContext.hasOwnProperty(key);
    }
  }

  private registerRunningApp(name: string, proxy: Window) {
    if (this.sandboxRunning) {
      // 获取当前运行的微应用
      const currentRunningApp = getCurrentRunningApp();
      // 没有就设置当前传入name的为当前运行微应用
      if (!currentRunningApp || currentRunningApp.name !== name) {
        setCurrentRunningApp({ name, window: proxy });
      }
      // 这个方法用于判断当前运行环境是否为微应用 
      nextTask(clearCurrentRunningApp);
    }
  }
}

```

### LegacySandbox

**基于proxy实现的沙箱**

实现原理：

1.  沙箱里新增的全局变量叫addProps(Map类型)，更新的全局变量叫modifyProps(Map类型)
2.  新增的全局变量加入到addProps，更新的变量把旧的键值对存的prevProps，新的存到modifyProps
3.  监听这三个变量值，根据这些变量可以知道微应用和主应用之间的变化

```typescript
export default class LegacySandbox implements SandBox {
  /** 沙箱期间新增的全局变量 */
  private addedPropsMapInSandbox = new Map<PropertyKey, any>();

  /** 沙箱期间更新的全局变量 */
  private modifiedPropsOriginalValueMapInSandbox = new Map<PropertyKey, any>();

  /** 持续记录更新的(新增和修改的)全局变量的 map，用于在任意时刻做 snapshot */
  private currentUpdatedPropsValueMap = new Map<PropertyKey, any>();
 
 // 省略变量声明
  ...

  // 设置全局属性方法
  private setWindowProp(prop: PropertyKey, value: any, toDelete?: boolean) {
    // 如果value不存在且toDelete为true(需要删除该变量)
    if (value === undefined && toDelete) {
      // 删除window上的变量
      delete (this.globalContext as any)[prop];
    } else if (isPropConfigurable(this.globalContext, prop) && typeof prop !== 'symbol') {
      // 属性可配置 使用Object.defineProperty
      Object.defineProperty(this.globalContext, prop, { writable: true, configurable: true });
      // 更新对应prop的值
      (this.globalContext as any)[prop] = value;
    }
  }

  active() {
    // 沙箱不是运行状态
    if (!this.sandboxRunning) {
      // 遍历当前需要更新value,更新每个值
      this.currentUpdatedPropsValueMap.forEach((v, p) => this.setWindowProp(p, v));
    }
    // 设置为沙箱运行状态
    this.sandboxRunning = true;
  }

  inactive() {
    // 开发环境告警打印所有addProps/modifyProps的键
    if (process.env.NODE_ENV === 'development') {
      console.info(`[qiankun:sandbox] ${this.name} modified global properties restore...`, [
        ...this.addedPropsMapInSandbox.keys(),      ...this.modifiedPropsOriginalValueMapInSandbox.keys(),
      ]);
    }

  
    // 重置到初始状态(旧modifyProps,旧addProps遍历重置)
    this.modifiedPropsOriginalValueMapInSandbox.forEach((v, p) => this.setWindowProp(p, v));
    this.addedPropsMapInSandbox.forEach((_, p) => this.setWindowProp(p, undefined, true));
    // 将沙箱运行状态设置为false
    this.sandboxRunning = false;
  }
}

```

核心方法：

1.  定义好addProps(新增的变量)、modifyOldProps(修改的旧变量)、modifyProps(修改的变量)
2.  拦截get/set/has/hasProperty等方法，进行属性读取/设置/判断是否存在/设置操作符操作

```typescript
constructor(name: string, globalContext = window) {
    // 微应用name(唯一标识)
    this.name = name;
    // 全局上下文
    this.globalContext = globalContext;
    // 沙箱类型为LegacySandbox
    this.type = SandBoxType.LegacyProxy;
    // addProps(新增的变量)、modifyOldProps(修改的旧变量)、modifyProps(修改的变量)
    const { addedPropsMapInSandbox, modifiedPropsOriginalValueMapInSandbox, currentUpdatedPropsValueMap } = this;

    // 全局上下文赋值给rawWindow
    const rawWindow = globalContext;
    // fakeWindow初始为空对象
    const fakeWindow = Object.create(null) as Window;

    // **设置属性核心方法**
    const setTrap = (p: PropertyKey, value: any, originalValue: any, sync2Window = true) => {
      if (this.sandboxRunning) {
        // window中不存在这个key，说明是需要新增的 
        if (!rawWindow.hasOwnProperty(p)) {
          addedPropsMapInSandbox.set(p, value);
        } else if 
        (!modifiedPropsOriginalValueMapInSandbox.has(p)) {
          // 如果当前 window 对象存在该属性，且 record map 中未记录过，则记录该属性初始值
          modifiedPropsOriginalValueMapInSandbox.set(p, originalValue);
        }
        // 更新值
        currentUpdatedPropsValueMap.set(p, value);

        if (sync2Window) {
          // 必须重新设置 window 对象保证下次 get 时能拿到已更新的数据
          (rawWindow as any)[p] = value;
        }

        this.latestSetProp = p;

        return true;
      }

      ...

    const proxy = new Proxy(fakeWindow, {
      set: (_: Window, p: PropertyKey, value: any): boolean => {
        const originalValue = (rawWindow as any)[p];
        return setTrap(p, value, originalValue, true);
      },

      get(_: Window, p: PropertyKey): any {
        if (p === 'top' || p === 'parent' || p === 'window' || p === 'self') {
          return proxy;
        }

        const value = (rawWindow as any)[p];
        return rebindTarget2Fn(rawWindow, value);
      },

      has(_: Window, p: string | number | symbol): boolean {
        return p in rawWindow;
      },

      getOwnPropertyDescriptor(_: Window, p: PropertyKey): PropertyDescriptor | undefined {
        const descriptor = Object.getOwnPropertyDescriptor(rawWindow, p);
        // A property cannot be reported as non-configurable, if it does not exists as an own property of the target object
        if (descriptor && !descriptor.configurable) {
          descriptor.configurable = true;
        }
        return descriptor;
      },

      defineProperty(_: Window, p: string | symbol, attributes: PropertyDescriptor): boolean {
        const originalValue = (rawWindow as any)[p];
        const done = Reflect.defineProperty(rawWindow, p, attributes);
        const value = (rawWindow as any)[p];
        setTrap(p, value, originalValue, false);

        return done;
      },
    });

    this.proxy = proxy;
  }

```

## micro app 沙箱隔离

micro app的样式隔离是默认开启的(Scoped CSS),开启后会在运行时自动添加micro-app\[name=xxx\]前缀

```css
.test { color: red; } 
/* 转换为 */ 
micro-app[name=xxx] .test { color: red; }

```

主应用的样式还是会产生污染，因此需要通过规范约束，可以通过css前缀或css module的方案解决，具体使用参考[zeroing.jd.com/micro-app/d…](https://link.juejin.cn?target=https%3A%2F%2Fzeroing.jd.com%2Fmicro-app%2Fdocs.html%23%2Fzh-cn%2Fscopecss "https://zeroing.jd.com/micro-app/docs.html#/zh-cn/scopecss")

通过Shadow DOM实现元素隔离,ShadowDOM可以跟外部元素重名，但是互不影响

> HTML标签&lt;video&gt;,&lt;img&gt;也是Shadow DOM元素

具体使用参考[zeroing.jd.com/micro-app/d…](https://link.juejin.cn?target=https%3A%2F%2Fzeroing.jd.com%2Fmicro-app%2Fdocs.html%23%2Fzh-cn%2Fdom-scope "https://zeroing.jd.com/micro-app/docs.html#/zh-cn/dom-scope")

## icestark沙箱隔离

基于Proxy实现，劫持addEventListener/removeEventListener/setTimeout/setInterval

```ini
 // 劫持addEventListener
    proxyWindow.addEventListener = (eventName, fn, ...rest) => {
      this.eventListeners[eventName] = (this.eventListeners[eventName] || []);
      this.eventListeners[eventName].push(fn);

      return originalAddEventListener.apply(originalWindow, [eventName, fn, ...rest]);
    };
    // 劫持removeEventListener
    proxyWindow.removeEventListener = (eventName, fn, ...rest) => {
      const listeners = this.eventListeners[eventName] || [];
      if (listeners.includes(fn)) {
        listeners.splice(listeners.indexOf(fn), 1);
      }
      return originalRemoveEventListener.apply(originalWindow, [eventName, fn, ...rest]);
    };
    // 劫持setTimeout
    proxyWindow.setTimeout = (...args) => {
      const timerId = originalSetTimeout(...args);
      this.timeoutIds.push(timerId);
      return timerId;
    };
    // 劫持setInterval
    proxyWindow.setInterval = (...args) => {
      const intervalId = originalSetInterval(...args);
      this.intervalIds.push(intervalId);
      return intervalId;
    };

```

查看Proxy做了什么，核心实现就是通过Prxoy代理了代理proxyWindow，对其set/get/has进行覆写

```kotlin
 const sandbox = new Proxy(proxyWindow, {
      set(target: Window, p: PropertyKey, value: any): boolean {
        if (!originalWindow.hasOwnProperty(p)) {
          // 原来的window没有这个属性，说明需要新增
          propertyAdded[p] = value;
  
        } else if (!originalValues.hasOwnProperty(p)) {
          // 原来的window没有原来的值有 记录原始值
          originalValues[p] = originalWindow[p];
        }
        // 向原生window设置新值(jsonp的情况), js bundle 会在沙箱外执行
        if (!multiMode) {
          originalWindow[p] = value;
        }
        // 设置值
        target[p] = value;
        return true;
      },
      get(target: Window, p: PropertyKey): any {
        if (p === Symbol.unscopables) {
          return undefined;
        }
        if (['top', 'window', 'self', 'globalThis'].includes(p as string)) {
          return sandbox;
        }
        // 代理hasOwnProperty, 处理proxy.hasOwnProperty value 代表originalWindow.hasOwnProperty的情况
        if (p === 'hasOwnProperty') {
          // eslint-disable-next-line no-prototype-builtins
          return (key: PropertyKey) => !!target[key] || originalWindow.hasOwnProperty(key);
        }

        const targetValue = target[p];
     
        if (targetValue !== undefined) {
          return targetValue;
        }

        // 处理injection情况
        const injectionValue = injection && injection[p];
        if (injectionValue) {
          return injectionValue;
        }

        const value = originalWindow[p];

        if (p === 'eval') {
          return value;
        }

        if (isWindowFunction(value)) {
          // 处理全局函数例如`console.table`,
          const boundValue = value.bind(originalWindow);

          // 处理Axios, Moment等库，有额外参数的情况
          // Simply copy them into boundValue.
          for (const key in value) {
            boundValue[key] = value[key];
          }

          return boundValue;
        } else {
          // case of window.clientWidth、new window.Object()
          return value;
        }
      },
      has(target: Window, p: PropertyKey): boolean {
        return p in target || p in originalWindow;
      },
    });

```

此时有一个疑问，icestark是怎样做到js隔离的？

核心方法在SandBox类中的**execScriptInSandbox**方法里

主要功能：

1.  在执行脚本前创建沙箱
2.  将script脚本包裹在with(sandbox){;${script}\\n}中

> with语句用于扩展一个语句的作用域链,这样可以保证脚本在独立的沙箱作用域中执行，进而避免变量污染

```typescript
  execScriptInSandbox(script: string): void {
    if (!this.sandboxDisabled) {
      // create sandbox before exec script
      if (!this.sandbox) {
        this.createProxySandbox();
      }
      try {
        const execScript = `with (sandbox) {;${script}\n}`;
        // eslint-disable-next-line no-new-func
        const code = new Function('sandbox', execScript).bind(this.sandbox);
        // run code with sandbox
        code(this.sandbox);
      } catch (error) {
        console.error(`error occurs when execute script in sandbox: ${error}`);
        throw error;
      }
    }
  }

```

createProxySandbox就是上面提到的劫持定时器/事件监听器/事件解绑并通过Proxy代理ProxyWindow的功能函数

## wujie沙箱隔离

wujie是基于proxy和iframe实现的沙箱

非降级情况下window/location/document都使用proxy进行代理，降级情况下使用Object.defineProperty进行代理

什么时候降级取决于当前浏览器的兼容性，是否支持Proxy

定义了一个类，名字为Wujie

```css
export default class Wujie {
  /** 子应用的唯一标识 **/
  public id: string;
  /** 激活时路由地址 */
  public url: string;
  /** 子应用保活 */
  public alive: boolean;
  /** window代理 */
  public proxy: WindowProxy;
  /** document代理 */
  public proxyDocument: Object;
  /** location代理 */
  public proxyLocation: Object;
  /** 事件中心 */
  public bus: EventBus;
  /** 容器 */
  public el: HTMLElement;
  /** js沙箱 */
  public iframe: HTMLIFrameElement;
  /** css沙箱 */
  public shadowRoot: ShadowRoot;
  /** 子应用的template */
  public template: string;
  /** 子应用代码替换钩子 */
  public replace: (code: string) => string;
  /** 子应用自定义fetch */
  public fetch: (input: RequestInfo, init?: RequestInit) => Promise<Response>;
  /** 子应用的生命周期 */
  public lifecycles: lifecycles;
  /** 子应用的插件 */
  public plugins: Array<plugin>;
  /** js沙箱ready态 */
  public iframeReady: Promise<void>;
  /** 子应用预加载态 */
  public preload: Promise<void>;
  /** 降级时渲染iframe的属性 */
  public degradeAttrs: { [key: string]: any };
  /** 子应用js执行队列 */
  public execQueue: Array<Function>;
  /** 子应用执行过标志 */
  public execFlag: boolean;
  /** 子应用激活标志 */
  public activeFlag: boolean;
  /** 子应用mount标志 */
  public mountFlag: boolean;
  /** 路由同步标志 */
  public sync: boolean;
  /** 子应用短路径替换，路由同步时生效 */
  public prefix: { [key: string]: string };
  /** 子应用跳转标志 */
  public hrefFlag: boolean;
  /** 子应用采用fiber模式执行 */
  public fiber: boolean;
  /** 子应用降级标志 */
  public degrade: boolean;
  /** 子应用降级document */
  public document: Document;
  /** 子应用styleSheet元素 */
  public styleSheetElements: Array<HTMLLinkElement | HTMLStyleElement>;
  /** 子应用head元素 */
  public head: HTMLHeadElement;
  /** 子应用body元素 */
  public body: HTMLBodyElement;
  /** 子应用dom监听事件留存，当降级时用于保存元素事件 */
  public elementEventCacheMap: WeakMap<
    Node,
    Array<{ type: string; handler: EventListenerOrEventListenerObject; options: any }>
  > = new WeakMap();

  /** $wujie对象，提供给子应用的接口 */
  public provide: {
    bus: EventBus;
    shadowRoot?: ShadowRoot;
    props?: { [key: string]: any };
    location?: Object;
  };

  /** 子应用嵌套场景，父应用传递给子应用的数据 */
  public inject: {
    idToSandboxMap: Map<String, SandboxCache>;
    appEventObjMap: Map<String, EventObj>;
    mainHostPath: string;
  };

  
}

```

WuJie类除了定义自身使用的属性，还定义了**provide/inject**属性，用于父应用和子应用的数据通信(看到这里有没有觉得很熟悉，vue2也用到了provide/inject)

**Provide**

provide对象接收bus(EventBus类，数据通信使用),可选的shadowRoot(css隔离专用)，props(用于传递data数据),location（用于传递路由信息）

**Inject**

inject对象接收参数有：idToSandboxMap（Map类型，键为子应用的唯一标识，值为沙箱的缓存），appEventObjMap(存放事件缓存，Map类型，键为子应用的唯一标识，值为事件对象)，mainHostPath(主应用的路径)

接下来我们看下Wujie类的初始化

```kotlin
constructor(options: {
    name: string;// 子应用的name(唯一标识)
    url: string;// 子应用的url，可以包含protocol、host、path、query、hash
    attrs: { [key: string]: any };// 属性
    degradeAttrs: { [key: string]: any }; // 降级时渲染iframe的属性
    fiber: boolean;// 是否fiber模式运行
    degrade; //子应用降级标志
    plugins: Array<plugin>;// 插件队列
    lifecycles: lifecycles;// 生命周期
  }) {
    // 传递inject给嵌套子应用(有这个属性当前环境是子应用且初始化完成)
    if (window.__POWERED_BY_WUJIE__) this.inject = window.__WUJIE.inject;
    else {
      // 子应用没有初始化完成，初始化inject对象  
      this.inject = {
        idToSandboxMap: idToSandboxCacheMap,
        appEventObjMap,
        mainHostPath: window.location.protocol + "//" + window.location.host,// 路径拼接，协议+//+主机名
      };
    }
    // 解构options属性拿到constructor里定义的一组属性
    const { name, url, attrs, fiber, degradeAttrs, degrade, lifecycles, plugins } = options;
    // 属性赋值
    this.id = name;
    this.fiber = fiber;
    this.degrade = degrade || !wujieSupport;
    // 实例化一个EventBus类
    this.bus = new EventBus(this.id);
    this.url = url;
    this.degradeAttrs = degradeAttrs;
    // 父应用提供的数据
    this.provide = { bus: this.bus };
    this.styleSheetElements = [];
    this.execQueue = [];
    this.lifecycles = lifecycles;
    this.plugins = getPlugins(plugins);

    // 创建目标地址的解析
    const { urlElement, appHostPath, appRoutePath } = appRouteParse(url);
    const { mainHostPath } = this.inject;
    // 创建iframe
    this.iframe = iframeGenerator(this, attrs, mainHostPath, appHostPath, appRoutePath);

    // 子应用需要降级
    if (this.degrade) {
      // 降级情况下document、location代理处理（Object.defineProperty）
      const { proxyDocument, proxyLocation } = localGenerator(this.iframe, urlElement, mainHostPath, appHostPath);
      this.proxyDocument = proxyDocument;
      this.proxyLocation = proxyLocation;
    } else {
     // 非降级情况下window、document、location代理(proxy)，说明支持沙箱
      const { proxyWindow, proxyDocument, proxyLocation } = proxyGenerator(
        this.iframe,
        urlElement,
        mainHostPath,
        appHostPath
      );
      this.proxy = proxyWindow;
      this.proxyDocument = proxyDocument;
      this.proxyLocation = proxyLocation;
    }
    this.provide.location = this.proxyLocation;

    addSandboxCacheWithWujie(this.id, this);
  }

```

核心方法包括active、start、mount、destroy等

**active**

激活子应用，主要的功能：

1.  同步路由
2.  动态修改iframe的fetch
3.  准备shadow
4.  准备子应用注入

```kotlin
  public async active(options: {
    url: string;
    sync?: boolean;
    prefix?: { [key: string]: string };
    template?: string;
    el?: string | HTMLElement;
    props?: { [key: string]: any };
    alive?: boolean;
    fetch?: (input: RequestInfo, init?: RequestInit) => Promise<Response>;
    replace?: (code: string) => string;
  }): Promise<void> {
    // 省略属性初始化
    ...
    // 等待iframe初始化
    await this.iframeReady;

    // 处理子应用自定义fetch
    const iframeWindow = this.iframe.contentWindow;
    const iframeFetch = fetch
      ? (input: RequestInfo, init?: RequestInit) =>
          fetch(typeof input === "string" ? getAbsolutePath(input, (this.proxyLocation as Location).href) : input, init)
      : this.fetch;
    if (iframeFetch) {
      iframeWindow.fetch = iframeFetch;
      this.fetch = iframeFetch;
    }

    // 处理子应用路由同步
    if (this.execFlag && this.alive) {
      // 当保活模式下子应用重新激活时，只需要将子应用路径同步回主应用
      syncUrlToWindow(iframeWindow);
    } else {
      // 先将url同步回iframe，然后再同步回浏览器url
      syncUrlToIframe(iframeWindow);
      syncUrlToWindow(iframeWindow);
    }

    // 初始化template模板
    this.template = template ?? this.template;

    /* 降级处理 */
    if (this.degrade) {
      const iframeBody = rawDocumentQuerySelector.call(iframeWindow.document, "body") as HTMLElement;
      const { iframe, container } = initRenderIframeAndContainer(this.id, el ?? iframeBody, this.degradeAttrs);
      this.el = container;
      // 销毁js运行iframe容器内部dom
      if (el) clearChild(iframeBody);
      // 修复vue的event.timeStamp问题
      patchEventTimeStamp(iframe.contentWindow, iframeWindow);
      // 当销毁iframe时主动unmount子应用
      iframe.contentWindow.onunload = () => {
        this.unmount();
      };
      if (this.document) {
        if (this.alive) {
          iframe.contentDocument.replaceChild(this.document.documentElement, iframe.contentDocument.documentElement);
          // 保活场景需要事件全部恢复
          recoverEventListeners(iframe.contentDocument.documentElement, iframeWindow);
        } else {
          await renderTemplateToIframe(iframe.contentDocument, this.iframe.contentWindow, this.template);
          // 非保活场景需要恢复根节点的事件，防止react16监听事件丢失
          recoverDocumentListeners(this.document.documentElement, iframe.contentDocument.documentElement, iframeWindow);
        }
      } else {
        await renderTemplateToIframe(iframe.contentDocument, this.iframe.contentWindow, this.template);
      }
      this.document = iframe.contentDocument;
      return;
    }

    if (this.shadowRoot) {
      /*
       document.addEventListener was transfer to shadowRoot.addEventListener
       react 16 SyntheticEvent will remember document event for avoid repeat listen
       shadowRoot have to dispatchEvent for react 16 so can't be destroyed
       this may lead memory leak risk
       */
      this.el = renderElementToContainer(this.shadowRoot.host, el);
      if (this.alive) return;
    } else {
      // 预执行无容器，暂时插入iframe内部触发Web Component的connect
      const iframeBody = rawDocumentQuerySelector.call(iframeWindow.document, "body") as HTMLElement;
      this.el = renderElementToContainer(createWujieWebComponent(this.id), el ?? iframeBody);
    }

    await renderTemplateToShadowRoot(this.shadowRoot, iframeWindow, this.template);
    this.patchCssRules();

    // inject shadowRoot to app
    this.provide.shadowRoot = this.shadowRoot;
  }

```

**start**

start用于启动子应用，主要功能：

1.  运行js
2.  处理兼容样式

**mount**

mount是子应用挂载生命周期，主要功能：

1.  子应用是异步渲染实例，子应用异步函数里面最后加上window.\__WUJIE.mount()来主动调用
2.  子应用是[保活模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html "https://wujie-micro.github.io/doc/guide/mode.html")就执行activated生命周期
3.  异步执行队列事件执行

**unmount**

unmount是子应用卸载生命周期（保活模式和使用proxyLocation.href跳转链接都不应该销毁shadow），主要功能：

1.  清理子应用过期的同步参数
2.  清空事件和定时器
3.  清空EventBus数据

**destroy**

destroy是子应用销毁的生命周期，它比unmount清空的更彻底，除了unmount做的事情，它的其他功能：

1.  清空所有数据和事件缓存、样式
2.  清空代理proxy和documet
3.  清空provide/inject
4.  清除head、body元素
5.  清除DOM和iframe沙箱
6.  删除沙箱缓存

# 通信

主应用和微应用通信需要借助框架设计实现，比如qiankun是通过自定义了一套发布订阅机制实现，micro app通过，icestark通过event emitter或Store实现,wujie通过自定义Event Bus、provide/inject实现

## qiankun的通信设计

### 用法

qiankun提供了`initGlobalState(state)`定义全局状态，返回通信方法:

* onGlobalStateChange 监听state变化，通知触发更新
* setGlobalState 更新state
* offGlobalStateChange 注销函数，不再监听state

主应用

```ini
import { initGlobalState, MicroAppStateActions } from 'qiankun';


// 初始化 state
const actions: MicroAppStateActions = initGlobalState(state);


actions.onGlobalStateChange((state, prev) => {
  // state: 变更后的状态; prev 变更前的状态
  console.log(state, prev);
});
actions.setGlobalState(state);
actions.offGlobalStateChange();


```

微应用

```javascript
// 从生命周期 mount 中获取通信方法，使用方式和 master 一致
export function mount(props) {
  props.onGlobalStateChange((state, prev) => {
    // state: 变更后的状态; prev 变更前的状态
    console.log(state, prev);
  });

  props.setGlobalState(state);
}

```

### 源码解析

qiankun的通信基于**发布订阅模式**设计

定义了两个变量,globalState用于记录全局数据状态，deps用于记录依赖

```typescript
// 空对象
let globalState: Record<string, any> = {};

// 记录依赖
const deps: Record<string, OnGlobalStateChangeCallback> = {};

```

初始化全局状态函数，主要做了以下几件事情:

1.  数据没有发生变化就告警
2.  数据变化了，记录新旧state，触发全局监听函数

```typescript
export function initGlobalState(state: Record<string, any> = {}) {
  ...
  // 当前state等于globalState(数据没有发生变化)
  if (state === globalState) {
    // 打印告警提示数据没有改变
    console.warn('[qiankun] state has not changed！');
  } else {
    // cloneDeep是lodash深拷贝方法
    // 记录旧的state
    const prevGlobalState = cloneDeep(globalState);
    // 新的state赋值给globalState
    globalState = cloneDeep(state);
    // 触发全局状态变更函数,第一个参数为新的state，第二个参数为旧的state
    emitGlobal(globalState, prevGlobalState);
  }
  return getMicroAppStateActions(`global-${+new Date()}`, true);
}

```

接下来看全局监听函数emitGlobal做了什么

```typescript
// 触发全局监听
function emitGlobal(state: Record<string, any>, prevState: Record<string, any>) {
  Object.keys(deps).forEach((id: string) => {
    // deps[id]是函数
    if (deps[id] instanceof Function) {
      // 传入参数执行，参数分别为state(新state),prevState(旧state)
      deps[id](cloneDeep(state), cloneDeep(prevState));
    }
  });
}

```

遍历deps的所有键，是函数就执行

**核心方法getMicroAppStateActions**

返回了三个方法:

1.  **onGlobalStateChange** 用于监听全局依赖,收集 setState 时所需要触发的依赖

> 限制条件:每个子应用只有一个激活状态的全局监听，新监听覆盖旧监听，若只是监听部分属性，请使用 onGlobalStateChange,这么设计是为了减少全局监听滥用导致的内存爆炸

3.  **setGlobalState** 更新依赖
4.  **offGlobalStateChange** 注销应用下的依赖

首先查看**onGlobalStateChange**，这个函数执行流程如下：

* 判断callback参数，不是函数类型就告警返回
* 根据传入id判断deps\[id\]是否存在，存在就告警旧的deps\[id\]会被覆盖
* callback参数赋值给deps\[id\]
* 判断fireImmediately，为真就立即执行callback函数

```typescript
export function getMicroAppStateActions(id: string, isMaster?: boolean): MicroAppStateActions {
  return {
    onGlobalStateChange(callback: OnGlobalStateChangeCallback, fireImmediately?: boolean) {
      // callback不是函数就打印告警
      if (!(callback instanceof Function)) {
        console.error('[qiankun] callback must be function!');
        return;
      }
      // deps[id]存在，说明之前已经定义过了，告警旧的deps[id]将会被覆盖
      if (deps[id]) {
        console.warn(`[qiankun] '${id}' global listener already exists before this, new listener will overwrite it.`);
      }
      // callback回调赋值给deps[id]
      deps[id] = callback;
      // fireImmediately为true 立即触发回调
      if (fireImmediately) {
        // 拿到新的state
        const cloneState = cloneDeep(globalState);
        // 触发回调，参数分别为新旧state
        callback(cloneState, cloneState);
      }
    },

    /**
     * setGlobalState 更新 store 数据
     *
     * 1. 对输入 state 的第一层属性做校验，只有初始化时声明过的第一层（bucket）属性才会被更改
     * 2. 修改 store 并触发全局监听
     *
     * @param state
     */
    setGlobalState(state: Record<string, any> = {}) {
      ...
    },

    // 注销该应用下的依赖
    offGlobalStateChange() {
      delete deps[id];
      return true;
    },
  };
}

```

**setGlobalState**更新store数据是如何做到的？

这个函数的返回值为boolean类型，如果执行了state更新就返回true反之返回false

* 如果新state等于旧state，就无需更新store(直接告警返回)
* 遍历新state，满足条件就加入changedKey数组(存储发生了变化的key)，浅拷贝更新state
* changedKey数组长度为0，则没有需要更新的数据，直接返回
* 传入新旧state执行emitGlobal更新数据，返回true

```typescript
 setGlobalState(state: Record<string, any> = {}) {
      // 如果新state等于旧state，就无需更新store(直接告警返回)
      if (state === globalState) {
        console.warn('[qiankun] state has not changed！');
        return false;
      }
      // 定义changedKeys变量，存储发生了变化的state的key 
      const changeKeys: string[] = [];
      // 拿到旧的state
      const prevGlobalState = cloneDeep(globalState);
      globalState = cloneDeep(
        // 遍历新的state
        Object.keys(state).reduce((_globalState, changeKey) => {
          // isMaster为真或者新的state有changedKey
          if (isMaster || _globalState.hasOwnProperty(changeKey)) {
            // 加入changedKeys数组
            changeKeys.push(changeKey);
            // 浅拷贝更改state
            return Object.assign(_globalState, { [changeKey]: state[changeKey] });
          }
          console.warn(`[qiankun] '${changeKey}' not declared when init state！`);
          return _globalState;
        }, globalState),
      );
      // 如果changedKeys数组为空，告警没有state发生了变化
      if (changeKeys.length === 0) {
        console.warn('[qiankun] state has not changed！');
        return false;
      }
      emitGlobal(globalState, prevGlobalState);
      return true;
    },

```

**offGlobalStateChange** 注销应用监听很简单，就两行代码

```arduino
offGlobalStateChange() 
      // 清空deps
      delete deps[id];
      // 返回true表明已经执行完成
      return true;
},

```

## micro app的通信设计

### 用法

参考[zeroing.jd.com/micro-app/d…](https://link.juejin.cn?target=https%3A%2F%2Fzeroing.jd.com%2Fmicro-app%2Fdocs.html%23%2Fzh-cn%2Fdata "https://zeroing.jd.com/micro-app/docs.html#/zh-cn/data")

### 原理

维护了三份数据：全局数据、主应用的数据、微应用的数据

**微应用数据类方法** 微应用可以使用以下方法:

* addDataListener 绑定监听函数
* removeDataListener 移除事件监听
* clearDataListener 清空微应用下的所有监听函数
* dispatch 微应用向主应用发送数据
* getData 获取主应用发送过来的数据

```kotlin
export class EventCenterForMicroApp extends EventCenterForGlobal {
  appName: string
  umdDataListeners?: {
    global: Set<CallableFunctionForInteract>,
    normal: Set<CallableFunctionForInteract>,
  }

  constructor (appName: string) {
    super()
    this.appName = formatAppName(appName)
    !this.appName && logError(`Invalid appName ${appName}`)
  }

  /**
   * add listener, 模拟主应用发送的数据
   * @param autoTrigger 是否主动触发一次，子应用异步渲染，主应用发送数据是同步的，为了能拿到子应用渲染前主应用发送的数据，可以将这个值传递true
   */
  addDataListener (cb: CallableFunctionForInteract, autoTrigger?: boolean): void {
    cb.__AUTO_TRIGGER__ = autoTrigger
    eventCenter.on(createEventName(this.appName, true), cb, autoTrigger)
  }

  /**
   * remove listener，移除数据监听
   * @param cb listener
   */
  removeDataListener (cb: CallableFunctionForInteract): void {
    isFunction(cb) && eventCenter.off(createEventName(this.appName, true), cb)
  }

  /**
   * get data from base app
   */
  getData (fromBaseApp = true): Record<PropertyKey, unknown> | null {
    return eventCenter.getData(createEventName(this.appName, fromBaseApp))
  }

  /**
   * dispatch data to base app
   * @param data data
   */
  dispatch (data: Record<PropertyKey, unknown>, nextStep?: CallableFunction, force?: boolean): void {
    removeDomScope()

    eventCenter.dispatch(
      createEventName(this.appName, false),
      data,
      (resArr: unknown[]) => isFunction(nextStep) && nextStep(resArr),
      force,
      () => {
        const app = appInstanceMap.get(this.appName)
        if (app?.container && isPlainObject(data)) {
          const event = new CustomEvent('datachange', {
            detail: {
              data: eventCenter.getData(createEventName(this.appName, false))
            }
          })

          getRootContainer(app.container).dispatchEvent(event)
        }
      })
  }

  forceDispatch (data: Record<PropertyKey, unknown>, nextStep?: CallableFunction): void {
    this.dispatch(data, nextStep, true)
  }

  /**
   * clear data from child app
   * @param fromBaseApp whether clear data from base app, default is false
   */
  clearData (fromBaseApp = false): void {
    eventCenter.clearData(createEventName(this.appName, fromBaseApp))
  }

  /**
   * clear all listeners
   */
  clearDataListener (): void {
    eventCenter.off(createEventName(this.appName, true))
  }
}

```

## icestark的通信设计

### 用法

参考官网示例[icestark.gitee.io/docs/guide/…](https://link.juejin.cn?target=https%3A%2F%2Ficestark.gitee.io%2Fdocs%2Fguide%2Fadvanced%2Fcommunication "https://icestark.gitee.io/docs/guide/advanced/communication")

### 原理

基于Event Emitter实现数据通信

```php
interface Hooks {
  // 触发事件函数 
  emit(key: StringSymbolUnion, value: any): void;
  // 监听
  on(key: StringSymbolUnion, callback: (value: any) => void): void;
  // 取消监听
  off(key: StringSymbolUnion, callback?: (value: any) => void): void;
  // 判断事件是否存在
  has(key: StringSymbolUnion): boolean;
}

```

定义Event类，继承了Hooks，实现了on/off/emit/has方法：

* emit为事件触发方法，根据事件key拿到事件名，事件为数组且长度大于1则遍历执行每一项
* on为事件监听方法，初始化数组存储所有需要监听的事件
* off为取消事件监听方法，根据事件名匹配，匹配到了则移除事件监听
* has用于判断当前监听的事件是否为大于0的数组

```typescript
class Event implements Hooks {
  eventEmitter: object;

  constructor() {
    // 初始化为空对象
    this.eventEmitter = {};
  }

  emit(key: StringSymbolUnion, ...args) {
    // 根据事件名获取事件
    const keyEmitter = this.eventEmitter[key];
    
    // 事件不为数组或长度为0，告警返回
    if (!isArray(keyEmitter) || (isArray(keyEmitter) && keyEmitter.length === 0)) {
      warn(`event.emit: no callback is called for ${String(key)}`);
      return;
    }
    // 遍历执行
    keyEmitter.forEach(cb => {
      cb(...args);
    });
  }

  on(key: StringSymbolUnion, callback: (value: any) => void) {
    // 事件名必须为字符串或symbol类型
    if (typeof key !== 'string' && typeof key !== 'symbol') {
      warn('event.on: key should be string / symbol');
      return;
    }
    // 传入的回调函数必须为函数
    if (callback === undefined || typeof callback !== 'function') {
      warn('event.on: callback is required, should be function');
      return;
    }
    // 对应事件名不存在，初始化为空数组
    if (!this.eventEmitter[key]) {
      this.eventEmitter[key] = [];
    }
    // 将回调函数加入事件数组 
    this.eventEmitter[key].push(callback);
  }

  off(key: StringSymbolUnion, callback?: (value: any) => void) {
    // 事件名必须为字符串或Symbol 
    if (typeof key !== 'string' && typeof key !== 'symbol') {
      warn('event.off: key should be string / symbol');
      return;

    }
    // 事件监听必须为数组
    if (!isArray(this.eventEmitter[key])) {
      warn(`event.off: ${String(key)} has no callback`);
      return;
    }
    // 回调函数不存在则返回
    if (callback === undefined) {
      this.eventEmitter[key] = undefined;
      return;
    }
    // 从事件监听数组中移除需要取消监听的回调函数
    this.eventEmitter[key] = this.eventEmitter[key].filter(cb => cb !== callback);
  }

  has(key: StringSymbolUnion) {
    // 根据key拿到事件
    const keyEmitter = this.eventEmitter[key];
    // 判断事件是否为大于0的数组
    return isArray(keyEmitter) && keyEmitter.length > 0;
  }
}

```

初始化逻辑

```csharp
const eventNameSpace = 'event';
// 从缓存中获取事件
let event = getCache(eventNameSpace);
// 事件不存在就根据通过new Event初始化
if (!event) {
  event = new Event();
  // 加入缓存
  setCache(eventNameSpace, event);
}

```

## wujie的通信设计

### 用法

wujie的通信有三种方法

**props**

用于主应用向微应用发送数据

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3b8008e56868490792144254e76fa84d~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1291&h=683&s=76249&e=png&b=fbfbfb)

**window**

用于主应用和微应用互相通信

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e318115128d41a58550341685c31a93~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1341&h=747&s=84735&e=png&b=ffffff)

**EventBus**

用于主应用和微应用、微应用和微应用通信

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a00fc68c6a643e5aa15e6f815dfccd9~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1287&h=1017&s=137872&e=png&b=292b30)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a246e3ce84d0459e80372a9bdcc35390~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=1235&h=479&s=74645&e=png&b=292b30)

### 原理

通过Event Bus实现

```kotlin
// 全部事件存储map
export const appEventObjMap = window.__POWERED_BY_WUJIE__
  ? window.__WUJIE.inject.appEventObjMap
  : new Map<String, EventObj>();
export class EventBus {
  private id: string;
  private eventObj: EventObj;

  constructor(id: string) {
    this.id = id;
    this.$clear();
    // 事件未定义过，加入事件map
    if (!appEventObjMap.get(this.id)) {
      appEventObjMap.set(this.id, {});
    }
    // 根据id拿到对应事件
    this.eventObj = appEventObjMap.get(this.id);
  }

  // 监听事件
  public $on(event: string, fn: Function): EventBus {
    // 拿到对应事件的回调函数
    const cbs = this.eventObj[event];
    // 回调函数不存在
    if (!cbs) {
      // 定义当前事件为fn并返回整个事件 
      this.eventObj[event] = [fn];
      return this;
    }
    // fn不在cbs回调中，加入cbs数组,返回整个事件
    if (!cbs.includes(fn)) cbs.push(fn);
    return this;
  }

  /** 任何$emit都会导致监听函数触发，第一个参数为事件名，后续的参数为$emit的参数 */
  public $onAll(fn: (event: string, ...args: Array<any>) => any): EventBus {
    return this.$on(WUJIE_ALL_EVENT, fn);
  }

  // 一次性监听事件
  public $once(event: string, fn: Function): void {
    const on = function (...args: Array<any>) {
      // 卸载事件 
      this.$off(event, on);
      // 传入参数
      fn(...args);
    }.bind(this);
    // 执行时间监听，监听后立即卸载(保证事件只执行一次)
    this.$on(event, on);
  }

  // 取消监听
  public $off(event: string, fn: Function): EventBus {
    // 根据事件名取出对应回调函数 
    const cbs = this.eventObj[event];
    // 事件不存在则返回  
    if (!event || !cbs || !cbs.length) {
      warn(`${event} ${WUJIE_TIPS_NO_SUBJECT}`);
      return this;
    }

    let cb;
    let i = cbs.length;
    // 遍历执行回调函数
    while (i--) {
      cb = cbs[i];
      if (cb === fn) {
        // 执行完成后从cbs数组中删除
        cbs.splice(i, 1);
        break;
      }
    }
    return this;
  }

  // 取消监听$onAll
  public $offAll(fn: Function): EventBus {
    return this.$off(WUJIE_ALL_EVENT, fn);
  }

  // 发送事件
  public $emit(event: string, ...args: Array<any>): EventBus {
    let cbs = [];
    let allCbs = [];
    
    // 组合拼接所有事件
    appEventObjMap.forEach((eventObj) => {
      if (eventObj[event]) cbs = cbs.concat(eventObj[event]);
      if (eventObj[WUJIE_ALL_EVENT]) allCbs = allCbs.concat(eventObj[WUJIE_ALL_EVENT]);
    });
    // 事件不存在告警
    if (!event || (cbs.length === 0 && allCbs.length === 0)) {
      warn(`${event} ${WUJIE_TIPS_NO_SUBJECT}`);
    } else {
      // 事件数组存在 遍历执行每一项
      try {
        for (let i = 0, l = cbs.length; i < l; i++) cbs[i](...args);
        for (let i = 0, l = allCbs.length; i < l; i++) allCbs[i](event, ...args);
      } catch (e) {
        error(e);
      }
    }
    return this;
  }

  // 清空当前所有的监听事件
  public $clear(): EventBus {
    const eventObj = appEventObjMap.get(this.id) ?? {};
    // 拿到所有事件名
    const events = Object.keys(eventObj);
    // 遍历events数组，删除每一个事件
    events.forEach((event) => delete eventObj[event]);
    return this;
  }
}


```

个人整理不易，如果有问题欢迎指出

下一篇文章将会整理对比wujie/micro app/icestark/qiankun框架的生命周期原理/路由系统/多实例激活/资源加载

这篇文章如果对你有帮助，欢迎关注/点赞/评论

# 参考文档

[icestark官方文档](https://link.juejin.cn?target=https%3A%2F%2Ficestark.gitee.io%2Fdocs%2Fguide "https://icestark.gitee.io/docs/guide")

[qiankun github](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fumijs%2Fqiankun "https://github.com/umijs/qiankun")

[wujie官方文档](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2F "https://wujie-micro.github.io/doc/guide/")