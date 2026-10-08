Thrive-x部署文档

# 项目预览

```md
 项目              技术栈              本地要求              当前状态
  ━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━
   ThriveX-Admin     Vite + React        Node ≥20              已启动：
                                                               http://127.0.0.1
                                                               :9100
  ────────────────  ──────────────────  ────────────────────  ──────────────────
   ThriveX-Blog      Next.js 16 +        Node ≥20.9、npm       已启动：
                     React 19            ≥10                   http://127.0.0.1
                                                               :3000
  ────────────────  ──────────────────  ────────────────────  ──────────────────
   ThriveX-Server    Spring Boot +       Java 8、MySQL、       暂未启动
                     Maven               Redis

```

要启动三个项目

Admin是博客后台的平时发文章发 照片在这个页面管理

Blog 是前端页面，展示给大家看的，[liuyuyang.net](https://liuyuyang.net/)   那Admin 是前端吗？是啊，他是管理员使用的，这两个前端页面都依托于后端服务

Sever后端服务是没有页面的 只跑逻辑，像我们在admin页面写文章，blog页面发表评论，这些功能点都是Sever这个服务实现的。所以要有一个共识，前端做展示，后端做逻辑处理。



# 环境准备

前端依托node环境

https://nodejs.org/en/download/current 安装 安装指引启动

后端依托java jdk8

https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html

win x64



# 启动前端

## Admin

在idea中打开这个项目的终端，如下状态

```
PS E:\project\new-blog\ThriveX-Admin>
```

记得改一下.env文件内容为一下内容（我们只做本地运行）

解释一下http://localhost:9003/api这是管理员页面和后端建立连接的地址和端口

```
# 项目后端API地址
#VITE_PROJECT_API=https://你的后端域名/api
VITE_PROJECT_API=http://localhost:9003/api
```

在项目终端运行命令（直接粘贴）

```
npm run dev -- --host 127.0.0.1

```

这个运行完成会出现

```
(base) PS E:\project\new-blog\ThriveX-Admin> npm run dev -- --host 127.0.0.1

> thrivex-admin@4.0 dev
> vite --host 127.0.0.1


  VITE v7.3.1  ready in 4717 ms

  ➜  Local:   http://127.0.0.1:9100/
  ➜  press h + enter to show help


```

## Blog

同理进入这个项目的终端

运行命令

```
npm ci

```

出现下面这行即可，

added 588 packages, and audited 589 packages in 43s

不用管为什么红色高危险之类的报错 如：

```

272 packages are looking for funding
  run `npm fund` for details

25 vulnerabilities (9 moderate, 15 high, 1 critical)

To address issues that do not require attention, run:
  npm audit fix


```

运行启动命令

```
 npx next dev --hostname 127.0.0.1

```



出现下面即启动成功

```
▲ Next.js 16.2.10 (Turbopack)
- Local:         http://127.0.0.1:3000
- Network:       http://127.0.0.1:3000
- Environments: .env
✓ Ready in 5.5s
- Cache Components enabled

```

你访问页面后报错是因为后端还没有启动



# 启动后端

中间件准备、数据准备

