一个基础的 node + webpack 环境搭建

## KOA

express：基于回调，自带路由

koa：基于promise，更轻量，不带路由，增强错误处理，无任何预置中间件，核心就是实现了http协议的处理

### 核心概念

![koa核心概念](/Users/huangsiying/project/00 github/notes/images/koa核心概念.png)

#### koa application

主程序

#### context

ctx：上下文，类似于管道流水线

#### req res

请求响应

#### 工作原理

洋葱模型：顺序执行，先进后出

### koa-router

```js
const Koa = require('Koa')
const Router = require('koa-router')

let server = new Koa()
server.listen(8080)

let router = new Router()

// 路径前缀设置
router.prefix = '/api'

// router.get('/a', (ctx, next) => {
// 	ctx.request
// 	ctx.response
// })
router.get('/a', async ctx => {
	ctx.body = 'aa'
	ctx.body += 'bb'
})

server.use(router.routes())
```

#### 路由压缩

路由合并：koa-combine-routers

#### 路由优化

推荐目录：api各个路由的处理逻辑，router/routes.js...合并所有路由，index.js使用路由

### 安全header处理

koa-helmet：加入一些安全的header

```
// This...
app.use(helmet());
 
// ...is equivalent to this:
app.use(helmet.contentSecurityPolicy());
app.use(helmet.dnsPrefetchControl());
app.use(helmet.expectCt());
app.use(helmet.frameguard());
app.use(helmet.hidePoweredBy());
app.use(helmet.hsts());
app.use(helmet.ieNoOpen());
app.use(helmet.noSniff());
app.use(helmet.permittedCrossDomainPolicies());
app.use(helmet.referrerPolicy());
app.use(helmet.xssFilter());
```

### koa-json

格式化

```
var Koa = require('koa');
var app = new Koa();
 
// 只在参数传递的时候启用
app.use(json({ pretty: false, param: 'pretty' }));
 
app.use((ctx) => {
  ctx.body = { foo: 'bar' };
});

// 方法二
JSON.stringify("{...}", null, 2)
```

### koa-static

可缓存

```js
const Koa = require('koa')
const Router = require('koa-router')
const static = require('koa-static')

let server = new Koa()
server.listen(8080)

let router = new Router()
server.use(router.routes())

server.use(static('./static', {
    // 缓存时间
    maxage: 86400*1000,
    // 默认访问文件
    index: '1.html'
}))
```

结合路由设置缓存

```js
// 结合路由设置缓存时间
let staticRouter = new Router()
// .表示除换行符以外的单个字符需要转义，i忽略大小写
// 图片资源
staticRouter.all((/(\.jpg|\.png|\.gif)$/i), static('./static', {
    maxage: 60*86400*1000
}))
// 其余资源
staticRouter.all('*', static('./static', {
    maxage: 30*86400*1000
}))

server.use(staticRouter.routes())
```

### koa-body

### @koa/cors

跨域处理

```
const Koa = require('koa');
const cors = require('@koa/cors');
 
const app = new Koa();
app.use(cors());
```

### 开发热加载

nodemon

### 配置webpack

见webpack笔记

clean-webpack-plugin，webpack-node-externals，@babel/core，@babel/node，@babel/preset-env，babel-loader，cross-env

执行使用了babel的node`npx babel-node src/index.js`

监控使用了babel的node`npx nodemon --exec babel-node src/index.js`

#### webpack调试

`npx node --inspect-brk ./node_modules/.bin/webpack --inline --progress`

#### 优化

##### npm-check-updates

检查更新npm-check-updates，全局安装

`ncu `检查`ncu -u`更新，更新完成后要重新安装依赖

##### koa-compose

整合中间件

##### Koa-compress

##### 根据环境变量区分不同的配置

DefinePlugin：定义常量

```
new webpack.DefinePlugin({
	// ...
})
```

**webpack-merge**

**terser-webpack-plugin**

### vscode调试

配置：launch.js，修改路径和参数