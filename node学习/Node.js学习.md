# Node.js

## 命令行窗口

dir  当前目录下所有文件

cd 目录名 进入指定目录

md 目录名 创建一个文件夹

rd 目录名 删除一个文件夹

## 简介

在```服务器运行```、跨平台javascript运行环境。

使用```事件驱动```、```非阻塞```、```异步I/O模型```等提高性能

偶数版：稳定版；奇数版：开发版

## 文件读写和错误处理

```node.js
var fs = require('fs')

fs.readFile('./data/hello.txt',function(error,data){

	...
})

fs.writeFile('文件路径'，'文件内容'，function(error){
		回调函数...
})
```

## http模块

```js
var http = require('http')
//使用http.createServer()创建一个web服务器
//返回一个Server实例
var server = http.createServer()
//注册request请求事件
server.on('request',function(request,response){
    console.log('收到客户端的请求 路径：'+request.url)
    
    //write可以使用多次，但是最后得end()结束
    response.write("hello")
    //告诉客户端，服务端的话说完了，可以呈现给用户看了
    response.end(" world!")
})
//绑定端口号，启动服务器
server.listen(3000,function(){
    console.log('服务器启动成功，可以通过http://127.0.0.1:3000 来进行访问')
})

```

## 核心模块

```js
var 核心模块名 = requrie('核心模块名')
```

require是一个方法，

**作用：**

- 是用来加载模块

- 拿到被加载文件模块导出的接口对象

用户可以自己编写模块

相对路径必须加 ./

可以省略后缀名

`node中没有全局作用域，只有模块作用域`

外部访问不到内部，内部也访问不到外部

```js
require("文件名.js")
```

每个文件模块中都提供了一个对象：`exports`

```a.js
var data = require('./b')
console.log(data.age)
```

```b.js
var age = 18
exports.age = age
```

### 编码设置

```js
res.seHeader('Content-Type','text/plain;charset=tf-8')
```

node中只能	通过 require方法来加载多个javascript脚本文件。

### 安装'命令提示符'

```
npm install --save-dev @types/node
```

### npm命令

```js
npm -v
-查看版本
npm
-帮助说明
npm search 包名
-搜索模块包
npm install 包名
-在当前目录安装包
npm install	/i 包名 -g
-全局模式安装包
npm remove /r 包名 删除包
npm install 包名 --save 安装包并添加到依赖中
npm install 下载当前项目所依赖的包
npm install 包名 -g 全局安装的包（一般都是一些工具）

```

`package.json（包描述文件）`

node在使用模块名字来引入模块时，会在`当前`的mode_modules中寻找，如果找不到，就会去`上一级目录`上寻找，直到找到为止，如果到`根目录`了还没有找到就会`报错`。

## buffer缓冲区

buffer的结构和数组很像，操作方法和数组类似

数组不能存二进制，buffer是专门存二进制数据的

使用buffer不需要引入模块。

buffer存储是二进制，显示是十六进制。

```js
var name = "hello node"
var buf = Buffer.from (name)
console.log(buf)

buf.length  //占用内存的大小
```

buffer大小一旦确定就不能修改，buffer实际是对底层内存的直接访问。

只要在控制台和页面输出一定是十进制。

```js
var buffer = Buffer.alloc(10)

console.log(buffer[0].toString)

var buf = Buffer.alloc(10)
buf[0] = 0xaa
console.log(buf[0])
console.log(buf[0].toString(16))

//创建一个指定大小的buffer,但是buffer中可能含有敏感数据
var buf2 = Buffer.allocUnsafe(10)

/*
* Buffer.from(str)  将一个字符串转换为buffer
* Buffer.alloc(size) 创建一个指定大小的buffer
* uffer.allocUnsafe(size) 创建一个指定大小的buffer,但是buffer中可能含有敏感数据
* buf.toString()  将缓冲区数据转换成字符串
* */
```

## 文件写入

### 同步文件写入

```js
fs.openSync(path,flags[,mode])

/*
* path:打开文件的路径
* flags: 打开文件要做的操作的类型
*       r 只读
*       w 可写
* */
 var fs = require("fs")
var fd = fs.openSync("hello.txt","w")
console.log(fd)
```

### 异步文件写入

```js
var fs = require("fs")
fs.open("hello2.txt","w",function (error,fd) {
    if (!error){
        console.log(fd)
        fs.write(fd,"这是异步写入的内容",function (error) {
            if(!error){
                console.log("文件已写入")
            }
            fs.close(fd,function (error) {
            if (!error){
                console.log("文件已关闭")
            }
            })
        })
    }else {
        console.log(err)
    }
})
```

### 简单文件写入

```js
/*
*   w: 写入，，没有就创建
*   r:只读
*   R+: 写入，没有不创建
*   a: 不覆盖内容，追加内容
* */

var fs = require("fs")
fs.writeFile("hello3.txt","这是通过writeFile写入的",{flag:w},function (err) {
    if (!err){
        console.log("文件已写入")
    }else {
        console.log(err)
    }
})
```

### 流式文件写入

同步、异步、简单文件写入都不适合大文件的写入。

性能较差，容易导致内存溢出。

```js
var fs = require("fs")
//流式文件写入

//创建一个可写流
var ws = fs.createWriteStream("hello3.txt")

//可通过监听流的open和close事件来监听流的打开和关闭
/*
* once 一次性的为对象绑定一个事件
* on  长久的为对象绑定一个事件
* */
ws.on("open",function () {
    console.log("流打开了")
})
ws.on("close",function () {
    console.log("流关闭了")
})


ws.write("通过可写流写入文件的内容/")
ws.write("通过可写流写入文件的内容/")
ws.write("通过可写流写入文件的内容/")
ws.write("通过可写流写入文件的内容/")

//关闭流
ws.end()
```

### 简单文件读取

```js
var fs = require("fs")
fs.readFile("hello3.txt",function (err,data) {
    if (!err){
        console.log(data)
    }
})
```

### 流式文件读取

适用于一些大文件，可以分多次将文件读取到内存中。

```js
// 流式文件读取
var fs = require("fs")

var rs = fs.createReadStream("hello3.txt")
var ws = fs.createWriteStream("hello4.txt")
//监听
// rs.once("open",function (){
//     console.log("可读流开启了")
// })
//
// rs.once("close",function (){
//     console.log("可读流关闭了")
//     ws.end()
// })
// ws.once("open",function (){
//     console.log("可写流开启了")
// })
//
// ws.once("close",function (){
//     console.log("可写流关闭了")
// })
//
//
// rs.on("data",function (data) {
//         ws.write(data)
//
// })


//pipe() 可以将可读流中的数据直接输出到可写流中。
rs.pipe(ws);
```

### 检测文件是否存在/文件信息

```js
var fs = require("fs")

flag = fs.existsSync(`hello5.txt`) 
console.log(flag)

//文件信息
fs.stat("hello4.txt",function (err,stat){
    console.log(stat)
})
//删除文件
fs.unlinkSync("hello.txt")

//查找文件目录结构
fs.readdir(".",function (err,files){
    if (!err){
        console.log(files)
    }
})

// 3是截取3个字节。一个汉字3个字节
fs.truncateSync("hello4.txt",3)

//创建目录
fs.mkdirSync("hello")
//删除目录
fs.rmdirSync("hello")
//改名
fs.rename("hello2.txt","hello1.txt",function (err) {
    if (!err){
        console.log("修改成功")
    }
    
})
//改名
fs.renameSync()

//文件监听
fs.watchFile("hello3.txt",{interval:1000},function (curr,prev) {
    ...
})
```

## 项目上线

### 创建服务器

```js
//安装包
npm i compression -S

const express = require("express")
const app = express()

app.use(express.static("./dist"))

app.listen(80,()=>{
    console.log('server running at http://localhost:80')
})
```

### 开启gzip包

```js
//安装包
npm i compression -S
//导包
const compression = resquire('compression')
//使用
app.use(compression())
```

### 配置HTTPS服务

`申请SSL证书`（https://freessl.org)

- 进入https://freessl.cn/官网，输入要申请的域名并选择品牌。
- 输入自己的邮箱并选择相关选项。
- 验证DNS(在域名管理后台添加TXT记录)。
- 验证通过之后，下载SSL证书（( full_chain.pem公钥; private.key私钥)。

在后台项目中导入证书

```js
const https = require('https')
const fs = require('fs')
const options = {
    cert: fs.readFileSync('./full_chain.pem'),
    key: fs.readFileSync('./private.key')
}
https.createServer(options,app).listen(443)  
```

### 使用pm2管理应用

**在服务器中安装pm2:** 

```js
npm i pm2 -g
pm2 start 脚本 --name 自定义名称 //启动项目
pm2 ls //查看运行项目
pm2 restart 自定义名称 //重启项目
pm2 stop 自定义名称   //停止项目
pm2 delete 自定义名称  //删除项目
```

