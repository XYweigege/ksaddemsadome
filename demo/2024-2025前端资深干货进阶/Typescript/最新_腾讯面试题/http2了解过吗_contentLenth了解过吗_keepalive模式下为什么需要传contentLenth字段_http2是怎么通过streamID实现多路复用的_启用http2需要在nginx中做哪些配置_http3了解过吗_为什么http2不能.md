### <font style="color:rgb(64, 64, 64);">HTTP/2 了解过吗？</font>
<font style="color:rgb(64, 64, 64);">HTTP/2 是 HTTP 协议的第二个主要版本，旨在提高 Web 性能。它引入了多路复用、头部压缩、服务器推送等特性，减少了延迟并提升了页面加载速度。</font>

### <font style="color:rgb(64, 64, 64);">Content-Length 了解过吗？</font>
`<font style="color:rgb(64, 64, 64);">Content-Length</font>`<font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">是 HTTP 头部字段，表示消息体的字节长度。它帮助接收方确定消息体的结束位置，尤其在持久连接（Keep-Alive）中，用于区分多个请求或响应的边界。</font>

### <font style="color:rgb(64, 64, 64);">Keep-Alive 模式下为什么需要传 Content-Length 字段？</font>
<font style="color:rgb(64, 64, 64);">在 Keep-Alive 模式下，多个请求和响应通过同一个 TCP 连接传输。</font>`<font style="color:rgb(64, 64, 64);">Content-Length</font>`<font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">用于明确每个消息体的长度，确保接收方能够正确解析每个请求或响应，避免混淆。</font>

### <font style="color:rgb(64, 64, 64);">HTTP/2 如何通过 Stream ID 实现多路复用？</font>
<font style="color:rgb(64, 64, 64);">HTTP/2 使用 Stream ID 标识每个独立的请求/响应流。多个流可以在同一个 TCP 连接上并行传输，接收方通过 Stream ID 区分和重组数据，从而实现多路复用，提升传输效率。</font>

### <font style="color:rgb(64, 64, 64);">启用 HTTP/2 需要在 Nginx 中做哪些配置？</font>
<font style="color:rgb(64, 64, 64);">在 Nginx 中启用 HTTP/2 的步骤如下：</font>

1. <font style="color:rgb(64, 64, 64);">确保 Nginx 版本支持 HTTP/2（1.9.5 及以上）。</font>

<font style="color:rgb(64, 64, 64);">在配置文件的 </font>`<font style="color:rgb(64, 64, 64);">listen</font>`<font style="color:rgb(64, 64, 64);"> 指令中添加 </font>`<font style="color:rgb(64, 64, 64);">http2</font>`<font style="color:rgb(64, 64, 64);"> 参数：</font>

```nginx
server {
  listen 443 ssl http2;
  server_name example.com;
  ssl_certificate /path/to/certificate.crt;
  ssl_certificate_key /path/to/private.key;
  ...
}
```

<font style="color:rgb(64, 64, 64);">重新加载 Nginx 配置：</font>

```nginx
sudo nginx -s reload
```

### <font style="color:rgb(64, 64, 64);">HTTP/3 了解过吗？</font>
<font style="color:rgb(64, 64, 64);">HTTP/3 是 HTTP 协议的最新版本，基于 QUIC 协议。QUIC 运行在 UDP 上，提供更低的延迟和更好的连接迁移支持。HTTP/3 进一步优化了性能，尤其是在高丢包率环境下。</font>

### <font style="color:rgb(64, 64, 64);">为什么 HTTP/2 不能完全解决队头阻塞，而 HTTP/3 却可以？</font>
+ **<font style="color:rgb(64, 64, 64);">HTTP/2</font>**<font style="color:rgb(64, 64, 64);">：虽然通过多路复用减少了队头阻塞，但在 TCP 层仍可能因单个丢包导致整个连接阻塞。</font>
+ **<font style="color:rgb(64, 64, 64);">HTTP/3</font>**<font style="color:rgb(64, 64, 64);">：基于 QUIC 协议，每个流独立传输，丢包只影响单个流，不会阻塞其他流，从而彻底解决了队头阻塞问题。</font>

<font style="color:rgb(64, 64, 64);">总结：HTTP/2 通过多路复用提升了性能，但 TCP 层的队头阻塞问题依然存在；HTTP/3 通过 QUIC 协议彻底解决了这一问题。</font>

