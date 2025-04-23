
<font style="color:rgb(43, 43, 43);">程序运行中会有一些垃圾数据不再使用，需要及时释放出去，如果我们没有及时释放，这就是</font>**<font style="color:rgb(38, 198, 218);">内存泄露</font>**<font style="color:rgb(43, 43, 43);">。然而</font><font style="color:rgb(38, 198, 218);">GC</font><font style="color:rgb(43, 43, 43);">负责自动管理内存，确保不再使用的对象能够被及时清理，防止内存泄漏和内存溢出，从而保证浏览器的稳定运行和用户体验的流畅。</font>

<br/>

#### 浏览器的垃圾回收过程
浏览器的垃圾回收过程通常包括以下几个步骤：

1. **标记阶段**：垃圾回收器从根（roots）开始，递归访问对象的属性，将访问过的对象都标记为“活动”的。根通常是全局对象，或者是执行上下文栈中的变量和函数。
2. **清除阶段**：在标记阶段完成后，垃圾回收器会遍历堆中的所有对象，将那些没有被标记为“活动”的对象识别为垃圾，并释放它们占用的内存。

 

#### 浏览器的垃圾回收算法
浏览器通常使用以下几种垃圾回收算法：

1. **引用计数算法**：每个对象都有一个引用计数器，当有一个地方引用它时，**计数器加1**；当引用被删除或超出作用域时，**计数器减1**。当计数器为0时，对象就被视为垃圾。但这种算法存在循环引用的问题，因此现代浏览器已不再使用。
2. **标记-清除算法**：这是目前浏览器中最常用的垃圾回收算法。它分为标记和清除两个阶段，如前所述。标记-清除算法可以处理循环引用问题，但可能会导致**内存碎片化**。
3. **标记-整理算法**：这种算法在标记-清除的基础上，将所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。这样可以解决内存碎片化的问题，但效率相对较低。



#### 标记回收
<font style="color:rgb(43, 43, 43);">在我们开发过程中，如果想让</font><font style="color:rgb(38, 198, 218);">GC</font><font style="color:rgb(43, 43, 43);">回收一个对象，我们可以这样做：</font>

```javascript
let a = {}
a = null
```


我们可以给引用的值设置设置为null，这时这个对象就失去了所有对它的引用，从而变得不可达。GC在运行时，会检测到这些不可达的对象，并将它们标记为垃圾，进而在后续的某个时刻释放它们占用的内存。

<br/>

  
<font style="color:rgb(43, 43, 43);">但是如果一个对象被多次引用时，例如作为另一对象的键、值或子元素时，将该对象引用设置为</font><font style="color:rgb(38, 198, 218);">null</font><font style="color:rgb(43, 43, 43);">，该对象不会被回收。</font>

```javascript
let a = {}
let b = {}

b = [a]
a = null
console.log(b) // [{}]

```

 

#### WeakMap VS Map
先说说他们的区别：

1. 类型：Map的键可以是任何类型的值，包括基本类型（如字符串或数字）和对象引用。而WeakMap的键只能是对象类型。这是因为WeakMap的设计初衷是存储对象及其对应的值，并且其键对对象的引用是**弱**的，不会阻止GC回收键所引用的对象。
2. 引用关系：Map中的键值对是强引用关系，只要Map对象存在，其中的键值对就不会被自动回收。相反，WeakMap中的键值对是弱引用关系。如果WeakMap中使用的某个键对象没有被其他地方引用，那么在垃圾回收时，这个键对象及其对应的值都会被自动回收。
3. 可枚举性与大小：Map支持对键和值进行迭代，可以使用size属性获取键值对的数量，也可以使用clear方法清空Map。然而，WeakMap不支持对键和值进行迭代，也没有size属性和clear方法，无法获取WeakMap的所有键或值。

```javascript
// 创建一个Map对象  
let map = new Map();  
  
// 创建一个对象作为键  
let key = {};  
  
// 在Map中存储键值对  
map.set(key, 'value');  
  
// 此时，即使没有其他地方引用key对象，它也不会被垃圾回收，因为Map仍然强引用着它  
  
// 创建一个WeakMap对象  
let weakMap = new WeakMap();  
  
// 使用相同的对象作为键  
weakMap.set(key, 'weak value');  
  
// 假设我们现在删除了所有对key对象的引用（除了在WeakMap中的引用）  
key = null;  
  
// 在下一次垃圾回收时，key对象会被回收，因为它在WeakMap中的引用是“弱”的  
// 因此，与key对象相关联的值也会从WeakMap中消失
 
```


WeakMap和Map在处理键对象引用时的不同行为。在Map中，即使没有其他地方引用键对象，它也不会被垃圾回收，因为Map强引用着它。而在WeakMap中，如果键对象没有其他强引用，它会在垃圾回收时被回收，同时与之相关联的值也会从WeakMap中消失。

<br/>



#### WeakMap 回收过程
+ process.memoryUsage(): 是 Node.js 中的一个方法，用于获取 Node.js 进程的内存使用情况。它返回一个对象，描述了 Node.js 进程的内存使用情况，单位是字节（bytes）。
+ global.gc()：配合 **--expose-gc** 实现手动GC。

 

```javascript
const toMb = (num) => num / 1024 / 1024 + "MB";

if (global.gc === undefined) {
    console.error("GC is 不支持. 请使用--expose-gc启动node.");
    process.exit(1); // 退出进程
}

const weakMap = new WeakMap();

let arr = new Array(100 * 10000);

weakMap.set(arr, 1);
// console.log("memoryUsage大小:", process.memoryUsage());
// 打印当前的内存使用情况
console.log("当前内存使用情况:", toMb(process.memoryUsage().heapTotal));

// 移除对arr的所有引用，除了 WeakMap 中的那个
arr = null;

// 请求垃圾回收
global.gc();

// 等待一段时间，让垃圾回收器有机会运行
setTimeout(() => {
    // 再次打印内存使用情况
    console.log("GC之后的内存使用情况:", toMb(process.memoryUsage().heapTotal));
    // 尝试从 WeakMap 中获取之前存储的值
    // 如果 arr 已经被垃圾回收，这里将返回 undefined
    const value = weakMap.get(arr);
    console.log("WeakMap 中的值:", value); // 应该输出 undefined
}, 100); // 等待 100 毫秒以确保垃圾回收器有时间运行

```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1719282766256-cd0fb515-864a-4e1d-a709-dbb156b7695e.png)

#### 实际应用场景


**1：WeakMap**： 在开发中我们可能需要将某些数据与DOM元素关联起来。使用WeakMap，我们可以将DOM元素作为键，相关的数据作为值。这样，当DOM元素被从DOM树中移除时，由于WeakMap的弱引用特性，与该元素关联的数据也会自动被垃圾回收，无需手动清理，从而避免了内存泄漏。

```javascript
const root = document.querySelector('.root')
const map = new WeakMap()
map.set(root, '啦啦啦')

// 删除root节点
root.remove()
// 弱引用特性
map.get(root) // undefined
 
```

<font style="color:rgb(43, 43, 43);">由于 </font><font style="color:rgb(38, 198, 218);">root</font><font style="color:rgb(43, 43, 43);"> 元素已经被垃圾回收，</font><font style="color:rgb(38, 198, 218);">WeakMap</font><font style="color:rgb(43, 43, 43);"> 中不再有这个键的条目，所以返回的结果是 </font><font style="color:rgb(38, 198, 218);">undefined</font><font style="color:rgb(43, 43, 43);">。</font>

**<font style="color:rgb(38, 198, 218);">2：</font>****Map**：Map则是一个更通用的键值对集合，其键可以是任何类型（不仅仅是对象或字符串）。这使得Map在许多场景中都非常有用。

<font style="color:rgb(43, 43, 43);">利用</font><font style="color:rgb(38, 198, 218);">Map</font><font style="color:rgb(43, 43, 43);">实现计数器：</font>

```javascript
const str = 'hello world';  
const charNumMap = new Map();  
  
// 遍历字符串中的每个字符  
for (let char of str) {  
  // 如果字符已经在 Map 中，则增加计数  
  if (charNumMap.has(char)) {  
    charNumMap.set(char, charNumMap.get(char) + 1);  
  } else {  
    // 如果字符不在 Map 中，则添加到 Map 并设置计数为 1  
    charNumMap.set(char, 1);  
  }  
}  

```

<font style="color:rgb(43, 43, 43);"></font>



Map和WeakMap在不同场景下具有各自的优势和适用性。在选择使用哪种数据结构时，应根据具体需求来选择。

