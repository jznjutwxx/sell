# sell

> sell app

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 知识点
+ sticky footer 布局：高度不够footer置于底部，高度超过footer依旧置于底部
+ 添加购物车小球抛物线效果
+ flex布局
+ nextTick
+ props传值,emit事件分发
+ transition 过渡
+ 滚动插件,高度固定,内容滑动,不出现滚动条
+ backdropfilter ios毛玻璃
+ 图片异步加载,自适应图片高度，加载完成后不出现页面抖动
+ 引入一个export function和export default的区别,模块化编程思想
+ 正则表达式替换日期格式,特别的补零技巧
+ es6模板字符串, `用户 ${user.name} 未被授权执行 ${action} 操作。` 这样的插值操作就可以通过模板字符串自然优雅的拼接字符串
+ 可以配置引入component的路径，在webpack.base.conf的resolve里面
+ 自定义eslint配置,在.eslintrc.js中


