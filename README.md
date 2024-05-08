[![Docker Pulls][1]](https://hub.docker.com/r/adam8lee/freegpt35)

[1]: https://img.shields.io/docker/pulls/missuo/freegpt35?logo=docker


使用代理访问无需登录的OpenAI ChatGPT Web提供的无限免费的**GPT-3.5-Turbo** API服务。


二开自https://github.com/missuo/FreeGPT35


## 如何开始
### Node

```bash
npm install
node app.js
```
### Docker

```bash
docker run -p 3040:3040 -e PROXY_HOST="127.0.0.1" -e PROXY_PORT="7890" adam8lee/freegpt35
```





### 反向代理

```nginx
location ^~ / {
        proxy_pass http://127.0.0.1:3040; 
        proxy_set_header Host $host; 
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        proxy_set_header REMOTE-HOST $remote_addr; 
        proxy_set_header Upgrade $http_upgrade; 
        proxy_set_header Connection "upgrade"; 
        proxy_http_version 1.1; 
        add_header Cache-Control no-cache; 
        proxy_cache off;
        proxy_buffering off;
        chunked_transfer_encoding on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 300;
    }
```

### 负载均衡

```nginx
upstream freegpt35 {
        server 1.1.1.1:3040;
        server 2.2.2.2:3040;
}

location ^~ / {
        proxy_pass http://freegpt35; 
        proxy_set_header Host $host; 
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        proxy_set_header REMOTE-HOST $remote_addr; 
        proxy_set_header Upgrade $http_upgrade; 
        proxy_set_header Connection "upgrade"; 
        proxy_http_version 1.1; 
        add_header Cache-Control no-cache; 
        proxy_cache off;
        proxy_buffering off;
        chunked_transfer_encoding on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 300;
    }
```

## 如何请求



```bash
curl http://127.0.0.1:3040/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 随便填" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      {
        "role": "user",
        "content": "Hello!"
      }
    ],
    "stream": true
    }'
```



