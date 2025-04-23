<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">代码：</font>[broadcastAnimation.zip](https://www.yuque.com/attachments/yuque/0/2024/zip/207857/1728530933199-f3e30726-c867-4191-b05f-49899f82ed58.zip)

 

<font style="color:rgb(51, 51, 51);">最近，相信大家一定被这么个动效给刷屏了：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530403322-acf0e232-d6c0-4cab-a1b6-c37c6608cff1.gif)

<font style="color:rgb(51, 51, 51);">以至于，基于这个效果的二次创作层出不穷，眼花缭乱。</font>

<font style="color:rgb(51, 51, 51);">基于跨窗口通信的弹弹球：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530403372-77692c62-1395-4120-a622-3cbf925fd1b0.gif)

<font style="color:rgb(51, 51, 51);">基于跨窗口通信的 Flippy Bird：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530403431-0df7a434-3e12-4812-a34d-d59e2f52fc5e.png)

<font style="color:rgb(51, 51, 51);">我也尝试制作了一个跨 Tab 窗口的 CSS 动画联动，效果如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530403418-431f9606-d08a-4075-920a-fb9cf8170590.gif)

<font style="color:rgb(51, 51, 51);">代码不多，核心代码 200 行</font>

<font style="color:rgb(51, 51, 51);">当然，本文的核心不是去一一剖析上面的效果具体的实现方式，而是讲讲其中比较关键的一个技术点：</font>

<font style="color:rgb(51, 51, 51);">而是</font>**<font style="color:rgb(51, 51, 51);">应用如何在多窗口下进行互相通信</font>**<font style="color:rgb(51, 51, 51);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530403609-76253f02-359d-4636-be56-b283f5f9877f.png)

<font style="color:rgb(51, 51, 51);">所谓</font>**<font style="color:rgb(51, 51, 51);">多窗口下进行互相通信，是指在浏览器中，不同窗口（包括不同标签页、不同浏览器窗口甚至不同浏览器实例）之间进行数据传输和通信的能力。</font>**

<font style="color:rgb(51, 51, 51);">当然，本文我们探讨的是</font>**<font style="color:rgb(51, 51, 51);">纯前端的跨 Tab 页面通信</font>**<font style="color:rgb(51, 51, 51);">，在非纯前端的方式下，我们可以借助诸如 Web Socket 等方式，藉由后端这个中间载体，进行跨页面通信。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530404037-94eece98-b2c9-463d-b4d6-950144c7b707.png)

<font style="color:rgb(51, 51, 51);">因此，本文我们更多的重心将放在，如何基于纯前端技术，实现</font>**<font style="color:rgb(51, 51, 51);">多窗口下进行互相通信。</font>**

<font style="color:rgb(51, 51, 51);">为了实现跨窗口通信，它应该需要具备以下能力：</font>

1. **<font style="color:rgb(51, 51, 51);">数据传输能力</font>**<font style="color:rgb(51, 51, 51);">：能够将数据从一个窗口发送到另一个窗口，以及接收来自其他窗口的数据。</font>
2. **<font style="color:rgb(51, 51, 51);">实时性</font>**<font style="color:rgb(51, 51, 51);">：能够实现实时或近实时的数据传输，以便及时更新不同窗口的内容。</font>
3. <font style="color:rgb(51, 51, 51);">安全性：确保通信过程中的数据安全，防止恶意窃取或篡改通信数据。当然，这个不是本文讨论的重点，但是是实际应用中不应该忽视的一个重点。</font>

## <font style="color:rgb(51, 51, 51);">方式一：Broadcast Channel()</font>
<font style="color:rgb(51, 51, 51);">Broadcast Channel 是一个较新的 Web API，用于在不同的浏览器窗口、标签页或框架之间</font>**<font style="color:rgb(51, 51, 51);">实现跨窗口通信</font>**<font style="color:rgb(51, 51, 51);">。它基于发布-订阅模式，允许一个窗口发送消息，并由其他窗口接收。</font>

<font style="color:rgb(51, 51, 51);">其核心步骤如下：</font>

1. <font style="color:rgb(51, 51, 51);">创建一个 BroadcastChannel 对象：在发送和接收消息之前，首先需要在每个窗口中创建一个 BroadcastChannel 对象，使用相同的频道名称进行初始化。</font>
2. <font style="color:rgb(51, 51, 51);">发送消息：通过 BroadcastChannel 对象的 postMessage() 方法，可以向频道中的所有窗口发送消息。</font>
3. <font style="color:rgb(51, 51, 51);">接收消息：通过监听 BroadcastChannel 对象的 message 事件，可以在窗口中接收到来自其他窗口发送的消息。</font>

<font style="color:rgb(51, 51, 51);">同时，Broadcast Channel 遵循浏览器的同源策略。这意味着只有在同一个协议、主机和端口下的窗口才能正常进行通信。如果窗口不满足同源策略，将无法互相发送和接收消息。</font>

<font style="color:rgb(51, 51, 51);">因为有同源限制，我们需要起一个服务，这里我基于 Vite 快速起了一个 Vue 项目，简单的基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.vue</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件下进行一个演示。</font>

<font style="color:rgb(51, 51, 51);">其核心代码非常简单：</font>

```javascript
<template>
  <div class="g-container" id="j-main">
    // ...  
  </div>
</template>

<script>
import { onMounted } from 'vue';

export default {
  setup() {
    function createBroadcastChannel() {
      broadcastChannel = new BroadcastChannel('broadcast');
      broadcastChannel.onmessage = handleMessage;
    }

    function sendMessage(data) {
      broadcastChannel.postMessage(data);
    }

    function handleMessage(event) {
        console.log('接收到 event', event);
        // TODO: 处理接收到信息后的逻辑
    }

    function resizeEventBind() {
      window.addEventListener('resize', () => {
         const pos = getCurPos();
         sendMessage(pos);
       });
    }

    // 计算当前元素距离显示器窗口右上角的距离
    function getCurPos() {
      const barHeight = window.outerHeight - window.innerHeight;
      const element = document.getElementById('j-main');
      const rect = element.getBoundingClientRect();

      // 获取元素相对于屏幕左上角的 X 和 Y 坐标
      const x = rect.left + window.screenX; // 元素左边缘相对于屏幕左边缘的距离
      const y = rect.top + window.screenY + barHeight;// 元素顶部边缘相对于屏幕顶部边缘的距离

      return [x, y];
    }
    
    onMounted(() => {
      createBroadcastChannel();
      resizeEventBind();
    });

    return {};
  }
};
</script>

<style lang="scss"></style>
```

<font style="color:rgb(51, 51, 51);">这里，我们的核心逻辑在于：</font>

+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">createBroadcastChannel()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数用于创建一个 BroadcastChannel 对象，并设置消息处理函数。</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">sendMessage(data)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数用于向 BroadcastChannel 发送消息。</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">handleMessage(event)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数用于处理接收到的消息。</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">resizeEventBind()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数用于监听窗口大小变化事件，并在事件发生时获取当前元素的位置信息，并通过</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">sendMessage()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数发送位置信息到 BroadcastChannel。</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">getCurPos()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数用于计算当前元素相对于显示器窗口右上角的距离。</font>

<font style="color:rgb(51, 51, 51);">在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">onMounted()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">生命周期钩子中，调用了</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">createBroadcastChannel()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">resizeEventBind()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">函数，用于在组件挂载后执行相关的初始化操作。</font>

<font style="color:rgb(51, 51, 51);">这样，当我们同时打开两个窗口，移动其中一个窗口，就可以向另外一个窗口发生当前窗口希望传递过去的信息，在本例子中就是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">#j-main</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">元素距离显示器右上角的距离。</font>

<font style="color:rgb(51, 51, 51);">假设</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">#j-main</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">只是一个在浏览器正中心矩形，我们同时打开两边的控制台，看看会发生什么：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530404057-a7a98428-0f10-43ce-9a3c-fbebd27f8051.gif)

<font style="color:rgb(51, 51, 51);">可以看到，如果我们同时打开两个一个的页面，当触发右边页面的 Resize，左边的页面会收到基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">broadcastChannel.onmessage = handleMessage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">接收到的信息，反之同理。</font>

<font style="color:rgb(51, 51, 51);">而一个完整的 Event 信息如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530404181-08190973-48e1-4daf-aff6-2f1e6fafe278.png)

<font style="color:rgb(51, 51, 51);">譬如，传递过来的信息放在 data 属性内、同时也可以获取当前的的 Broadcast Name 等。</font>

<font style="color:rgb(51, 51, 51);">基于 BroadcastChannel，就可以实现每个 Tab 内的核心信息互传， 可以得知当前在线设备数，再基于这些信息去完成我们想要的动画、交互等效果。</font>

<font style="color:rgb(51, 51, 51);">这里的核心点，还是：</font>

1. <font style="color:rgb(51, 51, 51);">数据向其他 Tab 页面传递的能力</font>
2. <font style="color:rgb(51, 51, 51);">Tab 页面接受其他页面传递过来的数据的能力</font>

<font style="color:rgb(51, 51, 51);">其本质就是一个</font>**<font style="color:rgb(51, 51, 51);">数据共享池</font>**<font style="color:rgb(51, 51, 51);">子。</font>

## <font style="color:rgb(51, 51, 51);">方式二：SharedWorker API</font>
<font style="color:rgb(51, 51, 51);">好，介绍完 Broadcast Channel()，我们再来看看 SharedWorker API。</font>

[<font style="color:rgb(51, 51, 51);">SharedWorker</font>](https://developer.mozilla.org/zh-CN/docs/Web/API/SharedWorker)<font style="color:rgb(51, 51, 51);">API 是 HTML5 中提供的一种多线程解决方案，它可以在多个浏览器 TAB 页面之间共享一个后台线程，从而实现跨页面通信。</font>

<font style="color:rgb(51, 51, 51);">与其他 Worker 不同的是，SharedWorker 可以被多个浏览器 TAB 页面共享，且可以在同一域名下的不同页面之间建立连接。这意味着，多个页面可以通过 SharedWorker 实例之间的消息传递，实现跨 TAB 页面的通信。</font>

<font style="color:rgb(51, 51, 51);">它的实现与上面的 Broadcast Channel 非常类似，我们来看一看实际的代码：</font>

```javascript
<template>
  <div class="g-container" id="j-main">
  // ...  
  </div>
  </template>

  <script>
  import { onMounted } from 'vue';

export default {
  setup() {
    // 创建一个 SharedWorker 对象
    let worker;

    function initWorker() {
      // 创建一个 SharedWorker 对象
      worker = new SharedWorker('/shared-worker.js', 'tabWorker');

      // 监听消息事件
      worker.port.onmessage = function (event) {
        console.log('接收到 event', event);
        handleMessage(event);
      };
    }

    function handleMessage(data) {
      // TODO: 处理接收到信息后的逻辑
    }

    function sendMessage(data) {
      // 发送消息
      worker.port.postMessage(data);
    }

    function resizeEventBind() {
      window.addEventListener('resize', () => {
        const pos = getCurPos();
        sendMessage(pos);
      });
    }

    function getCurPos() {
      const barHeight = window.outerHeight - window.innerHeight;
      const element = document.getElementById('j-main');
      const rect = element.getBoundingClientRect();

      // 获取元素相对于屏幕左上角的 X 和 Y 坐标
      const x = rect.left + window.screenX; // 元素左边缘相对于屏幕左边缘的距离
      const y = rect.top + window.screenY + barHeight;// 元素顶部边缘相对于屏幕顶部边缘的距离

      return [x, y];
    }

    onMounted(() => {
      initWorker();
      resizeEventBind();
    });

    return {};
  }
};
</script>

  <style lang="scss"></style>
```

<font style="color:rgb(51, 51, 51);">简单描述一下，上面也说了，跨 Tab 页通信的核心在于数据向外的发送与接收的能力：</font>

1. `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">initWorker()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">方法中，使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">worker = new</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SharedWorker</font>_``<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">('/shared-worker.js', 'tabWorker')</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">创建了一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SharedWorker</font>`<font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(51, 51, 51);">，</font>_<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">后面每一个被打开的同域浏览器 TAB 页面，都是共享这个 Worker 线程，从而实现跨页面通信</font>
2. <font style="color:rgb(51, 51, 51);">基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">worker.port.postMessage(data)</font>`<font style="color:rgb(51, 51, 51);">实现数据的传输</font>
3. <font style="color:rgb(51, 51, 51);">基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">worker.port.onmessage = function() {}</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">实现传输数据的监听</font>

<font style="color:rgb(51, 51, 51);">当然，上面有引入一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">/shared-worker.js</font>`<font style="color:rgb(51, 51, 51);">，这个是需要额外定义的，一个极简版本的代码如下：</font>

```javascript
//shared-worker.js
const connections = [];

onconnect = function (event) {
  var port = event.ports[0];
  connections.push(port);

  port.onmessage = function (event) {
    // 接收到消息时，向所有连接发送该消息
    connections.forEach(function (conn) {
      if (conn !== port) {
        conn.postMessage(event.data);
      }
    });
  };

  port.start();
};
```

<font style="color:rgb(51, 51, 51);">简单解析一下，下面对其进行解析：</font>

1. <font style="color:rgb(51, 51, 51);">上面的代码中，定义了一个数组 connections，用于存储与 SharedWorker 建立连接的各个页面的端口对象；</font>
2. <font style="color:rgb(51, 51, 51);">onconnect 是事件处理程序，当有新的连接建立时会触发该事件；</font>
3. <font style="color:rgb(51, 51, 51);">在 onconnect 函数中，通过 event.ports[0] 获取到与 SharedWorker 建立的连接的第一个端口对象，并将其添加到 connections 数组中，表示该页面与共享 Worker 建立了连接。</font>
4. <font style="color:rgb(51, 51, 51);">在连接建立后，为每个端口对象设置了 onmessage 事件处理程序。当端口对象接收到消息时，会触发该事件处理程序。</font>
5. <font style="color:rgb(51, 51, 51);">在 onmessage 事件处理程序中，通过遍历 connections 数组，将消息发送给除当前连接端口对象之外的所有连接。这样，消息就可以在不同的浏览器 TAB 页面之间传递。</font>
6. <font style="color:rgb(51, 51, 51);">最后，通过调用 port.start() 启动端口对象，使其开始接收消息。</font>

<font style="color:rgb(51, 51, 51);">总而言之，shared-worker.js 脚本创建了一个共享 Worker 实例，它可以接收来自不同页面的连接请求，并将接收到的消息发送给其他连接的页面。通过使用 SharedWorker API，</font>**<font style="color:rgb(51, 51, 51);">实现跨 TAB 页面之间的通信和数据共享</font>**<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">同理，我们来看看基于 Worker 的数据传输效果，同样是简化 DEMO，当 Resize 窗口时，向另外一个窗口发送当前窗口下</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">#j-main</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">元素的坐标：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530404633-42dcd6ce-b0f6-48a1-8326-4476aca4b0da.png)

<font style="color:rgb(51, 51, 51);">可以看到，如果我们同时打开两个一个的页面，当触发右边页面的 Resize，左边的页面会利用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">worker.port.onmessage = function() {}</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">收到基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">worker.port.postMessage(</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">data</font>_``<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">发送的信息，反之同理。</font>

<font style="color:rgb(51, 51, 51);">而一个完整的 Event 信息如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530404776-f6a0db8b-3144-4473-b5be-20ccf1d6dc2e.png)

<font style="color:rgb(51, 51, 51);">可以看到，在 SharedWorker 方式中，传输数据与 Broadcast Channel 是一样的，都是利用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Message Event</font>`<font style="color:rgb(51, 51, 51);">。简单对比一下：</font>

1. <font style="color:rgb(51, 51, 51);">SharedWorker 通过在多个Tab页面之间共享相同的 Worker 实例，方便地共享数据和状态，SharedWorker 需要多定义一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">shared-worker.js</font>`<font style="color:rgb(51, 51, 51);">;</font>
2. <font style="color:rgb(51, 51, 51);">Broadcast Channel 通过向所有订阅同一频道的 Tab 页面广播消息，实现广播式的通信。</font>

<font style="color:rgb(51, 51, 51);">兼容性方面，到今天(2023-11-26)，broadcast Channel 看着是兼容性更好的方式：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530404994-37d68ad3-7600-4e52-bb7f-a400f44aa6c7.png)

<font style="color:rgb(51, 51, 51);">另外，需要注意的是，两个方法都使用了</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">postMessage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">方法。</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">window.postMessage()</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">方法可以安全地实现跨源通信。并且，本质上而言，单独使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">postMessage</font>`<font style="color:rgb(51, 51, 51);"> 就可以实现跨 Tab 通信。</font>

<font style="color:rgb(51, 51, 51);">但是，单独使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">postMessage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">适合简单的点对点通信。在更复杂的场景中，Broadcast Channel 和 SharedWorker 提供更强大的机制，可简化通信逻辑，有更广泛的通信范围和生命周期管理。Broadcast Channel 的通信范围是所有订阅该频道的窗口，而 SharedWorker 可在多个窗口之间共享状态和通信。</font>

## <font style="color:rgb(51, 51, 51);">方式三：localStorage/sessionStorage</font>
<font style="color:rgb(51, 51, 51);">OK，最后一种跨 Tab 窗口通信的方式是利用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">localStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">sessionStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">本地化存储 API 以及的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">storage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">事件。</font>

<font style="color:rgb(51, 51, 51);">与上面 Broadcast Channel、SharedWorker 稍微不同的地方在于：</font>

1. `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">localStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">方式，利用了本地浏览器存储，实现了同域下的数据共享；</font>
2. `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">localStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">方式，基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">window.addEventListener('storage',</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">function</font>_``<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">(</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">event</font>_``<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">) {})</font>`<font style="color:rgb(51, 51, 51);">事件实现了 localStore 变化时候的数据监听；</font>

<font style="color:rgb(51, 51, 51);">简单看看代码：</font>

```vue
<template>
  <div class="g-container" id="j-main">
    // ...  
  </div>
</template>

<script>
  import { ref, reactive, computed, onMounted } from 'vue';

  export default {
    setup() {
      function initLocalStorage() {
        let tabArray = JSON.parse(localStorage.getItem('tab_array'));
        if (!tabArray) {
          const tabIndex = 1;
          id = tabIndex;
          localStorage.setItem('tab_array', JSON.stringify([tabIndex]));
        } else {
          const tabIndex = tabArray[tabArray.length - 1] + 1;
          id = tabIndex;
          const newTabArray = [...tabArray, tabIndex];
          localStorage.setItem('tab_array', JSON.stringify(newTabArray));
        }
      }

      function setLocalStorage(data) {
        localStorage.setItem(`tab_index_${id}`, JSON.stringify(data));
      }

      function handleMessage(data) {
        const rArray = JSON.parse(data);
        remoteX.value = rArray[0];
        remoteY.value = rArray[1];
      }

      function resizeEventBind() {
        window.addEventListener('resize', () => {
          const pos = getCurPos();
          setLocalStorage(pos);
        });

        window.addEventListener('storage', (event) => {
          console.log('localStorage 变化了！', event);
          console.log('键名：', event.key);
          console.log('变化前的值：', event.oldValue);
          console.log('变化后的值：', event.newValue);
          handleMessage(event.newValue);
        });
      }

      function getCurPos() {
        const barHeight = window.outerHeight - window.innerHeight;
        const element = document.getElementById('j-main');
        const rect = element.getBoundingClientRect();

        // 获取元素相对于屏幕左上角的 X 和 Y 坐标
        const x = rect.left + window.screenX; // 元素左边缘相对于屏幕左边缘的距离
        const y = rect.top + window.screenY + barHeight;// 元素顶部边缘相对于屏幕顶部边缘的距离

        return [x, y];
      }

      onMounted(() => {
        initLocalStorage();
        resizeEventBind();
      });

      return {};
    }
  };
</script>

<style lang="scss"></style>
```

<font style="color:rgb(51, 51, 51);">同样的简单解析一下：</font>

1. <font style="color:rgb(51, 51, 51);">每次页面初始化时，都会首先有一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">initLocalStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">过程，用于给当前页面一个唯一 ID 标识，并且存入 localStorage 中</font>
2. <font style="color:rgb(51, 51, 51);">每次页面 resize，将当前页面元素</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">#j-main</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的坐标值，通过 ID 标识当 Key，存入 localStorage 中</font>
3. <font style="color:rgb(51, 51, 51);">其他页面，通过</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">window.addEventListener('storage', (</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">event</font>_``<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">)</font>``_<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">=></font>_`<font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">{})</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">监听 localStorage 的变化</font>

<font style="color:rgb(51, 51, 51);">交互传输结果，与上述两个动图是一致的，就不额外贴图了，但是基于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">storage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">事件传输的值有点不一样，我们展开看看：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728530405182-5c7e7cd5-7bd7-4ead-b245-9ac15bd1d3c1.png)

<font style="color:rgb(51, 51, 51);">我们通过</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">window.addEventListener('storage', (event)=>{})</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">，可以拿到此次变化的 localStorage key 是什么，前值</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">oldValue</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">与</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">newValue</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">等等。</font>

<font style="color:rgb(51, 51, 51);">当然，由于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">localStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">存储过程只能是字符串，在读取的时候需要利用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">JSON.stringify</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">JSON.parse</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">额外处理一层，调试的时候需要注意。</font>

<font style="color:rgb(51, 51, 51);">虽然看起来这种方式最不优雅，但是结合兼容性一起看， localstorage 反而是兼容性最好的方式。在数据量较小的时候，性能相差不会太大，反而可能是更好的选择。</font>

<font style="color:rgb(51, 51, 51);">我基于上面三种方式：Broadcast Channel、SharedWorker 与 localStorage，都实现了一遍下面这个跨 Tab 页的 CSS 联动动画：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530405259-5e533d25-f4d9-4d2a-b592-a8d5ef93c72c.gif)



## <font style="color:rgb(51, 51, 51);">实际应用思考</font>
<font style="color:rgb(51, 51, 51);">当然，上面的实现其实有很大一个瑕疵。</font>

<font style="color:rgb(51, 51, 51);">那就是我们只顾着实现通信，没有考虑实际应用中的一些实际问题：</font>

1. <font style="color:rgb(51, 51, 51);">如何确定何时开始通信？</font>
2. <font style="color:rgb(51, 51, 51);">Tab 页频繁的开关，如何知道当前还有多少页面处于打开状态？</font>

<font style="color:rgb(51, 51, 51);">基于实际应用，我们需要基于上述 3 种方式，进一步细化方案。</font>

<font style="color:rgb(51, 51, 51);">上面，为了方便演示，每次传输数据时，只传输动画需要的数据。而实际应用，我们可以需要细化整个传输数据，设定合理的协议。譬如：</font>

```json
{
  // 传输状态：
  // 1 - 首次传输
  // 2 - 正常通信
  // 3 - 页面关闭
  status: 1 | 2 | 3,
  data: {}
}
```

<font style="color:rgb(51, 51, 51);">接收方需要基于收到信息所展示的不同的状态，做出不同的反馈。</font>

<font style="color:rgb(51, 51, 51);">当然，还有一个问题，我们如何知道页面被关闭了？基于组件的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">onUnmounted</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">发送当前页面关闭的信息或者基于 window 对象的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">beforeunload</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">事件发送当前页面关闭的信息？</font>

<font style="color:rgb(51, 51, 51);">这些信息都有可能因为 Tab 页面失活，导致关闭的信息无法正常被发送出去。所以，实际应用中，我们经常用的一项技术是</font>**<font style="color:rgb(51, 51, 51);">心跳上报/心跳广播</font>**<font style="color:rgb(51, 51, 51);">，一旦建立连接后，间隔 X 秒发送一次心跳广播，告诉其他接收端，我还在线。一旦超过某个时间阈值没有收到心跳上报，各个订阅方可以认为该设备已经下线。</font>

<font style="color:rgb(51, 51, 51);">总而言之，跨 Tab 窗口通信应用在实际应用的过程中，我们需要思考更多可能隐藏的问题。</font>

## <font style="color:rgb(51, 51, 51);">跨 Tab 窗口通信应用场景</font>
<font style="color:rgb(51, 51, 51);">当然，除了最近大火的跨 Tab 动画应用场景，实际业务中，还有许多场景是它可以发挥作用的。这些场景利用了跨 Tab 通信技术，增强了用户体验并提供了更丰富的功能。</font>

<font style="color:rgb(51, 51, 51);">以下是一些常见的应用场景：</font>

1. <font style="color:rgb(51, 51, 51);">实时协作：多个用户可以在不同的 Tab 页上进行实时协作，比如编辑文档、共享白板、协同编辑等。通过跨Tab通信，可以实现实时更新和同步操作，提高协作效率。</font>

<font style="color:rgb(51, 51, 51);">譬如这个：</font>

![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1728530405654-a44da221-91cb-4fe3-aaf1-b2ae3eb6dc07.gif)

1. <font style="color:rgb(51, 51, 51);">多标签页数据同步：当用户在一个标签页上进行了操作，希望其他标签页上的数据也能实时更新时，可以使用跨 Tab 通信来实现数据同步，保持用户在不同标签页上看到的数据一致性。</font>
2. <font style="color:rgb(51, 51, 51);">跨标签页通知：在某些场景下，需要向用户发送通知或提醒，即使用户不在当前标签页上也能及时收到。通过跨 Tab 通信，可以实现跨页面的消息传递，向用户发送通知或提醒。</font>
3. <font style="color:rgb(51, 51, 51);">多标签页状态同步：有些应用可能需要在不同标签页之间同步用户的状态信息，例如登录状态、购物车内容等。通过跨 Tab 通信，可以确保用户在不同标签页上看到的状态信息保持一致。</font>
4. <font style="color:rgb(51, 51, 51);">页面间数据传输：有时候用户需要从一个页面跳转到另一个页面，并携带一些数据，通过跨Tab通信可以在页面之间传递数据，实现数据的共享和传递。</font>

<font style="color:rgb(51, 51, 51);">举几个实际的例子：</font>

1. <font style="color:rgb(51, 51, 51);">某系统是一个国际化电商的仓库管理系统，系统能切换到全球各地不同的仓库进行数据操作，当用户打开了页面后，又新开了一个 Tab 页面，并且切换到另外一个仓库进行操作。当用户重新回到第一个打开的页面时，为了防止用户错误操作数据（前端界面是一致的，可能忘记了自己切换过仓库），通过弹窗提醒用户你已经切换过仓库；</font>
2. <font style="color:rgb(51, 51, 51);">某音乐播放器 PC 页面，在列表页面进行歌曲播放点击，如果当前没有音乐播放详情页，则打开一个新的播放详情页。但是，如果页面已经存在一个音乐播放详情页，则不会打开新的音乐播放详情页，而是直接使用已经存在的播放详情页面；</font>
3. <font style="color:rgb(51, 51, 51);">系统有与列表页与内容页，在内容页点击已阅，如果用户同时打开了上级列表页，要取消列表页关于该内容页的未读的提示；</font>

<font style="color:rgb(51, 51, 51);">总之，跨 Tab 窗口通信在实时协作、数据同步、通知提醒等方面都能发挥重要作用，为用户提供更流畅、便捷的交互体验。</font>

## <font style="color:rgb(51, 51, 51);">最后</font>
<font style="color:rgb(51, 51, 51);">本文只罗列了 3 种较为常见，适用性强的方式。除去本文罗列的方式，肯定还有其他方式能够实现跨 Tab 通信。</font>

<font style="color:rgb(51, 51, 51);">譬如，基于</font><font style="color:rgb(51, 51, 51);"> </font>[<font style="color:rgb(51, 51, 51);">Window: opener property</font>](https://developer.mozilla.org/en-US/docs/Web/API/Window/opener)<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">配合</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">postMessage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">也可以实现跨 Tab 窗口通信，但是这种通信仅仅适用于当前窗口以及通过当前窗口新开的窗口之间的通信。</font>

 

