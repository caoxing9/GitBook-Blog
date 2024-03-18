# first npm com

## 前言

我在学习vue的时候就觉得npm很酷，如果我能通过发布一个组件，让大家都能使用那该有多好，于是我开始通过仿造element-ui来做一个个性化的组件库，并上传到npm。

## 开始

我是在掘金上找了一些文章参考如何发布npm，[原文链接](http://www.rxshc.com/180.html)

1.  创建项目 我在我的项目目录中创建了一个即将发布的组件，名叫giao-ui，vue-cli用的是3.11，注意项目名不能写大写。

    ```
    vue create giao-ui
    ```
2. 修改目录，仿造element-ui修改了目录，将原src改成了example,添加了一个packages存放组件。example中主要存放示例。packages存放组件。 ![](http://148.70.128.231:8080/staic/blog/npm-publish/1.png)
3. 接下来可以开始写组件了。

* 组件目录

```
# packages
#--radio   
#---index.js //单个组件引出配置
#---src  
#----radio.vue // 单选按钮组件
#--index.js // 所有组件汇总配置
```

* ./packages/index.js

```
import radio from './radio'
// 存储组件列表
const components = [
  radio
]
// 定义install方法，接受vue做参数，使用use注册插件
const install = function (Vue) {
  // 判断是否安装
  if (install.installed) return
  components.map(component => Vue.component(component.name, component))
}
// 判断是否引入文件
if (typeof window !== 'undefined' && window.Vue) {
  install(window.Vue)
}
export default{
  install,
  radio
}
```

* ./packages/radio/index.js

```
import radio from './src/radio.vue'

radio.install = function (Vue) {
  Vue.component(radio.name, radio)
}

export default radio

```

* ./packages/radio/src/radio.vue

```
<template>
  <label>
    <input type="radio" name="vaule" :model='model' :value="label"/>
    <span class="my-radio-border"></span>
    <slot>默认值</slot>
  </label>
</template>
<script>
export default {
  name: "radio",
  data() {
    return {
      model:''
    };
  },
  props: {
    value: {
      type: String
    },
    label: {
      type: String
    }
  },
  compute: {
    model: {
    get() {
      return this.value; // 设置单选框 选项.  是通过当前值来判断当前选中项.
    },
    set(val) {
      this.$emit("input", val); // 选中单选框后,发布input事件; 这时子组件与父组件形成双向绑定.
    }
    }
  },
  methods: {
  }
};
</script>
<style scoped>
.my-radio-border {
  border: 1px solid #dcdfe6;
  border-radius: 100%;
  width: 14px;
  height: 14px;
  background-color: #fff;
  position: relative;
  cursor: pointer;
  display: inline-block;
  box-sizing: content-box;
  margin-top: 2px;
}
.my-radio-border:hover {
  border-color: #409eff;
}

label input {
  opacity: 0;
  outline: none;
  position: absolute;
  z-index: -1;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: 0;
}
label .text {
  font-size: 14px;
  vertical-align: top;
  padding: 0 5px;
}
label input:checked + span::after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  /* height: 6px;
  width: 6px; */
  border-radius: 100%;
  background: #409eff;
  border-color: #409eff;
  transition: 1s;
  animation: bescale 0.3s ease;
  animation-fill-mode: forwards;
}
label input:checked + span + span {
  color: #409eff;
}
@keyframes bescale {
  from {
    height: 0px;
    width: 0px;
  }
  to {
    height: 12px;
    width: 12px;
  }
}
</style>
```

4. 配置package.json

```
 // vue-config.js 基于vue-cli3.11
module.exports = {
  // 修改 src 目录 为 examples 目录
  pages: {
    index: {
      // page 的入口
      entry: 'examples/main.js',   // 把src 修改为examples
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html'
    }
  },
  // 扩展 webpack 配置，使 packages 加入编译
  /* chainWebpack 是一个函数，会接收一个基于 webpack-chain 的 ChainableConfig 实例。允许对内部的 webpack 配置进行更细粒度的修改。 */
  // 扩展 webpack 配置，使 packages 加入编译
  chainWebpack: config => {
    config.module
      .rule('js')
      .include
        .add('/packages')
        .end()
      .use('babel')
        .loader('babel-loader')
        .tap(options => {
          // 修改它的选项...
          return options
        })
  }
}
  然后运行npm run lib于是就会多出一个lib文件
```

5. 新建一个.npmignore文件配置打包上传过滤掉的文件.

```
# 忽略目录
examples/
packages/
public/

# 忽略指定文件
vue.config.js
babel.config.js
*.map
```

6. 上传 一切搞定开始上传，首先去[npm官网](https://www.npmjs.com)注册账号并激活邮箱(未激活不能上传) 然后在命令行输入:

```
npm login #输入用户名、密码、绑定邮箱，然后登录成功。
npm publish #上传
# 此处要注意上传的名字不能和已有的npm冲突。
```

[本文的源码目录](https://github.com/Mikeccx/giao-ui)这是我发布npm的全过程，过程的细节还会补充，遇到问题，可以留言或者微信联系交流.

## 问题

1. 组件名一定要小写
