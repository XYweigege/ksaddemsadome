

## 对ELMessage组件二次封装
### 为什么要封装？
在**封装请求拦截器**时，使用**ElMessage进行弹窗提示**成功或失败，但是**如果页面用到多个接口**，这时就会导致页面出现**很多弹窗**，导致用户体验不好，有可能出现卡顿，

这时就需要进行一些判断，如果**前面的ElMessage还没关闭并且类型是一致的就return，不再弹窗提示**，类型不一致时就要弹窗提示 

 类似下面的代码

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722999569608-064afb8c-284a-4b12-bef9-384dc28dd332.png)



### 现象，存在的问题
使用**<font style="color:#DF2A3F;"> elment ui 或者 element-plus  Antd React组件库 </font>**的时候，使用里面的弹窗组件，在同一按钮多次点击的时候，会触发很多次这个弹窗，看着很不舒服

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722999615336-97737d35-95a0-4099-a950-503bb7a3a9f8.png)

要进行优化

[代码地址：](https://gitee.com/sohucw/vite-full.git )注意是 chunk分支   在views/msg文件夹下  路由/msg

## 
## 对pagination组件进行二次封装
### 为什么要封装
当我们使用表格时，常会使用分页组件，如果直接用element plus的分页组件，用得多了，未免有些繁琐，这时候将其二次封装会更加方便使用。

封装分页组件，主要使用了Vue3的v-model特性，可以在子组件中改变page,size这些属性，就不用额外在父组件中修改，让组件更加轻巧。

[代码地址：](https://gitee.com/sohucw/vite-full.git )注意是 chunk分支 





 



  


 

