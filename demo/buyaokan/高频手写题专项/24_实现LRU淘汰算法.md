**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font>****<font style="color:rgb(44, 62, 80);"> 缓存算法是一个非常经典的算法，在很多面试中经常问道，不仅仅包括前端面试</font>**

<br/>color1
<font style="color:rgb(199, 37, 78);">LRU</font><font style="color:rgb(85, 85, 85);"> 英文全称是 </font><font style="color:rgb(199, 37, 78);">Least Recently Used</font><font style="color:rgb(85, 85, 85);">，英译过来就是”</font>**<font style="color:rgb(85, 85, 85);">最近最少使用</font>**<font style="color:rgb(85, 85, 85);">“的意思。</font><font style="color:rgb(199, 37, 78);">LRU</font><font style="color:rgb(85, 85, 85);"> 是一种常用的页面置换算法，选择最近最久未使用的页面予以淘汰。该算法赋予每个页面一个访问字段，用来记录一个页面自上次被访问以来所经历的时间 </font><font style="color:rgb(199, 37, 78);">t</font><font style="color:rgb(85, 85, 85);">，当须淘汰一个页面时，选择现有页面中其 </font><font style="color:rgb(199, 37, 78);">t</font><font style="color:rgb(85, 85, 85);"> 值最大的，即最近最少使用的页面予以淘汰</font>

<br/>

<font style="color:rgb(44, 62, 80);">通俗的解释：</font>

<br/>color1
<font style="color:rgb(85, 85, 85);">假如我们有一块内存，专门用来缓存我们最近发访问的网页，访问一个新网页，我们就会往内存中添加一个网页地址，随着网页的不断增加，内存存满了，这个时候我们就需要考虑删除一些网页了。这个时候我们找到内存中最早访问的那个网页地址，然后把它删掉。这一整个过程就可以称之为 </font><font style="color:rgb(199, 37, 78);">LRU</font><font style="color:rgb(85, 85, 85);"> 算法</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718788551340-9bc0a88e-3c89-44e4-bb4a-37237671422a.png)

<font style="color:rgb(44, 62, 80);">上图就很好的解释了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法在干嘛了，其实非常简单，无非就是我们往内存里面添加或者删除元素的时候，遵循</font>**<font style="color:rgb(44, 62, 80);">最近最少使用原则</font>**

**<font style="color:rgb(44, 62, 80);">使用场景</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法使用的场景非常多，这里简单举几个例子即可：</font>

+ <font style="color:rgb(44, 62, 80);">我们操作系统底层的内存管理，其中就包括有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法</font>
+ <font style="color:rgb(44, 62, 80);">我们常见的缓存服务，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redis</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等等</font>
+ <font style="color:rgb(44, 62, 80);">比如浏览器的最近浏览记录存储</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keep-alive</font><font style="color:rgb(44, 62, 80);">组件使用了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font><font style="color:rgb(44, 62, 80);">算法</font>

**<font style="color:rgb(44, 62, 80);">梳理实现 LRU 思路</font>**

+ <font style="color:rgb(44, 62, 80);">特点分析：</font>
    - <font style="color:rgb(44, 62, 80);">我们需要一块有限的存储空间，因为无限的化就没必要使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU</font><font style="color:rgb(44, 62, 80);">算发删除数据了。</font>
    - <font style="color:rgb(44, 62, 80);">我们这块存储空间里面存储的数据需要是有序的，因为我们必须要顺序来删除数据，所以可以考虑使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数据结构来存储，不能使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">，因为它是无序的。</font>
    - <font style="color:rgb(44, 62, 80);">我们能够删除或者添加以及获取到这块存储空间中的指定数据。</font>
    - <font style="color:rgb(44, 62, 80);">存储空间存满之后，在添加数据时，会自动删除时间最久远的那条数据。</font>
+ <font style="color:rgb(44, 62, 80);">实现需求：</font>
    - <font style="color:rgb(44, 62, 80);">实现一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRUCache</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型，用来充当存储空间</font>
    - <font style="color:rgb(44, 62, 80);">采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数据结构存储数据，因为它的存取时间复杂度为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(1)</font><font style="color:rgb(44, 62, 80);">，数组为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(n)</font>
    - <font style="color:rgb(44, 62, 80);">实现</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，用来获取和添加数据</font>
    - <font style="color:rgb(44, 62, 80);">我们的存储空间有长度限制，所以无需提供删除方法，存储满之后，自动删除最久远的那条数据</font>
    - <font style="color:rgb(44, 62, 80);">当使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">获取数据后，该条数据需要更新到最前面</font>

**<font style="color:rgb(44, 62, 80);">具体实现</font>**

```javascript
class LRUCache {
  constructor(length) {
    this.length = length; // 存储长度
    this.data = new Map(); // 存储数据
  }
  // 存储数据，通过键值对的方式
  set(key, value) {
    const data = this.data;

    // 有的话 删除 重建放到map最前面
    if (data.has(key)) {
      data.delete(key)
    }

    data.set(key, value);

    // 如果超出了容量，则需要删除最久的数据
    if (data.size > this.length) {
      // 删除map最老的数据
      const delKey = data.keys().next().value;
      data.delete(delKey);
    }
  }
  // 获取数据
  get(key) {
    const data = this.data;
    // 未找到
    if (!data.has(key)) {
      return null;
    }
    const value = data.get(key); // 获取元素
    data.delete(key); // 删除元素
    data.set(key, value); // 重新插入元素到map最前面

    return value // 返回获取的值
  }
}
var lruCache = new LRUCache(5);
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set 方法</font><font style="color:rgb(44, 62, 80);">：往</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里面添加新数据，如果添加的数据存在了，则先删除该条数据，然后再添加。如果添加数据后超长了，则需要删除最久远的一条数据。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data.keys().next().value</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">便是获取最后一条数据的意思。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get 方法</font><font style="color:rgb(44, 62, 80);">：首先从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象中拿出该条数据，然后删除该条数据，最后再重新插入该条数据，确保将该条数据移动到最前面</font>

```javascript
// 测试

// 存储数据 set：

lruCache.set('name', 'test');
lruCache.set('age', 10);
lruCache.set('sex', '男');
lruCache.set('height', 180);
lruCache.set('weight', '120');
console.log(lruCache);
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718788551363-91cbbddd-8f60-42e6-b91e-d6242ff71eb7.png)

<font style="color:rgb(44, 62, 80);">继续插入数据，此时会超长，代码如下：</font>

```plain
lruCache.set('grade', '100');
console.log(lruCache);
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718788551451-1713bd5e-f67e-48bc-8251-70cf265140aa.png)

<font style="color:rgb(44, 62, 80);">此时我们发现存储时间最久的 name 已经被移除了，新插入的数据变为了最前面的一个。</font>

<font style="color:rgb(44, 62, 80);">我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">获取数据，代码如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718788551472-27f4175c-77c7-4337-a506-007d9ad9976b.png)

<font style="color:rgb(44, 62, 80);">我们发现此时</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sex</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段已经跑到最前面去了</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

<br/>color1
<font style="color:rgb(199, 37, 78);">LRU</font><font style="color:rgb(85, 85, 85);"> 算法其实逻辑非常的简单，明白了原理之后实现起来非常的简单。最主要的是我们需要使用什么数据结构来存储数据，因为 </font><font style="color:rgb(199, 37, 78);">map</font><font style="color:rgb(85, 85, 85);"> 的存取非常快，所以我们采用了它，当然数组其实也可以实现的。还有一些小伙伴使用链表来实现 </font><font style="color:rgb(199, 37, 78);">LRU</font><font style="color:rgb(85, 85, 85);">，这当然也是可以的。</font>

<br/>

