

#### 背景
<br/>color1
场景1： 做竞标的项目，不想被对方抓取到用户的信息（地址，联系方式 公司名字 等等）

场景2： 防止网络爬虫抓取关键核心数据

场景3： 一些销售系统，防止被竞争对手拿到关键有用信息，反推一些客户资料

<br/>



[代码地址](https://gitee.com/sohucw/vite-full.git) 注意是 terser分支

视频讲解： [https://v.douyin.com/iyNBDrnG/](https://v.douyin.com/iyNBDrnG/) 

#### 代码逻辑，开发编码插件
```javascript
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite';
import Components from 'unplugin-vue-components/vite';
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers';

import * as Babel from '@babel/core';
import traverse from '@babel/traverse';
import generate from '@babel/generator';

const isSuff = (id: string) => {
    if (id.endsWith('.vue')) {
        return true;
    } else {
        return false;
    }
};
const terserPlugin = (text: '') => {
    return {
        name: 'terserPlugin',
        // vite插件里面有很多钩子，transform钩子就是用来做代码转换的
        transform(code: string, id: string) {
            // 我们要处理什么文件呢？ vue  ts js 文件
            if (isSuff(id)) {
                // console.log('id', id);
                const { ast } = Babel.transformSync(code, {
                    ast: true
                });
                traverse.default(ast, {
                    StringLiteral(path) {
                        if (path.node.value.includes(text)) {
                            console.log(path.node.value);
                            // 运行的时候执行， 编译的时候不执行
                            // path.node.value = encodeURIComponent(path.node.value);
                            path.replaceWithSourceString(
                                `decodeURIComponent('${encodeURIComponent(path.node.value)}')`
                            );
                        }
                    }
                });
                const { code: newCode } = generate.default(ast);
                return newCode;
            }
        }
    };
};
// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        vue(),
        terserPlugin('大伟公司'),
        AutoImport({
            resolvers: [ElementPlusResolver()],
            dts: fileURLToPath(new URL('./auto-imports.d.ts', import.meta.url))
        }),
        Components({
            resolvers: [ElementPlusResolver()],
            dts: fileURLToPath(new URL('./components.d.ts', import.meta.url)),
            dirs: [fileURLToPath(new URL('./src/components', import.meta.url))]
        })
    ],
    resolve: {
        alias: {
            '@': fileURLToPath(new URL('./src', import.meta.url)),
            replacement: fileURLToPath(new URL('./src/components', import.meta.url))
        }
    },
    server: {
        port: 8081
    },
    build: {
        // outDir: 'dist',
        // emptyOutDir: true,
        rollupOptions: {
            output: {
                manualChunks: {
                    vue: ['vue'],
                    'vue-router': ['vue-router'],
                    about: ['./src/views/AboutView.vue'],
                    terser: ['./src/views/terser/index.vue']
                }
            }
        }
    }
});

```



ast网站：[https://astexplorer.net/](https://astexplorer.net/)

