## 前序准备
在本地安装 [node](http://nodejs.org/) 和 [git](https://git-scm.com/)。
 **本项目技术栈基于 [ES2015+](http://es6.ruanyifeng.com/)
 [vue](https://cn.vuejs.org/index.html)
 [vuex](https://vuex.vuejs.org/zh-cn/)
 [vue-router](https://router.vuejs.org/zh-cn/) 
 [axios](https://github.com/axios/axios) 
 [element-ui](https://github.com/ElemeFE/element)

 
 **注意：该项目使用 element-ui@2.3.0+ 版本，所以最低兼容 vue@2.5.0+**

 **该项目不支持低版本浏览器(如ie)，有需求请自行添加polyfill [详情](https://github.com/PanJiaChen/vue-element-admin/wiki#babel-polyfill)**


## 功能
```
- 登录 / 注销

- 权限验证
  - 页面权限
  - 指令权限
  - 二步登录

- 多环境发布
  - dev sit stage prod

- 全局功能
  - 国际化多语言
  - 多种动态换肤
  - 动态侧边栏（支持多级路由嵌套）
  - 动态面包屑
  - 快捷导航(标签页)
  - Svg Sprite 图标
  - 本地mock数据
  - Screenfull全屏
  - 自适应收缩侧边栏

- 编辑器
  - 富文本
  - Markdown
  - JSON 等多格式

- Excel
  - 导出excel
  - 导出zip
  - 导入excel
  - 前端可视化excel

- 表格
  - 动态表格
  - 拖拽表格
  - 树形表格
  - 内联编辑

- 错误页面
  - 401
  - 404

- 組件
  - 头像上传
  - 返回顶部
  - 拖拽Dialog
  - 拖拽看板
  - 列表拖拽
  - SplitPane
  - Dropzone
  - Sticky
  - CountTo

- 综合实例
- 错误日志
- Dashboard
- 引导页
- Echarts 图表
- Clipboard(剪贴复制)
- Markdown2html
```

## 目录结构
```
- build                    // 构建相关
- config                   // 配置相关
- src                      // 配置相关
  - api                    // 所有请求
    - article.js           // 文章相关
    - login.js             // 登录相关
    - qiniu.js             // 七牛相关
    - remoteSearch.js      // 搜索相关
    - transaction.js       // 交易相关
  - assets                 // 主题 字体等静态资源
  - components             // 全局公用组件
  - directive              // 全局指令
  - filters                // 全局 filter
    - index.js             // 全局 filter的具体方法
  - icons                  // 项目所有 svg icons
  - lang                   // 国际化 language
    - en.js                // 自翻译的英文语言包
    - zh.js                // 自翻译的中文语言包
    - index.js             // 引入了 element-ui的语言包
  - mock                   // 项目mock 模拟数据
  - router                 // 路由
  - store                  // 全局 store管理
    - modules              // 模块化区分各个状态
      - app.js             // 全局非业务性质组件的状态，如：侧边栏、国际化
      - errorLog.js        // 错误日志组件的状态
      - permission.js      // 用户权限组件的状态
      - tagsView.js
      - user.js            // 用户组件的状态
    - getters.js           // 所有vuex状态管理的Getters属性
    - index.js             // store管理注入全局的相关配置
  - styles                 // 全局样式
  - utils                  // 全局公用方法
  - vendor                 // 公用vendor
  - views                  // views 所有页面
  - App.vue                // 入口页面
  - main.js                // 入口文件 加载组件 初始化等
  - permission.js          // 权限管理
- static                   // 第三方不打包资源
  - Tinymce                // 富文本,组建内富文本编辑器依赖此处
- .babelrc                 // babel-loader 配置
- .eslintrc.js             // eslint 配置项
- .gitignore               // git 忽略项
- .travis.yml              // 自动化CI配置
- favicon.ico              // favicon图标
- index.html               // html模板
- package.json             // package.json
```

## 布局
```
页面整体布局是一个产品最外层的框架结构，往往会包含导航、侧边栏、面包屑以及内容等

# Layout
- @/views/layout
大部分页面都是基于这个 layout 的，除了个别页面如login , 404, 401等
  
# app-main
- @/views/layout/components/AppMain
这里在 app-main 外部包了一层 keep-alive 主要是为了缓存 <router-view> 的，
配合页面的 tabs-view 标签导航使用，如不需要可自行去除。
其中transition 定义了页面之间切换动画，可以根据自己的需求，自行修改转场动画。

```

## 路由和侧边栏
```
#配置项
hidden: true; // (默认 false)
当设置 true 的时候该路由不会再侧边栏出现 如401，login等页面，或者如一些编辑页面

redirect: noredirect
当设置 noredirect 的时候该路由在面包屑导航中不可被点击

alwaysShow: true
当一个路由下面的 children 声明的路由大于1个时，自动会变成嵌套的模式--如组件页面
只有一个时，会将那个子路由当做根路由显示在侧边栏--如引导页面
若想不管路由下面的 children 声明的个数都显示根路由
可以设置 alwaysShow: true，这样会忽略之前定义的规则，一直显示根路由

name: "router-name"
设定路由的名字，一定要填写不然使用<keep-alive>时会出现各种问题

meta: {
  roles: ["admin", "editor"]; //设置该路由进入的权限，支持多个权限叠加
  title: "title"; //设置该路由在侧边栏和面包屑中展示的名字
  icon: "svg-name"; //设置该路由的图标
  noCache: true; //如果设置为true ,则不会被 <keep-alive> 缓存(默认 false)
}
不设置roles 没有权限限制

# 路由
这里的路由分为两种，constantRouterMap 和 asyncRouterMap

constantRouterMap： 代表那些不需要动态判断权限的路由，如登录页、404、等通用页面。

asyncRouterMap： 代表那些需求动态判断权限并通过 addRouters 动态添加的页面。

# 侧边栏
侧边栏主要基于 element-ui 的 el-menu 改造。

侧边栏是通过读取路由并结合权限判断而动态生成的，而且还需要支持路由无限嵌套，所以
这里还使用到了递归组件。
同时改造了 element-ui 默认侧边栏的样式，所有的 css 都可以在 @/styles/sidebar.scss 
中找到，可以根据自己的需求进行修改。

# 面包屑
本项目中也封装了一个面包屑导航，它也是通过 watch $route 变化动态生成的。它和 menu 也一样，也可以
通过之前那些配置项控制一些路由在面包屑中的展现。可以结合自己的业务需求增改这些自定义属性

# 侧边栏滚动问题
之前版本的滚动都是用 css 来做处理的
overflow-y: scroll;

::-webkit-scrollbar {
  display: none;
}

这样写会有兼容性问题，在火狐或者其它低版本游览器中都会比较不美观。其次在侧边栏收起的情况下，受
限于 element-ui的 menu 组件的实现方式，不能使用该方式来处理。

版本中使用了 el-scrollbar 来处理侧边栏滚动问题。

```

## 权限验证
```
该项目中权限的实现方式是：通过获取当前用户的权限去比对路由表，生成当前用户具的权限可访问的路由表，通
过 router.addRoutes 动态挂载到 router 上。

# 逻辑修改
现在路由层面权限的控制代码都在 @/permission.js 中，如果想修改逻辑，直接在适当的判断逻辑中 next() 
释放钩子即可

# 指令权限
封装了一个指令权限，能简单快速的实现按钮级别的权限判断  v-permission

- admin能看见
  - <el-tag v-permission="['admin']">admin</el-tag>
- editor能看见
  - <el-tag v-permission="['editor']">editor</el-tag>
- admin或者editor都能看见
  - <el-tag v-permission="['admin','editor']">Both admin or editor can see this</el-tag>

```

## 和服务端进行交互
```
# 前端请求流程

- UI 组件交互操作
- 调用统一管理的 api service 请求函数
- 使用封装的 request.js 发送请求
- 获取服务端返回
- 更新 data
从上面的流程可以看出，为了方便管理维护，统一的请求处理都放在 @/src/api 文件夹中，并且一般按照 model 
纬度进行拆分文件

# request.js
@/src/utils/request.js 是基于 axios 的封装，便于统一处理 POST，GET 等请求参数，请求头，以及错误
提示信息等。具体参看 request.js。 它封装了全局 request拦截器、respone拦截器、统一的错误处理、
统一做了超时处理、baseURL设置等

```

## 新增一个业务组件
```
# 如需要新添加一个页面，以example为例

- @/views下新增一个文件夹example,example文件夹下新增一个index.vue
- @router/index asyncRouterMap 下新增一条路由
```

## 公共方法 
```
# @/src/utils

- auth.js
  - 使用cookie.js对token获取、设置、删除
- request.js
  - 对axios的封装
-validate.js
  - 表单验证规则
  ...
  
```

## 引入外部模块
```
除了 element-ui 组件以及脚手架内置的业务组件，有时我们还需要引入其他外部组件，这里以引入 
vue-count-to 为例进行介绍。

# 引入依赖
$ npm install vue-count-to --save

# 全局注册
@/src/main.js
import countTo from "vue-count-to"
Vue.component("countTo", countTo)

@/xxx.vue
<template>
  <countTo :startVal='startVal' :endVal='endVal' :duration='3000'></countTo>
</template>

# 局部注册
@/xxx.vue

<template>
  <countTo :startVal='startVal' :endVal='endVal' :duration='3000'></countTo>
</template>

<script>
import countTo from 'vue-count-to';
export default {
  components: { countTo },
  data () {
    return {
      startVal: 0,
      endVal: 2017
    }
  }
}
</script>

# 在 vue 中优雅的使用第三方库
在 Vuejs 项目中使用 JavaScript 库的一个优雅方式是将其代理到 Vue 的原型对象上去. 按照这种方式, 
我们引入 Moment 库

@/src/main.js
import moment from "moment"
Object.defineProperty(Vue.prototype, "$moment", { value: moment })

由于所有的组件都会从 Vue 的原型对象上继承它们的方法, 因此在所有组件/实例中都可以通过 
this.$moment: 的方式访问 Moment 而不需要定义全局变量或者手动的引入.

@/xxx.vue

export default {
  created() {
    console.log("The time is ".this.$moment().format("HH:mm"));
  }
}

# 自己封装一个非 vue 组件
很多时候我们会发现，有些组件并没有 vue 版本，其实在 vue 中引入第三方组件是很简单的。只要在合适的
声明周期里面初始化它就好了。一般在 mounted中，之后和正常使用它就没什么区别了

```

## 跨域问题
```
前端解决跨域主要是在 dev 开发模式下可以下使用 webpack 的 proxy
```

## ESLint
```
# 配置项
所有的配置文件都在 .eslintrc.js 中。 本项目基本规范是依托于 vue 官方的 eslint 规则 eslint-config-vue 
做了少许的修改

ESLint使用的最大问题除了配置项外，还有就是不同编辑器的格式化设置问题，有时候编辑器的格式化操作会与ESLint存
在出入，具体的编辑器设置需要自行设置，推荐vscode

```

## 国际化
```
# 全局 lang
代码地址: @/lang 目前配置了英文和中文两种语言。
同时在 @/lang/index.js 中引入了 element-ui的语言包

- 使用
  - $t("login.title")
  
# 异步 lang
有一些某些特定页面才需要的 lang

- 使用
  - import local from "./local"
  - this.$i18n.mergeLocaleMessage("en", local.en)
  - this.$i18n.mergeLocaleMessage("zh", local.zh)
```

## 错误处理
```
# 页面级的错误处理由 vue-router 统一处理，所有匹配不到正确路由的页面都会进 404页面
```

## 开发
```bash
# 安装依赖
npm install
   
# 建议不要用cnpm安装 会有各种诡异的bug 可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org

# 启动服务
npm run dev
```

## 发布
```bash
# 构建测试环境
npm run build:sit

# 构建生产环境
npm run build:prod
```

## 其它
```bash
# --report to build with bundle size analytics
npm run build:prod --report

# --preview to start a server in local to preview
npm run build:prod --preview

# lint code
npm run lint

# auto fix
npm run lint -- --fix
```

### 以上内容参考 [vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/)。

