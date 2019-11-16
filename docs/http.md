# http


```bash
    "x-real-ip": "172.19.167.228",
    "x-forwarded-for": "172.19.167.228",
    "x-forwarded-proto": "http",
// 协议 http
this.ctx.headers['x-forwarded-proto'];
// 地址 IP
this.ctx.headers['x-real-ip'];
```




