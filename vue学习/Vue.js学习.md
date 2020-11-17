# Vue.js学习

安装使用

vue-cli(2.x)

```
npm install -g vue-cli
vue init webpack vue_demo
cd vue_demo
npm install
npm run dev
```

2.x  ->   vue-cli(3.)

```
npm uninstall -g vue-cli  //卸载vue-cli
npm install -g @vue/cli
vue create 项目名称
npm run serve
npm run build //打包  dist文件夹
npm install -g serve //使用静态服务器工具包发布
serve dist
//使用动态web服务器发布
```

```
 1 ? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
 2 >( ) Babel //转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。 
 3 ( ) TypeScript// TypeScript是一个JavaScript（后缀.js）的超集（后缀.ts）包含并扩展了 JavaScript 的语法，需要被编译输出为 JavaScript在浏览器运行，目前较少人再用
 4 ( ) Progressive Web App (PWA) Support// 渐进式Web应用程序
 5 ( ) Router // vue-router（vue路由）
 6 ( ) Vuex // vuex（vue的状态管理模式）
 7 ( ) CSS Pre-processors // CSS 预处理器（如：less、sass）
 8 ( ) Linter / Formatter // 代码风格检查和格式化（如：ESlint）
 9 ( ) Unit Testing // 单元测试（unit tests）
10 ( ) E2E Testing // e2e（end to end） 测试
```

​	vue.config.js文件

```js
module.exports = {
    // 基本路径
    publicPath: '/',//注意新版本这里改成了publicpath
    // 输出文件目录
    outputDir: 'dist',
    // eslint-loader 是否在保存的时候检查
    lintOnSave: true,
    // use the full build with in-browser compiler?
    // https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only
    runtimeCompiler: false,
    // webpack配置
    // see https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md
    chainWebpack: () => {},
    configureWebpack: () => {},
    // vue-loader 配置项
    // https://vue-loader.vuejs.org/en/options.html
    // vueLoader: {},
    // 生产环境是否生成 sourceMap 文件
    productionSourceMap: true,
    // css相关配置
    css: {
     // 是否使用css分离插件 ExtractTextPlugin
     extract: true,
     // 开启 CSS source maps?
     sourceMap: false,
     // css预设器配置项
     loaderOptions: {},
     // 启用 CSS modules for all css / pre-processor files.
     modules: false
    },
    // use thread-loader for babel & TS in production build
    // enabled by default if the machine has more than 1 cores
    parallel: require('os').cpus().length > 1,
    // 是否启用dll
    // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#dll-mode
    // dll: false,
    // PWA 插件相关配置
    // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
    pwa: {},
    // webpack-dev-server 相关配置
    devServer: {
     open: process.platform === 'darwin',
     host: '0.0.0.0',
     port: 8088,
     https: false,
     hotOnly: false,
     proxy: null, // 设置代理
     before: app => {}
    },
    // 第三方插件配置
    pluginOptions: {
     // ...
    }
   }
```

```js
module.exports = {
  //生成环境部署路径，默认为'/'
  publicPath: process.env.NODE_ENV === 'production'
    ? '/production-sub-path/'
    : '/'
  //当运行 build 时生成的生产环境构建文件的目录
  outputDir:'dist',
 //放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录
  assetsDir:'',
 //指定生成的 index.html 的输出路径 (相对于 outputDir)
  indexPath:'index.html',
 //默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存,可设置false关闭
  filenameHashing:true,
  //在 multi-page （多页面应用）模式下构建应用
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js'
  },
 //是否在开发环境下通过 [eslint-loader](https://github.com/webpack-contrib/eslint-loader) 在每次保存时 lint 代码
 lintOnSave:true,
 //调整 webpack 配置最简单的方式就是在 vue.config.js 中的 configureWebpack 选项提供一个对象：1.如果这个值是一个对象，则会通过 [webpack-merge](https://github.com/survivejs/webpack-merge) 合并到最终的配置中
  configureWebpack: {
    plugins: [
      new MyAwesomeWebpackPlugin()
    ]
  },
//2、如果这个值是一个函数，则会接收被解析的配置作为参数。该函数及可以修改配置并不返回任何东西，也可以返回一个被克隆或合并过的配置版本。
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  },
  devServer: {
    proxy: 'http://localhost:4000',//这会告诉开发服务器将任何未知请求 (没有匹配到静态文件的请求) 代理到http://localhost:4000
  }
}
```



```js
module.exports = {
    baseUrl: process.env.NODE_ENV === 'production' ? '/online/' : '/',
    // outputDir: 在npm run build时 生成文件的目录 type:string, default:'dist'
    // outputDir: 'dist',
    // pages:{ type:Object,Default:undfind } 
    devServer: {
        port: 8888, // 端口号
        host: 'localhost',
        https: false, // https:{type:Boolean}
        open: true, //配置自动启动浏览器
        // proxy: 'http://localhost:4000' // 配置跨域处理,只有一个代理
        proxy: {
            '/api': {
                target: '<url>',
                ws: true,
                changeOrigin: true
            },
            '/foo': {
                target: '<other_url>'
            }
        },  // 配置多个代理
    }
}

```



## vue2.x

### vue插件

- **vue-cli:**vue脚手架
- **vue-resource(axios):**ajax
- **vue-router:**路由
- **vuex:**状态管理
- **vue-lazyload:**图片懒加载
- **vue-scroller:**页面滑动相关
- **mint-ui:**基于vue的UI组件库（移动端）
- **element-ui:**基于vue的UI组件库（PC端）

### 计算属性

getter/setter实现对熟属性对象的显示和监视

计算属性存在缓存，多次读取只执行一次getter计算

```js
new Vue({
    el: '#app',
    data: {
        x: '',
        y: '',
        
    },
    computed: {
        xy: function(){
            return this.x + this.y
        }
    }
})
```

### 监视属性

1. watch配置：watch: {}	
2. vm对象的￥watch()

### class绑定与style绑定

> class:

- ​	xxx是字符串
- ​	xxx是对象

```js
:class="{aClass:true,vClass:false}"
```

- ​	xxx是数组

```js
:class="['aClass','bClass']"
```

> style:

```js
:style="{color:activeColor,fontSize:fontSize + 'px'}"
//其中activeColor，fontSize是data中的数据
```

### 变更方法

```xxx.push```

 增加

```xxx.splice```

splice(index,len,[item])它也可以用来替换/删除/添加数组内某一个或者几个值（该方法会改变原始数组）

```xxx.sort()```

排序

### 过滤器

filter()方法

```html
...
<p>{{date | dateString(YYYY-MM-DD')}}</p>
...
<script>
    //自定义过滤器
Vue.filter('dateString',function(value,format){
return moment(value).format(format || 'YYYY-MM-DD HH:mm:ss')
})
Vue.filter('dateString',function(value,format='YYYY-MM-DD HH:mm:ss'){
return moment(value).format(format)
})
</script>

```

### 定义指令

自定义全局指令与局部指令

```js
//全局
Vue.directive('upper-text',function(el,binding){
    el.textContent = binding.value.toUpperCase()
})
//局部
new Vue({
    ...
    //只在当前vm管理范围有效
    directive: {
        'lower-text'(el,binding){
            el.textContent = binding.value.toLowerCase()
        }
    }
})

```





### 事件处理

$event

#### 事件修饰符

`停止事件冒泡`：@click.stop

`阻止事件的默认行为`

```js
@click.prevent="test"

//或
......
test(event){
    event.preventDefault()
    alter('点击了')
}

```

#### 按键修饰符

```js
@keyup.enter
@keyup.space
......
```

### 常用生命周期

**mounted():**

发送ajax请求，启动定时器等异步任务

**beforeDestory():**

做收尾工作，如：清除定时器

### 回退

```js
//可以返回到当前路由页面
this.$router.push(``)
//不可以返回到当前路由页面
//用新路由替换当前路由
this.$router.replace(``)

@click="$router.back()"
```

### 源码解析

1. **将伪数组转换为真数组**

   ```js
   const lis = document.getElementByTagName('li')
   console.log(lis instanceof Array,list[1].innerHTML,lis.forEach)//false 'test2' undefined
   const lis2 = Array.prototype.slice.call(lis)
   console.log(lis2 instanceof Array,lis2[1].innerHTML,lis2.forEach)//true 'test2' function
   ```

   

2. **ndoe.nodeType:得到节点类型**

   ```js
   const elementNode = document.getElementById('test')
   const attrNode = elementNode.getAttributeNode('id')
   const textNode = elementNode.firstChild
   console.log(elementNode.nodeType,attrNode.nodeType,textNode.nodeType)
   ```

   

3. **Object.defineProperty(obj,propertyName,{}) 给对象添加属性（指定描述符）**

   **数据描述符：**

   ​		configurable:是否可以重新定义

   ​		enumerable:是否可以枚举 ,默认：false

   ​		value：初始值

   ​		writable:是否可以修改属性值

   **访问描述符**（vue的计算属性computerd）

   ​		get:回调函数，根据相关属性动态计算当前属性

   ​		set:回调函数，`监视`当前属性变化，更新其他相关的属性

   ```js
   const obj = {
       fitstName: 'A',
       lastName: 'B'
   }
   Object.defineProperty(obj,'fullName',{
       get(){
           return this.firstName + '-' + this.lastName
       },
       set(value){
           const names = value.split('-')
           this.firstName = names[0]
           this.lastName = names[1]
       }
   })
   ...
   Object.defineProperty(obj,'fullName2',{ //不能重新定义
       configurable: false,
       enumerable:true,
       value: 'G-H',
       writable:false
   })
   ```

   

4. **Object.keys(obj)得到对象自身可枚举属性组成的数组**

5. **obj.hasOwnProperty(prop) 判断prop是否是obj自身的属性**

6. **DocumentFragment 文档碎片(高效批量更新多个节点)**

   document：对应显示的页面，包含n个element,一旦更新document内部的某个元素界面更新

   documentFragment:内存中保存n个element的容器对象（不与界面关联）如果更新fragment中的某个element,界面不变

   ```js
   //创建fragment
   const fragment = document.createDocumentFragment()
   //取出ul中所有子节点保存到fragment中
   let child
   wuile(child = ul.firstchild){ //一个节点只能有一个父亲
       fragment.appendChild(child)//先将从ul中移除，添加为fragment的子节点
   }
   //更新fragment中所有li的文本
   Array.prototype.slice.call(fragment.childNode).forEach(node => {
       if(node.ndoeType===1){
           node.textContent = 'mykang'
       }
   })
   //将fragment插入到ul中
   ul.appendChild(fragment)
   ```

   

#### 数据代理

通过vm对象来代理data中的所有属性的操作

​	给vm添加xxx属性，通过defineProperty（）给get，set方法

#### 模板解析

将el中的所有子节点添加到一个新建的fragment对象中

然后对fragment中的所有子节点进行递归编译解析处理。

- 对表达式文本节点进行解析
- 对元素节点的指令属性进行解析
  - 事件指令解析

#### 数据绑定

##### 		数据劫持

​				vue中实现数据绑定的一种技术。

​				基本思想：通过defineProperty()来监视data中发所有属性数据变化，变化就去更新界面

#### 双向数据绑定

解析v-model时，给当前元素添加input监听，当input的value值发生变化时，将最新的值赋给对应的data中的属性

#### mvvm结构图

![image-20201107211705604](upload%5Cimage-20201107211705604.png)

### vuex状态管理

![image-20201108150110518](upload%5Cimage-20201108150110518.png)

#### state

vuex管理状态的对象/数据源

```js
const state = {
    xxx:initValue
}
```

#### mutations

包含多个直接更新state的状态的方法的对象（回调函数）

actions中的commit来触发

只能包含同步代码，不能异步

```js
const mutations = {
    xxx(state,data){
        //更新state中的某个属性
    }
}
```

#### actions

包含多个事件回调函数的对象

通过commit（）来触发mutation的调用，间接更新state

通过组件中`$store.dispatch('action 名称'，数据) `

可以包含异步代码（定时器，ajax)

```js
const actions = {
    xxx({commit,state},data1){
        commit('yyy',{data1})
    }
}
```

#### getters

多个计算属性(get)的对象

读取：$store..getters.xxx

```js
const getters = {
    mmm(state){
        return ...
    }
}
```

#### modules

- 包含多个module
- 一个module是一个store配置对象
- 与一个组件对应

### vue-router两种模式

#### hash模式

例：http://www.abc.com/#/hello

`#`后面hash值的变化不会导致浏览器向服务器发送请求，不会刷新页面。对后端无影响.hash虽然会出现在url中，但是不会被包含在http请求中

#### **history**模式

新增：pushState()和replaceState()方法。

应用于浏览器的历史纪录栈。

在当前已有的 `back`、`forward`、`go` 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。

**history优点：**

- `pushState()` 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 `hash` 只可修改 `#` 后面的部分，因此只能设置与当前 URL 同文档的 URL；
- `pushState()` 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 `hash` 设置的新值必须与原来不一样才会触发动作将记录添加到栈中；
- `pushState()` 通过 `stateObject` 参数可以添加任意类型的数据到记录中；而 `hash` 只可添加短字符串；
- `pushState()` 可额外设置 `title` 属性供后续使用。

**区别：**

- `hash` 模式下，仅 `hash` 符号之前的内容会被包含在请求中，如 `http://www.abc.com`，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。
- `history` 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致。如 `http://www.abc.com/book/id`。如果后端缺少对 `/book/id` 的路由处理，将返回 404 错误。



## vue3.0

发布时间： 2020.9.18

### 内容：

​	diff优化：

​		vue2.x是对虚拟DOM进行全量对比，而vue3.0在创建虚拟DOM时根据DOM中的内容是否会发生变化添加静态标记。