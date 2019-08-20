# Nginx的配置文件

### 反向代理

Nginx能够提供反向代理，反向代理的意思是：客户端将请求打到Nginx上，Nginx再向后段服务器请求资源，然后再将资源相应给客户端，反向代理的好处有：

- 对客户端隐藏服务器（集群）的IP地址
- 安全：作为[应用层防火墙](https://zh.wikipedia.org/wiki/%E6%87%89%E7%94%A8%E5%B1%A4%E9%98%B2%E7%81%AB%E7%89%86)，为网站提供对基于Web的攻击行为（例如[DoS](https://zh.wikipedia.org/wiki/DoS)/[DDoS](https://zh.wikipedia.org/wiki/DDoS)）的防护，更容易排查[恶意软件](https://zh.wikipedia.org/wiki/%E6%83%A1%E6%84%8F%E8%BB%9F%E9%AB%94)等
- 为后端服务器（集群）统一提供加密和[SSL](https://zh.wikipedia.org/wiki/SSL)加速（如SSL终端代理）
- [负载均衡](https://zh.wikipedia.org/wiki/%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)，若服务器集群中有负荷较高者，反向代理通过[URL重写](https://zh.wikipedia.org/wiki/URL%E9%87%8D%E5%AF%AB)，根据连线请求从负荷较低者获取与所需相同的资源或备援
- 对于静态内容及短时间内有大量访问请求的动态内容提供[缓存服务](https://zh.wikipedia.org/wiki/Web%E7%BC%93%E5%AD%98)
- 对一些内容进行[压缩](https://zh.wikipedia.org/wiki/%E8%B3%87%E6%96%99%E5%A3%93%E7%B8%AE)，以节约[带宽](https://zh.wikipedia.org/wiki/%E9%A0%BB%E5%AF%AC)或为网络带宽不佳的网络提供服务
- 减速上传
- 为在私有网络下（如[局域网](https://zh.wikipedia.org/wiki/%E5%8D%80%E5%9F%9F%E7%B6%B2%E8%B7%AF)）的服务器集群提供[NAT穿透](https://zh.wikipedia.org/wiki/NAT%E7%A9%BF%E9%80%8F)及外网发布服务
- 提供HTTP访问认证[[2\]](https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86#cite_note-2)



~~~conf
worker_processes  1;  # 工作进程：数目。根据硬件调整，通常等于CPU数量或者2倍于CPU。

# 错误日志：存放路径。
error_log  logs/error.log  debug;

#pid        logs/nginx.pid;  # pid（进程标识符）：存放路径。


events {
    worker_connections  1024;
	# 每个工作进程的最大连接数量，表示Nginx服务来同时响应个转发请求。
}


####设定http服务器，利用它的反向代理功能提供负载均衡支持#################
http {
    include       mime.types; # 设定mime类型,类型由mime.type文件定义
    default_type  application/octet-stream;
    
    
~~~

## mime.types 文件

早期的电子邮件只支持ASCII字符集，为了在邮件中添加更多的内容，产生了MIME——Multipurpose Internet Mail Extension（多用途因特网邮件扩展）HTTP服务器在发送一份报文主体时，在HTTP报文头部插入解释自身数据类型的MIME头部信息（`Content-Type`）。客户端接收到这部分有关数据类型的信息，就能调用相应的程序处理数据。

Nginx的mime.types文件有如下的内容：

~~~xml
types {
    text/html                             html htm shtml;
    text/css                              css;
    text/xml                              xml;
    image/gif                             gif;
.....
~~~

打开OSC的一个页面，看到一个PNG格式的图片的时候，Nginx是这样发送格式信息的：

1.  服务器上有enter_narrow.png这个文件，后缀名是png；
2.  根据mime.types，这个文件的数据类型应该是image/png；
3.  将`Content-Type`的值设置为image/png，然后发送给客户端。



Nginx通过服务器端文件的后缀名来判断这个文件属于什么类型，再将该数据类型写入HTTP头部的`Content-Type`字段中，发送给客户端。



### 暂时学到这里，继续学习，参见：

[Nginx 配置文件学习](https://segmentfault.com/a/1190000002797601)

[Nginx相关介绍](https://www.cnblogs.com/wcwnina/p/8728391.html)

[关于proxy_pass 的解释](https://blog.csdn.net/zhongzh86/article/details/70173174)



### proxy_pass

在 ```nginx``` 中配置```proxy_pass```代理转发的配置规则，

* 如果在proxy_pass后面的```url```加 ```/```，表示绝对路径；
* 如果没有/，表示相对路径，把匹配的路径部分也给代理走。

假设下面四种情况分别用***``` http://192.168.1.1/proxy/test.html ```***进行访问。

~~~conf
location /proxy/ 
    proxy_pass http://127.0.0.1/;
} #代理到URL：http://127.0.0.1/test.html
~~~

~~~conf
location /proxy/ {
	 proxy_pass http://127.0.0.1;
} # 代理到URL：http://127.0.0.1/proxy/test.html
~~~

~~~conf
location /proxy/ {
	 proxy_pass http://127.0.0.1/aaa/;
} # 代理到URL：http://127.0.0.1/aaa/test.html
~~~

~~~conf
location /proxy/ {
	proxy_pass http://127.0.0.1/aaa;
} # 代理到URL：http://127.0.0.1/aaatest.html
~~~

## 命令

~~~shell
# 启动 后台启动
./nginx &

# 重启
./nginx -s reload

# 关闭
./nginx -s stop
~~~





