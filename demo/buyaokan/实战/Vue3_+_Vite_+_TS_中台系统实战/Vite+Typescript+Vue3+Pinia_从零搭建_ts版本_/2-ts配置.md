## tsconfig.json
<font style="color:rgb(51, 51, 51);">文件修改如下:</font>

```json
{
  "compilerOptions": {
    "target": "ESNext", // 将代码编译为最新版本的 JS
    "useDefineForClassFields": true,
    "module": "ESNext", // 使用 ES Module 格式打包编译后的文件
    "lib": ["ESNext", "DOM", "DOM.Iterable"], // 引入 ES 最新特性和 DOM 接口的类型定义
    "skipLibCheck": true, // 跳过对 .d.ts 文件的类型检查
    "esModuleInterop": true, // 允许使用 import 引入使用 export = 导出的内容
    "sourceMap": true, // 用来指定编译时是否生成.map文件
    "allowJs": false, // 是否允许使用js
    "baseUrl": ".", // 查询的基础路径
    "paths": { // 路径映射,配合别名使用
      "@/*": ["src/*"],
      "@build/*": ["build/*"],
      "#/*": ["types/*"]
    },
 
    /* Bundler mode */
    "moduleResolution": "node", // 使用 Node/bundler 的模块解析策略
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true, // 允许引入 JSON 文件
    "isolatedModules": true, // 要求所有文件都是 ES Module 模块。
    "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
    "jsx": "preserve", // 保留原始的 JSX 代码，不进行编译
 
    /* Linting */
    "strict": true, // 开启所有严格的类型检查
    "noUnusedLocals": true, // 报告未使用的局部变量的错误
    "noUnusedParameters": true, // 报告函数中未使用参数的错误
    "noFallthroughCasesInSwitch": true // 确保switch语句中的任何非空情况都包含
  },
  "include": [ // 需要检测的文件
    "src/**/*.ts",
    "src/**/*.d.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "mock/*.ts",
    "types/*.d.ts",
    "vite.config.ts"
  ], 
  "exclude": [ // 不需要检测的文件
    "dist",
    "**/*.js",
    "node_modules"
  ],
  "references": [{ "path": "./tsconfig.node.json" }] // 为文件进行不同配置
}
```

## tsconfig.node.json
<font style="color:rgb(51, 51, 51);">修改文件如下：</font>

```json
{
  "compilerOptions": {
    "composite": true, // 对于引用项目必须设置该属性
    "skipLibCheck": true, // 跳过对 .d.ts 文件的类型检查
    "module": "ESNext", // 使用 ES Module 格式打包编译后的文件
    "moduleResolution": "Node", // 使用 Node/bundler 的模块解析策略
    "allowSyntheticDefaultImports": true // 允许使用 import 导入使用 export = 导出的默认内容 
  },
  "include": ["vite.config.ts"]
}
```

## 类型定义
<font style="color:rgb(51, 51, 51);">新建文件夹</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">types</font>`<font style="color:rgb(51, 51, 51);">，用来存放类型定义。比如新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.d.ts</font>`<font style="color:rgb(51, 51, 51);">：</font>

```typescript
type TargetContext = "_self" | "_blank";
type EmitType = (event: string, ...args: any[]) => void;
type AnyFunction<T> = (...args: any[]) => T;
type PropType<T> = VuePropType<T>;
type Writable<T> = {
  -readonly [P in keyof T]: T[P];
};
type Nullable<T> = T | null;
type NonNullable<T> = T extends null | undefined ? never : T;
 
interface Fn<T = any, R = T> {
  (...arg: T[]): R;
}
interface PromiseFn<T = any, R = T> {
  (...arg: T[]): Promise<R>;
}
```

<font style="color:rgb(51, 51, 51);">后续也可以新增其他文件，比如</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">global.d.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">存放全局定义，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">router.d.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">存放路由定义等</font>

## 类型检查命令
<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">package.json</font>`<font style="color:rgb(51, 51, 51);">，新增以下命令：</font>

```plain
"scripts": {
  "type-check": "vue-tsc --noEmit"
},
```

<font style="color:rgb(51, 51, 51);">保存后，运行 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">npm run type-check</font>`<font style="color:rgb(51, 51, 51);">，即可项目中是否有类型错误</font>

