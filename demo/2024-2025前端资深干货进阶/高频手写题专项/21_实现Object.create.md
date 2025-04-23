<br/>color1
<font style="color:rgb(199, 37, 78);">Object.create()</font><font style="color:rgb(85, 85, 85);">方法创建一个新对象，使用现有的对象来提供新创建的对象的 </font><font style="color:rgb(199, 37, 78);">__proto__</font>

<br/>

```javascript
// 模拟 Object.create

function create(proto) {
  function F() {}
  F.prototype = proto;

  return new F();
}
```

