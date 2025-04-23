![](https://cdn.nlark.com/yuque/0/2025/png/207857/1739111895453-e871933e-0f82-4e48-a7d2-9a1987c065be.png)

**Vue项目入口文件main.ts**

```javascript
import { createApp } from "vue";
import App from "./App.vue";
import "@/styles/reset.less";
import "@/styles/common.less";
import "@/assets/iconfont/iconfont.css";
// import "@/assets/fonts/font.scss";
import elementIcon from "@/plugins/element-icon";
import "element-plus/dist/index.css";
import "@/styles/element.less";
import "@/styles/element-dark.less";
import "element-plus/theme-chalk/dark/css-vars.css";
// 路由
import router from "@/routers/index";
// i18n
import I18n from "@/languages/index";
// pinia
import pinia from "@/stores/index";
// svg icons
import "virtual:svg-icons-register";
// 全局错误提示
import errorHandler from "@/utils/errorHandler";
//  自定义指令
import directives from "@/directives/index";
//  mockJs
import "./mock/index";

const app = createApp(App);

app.config.errorHandler = errorHandler;

app.use(router).use(I18n).use(pinia).use(elementIcon).use(directives).mount("#app");

```



**具体的demo讲解**

```javascript
class Calculate {
  // 加法
  add(a: number, b: number): number {
    return a + b;
  }

  // 减法
  subtract(a: number, b: number): number {
    return a - b;
  }

  // 乘法
  multiply(a: number, b: number): number {
    return a * b;
  }

  // 除法
  divide(a: number, b: number): number {
    if (b === 0) {
      throw new Error("Cannot divide by zero");
    }
    return a / b;
  }
}

// 使用示例
const calculator = new Calculate();

console.log(calculator.add(2, 3));      // 5
console.log(calculator.subtract(10, 4)); // 6
console.log(calculator.multiply(3, 5));  // 15
console.log(calculator.divide(10, 2));   // 5

```

### **说明**
+ 这是一个普通的计算器类，包含基础的**加、减、乘、除**方法。
+ `divide` 方法里，**防止除以 **`**0**`，避免抛出错误。
+ 使用时直接创建 `Calculate` 实例，并调用相应方法进行计算。



<br/>color1
为了实现插件化思想的封装，除了基础的加减乘除方法外，我们还可以通过插件机制动态添加新的计算方法。插件化使得原有的计算器类可以在不修改其代码的情况下扩展更多的功能。下面是如何封装和调用插件化计算器的实现

<br/>

### **插件化思想封装**
1. **基础计算器**：提供基本的加减乘除运算方法。
2. **插件注册与管理**：通过 `usePlugin` 方法将外部自定义的运算插件注册到计算器中。
3. **插件调用**：通过 `execute` 方法执行已注册的插件。

```javascript
class Calculate {
  private plugins: Record<string, (a: number, b: number) => number> = {};

  // 加法
  add(a: number, b: number): number {
    return a + b;
  }

  // 减法
  subtract(a: number, b: number): number {
    return a - b;
  }

  // 乘法
  multiply(a: number, b: number): number {
    return a * b;
  }

  // 除法
  divide(a: number, b: number): number {
    if (b === 0) {
      throw new Error("Cannot divide by zero");
    }
    return a / b;
  }

  // 插件注册方法
  usePlugin(name: string, fn: (a: number, b: number) => number): void {
    if (this.plugins[name]) {
      console.warn(`Plugin ${name} already exists and will be overwritten.`);
    }
    this.plugins[name] = fn;
  }

  // 执行插件方法
  execute(name: string, a: number, b: number): number {
    const fn = this.plugins[name];
    if (!fn) {
      throw new Error(`Plugin ${name} not found`);
    }
    return fn(a, b);
  }
}

// 使用示例
const calculator = new Calculate();

// 基本运算
console.log(calculator.add(5, 3));      // 8
console.log(calculator.subtract(10, 3)); // 7
console.log(calculator.multiply(2, 4));  // 8
console.log(calculator.divide(12, 4));   // 3

// 插件化扩展：注册一个新的插件 `power`，用于计算幂
calculator.usePlugin("power", (a, b) => Math.pow(a, b));

// 使用 `power` 插件
console.log(calculator.execute("power", 2, 3)); // 8

// 插件化扩展：注册一个新的插件 `modulus`，用于计算取模
calculator.usePlugin("modulus", (a, b) => a % b);

// 使用 `modulus` 插件
console.log(calculator.execute("modulus", 10, 3)); // 1

```

### **说明**
1. **基础方法**：
    - `add`, `subtract`, `multiply`, `divide`：这些是基本的数学运算方法。
2. **插件管理**：
    - `usePlugin(name: string, fn: Function)`：用来注册新的插件。插件通过名字进行标识，插件方法可以是任何接受两个参数（`a` 和 `b`）并返回计算结果的函数。
    - `execute(name: string, a: number, b: number)`：根据插件的名字执行相应的插件方法。
3. **插件调用**：
    - 插件方法在外部注册后，可以通过 `execute` 方法动态调用。
    - 比如，在上述示例中，`power` 插件计算 `a^b`，而 `modulus` 插件计算 `a % b`。
4. **插件覆盖**：
    - 如果注册一个已存在名称的插件，系统会发出警告并覆盖原有插件，确保最新的插件生效。

### **优势**
+ **灵活扩展**：插件可以随时被添加、替换或移除，能够动态增加新的功能，符合插件化架构的思想。
+ **模块化**：各个功能（如幂运算、取模等）被封装成独立的插件，可以进行单独管理。
+ **可维护性强**：新的运算功能无需修改原有代码，只需注册新的插件即可。

这个插件化设计不仅限于数学计算，任何需要可扩展功能的场景都可以使用类似的设计模式。

