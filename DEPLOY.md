# 💻部署DEMO站点 
通过[PM2](http://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/)或者[Docker](https://docs.docker.com/get-started/)部署demo站点到自己服务器上 **（推荐docker）**

### ✅Docker部署

Platform
  - linux

Requirements
  - docker
  - docker-compose

先配置`nginx`, 配置完记得重启服务
```bash
location / {
  proxy_pass http://127.0.0.1:3005; # 服务地址 注意！这里我们用的是3005端口
  proxy_set_header Host $host:80; # 代理到80端口 自己配置
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

```bash
# 项目根目录
docker-compose up -d
# 完成之后应该会有如下输出 ✅
Recreating wechat-redirect_web_1 ... done

# docker部署完毕
```

### ✅PM2部署
Platform
  - linux

Requirements
  - nginx
  - nodejs [[doc]](https://nodejs.org)
```bash
# 同样先更新nginx配置
location / {
  proxy_pass http://127.0.0.1:3000; # 服务地址
  proxy_set_header Host $host:80; # 代理到80端口 自己配置
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
# 记得重启nginx
```
```bash
# 安装pm2
npm install -g pm2
# 编译前端代码
cd exmaple/front
npm install
npm run build
cd -  #回到项目根目录
npm install

# 启动pm2守护服务
pm2 start --env=production
# 成功之后pm2会输出类似如下信息
┌─────────────────┬────┬─────────┬─────────┬───────┬────────┬─────────┬────────┬──────┬───────────┬──────┬──────────┐
│ App name        │ id │ version │ mode    │ pid   │ status │ restart │ uptime │ cpu  │ mem       │ user │ watching │
├─────────────────┼────┼─────────┼─────────┼───────┼────────┼─────────┼────────┼──────┼───────────┼──────┼──────────┤
│ WX REDIRECT API │ 0  │ 1.0.0   │ cluster │ 14585 │ online │ 0       │ 0      │ 0.2% │ 48.6 MB   │ root │ disabled │
│ WX REDIRECT API │ 1  │ 1.0.0   │ cluster │ 14594 │ online │ 0       │ 0      │ 0.2% │ 49.6 MB   │ root │ disabled │
└─────────────────┴────┴─────────┴─────────┴───────┴────────┴─────────┴────────┴──────┴───────────┴──────┴──────────┘
```

