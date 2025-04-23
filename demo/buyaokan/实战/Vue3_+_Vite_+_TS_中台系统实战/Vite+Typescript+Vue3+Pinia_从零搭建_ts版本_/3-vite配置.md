## 初始内容
<font style="color:rgb(51, 51, 51);">项目初建后</font>，**vite.config.ts** <font style="color:rgb(51, 51, 51);">的默认内容如下：</font>

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
})
```

## 配置别名
### <font style="color:rgb(51, 51, 51);">1. 安装@types/node</font>
```shell
@types/nodenpm i @types/node -D
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730706052212-03c46732-5db6-43dd-a7c1-ba14a2ca9c68.png)

### <font style="color:rgb(51, 51, 51);">2. 修改 vite.config.ts</font>
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

// 路径查找
const pathResolve = (dir: string): string => {
  return resolve(__dirname, ".", dir);
};

// 设置别名，还可以添加其他路径
const alias: Record<string, string> = {
  "@": pathResolve("src"),
  "@views": pathResolve("src/views"),
  "@store": pathResolve("src/store/modules")
};

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias
  },
})
```

### <font style="color:rgb(51, 51, 51);">3. 使用</font>
<font style="color:rgb(51, 51, 51);">比如，修改</font> `App.vue`<font style="color:rgb(51, 51, 51);">:</font>

```vue
import HelloWorld from '@/components/HelloWorld.vue'
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730706052303-3de7c2aa-ee61-4208-a14f-a02896140aed.png)

## 配置环境变量
### <font style="color:rgb(51, 51, 51);">1. 新建env文件</font>
根目录下新建 .env、.env.development、.env.production 三个文件。

.env 文件内新增内容：

```javascript
# 本地运行端口号
VITE_PORT = 9001
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.env.development</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件内新增内容：</font>

```bash
# 本地环境
VITE_USER_NODE_ENV = development

# 公共基础路径
VITE_PUBLIC_PATH = /
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.env.production</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件内新增内容：</font>

```plain
# 线上环境
VITE_USER_NODE_ENV = production
 
# 公共基础路径
VITE_PUBLIC_PATH = /
```

### <font style="color:rgb(51, 51, 51);">2. 环境变量统一处理</font>
<font style="color:rgb(51, 51, 51);">根目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">build</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，其目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`<font style="color:rgb(51, 51, 51);">，内容如下：</font>

```javascript
// 环境变量处理方法
export function wrapperEnv(envConf: Recordable): ViteEnv {
  const ret: any = {};

  for (const envName of Object.keys(envConf)) {
    let realName = envConf[envName].replace(/\\n/g, "\n");
    realName = realName === "true" ? true : realName === "false" ? false : realName;
    if (envName === "VITE_PORT") realName = Number(realName);
    ret[envName] = realName;
    if (typeof realName === "string") {
      process.env[envName] = realName;
    } else if (typeof realName === "object") {
      process.env[envName] = JSON.stringify(realName);
    }
  }
  return ret;
}
```

### <font style="color:rgb(51, 51, 51);">3. 环境类型定义</font>
<font style="color:rgb(51, 51, 51);">在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">types\index.d.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件里新增对</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Recordable</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">ViteEnv</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的类型定义：</font>

```javascript
type Recordable<T = any> = Record<string, T>;

interface ViteEnv {
  VITE_USER_NODE_ENV: "development" | "production";
  VITE_PUBLIC_PATH: string;
  VITE_PORT: number;
}
```

<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">tsconfig.json</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件，将</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">build</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹内的文件包含进去：</font>

```json
"include": [ // 需要检测的文件
  "src/**/*.ts",
  "build/*.ts",
  "src/**/*.d.ts",
  "src/**/*.tsx",
  "src/**/*.vue",
  "mock/*.ts",
  "types/*.d.ts",
  "vite.config.ts"
],
```

<font style="color:rgb(51, 51, 51);">同理，修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">tsconfig.node.json</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件：</font>

```javascript
"include": [
  "build/*.ts",
  "types/*.d.ts",
  "vite.config.ts"
]
```

### <font style="color:rgb(51, 51, 51);">4. 使用</font>
<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vite.config.ts</font>`<font style="color:rgb(51, 51, 51);">：</font>

```javascript
import { defineConfig, loadEnv, ConfigEnv, UserConfig } from "vite"
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'
import { wrapperEnv } from './build'

// 路径查找
const pathResolve = (dir: string): string => {
  return resolve(__dirname, ".", dir);
};

// 设置别名，还可以添加其他路径
const alias: Record<string, string> = {
  "@": pathResolve("src"),
  "@views": pathResolve("src/views"),
  "@store": pathResolve("src/store")
};

// https://vitejs.dev/config/
export default defineConfig(({ mode }: ConfigEnv): UserConfig => {
  const root = process.cwd()
  const env = loadEnv(mode, root)
  const viteEnv = wrapperEnv(env)

  return {
    base: viteEnv.VITE_PUBLIC_PATH,
    plugins: [vue()],
    resolve: {
      alias
    },
    server: {
      host: "0.0.0.0",
      port: viteEnv.VITE_PORT,
      https: false,
      open: true,
      // 本地跨域代理 https://cn.vitejs.dev/config/server-options.html#server-proxy
      proxy: {
        "^/api": {
          target: "http://192.168.1.4:8688",
          changeOrigin: true,
          rewrite: path => path.replace(/^\/api/, "")
        }
      }
    },
  }
})
```

## 目录更新
<font style="color:rgb(51, 51, 51);">当前项目目录如下：</font>

```markdown
|   .env
|   .env.development
|   .env.production
|   .gitignore
|   index.html
|   package-lock.json
|   package.json
|   README.md
|   tree.txt
|   tsconfig.json
|   tsconfig.node.json
|   vite.config.ts
|   
+---.vscode
|       extensions.json
|       
+---build
|       index.ts
|              
+---node_modules 
+---public
|       vite.svg
|       
+---src
|   |   App.vue
|   |   main.ts
|   |   style.css
|   |   vite-env.d.ts
|   |   
|   +---assets
|   |       vue.svg
|   |       
|   \---components
|           HelloWorld.vue
|           
\---types
        index.d.ts
```

 

