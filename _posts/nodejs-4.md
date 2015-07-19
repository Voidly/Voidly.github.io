title: 玩转Node.js（四）-搭建简单的聊天室
date: 2015-04-12 21:27:16
tags: Node.js
categories: Node.js
description: 使用socket.io实现简单的聊天室，此项目主要用到了Socket.IO的emit()方法和on()方法，emit()方法用于创建和触发事件，on()方法用于监听事件。
---

Nodejs好久没有跟进了，最近想用它搞一个聊天室，然后便偶遇了socket.io这个东东，说是可以用它来简单的实现实时双向的基于事件的通讯机制。我便看了一些个教程使用它来搭建一个超级简单的聊天室。

##初始化项目

在电脑里新建一个文件夹，叫做“chatroom”，然后使用npm进行初始化：

	$ npm init	

然后根据提示以及相关信息一步一步输入，当然也可以一路回车下去，之后会在项目里生成一个package.json文件，里面的信息如下：

	$ cat package.json
	{
	  "name": "chatroom",
	  "version": "1.0.0",
	  "description": "a room which lets people chat",
	  "main": "index.js",
	  "scripts": {
	    "test": "nothing"
	  },
	  "author": "voidy",
	  "license": "ISC",
	 }

这个文件描述了项目的相关信息。

##安装socket.io

接下来，安装socket.io：

	$ npm install socket.io --save

安装Socket.IO的时候，--save参数用于保存模块依赖信息到package.json文件，安装完后，打开package.json文件会看到里面多了一项内容：

	"dependencies": {
	    "socket.io": "^1.2.1"
	}

同时Socket.IO安装在了node_modules文件夹下。

##实现聊天-服务器端

首先我们来编写服务器端程序，新建文件“index.js”，在里面导入如下模块：

	var http = require('http');           
	var socketio = require('socket.io');  
	var fs = require('fs'); 

第一行是导入http模块，先前我们已经见过了，用于创建http server。

第二行是导入socket.io模块，实现实时聊天必备的，不再赘述。

第三行是导入fs模块，用于读取文件。具体的一会你就会了解到。

通过http模块创建app，在刚刚的代码中加入如下语句：

	var app = http.createServer(function(req, res) {
	 
	    fs.readFile(__dirname + '/index.html', function (err, data) {
	        if (err) {
	            res.writeHead(500);
	            return res.end('Error loading index.html');
	        }
	        res.writeHead(200); 
	        res.write(data);   
	        res.end();
	    });
	}).listen(8888);

fs.readFile（）方法用于读取文件，在这里读取的是index.html文件，也就是一会即将要编写的前端聊天室的展示页面。

第8行是返回请求的状态码，第9行是返回读取到的内容给浏览器。然后这个http server使用listen方法监听8888端口。

接下来，就是使用socket.io来实现聊天的事件了。在刚刚的index.js文件的http server下面接着创建socket.io对象。

	var io = socketio(app);

然后监听connection事件，当浏览器连接到此Socket.IO服务时触发该事件：

	io.on('connection', function (socket) {
	    // 监听浏览器端的chat事件
	    socket.on('chat', function (data) {
	        console.log(data);
	        io.emit('sendmsg', data); 
	    });
	});

第4行用于将信息输入到后台的显示器，第5行用于将内容发送给客户端，为了知道服务器是否启动，我在后面又加了如下一句：

console.log("Server is running at http://localhost:8888")；

至此，服务器端编码完成。

##实现聊天-客户端

首先实现界面部分，仅仅有显示消息记录以及消息发送框，代码如下：

	<html>
	    <head>
	        <meta charset="utf-8">
	        <title>Socket.IO chat</title>
	        <style>
	            * { margin: 0; padding: 0; box-sizing: border-box; }
	            body { font: 13px Helvetica, Arial; }
	            #form { background: #000; padding: 3px; position: fixed; bottom: 	0; width: 100%; }
	            #form input { border: 0; padding: 10px; width: 90%; margin-right:	 .5%; }
	            #form button { width: 9%; background: rgb(130, 224, 255); border:	 none; padding: 10px; }
	            #messages { list-style-type: none; margin: 0; padding: 0; }
	            #messages li { padding: 5px 10px; }
	            #messages li:nth-child(odd) { background: #eee; }
	        </style>
	        <!-- 引入socket.io库和jQuery库 -->
	        <script src="https://cdn.socket.io/socket.io-1.2.1.js"></script>
	        <script src="http://code.jquery.com/jquery-1.11.1.js"></script>
	    </head>
	    <body>
	        <!-- 显示消息记录 -->
	        <ul id="messages"></ul>
	
	        <!-- 消息发送框 -->
	        <div id="form">
	            <input id="m" autocomplete="off" /><button id="send">Send</button	>
	        </div>
	    </body>
	</html>

然后需要在里面加上JS来实现与服务器端的通信，将服务器端获取的数据展示到客户端，主要代码如下：


	<script>
	// 连接到Socket.IO服务器
	var socket = io.connect();
	
	$(function() {
	 // 绑定发按钮发送消息的事件
	    $('#send').on('click', function() {
	        var data = $('#m').val();
	      // 创建chat事件并发送消息给服务器
	        // emit('event') 表示发送了一个event命令
	        // 使用io.on('event')接收对应事件的信息
	        // 所以客户端服务器端需要使用socket.on('chat')接收聊天信息
	        socket.emit('chat', { msg: data });
	        $('#m').val('');
	    });
	});
	
	$(function() {
	   // 接收消息并显示到消息记录框中
	    socket.on('sendmsg', function(msg) {
	        $('#messages').append($('<li>').text(msg.msg));
	    });
	});
	</script>

此时，可以执行：

	$ node index.js

然后在浏览器打开localhost:8888查看效果。

至此，一个简陋的聊天室就实现了，有兴趣的朋友可以在此基础上进行扩展，实现功能更为复杂的聊天室。

项目代码在github上：<https://github.com/Voidly/chatroom>。

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**