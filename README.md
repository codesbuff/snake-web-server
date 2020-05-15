## 小蛇搜搜项目简介
> 是一个专搜个人博客的小应用，迄今为止 建千万级别索引，爬取十万级别个人独立博客；项目demo地址：http://49.233.52.50/

现在先将server的前台放出去，后续会依次把ui层代码和索引层代码一并放出；

## 总体
> 所有的`services`和`models` 都放在`ctx`上；后端路由书写规则，根据`controller` js的文件目录定义路由路径；并且支持中间件各种粒度的控制，支持`全局中间件`，`分组中间件`与`某个url下中间件`。

## 开发
> npm run dev

## controller
> 接口path: 对应的`controller.js`js文件目录的地址;
如接口：`/api/mis/app/delete` 对应的目录： `src/controllers/api/mis/app/`,即在该目录下有含有`/delete`路由规则的js
这样写方便根据目录查找接口，并且能做好接口的整理

``` javascript
@router.post({url: '/delete', base: __dirname}, koaBody())
  async delete(ctx) {
    try {
      const { id } = ctx.request.body;
      const res =  ctx.services.App.delete(id);
      ctx.services.Response.ok('success');
    } catch(error) {
      console.log('error', error);
      ctx.services.Response.error(10050, error);
    }
  }
```
## middleware中间件
 中间件分为三种：
 1. 全局的中间件，放在`src/middlewares/global`目录下，这里的中间件是每个请求都会走到的中间件；

 2. 分组的中间件，放在每个分组的路由控制区，如`/api/admin/**`这种路由都要走是否是admin的控制，就可以在`controllers`的目录下`/api/admin`下添加一个`middleware.js`的文件，这里约定好该文件名称，添加完后，所有类似`/api/admin/**`这种请求都会走一遍该中间件；

 3. 细分到某个路由下的中间件，如上代码，也支持将中间写在`src/middlewares`目录下，通过文件名去指定中间件，如
 ``` javascript
 @router.post({url: '/delete', base: __dirname}, 'a', ...args)
 ```
 这种写法就是require(`src/middlewares/a.js`)(...args)并且返回一个中间件；


 ## 路由控制使用的是`find-my-way`