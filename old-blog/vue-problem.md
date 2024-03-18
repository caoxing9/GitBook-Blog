# vue problem

## 前言

```
    话说之前做了一个Vue项目，做完挂服务器上一直没太理睬。之前也会偶尔打开看看有没有用户使用。偶尔上线就会发现一些问题。某次上线发现数据库被删除了(坑爹呀！第一次见识什么叫删库)。well！回到正题，由于有些时日未使用vue，决定回忆项目中遇到的一些问题，及解决方法。此外，我在某次访问时发现首屏时间竟然达到15s简直难以忍受，于是，花了点时间优化到2.5s。下面是总结的一些问题和优化方法。
```

## 问题

1.在主页获取一个数组，通过索引对数组进行赋值，页面数据未能发生响应更新。this.arr\[index] = \[];

* 原因:通过查看官方文档及查看多个博客，有两种情况vue无法监听数组的变化。

1. 利用索引为一个数组赋值时，如此种情况this.arr\[index] = \[]。
2.  直接通过length修改数组长度,如arr.length = newLength。 此外，vue官方文档也说明了一种情况即Vue无法检测到对象属性的添加或者删除。对于已经创建的实例，Vue 不允许动态添加根级别的响应式属性。(不过最近这个问题在vue3.0种通过es新特性proxy解决了)

    ```
    var vm = new Vue({
       data:{
          a:1
         }
       })
    // `vm.a` 是响应式的
    vm.b = 2
    // `vm.b` 是非响应式的
    ```

#### 解决方法

* this.set() Vue.set(vm.someObject, 'b', 2),第一个参数是需要更新的对象，第二个参数是其属性值，第3个是要修改值。个人很喜欢这个方法，简单直接。
* Object.assign() this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 }) 创建一个新的对象赋值。
* splice() vm.items.splice(indexOfItem, 1, newValue)通过splice方法添加。可以通过vm.items.splice(newLength)解决直接给数组添加长度无法监听的问题。
* \_.extend方法 \_.extend()方法是Underscore.js库提供的一个方法，作用是将sources对象中的所有属性拷贝到destination对象中，并返回destination对象。 \_.extend(destination, \*sources)

2. 回复回复的回复(还挺绕)，是这样一个场景，有一个帖子，有人回复，而我要去回复回复的人。目前想到的一个办法是通过回复在input框中添加回复人的username，

* 如何判断是否取消？通过判断引号前的被回复者的username是否缩短，存在bug，但是暂时觉得这是个好办法。

## 优化

```
    某天打开之前做的一个vue项目，我的天，首屏加载完竟然要15s，受不了，于是，寻找方法来优化。
```

## 声明

```
   前端页面的优化，主要是减少首屏页面需要下载的css、js、和一些静态资源的大小，让页面能在3s内展示出来。
```

## 方法

1. 关闭sourceMap

  在vuejs项目目录下config/index.js中将productionSourceMap: false。如果是true的话，在打包文件下会有很多map文件.关闭后能有效减少打包后文件的大小。

2. 使用路由懒加载

  在页面加载时，发现app.js这个文件很大加载很耗时间，于是在vue官网找到路由懒加载，其会分割app.js，在其加载改页面时再加载相应文件，有效提升页面速度。具体就是将路由文件中的import页面全部注解掉，在路由表中异步引入。

* 2.1es6的import()方法(webpack>2.4) // import Homepage from '@/page/Homepage' // import Loginpage from '@/page/Loginpage' // import Userinfo from '@/page/Userinfo' export default new Router({ // mode: 'history', routes: \[ { path: '/', name: 'Homepage', component: () => import('@/page/Homepage') } ]
* 2.2 vue异步组件技术，一个组件生成一个js文件(一般多个) 分包方式有3种。

1. vue异步组件\
   此种方式，一个组件会被单独打包成一个js。

```
  {
        path: '/',
        name: 'Homepage',
        component: resolve => require(['@/page/Homepage'], resolve)
  }
```

2. es的import()\
   此种方式自定义打包的组别。

```
   // 下面2行代码，没有指定webpackChunkName，每个组件打包成一个js文件。
const ImportFuncDemo1 = () => import('../components/ImportFuncDemo1')
const ImportFuncDemo2 = () => import('../components/ImportFuncDemo2')
// 下面2行代码，指定了相同的webpackChunkName，会合并打包成一个js文件。
// const ImportFuncDemo = () => import(/* webpackChunkName: 'ImportFuncDemo' */ '../components/ImportFuncDemo')
// const ImportFuncDemo2 = () => import(/* webpackChunkName: 'ImportFuncDemo' */ '../components/ImportFuncDemo2')
```

3. webpack提供的require.ensure()\
   此类方法可以自己给一个页面的组件打包成一个通过命名。

```
{
　　path: '/promisedemo',
　　name: 'PromiseDemo',
　　component: r => require.ensure([], () => r(require('../components/PromiseDemo')), 'demo')
},
{
　　path: '/hello',
　　name: 'Hello',
　　component: r => require.ensure([], () => r(require('../components/Hello')), 'demo')
}
```

* 2.3 webpack提供的require.ensure()，会创建合成一个js文件demo path: '/', name: 'Homepage', component: resolve => require.ensure(\[], () => resolve(require('page/Homepage')), 'demo') }

1. cdn加载模块，取消引入

  在build/webpack.base.conf.js文件中排除打包的一些模块。 通过在index.html痛过cdn引入。

// 打包排除第3方库，提升首屏加载速度

```
   externals: {
   'vue': 'Vue',
      'vue-router': 'VueRouter',
      'vuex':'Vuex',
      'axios': 'axios',
      'element-ui': 'ELEMENT',
      'qs' : 'qs'
      },
 index.html


      <script src="https://cdn.bootcss.com/vue/2.5.2/vue.min.js"></script>
      <script src="https://cdn.bootcss.com/vuex/3.1.0/vuex.min.js"></script>
      <script src="https://cdn.bootcss.com/axios/0.18.0/axios.min.js"></script>
      <script src="https://cdn.bootcss.com/qs/6.7.0/qs.js"></script>
      <script src="https://cdn.bootcss.com/element-ui/2.7.2/index.js"></script>
      <script src="https://cdn.bootcss.com/vue-router/3.0.1/vue-router.min.js"></script>
```

4\. webpack插件压缩

在webpack.base.conf.js文件中

```
      const ExtractTextPlugin = require("extract-text-webpack-plugin");

      //css 插件优化
           {
           test: /\.css$/,
           exclude: /node_modules/,
           use: ExtractTextPlugin.extract({
                fallback: 'style-loader',
                use: ['css-loader?modules', 'postcss-loader'],
           }),
           },
```
