# ajax

## 课程介绍

### 上网的过程



![1571138744998](C:\Users\子非鱼焉\AppData\Roaming\Typora\typora-user-images\1571138744998.png)

* 域名：
  * 域名和IP地址一样，可以找到网络中的一台计算机
  * IP地址是标识网络设备的唯一标识，同一个网络中的计算机，IP地址绝对不会重复

``` 
客户端发送请求，服务器响应，发送资源到客户端通过浏览器工具可以查看每一次的网络请求（在开发者工具的network里面）；
服务器=计算机+服务器软件
```

### 搭建web服务器环境

* 编写程序，将自己的电脑变成服务器
* 注意点：
  - 开启服务后，终端面板可以关闭。关闭终端面板并不表示关闭服务。
  - 服务不能重复启动，否则报错。
  - 我们自己创建的文件必须放到 `public` 文件夹中。
  - ==访问 `public` 里面的文件，必须用访问服务器的方式去访问，即必须使用IP地址或域名访问，不能以文件方式打开

## ajax技术

> ajax是一种技术，可以通过这种技术实现客户端和服务器的请求响应过程。无需在浏览器地址栏回车，刷新，只需要执行一段jsdiam就可以实现请求响应过程

``` js
$.ajax({
    type:请求类型，
    url：请求地址，
    data:请求数据
    dataType：响应数据类型
    contentType：false，
    processData：false，
    success:function(res){
        //请求成功后执行的函数
    },
    beforeSend:function(){
        //开始发送前执行的函数
    },
    complete:function(){
        //请求结束后触发的函数
    }
})
```

### type  请求类型	

*  type:'GET',  GET类型
  * 可以在发送请求的时候携带一些数据
  * 都可以获取服务器返回的数据
  * get：获取，得到，这种方式的请求一般只用于请求数据，不会改变服务器上的资源。

* type：'POST'  POST类型
  * 可以在发送请求的时候携带一些数据
  * 可以得到服务器返回的数据
  * post：派送，投递，这种请求方式用于向服务器**提交数据**，可能会修改服务器上的资源

### url请求地址（必写属性）

* url：'http://localhost:4000/test.txt' 
* url:'/test.txt'  
* url:'/common/time'
  * url可以是一个服务器上的资源的一个地址，
  * 可以是相对于当前文件的一个相对地址
  * url可以是一个接口，后端提供的

### data请求数据

> ​	向接口发送请求的时候，携带的数据，也就是接口文档中请求的参数

* 写法

  * 对象形式:

    ```js
    data:{
        name:xxx,
        age:xxx,
        ……
    }
    ```

  * 字符串形式

    ```js
    data:'name=xxx&age=xxx'
    ```

**无论是哪种写法，发送给服务器的都是字符串形式，多个请求参数之间用&连接**

* get方式的请求时，字符串和接口之间使用字符串隔开

  ```txt
  http://localhost:4000/common/checkUser?username=zhangsan
  ```

### dataType

> ​	响应数据的格式，当我们知道服务器返回的数据的格式，最好指定该属性

* 可选的值
  * json  常见的数据格式，可以以简单的语法表示复杂的数据类型
  * text  表示服务器返回的文本类型的数据
  * xml   表示服务器返回的是xml格式的数据
  * jsonp、script、html 等其他值

**指定该项，jQuery会自动将服务器响应的数据处理成js数据（数组，对象，字符串，布尔等等）**

### beforeSend()  

> ​	在发送ajax请求之前，可以做的一些事情

* 示例  比如说是Nprogress(基于jQuery)加载条插件

``` html
<link rel="stylesheet" href="./assets/nprogress.css">
<script src="./assets/jquery.js"></script>
<script src="./assets/nprogress.js"></script>
$.ajax({
	…………
    beforeSend:function()
    	NProgress.start();
     },
     complete:function(){
        NProgress.done();
      }            
})


```

### complete()

> ​	在ajax请求结束之后，可以做的一些事情

## 原生Ajax请求

> 原生的ajax基于js的内置对象XMLHTTPRequest  提供的API实现的

### 基本语法

#### GET方式写法

```	js
//1.实例化XMLHTTPRequest对象
var xhr=new XMLHttpRequest();
	//1.1兼容IE5/6的xhr对象创建
var xhr=window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');
//2.创建一个ajax请求，指定类型和地址
xhr.open('GET','接口地址?参数=值&参数=值');
//3.发送ajax请求  此步骤宝石开始发送请求
	//在发送之前，指定服务器返回的数据类型
xhr.responseType='json'
xhr.send();
//4.准备一个事件，在响应结束后，会触发该事件，接收服务器响应的结果
xhr.onload=function(){
    xhr.response //响应的结果
}
//4.1  onreadystatechange事件，兼容性更好
xhr.onreadystatechange=function(){
    //readyState不同的值可以得到不同的ajax状态
    if(readyState==4){
        //状态下执行的函数
    }
}

```

**注意，get方式如果有请求参数，将参数拼接在接口地址的后面**

```	
http://localhost:4000/common/checkUser?username=zhangsan
```

#### post方式写法

```js
//1.实例化XMLHTTPRequest对象
var xhr=new XMLHttpRequest();
	//1.1兼容IE5/6的xhr对象创建
var xhr=window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');
//2.创建一个ajax请求，指定类型和地址
xhr.open('GET','接口地址?参数=值&参数=值');
//3.设置请求头
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
//4.发送ajax请求  此步骤开始发送请求  传入请求参数
	//在发送之前，指定返回的数据类型
xhr.responseType='json';
xhr.send('name=礼拜&content='低头思故乡');
//4.准备一个事件，在响应结束后，会触发该事件，接收服务器响应的结果
xhr.onload=function(){
    xhr.response //响应的结果
}
//4.1  onreadystatechange事件，兼容性更好
xhr.onreadystatechange=function(){
    //readyState不同的值可以得到不同的ajax状态
    if(readyState==4){
        //状态下执行的函数
    }
}
```

**和get方法相比多了一行设置请求头的代码，并且请求参数当做send(请求参数)的参数传入**

### 原生ajax的get和post的区别

* 由于get方式将请求参数拼接在url中，浏览器对于url的长度支持有线，所以只能附加少量数据
* post 要设置请求头，将请求参数写在send()里面，对参数的长度没有要求

## 浏览器的小问题

> ​	ajax是一个耗时操作，所以在一次ajax请求过程中有好几个阶段。

### readyState属性 和onreadystatechange 事件

| readyState | 状态描述         | 说明                                                         |
| :--------- | ---------------- | ------------------------------------------------------------ |
| 0          | UNSENT           | 代理（XHR）被创建，但尚未调用 `open()` 方法。                |
| 1          | OPENED           | `open()` 方法已经被调用，建立了连接。                        |
| 2          | HEADERS_RECEIVED | `send()` 方法已经被调用，并且已经可以获取状态行和响应头。    |
| 3          | LOADING          | 响应体（服务器返回的数据）下载中， `responseText` 属性可能已经包含部分数据。 |
| **4**      | **DONE**         | **响应体（服务器返回的数据）下载完成，可以直接使用 responseText或response 获取完整的结果。** |

### ajax 的两个版本

* 版本一：最初的XHR对象提供的API基本兼容所有的浏览器
  * open()--设置请求方式，请求地址，同步或者异步
  * send()--发送请求，接收post方式的请求参数
  * readyState---ajax的状态，值（0,1,2,3,4）
  * onreadystatechange---ajax状态发生改变时触发的函数
  * responseText--用于接收服务器返回的**文本类型**的结果
* 版本二：基本不再支持IE678
  * onload---当请求响应成功后会触发
  * onprogress--响应的数据，正在接收中会触发
  * onloadstart--当请求开始的时候，会触发
  * onloadend--当请求结束的时候会触发
  * response--可以接受任何类型的响应结果
  * responseType--响应的数据类型

### IE的缓存问题

> ​	缓存问题是当两次或者多次请求ajax  GET请求同一个url的时候，IE浏览器会在第二次请求的时候，不会真的向服务器发请求，而是把上次请求的数据返回

* 解决方案：

  * 保证每一次请求的url不同即可  在地址上拼接时间戳或者随机数

    ```js
     xhr.open('GET', '/time?abc=' + Date.now());
    ```

    

## 封装原生ajax!!!!

> ​	仿照jQuery封装的ajax方法，封装原生ajax

```js
 window.ajax = function (obj) {
     //兼容IE5/6
        var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');
        if (obj.type == "GET") {
            xhr.open(obj.type, obj.url+obj.data);
            xhr.send();

        } else if (obj.type == 'POST') {
            xhr.open(obj.type, obj.url + '?' + obj.data);
            xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
            xhr.send(obj.data);
        }
        xhr.responseType = obj.dataType;
        xhr.onreadystatechange = function () {
            if (this.readyState == 3) {
                obj.beforeend();
                // obj.success(this.response);
            } else if (this.readyState == 4) {
                obj.success(this.response);
                obj.complete();
            }
        }
    };
```

## FormData对象

> ​	FormData是H5中新增的一个内置对象，可以降数据编译成键值对，以便用XMLHTTPRequest来发送数据，主要用于发送表单数据，，也可以单独发送带键数据，独立于表单使用

* 使用方式一（发送表单数据）

  ```js
  //1.获取一个form表单的对象
  var fm=document.querSelector('form');
  //2.实例化一个formData  传入form表单对象，
  var formdata=new FormData(fm);//该对象中存有form的全部数据
  …………
  xhr.send(formdata);
  
  ```

  **使用FormData来提交表单数据时，表单必须有name属性，不然收集不到**

* 使用方式二

  ```
  <input type='text' id='user'>
  <script>
  	var formdata=new FormData();
  	var user=document.querySelector('#user')
  	formdata.append('username',user.value);
  	//获取数据
  	formdata.get('username');
  	//传递数据
  	…………
  	xhr.send(formdata);
  </script>
  ```

  

![formData](C:\Users\子非鱼焉\Desktop\formData.png)

