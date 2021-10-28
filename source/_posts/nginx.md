---
title: nginx
date: 2021-10-27 16:21:32
tags:
---

# 本地 https 请求转 http

1、生成证书

> openssl genrsa -out privkey.pem

> openssl req -new -x509 -key privkey.pem -out server.pem -days 365

[openssl genrsa 示意](https://www.openssl.org/docs/man1.1.1/man1/genrsa.html)
[openssl req 示意](https://www.openssl.org/docs/man1.1.1/man1/req.html)

证书信息可以随便填或者留空，只有 Common Name 要根据你的域名填写。如 xxx.com，或使用\*.xxx.com 匹配二级域名。

2、nginx 设置配置文件

```

#user  nobody;
worker_processes  1;




events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8080;
        server_name  tanglv.com;

        location / {
            proxy_pass http://tanglv.com:8016; # 将 http://tanglv.com:8080 转发到 http://tanglv.com:8016
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    # HTTPS server

    server {
        listen 443 ssl;
        server_name  tanglv.com;

        ssl_certificate      ./tanglvdata/server.pem; # 第一步生成的证书文件位置
        ssl_certificate_key  ./tanglvdata/privkey.pem; # 第一步生成的证书文件位置

        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照这个套件配置

        location / {
            proxy_pass http://tanglv.com:8080;  #将 https://tanglv.com 转发到 http://tanglv.com:8080
        }
    }

}

```

[参考配置 nginx](https://www.cnblogs.com/magotzis/p/9456695.html)

3、host 文件配置域名 ip 信息

4、[启动 ngnix](https://nginx.org/en/docs/beginners_guide.html) [参考](https://zhuanlan.zhihu.com/p/34362747)
