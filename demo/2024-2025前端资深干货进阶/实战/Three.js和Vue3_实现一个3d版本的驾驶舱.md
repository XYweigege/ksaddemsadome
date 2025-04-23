Three.js和Vue3 实现一个3d版本的驾驶舱

[代码地址：这里](https://gitee.com/sohucw/threejs-vue-demo)



## <font style="color:rgb(37, 41, 51);">一.搭建three场景</font>
<font style="color:rgb(37, 41, 51);">引入three,先初始化场景,相机,渲染器,光线,轨道控制器  
</font><font style="color:rgb(37, 41, 51);">先打印一下three 看一下有没有输出,然后在搭建场景等…</font>

```vue
<template>
  <div class="container" id="container"></div>
</tempalte>
  <script lang="ts" setup>
  let scene = null as any,//场景
  camera = null as any,//相机
  renderer = null as any,//渲染器
  controls = null as any//轨道控制器
  import {onMounted, reactive } from 'vue';
  import * as THREE from 'three';
  import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls'
  //设置three的方法
  const render = async () =>{
  //1.创建场景
  scene = new THREE.Scene();
  //2.创建相机
  camera = new THREE.PerspectiveCamera(105,window.innerWidth/window.innerHeight,0.1,1000);
  //3.设置相机位置
  camera.position.set(0,0,4);
  scene.add(camera);
  //4.建立3个坐标轴
  const axesHelper = new THREE.AxesHelper(5);
  scene.add(axesHelper);

  //6.设置环境光，要不然模型没有颜色
  let ambientLight = new THREE.AmbientLight(); //设置环境光
  scene.add(ambientLight); //将环境光添加到场景中
  let pointLight = new THREE.PointLight();
  pointLight.position.set(200, 200, 200); //设置点光源位置
  scene.add(pointLight); //将点光源添加至场景


  //7.初始化渲染器
  //渲染器透明
  renderer = new THREE.WebGLRenderer({
  alpha:true,//渲染器透明
  antialias:true,//抗锯齿
  precision:'highp',//着色器开启高精度
  });

  //开启HiDPI设置
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(window.innerWidth, window.innerHeight);
  //设置渲染器尺寸大小
  renderer.setClearColor(0x228b22,0.1);
  renderer.setSize(window.innerWidth,window.innerHeight);
  //将webgl渲染的canvas内容添加到div
  let container = document.getElementById('container') as any;
  container.appendChild(renderer.domElement);
  //使用渲染器 通过相机将场景渲染出来
  renderer.render(scene,camera);
  controls = new OrbitControls(camera,renderer.domElement);
  }
  const animate = () =>{
  requestAnimationFrame(animate);
  renderer.render(scene,camera);
  }
  onMounted(()=>{
  render()
  animate()
  })
</script>
  <style scoped>
  .container{
  width:100vw;
  height: 100vh;
  overflow: hidden;
  }
</style>

```

<font style="color:rgb(37, 41, 51);">现在我们就看到了three坐标轴了,接下来我们开始导入模型和天空图盒子</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736754128929-745aa3dc-3823-4d4b-8d3f-6cc1d5b46e3d.png)

## <font style="color:rgb(37, 41, 51);">二.加载gltf模型</font>
```vue
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
//5.导入gltf模型
  const gltfLoader = new GLTFLoader();
  
  gltfLoader.load('./model/scene.gltf',function(object){
    console.log(object)
    scene.add(object.scene);
  });

```

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736754162085-e446d944-41e3-4462-a4c6-f953dae2ac7a.png)



## <font style="color:rgb(37, 41, 51);">三.加载天空盒子</font>
```vue
//1.1 创建天空盒子
  const textureCubeLoader = new THREE.CubeTextureLoader();
  const textureCube = textureCubeLoader.load([
    "../public/img/right.jpg",//右
    "../public/img/left.jpg",//左
    "../public/img/top.jpg",//上
    "../public/img/bottom.jpg",//下
    "../public/img/front.jpg",//前
    "../public/img/back.jpg",//后
  ])
  scene.background = textureCube;
  scene.environment = textureCube;

```

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736754194329-b286ebc9-6c12-4db0-9f1a-5bb59ba37fd6.png)



## 四.加贴图文字
这里我们使用canvas写文字然后转成图片 最后使用three的纹理材质导入到three里面  
1.写一个canvas文字  
2.canvas转成图片  
3.three纹理材质导入图片  
4.定位到想要显示的地方

文字显示到three后,使用监听鼠标的方法点击了网页哪里的方法触发事件

```javascript
let canvas = null as any //文字
//创建three文字
const threeText = () => {
  //用canvas生成图片
  canvas = document.getElementById('canvas');
  canvas.width = 300
  canvas.height = 300
  let ctx = canvas.getContext('2d')
  //制作矩形
  ctx.fillStyle = "rgba(6,7,80,0.8)";
  ctx.fillRect(0,0,80,20);
  //设置文字
  ctx.fillStyle = "#fff";
  ctx.font = 'normal 15pt "楷体"'
  ctx.fillText('大伟聊前端', 12.5, 15)
  //生成图片
  let url = canvas.toDataURL('image/png');
  //将图片放到纹理中
  let geoMetry1 = new THREE.PlaneGeometry(30,30);
  let texture = new THREE.TextureLoader().load(url);
  let material1 = new THREE.MeshBasicMaterial({
    map:texture,
    side:THREE.DoubleSide,
    opacity:1,
    transparent:true
  })
  let rect = new THREE.Mesh(geoMetry1,material1)
  rect.position.set(10,1,-13)
  scene.add(rect)
}
//触发东方明珠点击事件
const threeTextClick = () =>{
  window.addEventListener('click',(event)=>{
    console.log(event.clientX)
    if(event.clientX > 855 && event.clientX < 1022){
      alert("触发了点击事件")
    }else{return}
  })
}
onMounted(()=>{  
  threeText()
  threeTextClick()
})

```

## <font style="color:rgb(37, 41, 51);">五.做一个three动态光圈</font>
<font style="color:rgb(37, 41, 51);">1.先创建一个three的圆柱几何体  
</font><font style="color:rgb(37, 41, 51);">2.给几何体加载一个合适的纹理  
</font><font style="color:rgb(37, 41, 51);">3.然后让他缓慢变大,重复运动</font>

<font style="color:rgb(37, 41, 51);"></font>

```javascript
let cylinderGeometry = null as any//光圈
//创建光圈
const aperture = () =>{
  //创建圆柱
  let gemetry = new THREE.CylinderGeometry(1,1,0.2,64);
  //加载纹理
  let texture = new THREE.TextureLoader().load('../public/img/cheng.png');
  texture.wrapS = texture.wrapT = THREE.RepeatWrapping;//每个都重复
  texture.repeat.set(1,1);
  texture.needsUpdate = true;

  let material = [
    //圆柱侧面材质,使用纹理贴图
    new THREE.MeshBasicMaterial({
      map:texture,
      side:THREE.DoubleSide,
      transparent:true
    }),
    //圆柱顶材质
    new THREE.MeshBasicMaterial({
      transparent:true,
      opacity:0,
      side:THREE.DoubleSide
    }),
    //圆柱顶材质
    new THREE.MeshBasicMaterial({
      transparent:true,
      opacity:0,
      side:THREE.DoubleSide
    })
  ];
  cylinderGeometry = new THREE.Mesh(gemetry,material);
  cylinderGeometry.position.set(0,-0.2,1);
  scene.add(cylinderGeometry);
}
onMounted(()=>{
  aperture()
})

```



**<font style="color:rgb(37, 41, 51);">让几合体(光圈)动起来</font>**  
<font style="color:rgb(37, 41, 51);">这个动态方法要放在animate方法里面</font>

```javascript
let cylinderRadius = 0;
let cylinderOpacity = 1;
//圆柱光圈扩散动画
const cylinderAnimate = () => {
  cylinderRadius += 0.01;
  cylinderOpacity -= 0.003;
  if (cylinderRadius > 1.6) {
    cylinderRadius = 0;
    cylinderOpacity = 1;
  }
  if (cylinderGeometry) {
    cylinderGeometry.scale.set(1 + cylinderRadius, 1, 1 + cylinderRadius); //圆柱半径增大
    cylinderGeometry.material[0].opacity = cylinderOpacity; //圆柱可见度减小
  }
}
const animate = () =>{
   cylinderAnimate()
   requestAnimationFrame(animate);
   renderer.render(scene,camera);
}

```

