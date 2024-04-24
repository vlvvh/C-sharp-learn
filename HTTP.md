# HTTP
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/677147a7-2d55-4600-ab22-20a09cd458e3) 

## 一、HTTP 简述
- HTTP 全称为 超文本传输协议。是一种应用比较广泛的应用层协议。
  
**那何为超文本❓**  
🈯️的是传输的内容不仅仅是文本，如：html、css等数据，还可以是一些其他资源，如图片、视频、音频等。
  
###  HTTP 工作过程
HTTP协议是超文本传输协议，是客户端或其他程序与 Web 服务器之间的应用层通信协议。
- HTTP 协议 是服务器 与客户端 进行数据传输 的规则

HTTP 的工作过程：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cdc65cb0-36e7-4c7b-9781-37731d52b71b)
:white_check_mark: 当我们在浏览器中输入一个 搜狗搜索的"网址"(URL)时，浏览器就给搜狗的服务器发送了一个 HTTP 请求，当搜狗的服务器收到 来自浏览器的HTTP 请求后就返回了一个 HTTP 响应给浏览器，浏览器解析 HTTP响应 的结果后，就展示成我们所看到的页面内容。   

在这个过程中，浏览器可能会给服务器发送多个 HTTP 请求，服务器会对应返回多个响应，这些响应里就包含了页面的HTML，CSS、图片、字体等

## 二、HTTP 方法 
HTTP 方法 指的是 HTTP 定义了一组请求方法，以表明要对 给定资源执行的操作 或者 指示针 对 给定资源 要执行 的期望动作。 
常见的 HTTP方法 如下：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/290d9162-5201-4190-9da2-199572311fab)
- 最常见 HTTP方法 就是 GET方法 和 POST方法

### 1.GET 请求方法
:large_orange_diamond: GET 是最常用的 HTTP 方法，用于获取服务器上某个资源。    
在浏览器中直接输入URL，此时浏览器就会发送出一个GET请求;另外HTML中的link，img，script等标签，也会触发GET请求。
除了上述两种触发GET请求的方法外，使用 JavaScript 中的 Ajax 也能构造GET请求。

:small_orange_diamond: GET 请求 的特点:

1 )请求行的HTTP方法为GET，URL的 query string 可以为空，也可以不为空。   

2 )请求报头header部分有若干组键值对结构的属性，每组键值对独占一行，header部分遇到空行结束    

3 )请求正文body部分为空

### 2.POST 请求方法    
:large_orange_diamond: POST 方法也是一种常见的 HTTP方法，常用于将用户输入的数据提交给服务器（如登录页面）。
通过 HTML 中的 form 标签可以构造 POST 请求，或者使用 JavaScript 的 ajax 也可以构造 POST 请求。

:small_orange_diamond:POST 请求 的特点:

1 )请求 行的HTTP方法 为POST，URL的 query string 一般为空，也可以不为空    

2 )请求 报头header 部分有若干组键值对结构的属性，每组键值对独占一行，header部分遇到空行结束。   

3 )请求 正文body部分 一般不为空，body内的数据格式通过header中的 Content-Type 指定，body的长度由 header 中的 Content-Length 指定。  

### 3.GET 和 POST 的区别 :pushpin:

（ 1 ）GET 请求 一般用于服务器中 获取数据，而 POST请求 一般用于给服务器提交数据

（ 2 ）GET 请求 的body一般为空，需要传递的数据通过 query string 传递，而 POST请求 的 query string 一般为空，需要传递的数据 通过body传递。

（ 3 ）GET 请求 一般是幂等的，而 POST 请求一般是 不幂等的 ‼️

（ 4 ）GET 请求 的结果 可以被缓存，而 POST请求 的结果不可以被缓存。(这一点是承接幂等性)

注意:GET请求和POST请求其实没有什么本质区别，上述区别只是使用习惯上的差别。在大多数场景中，两者之间可以相互转换使用拓展
     幂等性的核心特点 是任意多次执行所产生的影响均与一次执行的影响相同。

## 三、 HTTP 的 URL
URL 是 Uniform Resource Locator 的缩写，称为统一资源定位符，也就是网址。   
URL 格式示意图：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/5296b2ee-6bfa-4d00-b0f7-801eacb021a0)
1️⃣ https：协议方案名，常见的有 http 和 https    
2️⃣ user：pass ：登陆信息，现在网站进行身份认证一般不通过 URL 进行了，会被省略。    
3️⃣ www.example.jp ：服务器网址，此处是一个“域名”，会通过 DNS 系统 解析成一个具体的 IP 地址，真实 IP可通过 ping命令 看    
4️⃣ 80:端口号，用于描述使用是哪个程序，其被可省略。当端口号省略时，浏览器会根据协议类型自动决定使用哪个端口。     
- 例如 http 协议默认使用 80 端口，https 协议默认使用 443 端口.

:five: /dir/index.html : 带层次的文件路径，用于找到程序目录下的哪个文件，可以被省略。   
6️⃣ ?userId=1 :查询字符串(query string)，本质是一个键值对结构，键值对之间使用&分隔，键和值之间使用=分隔。       
7️⃣ #ch1:片段标识符，主要用于实现页面内跳转，可以被省略。

## 四、HTTP 状态码
**常见的HTTP状态码：**
1. 200 OK：表示请求成功，属于正常状态码 🆗
2. 301 Moved Permanently：永久重定向，用于指示请求的 URL 已永久移动到新的 URL，未来 不能再恢复到原来的URL地址。
   - 当收到这种响应时，客户端会自动跳转到 新的URL地址，后续的所有访问请求都会自动改成 新URL地址
3. 302 Move Temporarily：临时重定向，用于指示请求的 URL 已临时移动到新的 URL，但未来 可能恢复到原来的URL地址。
   - 当客户端访问 原始URL时，服务器会返回一个 301状态码和新的URL地址，客户端自动跳转 新URL地址。
4. 403 Forbidden：客户端请求的资源被服务器拒绝访问，因为客户端没有权限访问该资源。
5. 404 Not Found：客户端请求的资源在服务器上🙅不存在，通常是因为客户端请求的 URL地址有错，或该资源已被删除移动等。
6. 500 Internal Server Error：服务器在处理客户端请求时发生内部错误，导致服务器无法完成请求。
7. 504 Gateway Timeout：服务器没有响应，导致网关错误 ❌

### 1、信息响应 1xx
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/df13b1f4-1740-4909-8982-dff6d1a50643)

### 2、成功状态 2xx
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/62745b29-7ff2-475a-8b36-d5e51c0d8d3d)

### 3、重定向状态 3xx
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/15e6256d-db5d-4901-a6f9-ef27ed2ec207)

### 4、客户端错误 4xx
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/31de7a56-88af-4f8d-97e6-ab4251eeb040)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cd5a0b1b-88d1-426f-b513-eea128cfee6a)

### 5、服务器错误状态 5xx
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e6df0374-2895-420c-bfc6-2093ec77f925)






