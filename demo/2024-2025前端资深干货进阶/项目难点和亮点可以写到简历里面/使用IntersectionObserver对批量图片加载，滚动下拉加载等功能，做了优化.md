### 传统方式的问题：
传统的监听滚动事件方式可能会导致频繁的计算，影响页面性能。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1724287803511-49c70b79-39e9-45b2-8df0-0ec0e9422a91.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1724287776103-cf36e75e-95ed-4d04-8c49-bd2a0acc8fc2.png)

```javascript
        /**
         * 监听页面滚动
         * */
        window.addEventListener('scroll', function () {
            var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
            console.log(scrollTop);
        })
    </script>
 
 
```

使用传统的方式监听滚动事件需要手动计算元素的位置、判断元素是否进入视口，以及处理滚动事件的节流等。

##### <font style="color:rgb(13, 0, 22);">常见公式</font>
<br/>color1
#### **<font style="color:rgb(79, 79, 79);">当滚动条位于容器底部时，以下条件成立： </font>**
**<font style="color:rgb(79, 79, 79);">公式：</font>****<font style="color:rgb(173, 114, 13);">scrollHeight </font>****<font style="color:rgb(79, 79, 79);"> - </font>****<font style="color:rgb(173, 114, 13);">clientHeight </font>****<font style="color:rgb(79, 79, 79);"> = </font>****<font style="color:rgb(173, 114, 13);">scrollTop;</font>**

**<font style="color:rgb(79, 79, 79);">当然：</font>****<font style="color:rgb(173, 114, 13);">scrollTop </font>****<font style="color:rgb(79, 79, 79);">+ </font>****<font style="color:rgb(173, 114, 13);">clientHeight </font>****<font style="color:rgb(79, 79, 79);"> = </font>****<font style="color:rgb(173, 114, 13);">scrollHeight;</font>**

<br/>

### 优化后
##### 减少代码复杂性：
可以简化代码逻辑。

而通过 IntersectionObserver，只需定义回调函数，在元素进入或离开视口时触发相应操作，大大简化了代码

##### 更精确的可见性控制：
而 IntersectionObserver 是浏览器原生提供的 API，它使用异步执行，可以更高效地监听元素是否进入视口，减少了不必要的计算和性能开销



[代码地址](https://gitee.com/sohucw/vite-full.git)   chunk分支， /pic路由 



核心代码

```javascript
// 创建 IntersectionObserver 实例
const options = {
  root: null,  // 观察的元素和谁交叉  （视口就是null）
  rootMargin: '0px',
  threshold: 0.2  （0-1）// 元素交叉区域达到视口20%时触发回调
};

const observer = new IntersectionObserver(callback, options);

// 定义回调函数
function callback(entries, observer) {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const element = entry.target;
      element.classList.add('animate'); // 添加动画类或样式，触发动画效果

      observer.unobserve(element); // 停止观察该元素
    }
  });
}

// 找到所有需要触发动画效果的元素
const elements = document.querySelectorAll('.animated-element');

// 将元素添加到 IntersectionObserver 中进行观察
elements.forEach(element => {
  observer.observe(element);
});

```

