# node.js笔记

### 读取文件

```javascript
// 浏览器中的 JavaScript 是没有文件操作的能力的
// 但是 Node 中的 JavaScript 具有文件操作的能力

// fs 是 file-system 的简写，就是文件系统的意思
// 在 Node 中如果想要进行文件操作，就必须引入 fs 这个核心模块
// 在 fs 这个核心模块中，就提供了所有的文件操作相关的 API
// 例如：fs.readFile 就是用来读取文件的

// 1. 使用 require 方法加载 fs 核心模块
var fs = require('fs')

// 2. 读取文件
//    第一个参数就是要读取的文件路径
//    第二个参数是一个回调函数
//          
//        成功
//          data 数据
//          error null
//        失败
//          data undefined没有数据
//          error 错误对象
/*fs.readFile('./data/hello.txt', function (error, data) {
  // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
  // 文件中存储的其实都是二进制数据 0 1
  // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
  // 但是无论是二进制01还是16进制，人类都不认识
  // 所以我们可以通过 toString 方法把其转为我们能认识的字符
  // console.log(data)

  // console.log(error)
  // console.log(data)

  // 在这里就可以通过判断 error 来确认是否有错误发生
  if (error) {
    console.log('读取文件失败了')
  } else {
    // console.log(data.toString('utf-8'))
    console.log(data.toString())//不传参数的时候，也是utf-8
  }
})*/

fs.readFile('./data/hello.txt','utf-8', function (error, data) {
  // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
  // 文件中存储的其实都是二进制数据 0 1
  // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
  // 但是无论是二进制01还是16进制，人类都不认识
  // 所以我们可以通过 toString 方法把其转为我们能认识的字符
  // console.log(data)

  // console.log(error)
  // console.log(data)

  // 在这里就可以通过判断 error 来确认是否有错误发生
  if (error) {
    console.log('读取文件失败了')
  } else {
    // console.log(data.toString('utf-8'))  // 读取文件默认是读取的buffer类型的
    console.log(data.toString())  // 当读取文件的参数中已经传入编码的时候，就不需要toString()时候传编码了 
  }
})

```

- 读文件的时候，第二个参数可以传入，当传入的时候，打印读取的数据的时候，在toString方法中不需要写编码格式

### 写入文件

```javascript
var fs = require('fs')

// 第一个参数：文件路径
// 第二个参数：文件内容
// 第三个参数：回调函数
//    error
//    
//    成功：
//      文件写入成功
//      error 是 null
//    失败：
//      文件写入失败
//      error 就是错误对象
fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function (error) {
  // console.log('文件写入成功')
  // console.log(error)
  if (error) {
    console.log('写入失败')
  } else {
    console.log('写入成功了')
  }
})
```

注意：在读写文件的时候，需要注意文件的路径，

```javascript
// __dirname:表示当前执行的js的文件所在的路径
// __filename:表示当前正在执行的js的完整路径
console.log(__dirname);
console.log(__filename);
```

### 内置模块path

```javascript
var path=require('path');
filename=path.join(__filename,'test.txt');
console.log(filename);
//  可以用join对路径进行拼接，内部已经处理了斜杠/和反斜杠\，因为win和Linux等不同的操作系统的路径是不同的，如果写死路径，可能对文件的操作只能在某种系统下起作用，而在别的系统中不起作用
```

 ### HTTP程序

```javascript
// 接下来，我们要干一件使用 Node 很有成就感的一件事儿
// 你可以使用 Node 非常轻松的构建一个 Web 服务器
// 在 Node 中专门提供了一个核心模块：http
// http 这个模块的职责就是帮你创建编写服务器的

// 1. 加载 http 核心模块
var http = require('http')

// 2. 使用 http.createServer() 方法创建一个 Web 服务器
//    返回一个 Server 实例
var server = http.createServer()

// 3. 服务器要干嘛？
//    提供服务：对 数据的服务
//    发请求
//    接收请求
//    处理请求
//    给个反馈（发送响应）
//    注册 request 请求事件
//    当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理函数
server.on('request', function (req,res) {
//    解决乱码问题
  res.setHeader('Content-Type','text/plain;charset=utf-8');//告诉浏览器，编码方式
  res.write('hello world');
  console.log('收到客户端的请求了')
    // 对于每一个请求，服务器必须结束响应，否则客户端（浏览器）会一直等待服务器响应 
  res.end();
})

// 4. 绑定端口号，启动服务器
server.listen(3000, function () {
  console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
})

```

### Http-Request

```javascript
var http = require('http')

var server = http.createServer()

// request 请求事件处理函数，需要接收两个参数：
//    Request 请求对象
//        请求对象可以用来获取客户端的一些请求信息，例如请求路径
//    Response 响应对象
//        响应对象可以用来给客户端发送响应消息
server.on('request', function (request, response) {
  // http://127.0.0.1:3000/ /
  // http://127.0.0.1:3000/a /a
  // http://127.0.0.1:3000/foo/b /foo/b
  console.log('收到客户端的请求了，请求路径是：' + request.url)

  // response 对象有一个方法：write 可以用来给客户端发送响应数据
  // write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
  response.write('hello')
  response.write(' nodejs')

  // 告诉客户端，我的话说完了，你可以呈递给用户了
  response.end()

  // 由于现在我们的服务器的能力还非常的弱，无论是什么请求，都只能响应 hello nodejs
  // 思考：
  //  我希望当请求不同的路径的时候响应不同的结果
  //  例如：
  //  / index
  //  /login 登陆
  //  /register 注册
  //  /haha 哈哈哈
})

server.listen(3000, function () {
  console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
})
```

### Http-url

```javascript
var http = require('http')

// 1. 创建 Server
var server = http.createServer()

// 2. 监听 request 请求事件，设置请求处理函数
server.on('request', function (req, res) {
  console.log('收到请求了，请求路径是：' + req.url)
  console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)

  // res.write('hello')
  // res.write(' world')
  // res.end()

  // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
  // res.end('hello nodejs')

  // 根据不同的请求路径发送不同的响应结果
  // 1. 获取请求路径
  //    req.url 获取到的是端口号之后的那一部分路径
  //    也就是说所有的 url 都是以 / 开头的
  // 2. 判断路径处理响应

  var url = req.url

  if (url === '/') {
    res.end('index page')
  } else if (url === '/login') {
    res.end('login page')
  } else if (url === '/products') {
    var products = [{
        name: '苹果 X',
        price: 8888
      },
      {
        name: '菠萝 X',
        price: 5000
      },
      {
        name: '小辣椒 X',
        price: 1999
      }
    ]

    // 响应内容只能是二进制数据或者字符串
    //  数字
    //  对象
    //  数组
    //  布尔值
    res.end(JSON.stringify(products))
  } else {
    res.end('404 Not Found.')
  }
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('服务器启动成功，可以访问了。。。')
})
```

### Http-页面读取 ###

```javascript
//根据用户的请求读取不同的页面
var path=require('path');
var fs=require('fs');
var http=require('http');


// 1. 创建 Server
var server = http.createServer()

// 2. 监听 request 请求事件，设置请求处理函数
server.on('request', function (req, res) {
 

  var url = req.url

  if (url === '/') {
      fs.readFile(path.join(__dirname),'html','index.html'),function(err,data){
          if(err){
              throw err;
          }
          res.end(data);  // 如果读取到页面，就将页面返回给浏览器
      }
  } else if (url === '/login') {
    fs.readFile(path.join(__dirname),'html','login.html'),function(err,data){
          if(err){
              throw err;
          }
          res.end(data);  // 如果读取到页面，就将页面返回给浏览器
      }
  } else {
    fs.readFile(path.join(__dirname),'html','404.html'),function(err,data){
          if(err){
              throw err;
          }
          res.end(data);  // 如果读取到页面，就将页面返回给浏览器
      }
  }
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('服务器启动成功，可以访问了。。。')
})
```

### node中的js的核心模块

```javascript
// 用来获取机器信息的
var os = require('os')

// 用来操作路径的
var path = require('path')

// 获取当前机器的 CPU 信息
console.log(os.cpus())

// memory 内存
console.log(os.totalmem())

// 获取一个路径中的扩展名部分
// extname extension name
console.log(path.extname('c:/a/b/c/d/hello.txt'))
```

### ip地址、端口号

```javascript
// ip 地址用来定位计算机
// 端口号用来定位具体的应用程序
// 所有需要联网通信的应用程序都会占用一个端口号

var http = require('http')

var server = http.createServer()

// 2. 监听 request 请求事件，设置请求处理函数
server.on('request', function (req, res) {
  console.log('收到请求了，请求路径是：' + req.url)
  console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)

  res.end('hello nodejs')
})

server.listen(5000, function () {
  console.log('服务器启动成功，可以访问了。。。')
})
```

### Content-Type

```javascript
// require
// 端口号

var http = require('http')

var server = http.createServer()

server.on('request', function (req, res) {
  // 在服务端默认发送的数据，其实是 utf8 编码的内容
  // 但是浏览器不知道你是 utf8 编码的内容
  // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
  // 中文操作系统默认是 gbk
  // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
  // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
  // res.setHeader('Content-Type', 'text/plain; charset=utf-8')
  // res.end('hello 世界')

  var url = req.url

  if (url === '/plain') {
    // text/plain 就是普通文本
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    res.end('hello 世界')
  } else if (url === '/html') {
    // 如果你发送的是 html 格式的字符串，则也要告诉浏览器我给你发送是 text/html 格式的内容
    res.setHeader('Content-Type', 'text/html; charset=utf-8')
    res.end('<p>hello html <a href="">点我</a></p>')
  }
})

server.listen(3000, function () {
  console.log('Server is running...')
})
```

### http-fs

```javascript
// 1. 结合 fs 发送文件中的数据
// 2. Content-Type
//    http://tool.oschina.net/commons
//    不同的资源对应的 Content-Type 是不一样的
//    图片不需要指定编码
//    一般只为字符数据才指定编码

var http = require('http')
var fs = require('fs')

var server = http.createServer()

server.on('request', function (req, res) {
  // / index.html
  var url = req.url

  if (url === '/') {
    // 肯定不这么干
    // res.end('<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><title>Document</title></head><body><h1>首页</h1></body>/html>')

    // 我们要发送的还是在文件中的内容
    fs.readFile('./resource/index.html', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      }
    })
  } else if (url === '/xiaoming') {
    // url：统一资源定位符
    // 一个 url 最终其实是要对应到一个资源的
    fs.readFile('./resource/ab2.jpg', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        // 图片就不需要指定编码了，因为我们常说的编码一般指的是：字符编码
        res.setHeader('Content-Type', 'image/jpeg')
        res.end(data)
      }
    })
  }
})

server.listen(3000, function () {
  console.log('Server is running...')
})
```

### 异常

```javascript
//  异步操作，try-catch是无法捕获异常的
//  对于异步操作，要通过错误号（err.code）来进行错误处理
var fs=require('fs')
try{
    fs.writeFile('./yyy/text.txt','测试','utf-8',function(err){
        console.log('ok');
    })
}catch(e){
    console.log('出错了'+e);  // 此时如果没有yyy这个目录，也不会抛出异常，但是写文件不会成功
}
```

- 假如请求的页面中有图片或者css或者别的链接的时候，必须要对这些链接进行处理，在服务器代码中对每一个请求的url进行设置，才能在每一个页面中正确的访问。

### 静态资源

```javascript
var http = require('http')
var fs = require('fs')
var path=require('path')

var server = http.createServer()

server.on('request', function (req, res) {
  //  1.获取用户请求的路径
  var url = req.url
  //  2.拼接用户的路径
  var publicDir=path.join(__dirname,'public');  // 公共资源的路径
  //  3.根据public的路径和用户请求的路径，最终计算出用户请求的资源的完整路径
  var fileName=path.join(publicDir,req.url);
  //  console.log(filePath)
  //  4.根据文件的完整路径读取该文件，如果读取到了，返回该文件，如果读取不到，返回404页面

  fs.readFile(fileName, function (err, data) {
    if (err) {
      return res.end('404 Not Found.');
    }
    //  找到了用户的页面，直接返回
    //  通过第三方模块mime，来判断请求的资源的后缀，来确定content-type的类型
    res.setHeader('Content-Type',mime.getType(fileName));
    res.end(data);
  })
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('running...')
})
```

### 代码无分号问题

```javascript
function say() {
  console.log('hello world')
}

// TypeError: say(...) is not a function
say()

;(function () {
  console.log('hello')
})()

// ;['苹果', '香蕉'].forEach(function (item) {
//   console.log(item)
// })

// ` 是 EcmaScript 6 中新增的一种字符串包裹方式，叫做：模板字符串
// 它支持换行和非常方便拼接变量
// var foo = `
// 大家好
// hello                            床前明月光
// world
// 哈哈哈`
// console.log(foo)

;`hello`.toString()

// 当你采用了无分号的代码风格的时候，只需要注意以下情况就不会有上面的问题了：
//    当一行代码是以：
//        (
//        [
//        `
//        开头的时候，则在前面补上一个分号用以避免一些语法解析错误。
//    所以你会发现在一些第三方的代码中能看到一上来就以一个 ; 开头。
//  结论：
//    无论你的代码是否有分号，都建议如果一行代码是以 (、[、` 开头的，则最好都在其前面补上一个分号。
//    有些人也喜欢玩儿一些花哨的东西，例如可以使用 ! ~ 等。
```

### Request、Response

```javascript
var http=require('http')
http.createServer(function(req,res){
    //  1.res.headers返回的是一个对象，这个对象中包含了所有的请求报文头
    console.log(res.headers);
    //  2.res.rowHeaders返回的是一个数组，数组中保存的都是请求字符串
    console.log(res.rowHeaders);
    //  3.res.httpVersion 获取请求客户端使用的http版本
    console.log(res.httpVersion);
    //  4.res.method 获取客户端的请求方法（POST、GET。。。）
    console.log(res.method);
    //  5.res.url 获取请求的url,不包含主机名、端口号、协议等等
    console.lgo(res.url);
    res.end('over');
}).listen(8888,function(){
    console.log('服务器已经启动');
})
///////////////////////////////////////////////////////////////////////////////////////
var http=require('http')
http.createServer(function(req,res){
    //  1.res.headers返回的是一个对象，这个对象中包含了所有的请求报文头
    res.write('向浏览器发送数据');
    
    //  2.res.rowHeaders返回的是一个数组，数组中保存的都是请求字符串
    console.log(res.rowHeaders);
    //  3.res.httpVersion 获取请求客户端使用的http版本
    console.log(res.httpVersion);
    //  4.res.method 获取客户端的请求方法（POST、GET。。。）
    console.log(res.method);
    //  5.res.url 获取请求的url,不包含主机名、端口号、协议等等
    console.lgo(res.url);
    //  结束请求，告诉服务器，请求的响应已经完毕，可以关闭本次响应
    res.end('over');
}).listen(8888,function(){
    console.log('服务器已经启动');
})
```

### 设置响应状态码

```javascript
// res.setHeader('Content-Type','text/html;charset=utf-8');
// res.statusCode=404;
// res.statusMessage='404 NOT FOUND';
//  直接向客户端写入http响应头,上边的三句代码可以用下边的一句代替
res.writeHead(404,'not found',{'Content-Type':'text/html;charset=utf-8'});
//  writeHead()方法会自动被调用，如果仅仅写了前三行代码，调用的时候，会自动的将其设置到writeHead()方法中，如果上边的4行代码都写了，则只有最后的weriteHead()起作用
```

### NPM

```
//更新npm ,-g是全局安装， 全局安装后，就可以当做命令行来使用了
npm install npm@latest -g
// 查看版本
npm -v
// 用nvm切换node的版本
nvm use 8.5.0
npm unistall //  卸载包
nim init  //  初始化一个package.json文件,package.json是包描述文件
```

假如：npm install mime -g 安装后，就可以在dos下使用命令行查看，如果在每个项目下要使用，必须要本地安装，才可以在项目中使用，比如，如下：

```
mine a.css
mime a.png
```

package.json常见的项有哪些 ？

- name
  - 包名
- version
  - 版本
- description
  - 描述
- author
  - 作者
- main
  - 包的入口js文件，从main字段这里指定的那个js开始执行
- dependencies
  - 当前包依赖的其他包

如何创建 ###package.json### 文件(或者手动创建)，注意：不能包含中文，或者大写

```
npm init    或者 npm init -y  或者  npm init -yes
```

package-lock.json 里面不但会有每个依赖包的版本信息，而且还会有每个包的下载的地址，如果一个包依赖另外一个包，另外一个包又依赖其他的，也会将其他的包的下载路径直接保存在package-lock.json中，这样会提高下载的效率。

### url解析

有时候需要将url中提交的数据解析出来，需要使用到url这个模块，

```javascript
var url=require('url');
var urlObj=url.parse(req.url,true);  // 第二个参数可以传，可以不传，不传的时候，query解析为一个字符串，传入true的时候，query解析为一个json串，方便提取用户提交的各种数据
urlObj.query.title;  // 直接可以解析用户提交的数据中的title的内容
urlObj.query.name;  // 直接可以解析用户提交的数据中的name的内容
```

```javascript
var list=[];
JSON.stringfy(list);  // JSON.stringfy(list)可以将一个数组转换成一个字符串，方便将一个数组写入json文件中
//  在服务端让客户端进行客户端页面 重定向
res.statusCode=302;  // 302是found，一般是跳转
res.statusMessage='found';  // 设置状态消息
res.setHeader('Location','/itemlist');  // 让跳转到列表页面
```

将一个JSON转换成数组对象

```javascript
var list=JSON.parse(data);
```

### 获取request post提交的数据

```javascript
var querystring=require('querystring');  // 将查询字符串转换为ＪＳＯＮ
//  当post提交的数据量比较大的时候，需要监听request对象的data事件，每次提交数据，都会出发data事件
//  当request的end事件触发的时候，表示数据已经提交完毕
var array=[];  // 保存用户每次提交的数据
req.on('data',function(chunk){
    //  此处的chunk参数，就是浏览器本次提交过来的一部分数据，数据类型是Buffer类型（chunk 就是一个Buffer对象）
    array.put(chunk);
});
//  监听 request 对象的 end 事件
req.on('end',function(){
   //  在这个事件中，只需要把array的数据汇总起来就可以了
   //  把 array 中的每一个 Buffer 汇总，转换为一个 Buffer 对象
   var postBody=Buffer.concat(array);
   postBody=postBody.toString('utf-8');  // 将 Buffer 转换为字符串
   postBody=querystirng.parse(postBdoy);  // 将查询字符串转换为JSON
});
```

### underscore 模板引擎

```javascript
var _=require('underscore');
//  声明一段HTML
var html='<h2><%= name%></h2>';
//  template() 的返回值依然是一个函数
var fn=_.templage(html);
//  fn 接收一个数据对象，并用该数据对象，将 HTML 中的模板内容替换，生成最终的HTML代码
var html=fn({name:'哈哈'})；  // 这儿的返回值才是需要的 html 
 

```

###  异步调用函数封装

```javascript
//  封装一个读取文件内容的函数
function readFile(callback){
    fs.readFile(path.join(__dirname,'data'),'utf-8',function(err,data){
        if(err){
            throw err;
        }
        var list=JSON.parse(data || '[]');
        //  通过调用回调函数callback() 将读取到的数据list返回
        callback(list);
    });
}
```

```javascript
//  封装一个写入文件的方法
function writeFile(data,callback){
    fs.writeFile(path.join(__dirname,'data'),data,function(err,data){
        if(err){
            throw err;
        }
        //  通过调用回调函数 callback() 这儿不需要决定浏览器干什么事
        callback();  // 使用writeFile方法的时候，可以在回调函数中写一些写完文件后需要干的事情
    });
}
```

### 模块之间的通信

a.js：

```javascript
//  一个模块通过 require 加载后默认返回的是一个对象
module.exports.name='张三';  //  通过module.exports可以暴露一个模块中的一些信息
module.exports.show=function(){
    console.log('哈哈哈哈');
}
```

b.js

```javascript
var b=require('./a.js');  // 加载 a.js
console.log(b); 
b.show();  // 哈哈哈哈
```

也可以通过export来暴露一个模块中的内容，但是稍有区别，

- require 最终返回的是 module.exports 指向的内容，而不是 exports 指向的内容
- 默认 exports 和 module.exports 指向的是同一片内存

a.js

```javascript
//  一个模块通过 require 加载后默认返回的是一个对象
module.exports.name='张三';  //  通过module.exports可以暴露一个模块中的一些信息
module.exports.show=function(){
    console.log('哈哈哈哈');
}
exports.age=18;
module.exports='Hello World';
```

b.js

```javascript
var b=require('./a.js');  // 加载 a.js
console.log(b); // 此时返回的是一个 Hello World
```

a.js

```javascript
//  一个模块通过 require 加载后默认返回的是一个对象
module.exports.name='张三';  //  通过module.exports可以暴露一个模块中的一些信息
module.exports.show=function(){
    console.log('哈哈哈哈');
}
exports.age=18;
exports='Hello World';
```

b.js

```javascript
var b=require('./a.js');  // 加载 a.js
console.log(b); // 此时返回的是一个对象，因为最终返回的是 module.exports 而不是 exports ,但是exports  和 module.exports 指向的是同一片区域，如果不让module.exports不重新指向新的内存，则他们的数据是相同的，可以理解为为了方便使用，exports 是 module.expoerts 的快捷使用方式而已
```

### Buffer

```javascript
var len = Buffer.byteLength('你好cat','utf-8');
console.log(len);  // 9 utf-8一个中文三个字节，一个英文一个字节
var buf=Buffer.from('我的大中国')；  // 通过Buffer.from 可以转换为字节
var str=buf.toString('utf-8');  // 通过toString()方法转换为字符串
Buffer.isBuffer(obj);  // 判断obj是否是Buffer
```

### Express

请求路由的方法1：

```javascript
var express = require('express');
var app = express();  // app 类似于创建的server

app.get('/', function(req,res){
   res.end('hello world') ;
});
app.get('/index', function(req,res){
   res.end('get index hello world') ;
});
//  可以用send 也可以用end，但是 send 帮我们封装了请求头，可以设置很多编码，推荐使用send
//  如果用 end 直接向网页中输出的时候，遇到中文会乱码，但是send是不会乱码的，因为它不仅设置了响应头的编码，而且在响应头中设置了很多属性
app.post('/index', function(req,res){
   res.send('post index hello world') ;
});
app.listen(3000, function(){
    console.log('启动成功');
})
```

请求路由的方法2：

```javascript
var express = require('express');
var app = express();  // app 类似于创建的server
//  1.进行路由匹配的时候不论什么方法，什么请求都可以
//  2.请求路径中的第一部分只要与/index相等即可,并不要求路径完全相等
// /index/aaa/aaa/aaa可以  /indexaaaaa是不可以的
app.use('/index', function(req,res){
   res.end('hello world') ;
});
app.listen(3000, function(){
    console.log('启动成功');
})
```

请求路由的方法3：

```javascript
var express = require('express');
var app = express();  // app 类似于创建的server
//  1.进行路由匹配的时候不论什么方法，什么请求都可以
//  2.请求路径中的pathname要完全一样
app.all('/index', function(req,res){
   res.end('hello world') ;
});
app.listen(3000, function(){
    console.log('启动成功');
})
```

请求路由的方法4：正则表达式

```javascript
var express = require('express');
var app = express();  // app 类似于创建的server
//  1.进行路由匹配的时候不论什么方法，什么请求都可以
app.all(/^\/index(\/.+)*$/i, function(req,res){
   res.end('hello world') ;
});
app.listen(3000, function(){
    console.log('启动成功');
})
```

### 路由参数

```javascript
var express = require('express');
var app = express();  // app 类似于创建的server
//  1.进行路由匹配的时候不论什么方法，什么请求都可以
app.get('/index/:year/:month/:day', function(req,res){  //  获取参数的时候，以冒号开头
   res.send(req.params);
});
app.listen(3000, function(){
    console.log('启动成功');
})
```

静态资源处理：

```javascript
var express = require('express');
var path=require('path');
var app = express();  
var fn=express.static(path.join(__dirname,'public'));  // 将请求的路径+public下边的都看作是静态资源
app.use('/',fn);  // 请求/ 访问的静态资源都会返回
app.listen(3000, function(){
    console.log('启动成功');
})
```

```javascript
var express = require('express');
var path=require('path');
var app = express();  
app.use('/www',express.static(path.join(__dirname,'public')));  
app.use('/www',express.static(path.join(__dirname,'ppp')));  // 如果请求的public和ppp文件夹都有相同的一个文件，则返回第一个文件夹public中的，如果请求的资源pubic没有，而ppp文件夹有，则返回ppp中的
app.listen(3000, function(){
    console.log('启动成功');
})
```

其他用法：

```javascript
var express = require('express');
var path=require('path');
var app = express();  
app.get('/user',function(req,res){
    res.json({name:'张三',age:12});
});  
app.get('/ppp',function(req,res){
    //  res.redirect('http://www.baidu.com');  // 直接实现页面的重定向
    res.redirect(301,'http://www.baidu.com');  // 可以加一个状态码
});
app.get('/aaa',function(req,res){
    res.sendFile(path.join(__dirname,'public','a.jpg'),function(err){  // 返回一个文件 
        if(err){
            throw err;
        }
        console.log('OK')
    });  // 可以加一个状态码
});
app.get('/notfound',function(req,res){
    res.status(404).end('文件不存在');
});
//  app.user('/',function(){})   app.user(function(){} 这两种写法是等价的

app.listen(3000, function(){
    console.log('启动成功');
}
```

### 路由封装

router.js

```javascript
var express=require('express');
var router=express.Router();
var handler=require('./handler.js');
router.get('/index',function(req,res){
    //  返回index页面
    handler.index();
});
router.use('/resources',express.static(path.join(__dirname,'resources')));
router.get('/main',haldler.main);  // 因为Handler.main 保存的是function(req,res){},所以这儿不需要传递req,res这两个参数
module.exports=router;  // 将router暴露出去在别的js文件中使用
```

app.js

```javascript
var express = require('express');
var path=require('path');
var app = express();  
var router=require('router.js');
app.use(router);  // 通过app.use()将app和router挂在到一起
app.listen(3000, function(){
    console.log('启动成功');
}
```

handler.js

```javascript
var path=require('path');
var ejs=rquire('ejs');
var html='<h1><%= username%></h1>'
//  处理index
module.exports.main=function(req,res){
   // res.send('main页面');
    res.sendFile(path.join(__dirname,'page','index.html'));  // sendFile一般不使用，因为其中的一些模板引擎的代码不会执行。用res.render()方法
}
module.exports.index=function(req,res){
    //  默认 render 方法是不能使用的，需要为 express 配置一个模板引擎，然后才可使用
    var result=ejs.render(html,{username:'张三'});
}
module.exports.item=function(req,res){
    ejs.renderFile(path.join(__dirname,'index.html'),{title:'标题',msg:'hello world'},function(err,result){
         console.log(result)
    })
}
```

index.html页面：

```html
<!DOCTYPE html>	
<html lang="en">
    <head>
        <meta charsst="UTF-8">
        <title><%= title%></title>
    </head>
</html>
<body>
    <% for(var i =0;i<5;i++){ %>
    <li><%=msg %></li>
    <% }%>
</body>
```



以上就是express模块基本封装路由；















