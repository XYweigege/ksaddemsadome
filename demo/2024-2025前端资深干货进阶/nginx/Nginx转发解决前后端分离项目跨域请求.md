<font style="color:rgb(77, 77, 77);">前端使用</font><font style="color:rgb(78, 161, 219);">vue</font><font style="color:rgb(77, 77, 77);">，后端使用spring boot，前端学习到了发送请求，就测试了一下发送天天基金的接口（接口地址为: http://fundgz.1234567.com.cn/js/001186.js?rt=1463558676006），发现报了个</font>[跨域请求](https://so.csdn.net/so/search?q=%E8%B7%A8%E5%9F%9F%E8%AF%B7%E6%B1%82&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">的错，如下图</font>

<font style="color:rgb(77, 77, 77);"></font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1726651130107-3358c216-5c17-4894-836c-b0f1f86694ed.png)



##### 使用nginx反向代理 解决跨域


![](https://cdn.nlark.com/yuque/0/2024/png/207857/1726651188231-8a64e695-9cd2-41e9-b08b-122d8562a3e1.png)



<br/>color1
通过nginx来进行转发，nginx的作用相当于是个传话筒，用来做分发转发的作用，如下图，客户端请求服务端，服务端的9000端口被监听，如果nginx做了转发，将服务端的某个请求前缀转发到了不同的前端、后端服务器，那么就可以实现客户端请求同一个端口，但是服务器会响应不同的后台给客户端。

<br/>

 

1.nginx官网：nginx

下载地址：nginx: download

nginx常用命令：

```nginx
开启：start nginx

关闭：nginx.exe -s quit

重启：nginx.exe -s reload  （修改配置文件后需要重启才生效）
```

 



2.配置文件：

2.1配置监听端口

我这里监听的是9000

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1726651306334-4c0aee9f-76ef-48b2-b449-c641829aea3b.png)

2.2 配置转发地址：

这个proxy_pass一定要加，默认没有，不加的话访问不了

参考这个： [https://www.cnblogs.com/BoatGina/p/8409549.html](https://www.cnblogs.com/BoatGina/p/8409549.html)

我的自己的配置如下：可参考

```nginx
server {
        listen       9000;
        server_name  localhost;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        location / {
            root   html;
            index  index.html index.htm;
        }
        #自己前端的cli
		location /potato-cli-market/ {
			proxy_pass   http://localhost:8080/potato-cli-market/;
		}
        #自己后端的web
		location /potato-web-market/ {
			proxy_pass   http://localhost:8081/potato-web-market/;
		}
        #请求基金接口
		location /fund/ {
			proxy_pass    http://fundgz.1234567.com.cn/;
		}
}
```

监听的地址为localhost和端口为9000;

请求地址如下：

localhost:9000/potato-cli-market/mallTypeManage会直接转发到localhost:8080/potato-cli-market/mallTypeManage

同样如果请求

localhost:9000/potato-web-market/hello会直接转发到localhost:8081/potato-web-market/hello上

同样如果请求的接口

localhost:9000/fund/js/001186.js?rt=1463558676006会直接转发到 [http://fundgz.1234567.com.cn/js/001186.js?rt=1463558676006](http://fundgz.1234567.com.cn/js/001186.js?rt=1463558676006)



3.测试

我本地vue启动访问是：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1726651409645-13b95344-279e-46d1-b37c-f06f6aa18bf0.png)

启动nginx后，我访问[http://localhost:9000/potato-cli-market/mallTypeManage](http://localhost:9000/potato-cli-market/mallTypeManage)

就可以访问了：localhost:8080/potato-cli-market/mallTypeManage

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1726651426281-ecb67fee-a564-4df8-919d-b92b952fd571.png)

  

 



 

