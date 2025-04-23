# [<font style="color:rgb(0, 0, 0);">html2pdf.js导出分页内容截断的问题</font>](https://www.cnblogs.com/yaoyu7/p/17534703.html)
<font style="color:rgb(17, 17, 17);">今天遇到个导出PDF分页内容截断问题，如下</font>

<font style="color:rgb(17, 17, 17);"></font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723451152372-d48e5b03-7327-4d26-9ae7-240ccc56f99d.png)

<font style="color:rgb(17, 17, 17);">解决方法，加个: pagebreak: { mode : ' avoid-all ' } 自动分页即可</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723451152446-acaf59bd-3ff1-4bda-919b-b24ca1c3126f.png)

<font style="color:rgb(17, 17, 17);"></font>

<font style="color:rgb(17, 17, 17);">后面又遇到个问题,如下，配置了自动分页还是不行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723451152307-dd7264c3-9d48-4f8c-a895-346f81c795ea.png)

<font style="color:rgb(17, 17, 17);">最后发现是因为整个循环的元素没有用一个父元素包起来导致的，而我这里使用的是<></>包裹的，估计在解析的时候就变成空了。改成dlv后，整个内容自动分页就正常了。</font>

<font style="color:rgb(17, 17, 17);"></font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723451152278-ea6d9be9-d094-4a0c-bfe7-fb701eef011f.png)

