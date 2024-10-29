## 基本概念

### 常用命令

```shell
nginx -s reload  # 向主进程发送信号，重新加载配置文件，热重启
nginx -s reopen	 # 重启 Nginx
nginx -s stop    # 快速关闭
nginx -s quit    # 等待工作进程处理完成后关闭
nginx -T         # 查看当前 Nginx 文件夹位置

```

`systemctl` 是 Linux 系统应用管理工具 `systemd` 的主命令，用于管理系统，我们也可以用它来对 Nginx 进行管理，相关命令如下：

```shell
systemctl start nginx    # 启动 Nginx
systemctl stop nginx     # 停止 Nginx
systemctl restart nginx  # 重启 Nginx
systemctl reload nginx   # 重新加载 Nginx，用于修改配置后
systemctl enable nginx   # 设置开机启动 Nginx
systemctl disable nginx  # 关闭开机启动 Nginx
systemctl status nginx   # 查看 Nginx 运行状态
```

### 正向代理、反向代理

简单的说，一般给客户端做代理的都是正向代理，给服务器做代理的就是反向代理

正向代理：例如翻墙

反向代理：服务器内部的请求转发

### 负载均衡

```nginx
upstream myserver{
 		# 哈希算法，自动定位到该服务器 保证唯一ip定位到同一部机器 用于解决session登录态的问题
    ip_hash; 
    server 127.0.0.1:9090 down; (down 表示单前的server暂时不参与负载) 
    server 127.0.0.1:8080 weight=2; (weight 默认为1.weight越大，负载的权重就越大) 
    server 127.0.0.1:6060; 
    server 127.0.0.1:7070 backup; (其它所有的非backup机器down或者忙的时候，请求backup机器) 
}
server {
	listen 80;
	server_name localhot;
	location / {
		proxy_pass http://myserver;
		root index.html;
		index index.html index.htm;
	}
}
```

负载均衡分配策略

1. 默认策略 时间轮训
2. weight 设置权重
3. ip_hash  请求ip的hash结果分配，每个用户固定访问一个服务器，可以解决session的问题
4. fair(第三方) 安后端服务器响应时间分配，响应时间短的优先分配

### 处理跨域请求

```nginx
server{
  listen 80;
	server_name localhot;
  # 处理跨域
  add_header 'Access-Control-Allow-Origin' $http_origin;   # 全局变量获得当前请求origin，带cookie的请求不支持*
	add_header 'Access-Control-Allow-Credentials' 'true';    # 为 true 可带上 cookie
	add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  # 允许请求方法
	add_header 'Access-Control-Allow-Headers' $http_access_control_request_headers;  # 允许请求的 header，可以为 *
	add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
	
  if ($request_method = 'OPTIONS') {
		add_header 'Access-Control-Max-Age' 1728000;   # OPTIONS 请求的有效期，在有效期内不用发出另一条预检请求
		add_header 'Content-Type' 'text/plain; charset=utf-8';
		add_header 'Content-Length' 0;
		return 204;                  # 200 也可以
	}
  
  location / {
    root index.html
  }
}
```

### 开启gzip压缩

```nginx
server {
   gzip on;
  	# 10k以下文件不压缩，太小的文件压缩后反而会更大！
	 gzip_min_length 10k;  
	 gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php;
}	

```

### 使用webpack打包gzip

为什么需要webpack打包gzip文件，还要nginx开启gzip压缩？

webpack前置打包可以缓解Nginx的服务器压力，nginx接受请求后发现有现成的gzip包就会直接返回

如果不经过webpack打包好gzip文件，那么nginx就会帮你做这个压缩的操作，这样就给服务器带来了压力。

```javascript
/ vue-cli3 的 vue.config.js 文件
const CompressionWebpackPlugin = require('compression-webpack-plugin')
module.exports = {
  // gzip 配置
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 生产环境
      return {
        plugins: [new CompressionWebpackPlugin({
          test: /\.js$|\.html$|\.css/,    // 匹配文件名
          threshold: 10240,               // 文件压缩阈值，对超过10k的进行压缩
          deleteOriginalAssets: false     // 是否删除源文件
        })]
      }
    }
  },
}
```

### https配置

```nginx
server {
				listen 443 ssl; 
        server_name www.jsxming.cn;
  			# pem
        ssl_certificate  /root/config/https/6528360_jsxming.cn.pem;
  			# key
        ssl_certificate_key /root/config/https/6528360_jsxming.cn.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
}
```

### 其他

```nginx
server {
	location /download {
    alias	          /usr/share/nginx/html/static;  # 静态资源目录
    autoindex               on;    # 开启静态资源列目录
    autoindex_exact_size    off;   # on(默认)显示文件的确切大小，单位是byte；off显示文件大概大小，单位KB、MB、GB
    autoindex_localtime     off;   # off(默认)时显示的文件时间为GMT时间；on显示的文件时间为服务器时间
  }
	
  # 图片防盗链
  location ~* \.(gif|jpg|jpeg|png|bmp|swf)$ {
    valid_referers none blocked server_names ~\.google\. ~\.baidu\. *.qq.com;  # 只允许本机 IP 、百度、谷歌外链引用
    if ($invalid_referer){
      return 403;
    }
  }
}
```

