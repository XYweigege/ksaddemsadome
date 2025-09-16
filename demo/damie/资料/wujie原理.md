# wujie原理

声明文件



```
import type { plugin } from 'wujie'
type lifecycle = (appWindow: Window) => any;
interface Props {
    /** 唯一性用户必须保证 */
    name: string;
    /** 需要渲染的url */
    url: string;
    /** 需要渲染的html, 如果用户已有则无需从url请求 */
    html?: string;
    /** 渲染的容器 */
    loading?: HTMLElement;
    /** 路由同步开关， false刷新无效，但是前进后退依然有效 */
    sync?: boolean;
    /** 子应用短路径替换，路由同步时生效 */
    prefix?: { [key: string]: string };
    /** 子应用保活模式，state不会丢失 */
    alive?: boolean;
    /** 注入给子应用的数据 */
    props?: { [key: string]: any };
    /** js采用fiber模式执行 */
    fiber?: boolean;
    /** 子应用采用降级iframe方案 */
    degrade?: boolean;
    /** 自定义运行iframe的属性 */
    attrs?: { [key: string]: any };
    /** 自定义降级渲染iframe的属性 */
    degradeAttrs?: { [key: string]: any };
    /** 代码替换钩子 */
    replace?: (codeText: string) => string;
    /** 自定义fetch，资源和接口 */
    fetch?: (input: RequestInfo, init?: RequestInit) => Promise<Response>;
    /** 子应插件 */
    plugins: Array<plugin>;
    /** 子应用生命周期 */
    beforeLoad?: lifecycle;
    /** 没有做生命周期改造的子应用不会调用 */
    beforeMount?: lifecycle;
    afterMount?: lifecycle;
    beforeUnmount?: lifecycle;
    afterUnmount?: lifecycle;
    /** 非保活应用不会调用 */
    activated?: lifecycle;
    deactivated?: lifecycle;
};

export { Props } 

```





Proptype为什么要定义，  

```
import { startApp, bus } from 'wujie'
import { h, defineComponent, onMounted, getCurrentInstance, onBeforeUnmount } from 'vue'
import type { App, PropType } from 'vue'
import { Props } from './type'
const WuJie = defineComponent({
    props: {
        width: { type: String, default: "" },
        height: { type: String, default: "" },
        name: { type: String, default: "", required: true },
        loading: { type: HTMLElement, default: undefined },
        url: { type: String, default: "", required: true },
        sync: { type: Boolean, default: undefined },
        prefix: { type: Object, default: undefined },
        alive: { type: Boolean, default: undefined },
        props: { type: Object, default: undefined },
        attrs: { type: Object, default: undefined },
        replace: { type: Function as PropType<Props['replace']>, default: undefined },
        fetch: { type: Function as PropType<Props['fetch']>, default: undefined },
        fiber: { type: Boolean, default: undefined },
        degrade: { type: Boolean, default: undefined },
        plugins: { type: Array as PropType<Props['plugins']>, default: null },
        beforeLoad: { type: Function as PropType<Props['beforeLoad']>, default: null },
        beforeMount: { type: Function as PropType<Props['beforeMount']>, default: null },
        afterMount: { type: Function as PropType<Props['afterMount']>, default: null },
        beforeUnmount: { type: Function as PropType<Props['beforeUnmount']>, default: null },
        afterUnmount: { type: Function as PropType<Props['afterUnmount']>, default: null },
        activated: { type: Function as PropType<Props['activated']>, default: null },
        deactivated: { type: Function as PropType<Props['deactivated']>, default: null },
    },
    setup(props: Props, { emit }) {
        const instance = getCurrentInstance()
        const handlerEmit = (event: string, ...args: any[]) => {
            emit(event, ...args)
        }
        onMounted(() => {
            bus.$onAll(handlerEmit) //添加事件订阅
            //初始化无界
            startApp({
                name: props.name,
                url: props.url,
                el: instance?.refs.wujie as HTMLElement,
                loading: props.loading,
                alive: props.alive,
                fetch: props.fetch,
                props: props.props,
                attrs: props.attrs,
                replace: props.replace,
                sync: props.sync,
                prefix: props.prefix,
                fiber: props.fiber,
                degrade: props.degrade,
                plugins: props.plugins,
                beforeLoad: props.beforeLoad,
                beforeMount: props.beforeMount,
                afterMount: props.afterMount,
                beforeUnmount: props.beforeUnmount,
                afterUnmount: props.afterUnmount,
                activated: props.activated,
                deactivated: props.deactivated,
            })
        })

        onBeforeUnmount(() => {
            bus.$offAll(handlerEmit) //取消事件订阅
        })

        return () => h('div', {
            style: {
                width: 200,
                height: 200
            },
            ref: "wujie"
        }, '')
    }
})


WuJie.install = (app: App) => {
    app.component('wujie', WuJie)
}

export default WuJie

```







index.d.ts 声明文件



```
import { bus, preloadApp, destroyApp, setupApp } from "wujie";
import type { App } from 'vue';

declare const WujieVue: {
    bus: typeof bus;
    setupApp: typeof setupApp;
    preloadApp: typeof preloadApp;
    destroyApp: typeof destroyApp;
    install: (app: App) => void
};

export default WujieVue;

```







预加载

requestidlecallback 这个API 他会在浏览器空闲的时候执行，这时候有个疑问什么是浏览器空闲的时候？



主流的PC屏幕刷新率（FPS）大多在60Hz，即1秒钟对屏幕进行60次刷新，平均每次刷新耗时大概是16.6ms。
在一帧的输入渲染合成完成后才会有空闲时间，就是16.6ms之内完成输入渲染合成，空闲的时间才会留给 requestidlecallback

第二种情况是没有任务执行浏览器会有50ms空闲时间，这个时间段也会执行 requestidlecallback

一帧内做了什么事呢

1.处理用户的事件，就是event 例如 click，input change 等。

2.执行定时器任务

3.执行 requestAnimationFrame

4.执行dom 的回流与重绘

5.计算更新图层的绘制指令

6.绘制指令合并主线程 如果有空余时间会执行 requestidlecallback



这个跟react 的 fiber 的有什么关系?

因为react也有该机制 但是react并没有用 requestidlecallback，说是这个东西经过测试可能会超过16ms，超过16ms绘制就会看起来很卡 所以react16是用的 requestAnimationFrame + postMessage 实现的那为什么不用setTimeOut setTimeOut 及时为0 也会有一个最小毫秒延迟4ms，所以是用了postMessage，react18又换成了MessageChannel 实现了队列方式去执行任务。