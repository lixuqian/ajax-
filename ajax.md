# ajax

## 课程介绍

### 上网的过程



![1571138744998](E:\培训文件夹\就业班\ajax\总结笔记\assets\1.png)

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

  **ajax只可以提交字符串，现在可以提交二进制数**



![formData](E:\培训文件夹\就业班\ajax\总结笔记\assets\formData.png)

* jQuery使用formdata
  * formData是一种特殊的数据类型，接收的form对象必须是**DOM对象**，而不是jQuery对象

```js

    var fm=$('form');
    var formdata=new FormData(fm[0]);//此处将jQuery对象转为DOM对象
    $.ajax({
    	type:'POST',
    	url:'',
    	//data只能是字符串类型，传入{}时也会转为'name=xxx&age=xx'
    	data:formdata,
    	//此时是formData类型的需要做如下操作
    	processData:false,//不让jQuery把formData对象转为字符串
    	contentType:false,//不让jQuery去设置content-type，让FromData去处理
    	success:function(res){
            
    	}
})
```

* 参考链接

  https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects

## ajax综合案例

### 1.加载数据

* 根据接口文档来写，具体参考接口文档
* 功能实现的注意点
  * 在获取到接口数据后渲染到页面上来是用到$('selector').append(jquer对象/str);
    * append()可以接收一个jQuery对象也可以接收一个字符串，可以自动解析HTML

### 2.懒加载

* 确定加载的时机----在即将触底的时候
  * 文档的高度-浏览器的高度-卷入的高度<100 为条件
    * 涉及到的文档高度  $(document).height();
    * 浏览器的高度  $(window).height();
    * 卷入的高度(只能在scroll事件中才可以获取到)$(document).scrollTop();

* jQuery中的height()方法

  * height() 方法设置或返回被选元素的高度。

  * 当该方法用于**返回**高度时， 则返回第一个匹配元素的高度。

  * 当该方法用于**设置**高度时，则设置所有匹配元素的高度。

  * 如下面的图像所示，该方法不包含 padding、border 或 margin。

![height](E:\培训文件夹\就业班\ajax\总结笔记\assets\height.png)

* 懒加载的问题

  如果手抖的话，可能重复触发，重复加载数据，就是在正在加载的时候又触发加载

  * 解决方法--开关思想
    * 在开始加载的时候将开关关闭，在加载结束后再次打开开关
    * 具体实现

```js
 function loadData() {
        //在加载数据的开始置假
        flag = false;
        $.ajax({
            type: 'GET',
            url: '/member/list-page',
            data: {
                page: page,
            },
            dataType: 'json',
            success: function (res) {
                .....
                //将数据成功渲染到页面后，表示加载结束了
                $('#members').append(str);
                //将开关打开
                flag = true;
            }
        })
    }
    //判断的时候验证开关
    if (documentHeight - windowHeight - scrollTop <= 100 && flag){
        //加载数据
    }
    
```

### 3.删除功能

删除功能首先是给  删除  加上点击事件，点击删除，由于是后来动态添加的元素，所以用到了事件委托，其次是要在数据库删除，也要在DOM树上删除

* 事件委托实现点击事件

  $('body').on('事件名称','委托的元素'，function(){})

  * 委托的元素传入的是一个选择器

* 调用删除接口删除接口的数据，传入删除的id，
  * 前提是当前元素上有id这个属性，所以要添加一个自定义属性的id,以便获取
  * 获取自定义属性$('selector').attr('自定义属性');

* 接口数据的数据删除成功后，删除DOM树上节点
  * jQuery对象.remove();
  * 注意问题
    * 在success中this的指向会改变，注意这个问题

### 4.查看详情功能

查看详情首先是给 view  加上一个点击事件，点击跳转detial页面，获取详细信息的接口中要求的请求参数是id，所以在跳转的时候需要携带一个id参数，

**a标签的跳转就是根据href中的url进行跳转，  是get请求方式，可以将id拼接在地址中**

* 具体方法  将id拼接在地址中

```html
<a href="detail.html?id=${item.id}" class="card-link">View</a>
```

* 跳转后获取id信息
  * location.search  获取地址栏的参数  --->"?id=123"
  * 字符串截取方法  substr/replace

```js
//substr方法
stringObject.substr(start,[length])//包前不包后  返回截取的字符串
location.search.substr(4);

//replce方法
stringObject.replace(regexp/substr,replacement)//返回替换后的字符串
location.replace(/^\D/g,'');//将非数字的字符替换为空  
```

* 注意问题：
  * **一定要注意返回的数据的格式，决定怎样进行渲染，不要思维固化**

### 5.添加会员

添加会员  也是 在点击之后跳转到add.html 页面，然后，上传图片，文本等表单控件的值

* 预览图片

给file表单控件添加一个change事件，此事件在选择文件发生变化时触发执行，需要得到一个临时地址 进行图片预览

* 获取临时地址的具体做法

```
var file=this.files[0]//DOMfile文件对象通过DOM对象的files属性获取，
//临时地址url的获取
var url=URL.createObjectURL(file);//传入一个文件对象

```

* 表单控件的属性介绍

  input:file 标签属性介绍

  - accept：限制上传文件的文件类型
    - accept=".jpg,.png,.gif"       一个一个后缀指定
    - accept="image/*"    表示允许任何的图片类型
  - multiple：多选

* 提交给接口 使用FormData

  * input的name属性必须和接口中要求的参数名称一致

  * 注意使用formData作为数据传递给接口，必须设置一下两个属性
    * processData:false;
    * contentType:false;

* 上传到接口数据成功后跳转到index页面，index页面上来就加载数据，会展示新添加的数据

  * 阻止a标签的默认跳转行为，e.preventDefault();

## Ajax补充

> Ajax就是Asynchronous Javascript And XML(异步的javascript和XML)
>
> 是一种通过js代码完成客户端和服务器交互的技术

## jQuery中的ajax补充

* $.ajax({})
* $.get('url',[data],[fn],[dataType])
* $.post('url',[data],[fn],[dataType])

**其中请求参数data可以是对象，可以是字符串的写法**

##  其他的封装库anxios

中文网站：<http://www.axios-js.com/zh-cn/docs/

```js
// 获取服务器的返回值
axios.get('/common/get?id=123').then(function(res) {
    // res 就是本次请求的信息
    console.info(res);
    // 获取 从服务器返回的数据
    console.info(res.data);
});

axios.get('/common/get', { params: { id: 123, name: 'jake' } }).then(function(res) {
    // res 就是本次请求的信息
    console.info(res);
    // 获取 从服务器返回的数据
    console.info(res.data);
});

// post请求，不带参数
axios.post('/common/post').then(function(res) {
    console.info(res.data);
});
// post请求，带参数
axios.post('/common/post', { id: 123, name: 'jake' }).then(function(res) {
    console.info(res.data);
});
```

## 模板引擎

> 客户端中拿到请求数据后最常见的就是把这些数据呈现在页面上，
>
> 如果数据结构接单，可以直接通过字符串操作（拼接）的方式，但是数据过于复杂，字符串拼接维护的成本太大
>
> 模板引擎其实就是一个API，模板引擎有很多种，使用方式大同小异，目的是为了更容易更高效的将数据渲染到HML字符串中。将HTML和js整合在一起

### 使用模板引擎的步骤

* 1.准备一个存放数据的盒子(不是必须的，使用body也可以)；
* 2.引入template-web.js 文件
* 3.定义模板，一定要指定script的type属性  
* 4.调用template函数，为末班分配数据，
  * template(模板的id,分配的数据)//分配的数据必须是js对象形式
  * template的返回值是数据和模版组合好的结果

```js
//引入文件
<script src="./assets/template-web.js"></script>
//模板
<script type="text/html" id="test">
        <h2>{{title}}</h2>
        <p>{{age}}</p>
        <ul>
            <li>{{heroes[0]}}</li>
            <li>{{heroes[1]}}</li>
            <li>{{heroes[2]}}</li>
    </ul>
</script>

<script>
    // 下面的数据是模拟的，相当于ajax请求之后，服务器返回的数据
    var data = {
        title: '模板引擎练习',
        age: 20,
        heroes: ['曹操', '刘备', '李逵', '张飞']
    };
    // 调用template函数
    /*
        var str = template(模板的id, 数据); // 数据必须是JS对象格式
        */
    var str = template('test', data);
    console.log(str);
</script>

```

### 模板字符串的语法

* 输出普通的数据  {{}}
* 条件判断

```js
{{if age>18}}
//大于18要显示的内容
{{else}}
//小于等于18要显示的内容
{{/if}}
```

* 循环

```js
{{each 遍历的数组}}
	{{$index}}//$index 是当前遍历项的下标
	{{$value}}//$value 是当前遍历的值
{{/each}}
```

### 案例中具体用法示例

```js
<script src="./assets/template-web.js"></script>

<script type="text/html" id="li">
  {{each msg}}
  <li>
      <div class="info"><img src="images/03.jpg"><span>{{$value.name}}</span>
        <p>发布于：{{$value.created}}</p>
      </div>
      <div class="content">{{$value.content}}</div>
    </li>
    {{/each}}
</script>


<script>
  //模板引擎：可以将分开的HTML和js整合在一起的一个库
  $.get('/message/getMsg',function(res){
    var str='';
    res.forEach(element=>{
      // console.log(res);
        //传入的必须是一个对象，所以要用花括号包裹
      str=template('li',{
        msg:res
      });
      
    });
    console.log(str);
    
    $('ul').html(str);
  },'json')
</script>

```

