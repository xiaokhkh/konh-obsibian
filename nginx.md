```php
// nginx.conf 文件
http {
# 变量设置的映射表
  map $http_upgrade $connection_upgrade {
      default upgrade;
      ''      close;
  }
  upstream websocket{
      server 192.168.5.157:15674;
  }
  server {
        listen       8081;
        server_name  localhost;
        root   html/dist;

        location / {
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }
        # 重点在这里
        location ^~/webSocket {
        proxy_buffering off;
        rewrite ^/webSocket/(.*)$ /$1 break;
        proxy_pass http://websocket;
            proxy_read_timeout 300s;
            proxy_send_timeout 300s;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            #升级http1.1到 websocket协议
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection  $connection_upgrade;
        }
    }

}
```