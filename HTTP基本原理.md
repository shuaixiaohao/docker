## HTTP基本原理

URI 的全称为 Uniform Resource Identifier，即统一资源标志符

URL 的全称为 Universal Resource Locator，即统一资源定位符

 

HTTP 的全称是 Hyper Text Transfer Protocol，中文名叫做超文本传输协议

HTTPS 的全称是 Hyper Text Transfer Protocol over Secure Socket Layer，是以安全为目标的 HTTP 通道，简单讲是 HTTP 的安全版，即 HTTP 下加入 SSL 层，简称为 HTTPS，它传输的内容都是经过 SSL 加密的。

作用：

- 是建立一个信息安全通道，来保证数据传输的安全。
- 确认网站的真实性，凡是使用了 https 的网站，都可以通过点击浏览器地址栏的锁头标志来查看网站认证之后的真实信息，也可以通过 CA 机构颁发的安全签章来查询

 

**请求**

可以分为 4 部分内容：请求方法（Request Method）、请求的网址（Request URL）、请求头（Request Headers）、请求体（Request Body）

GET 和 POST 区别：

- GET 请求中的参数包含在 URL 里面，数据可以在 URL 中看到，而 POST 请求的 URL 不会包含这些数据，数据都是通过表单形式传输的，会包含在请求体中。
- GET 请求提交的数据最多只有 1024 字节，而 POST 方式没有限制

 

请求头，用来说明服务器要使用的附加信息，比较重要的信息有 Cookie、Referer、User-Agent 等

请求体，一般承载的内容是 POST 请求中的表单数据，而对于 GET 请求，请求体则为空

 

**响应**

可以分为三部分：响应状态码（Response Status Code）、响应头（Response Headers）和响应体（Response Body）。

响应状态码：

100   继续

200   成功

301   永久移动     请求的网页已永久移动到新位置，即永久重定向

302   临时移动

400   错误请求     服务器无法解析该请求

401   未授权         请求没有进行身份验证或验证未通过

403   禁止访问     服务器拒绝此请求

404   未找到         服务器找不到请求的网页

500   服务器内部错误

 

响应头，包含了服务器对请求的应答信息，如 Content-Type、Server、Set-Cookie 等

响应体，最重要的当属响应体的内容了。响应的正文数据都在响应体中

 

## 爬虫基本原理

获取网页

urllib、requests

提取信息

正则表达式，Beautiful Soup、pyquery、lxml库

保存数据

TXT 文本、JSON 文本、数据库

 

## 会话和Cookies

HTTP 的一个特点，叫作无状态，保持 HTTP 连接状态的技术就出现了，它们分别是会话和 Cookies。会话在服务端，也就是网站的服务器，用来保存用户的会话信息；Cookies 在客户端，也可以理解为浏览器端

Cookies 指某些网站为了辨别用户身份、进行会话跟踪而存储在用户本地终端上的数据

**会话维持**

客户端第一次请求服务器时，服务器会返回一个响应头中带有 Set-Cookie 字段的响应给客户端，用来标记是哪一个用户，客户端浏览器会把 Cookies 保存起来。当浏览器下一次再请求该网站时，浏览器会把此 Cookies 放到请求头一起提交给服务器，Cookies 携带了会话 ID 信息，服务器检查该 Cookies 即可找到对应的会话是什么，然后再判断会话来以此来辨认用户状态

 

Cookie 的 Max Age 或 Expires 字段决定了过期的时间

 

## 代理基本原理

代理实际上指的就是代理服务器，英文叫作 proxy server，它的功能是代理网络用户去取得网络信息。形象地说，它是网络信息的中转站。在我们正常请求一个网站时，是发送了请求给 Web 服务器，Web 服务器把响应传回给我们。如果设置了代理服务器，实际上就是在本机和服务器之间搭建了一个桥，此时本机不是直接向 Web 服务器发起请求，而是向代理服务器发出请求，请求会发送给代理服务器，然后由代理服务器再发送给 Web 服务器，接着由代理服务器再把 Web 服务器返回的响应转发给本机。这样我们同样可以正常访问网页，但这个过程中 Web 服务器识别出的真实 IP 就不再是我们本机的 IP 了，就成功实现了 IP 伪装，这就是代理的基本原理

 

## urllib

包含如下 4 个模块：

- **request**：它是最基本的 HTTP 请求模块，可以用来模拟发送请求。就像在浏览器里输入网址然后回车一样，只需要给库方法传入 URL 以及额外的参数，就可以模拟实现这个过程了。
- **error**：异常处理模块，如果出现请求错误，我们可以捕获这些异常，然后进行重试或其他操作以保证程序不会意外终止。
- **parse**：一个工具模块，提供了许多 URL 处理方法，比如拆分、解析、合并等。
- **robotparser**：主要是用来识别网站的 robots.txt 文件，然后判断哪些网站可以爬，哪些网站不可以爬，它其实用得比较少。



 

**request** 模块，发送请求

*urlopen*

import urllib.request    response = urllib.request.urlopen('<https://www.python.org>')   print(response.read().decode('utf-8'))

urlopen 方法的 API

urllib.request.urlopen(url, data=None, [timeout,]*, cafile=None, capath=None, cadefault=False, context=None)

data 参数是可选的。如果要添加该参数，需要使用 bytes 方法将参数转化为字节流编码格式的内容，如果传递了这个参数，则它的请求方式就不再是 GET 方式，而是 POST 方式

import urllib.parse  

import urllib.request  



data = bytes(urllib.parse.urlencode({'word': 'hello'}), encoding='utf8')  

response = urllib.request.urlopen('<http://httpbin.org/post>', data=data)  

print(response.read())

 

*Request*

urlopen 方法可以实现最基本请求的发起，但这几个简单的参数并不足以构建一个完整的请求。如果请求中需要加入 Headers 等信息，就可以利用更强大的 Request 类来构建

Request 的用法：

import urllib.request  



request = urllib.request.Request('<https://python.org>')  

response = urllib.request.urlopen(request)  

print(response.read().decode('utf-8'))

依然是用 urlopen 方法来发送这个请求，只不过这次该方法的参数不再是 URL，而是一个 Request 类型的对象。通过构造这个数据结构，一方面我们可以将请求独立成一个对象，另一方面可更加丰富和灵活地配置参数

class urllib.request.Request(url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None)

- 第一个参数 url 用于请求 URL，这是必传参数，其他都是可选参数 
- 第二个参数 data 如果要传，必须传 bytes（字节流）类型的。如果它是字典，可以先用 urllib.parse 模块里的 urlencode() 编码
- 第三个参数 headers 是一个字典，它就是请求头，我们可以在构造请求时通过 headers 参数直接构造，也可以通过调用请求实例的 add_header() 方法添加
- 第四个参数 origin_req_host 指的是请求方的 host 名称或者 IP 地址
- 第五个参数 unverifiable 表示这个请求是否是无法验证的，默认是 False，意思就是说用户没有足够权限来选择接收这个请求的结果
- 第六个参数 method 是一个字符串，用来指示请求使用的方法，比如 GET、POST 和 PUT 等

 

from urllib import request, parse  



url = '<http://httpbin.org/post>'  

headers = {'User-Agent': 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)',  

​    'Host': '[httpbin.org](http://httpbin.org)'  

}  

dict = {'name': 'Germey'}  

data = bytes(parse.urlencode(dict), encoding='utf8')  

req = request.Request(url=url, data=data, headers=headers, method='POST')  

response = request.urlopen(req)  

print(response.read().decode('utf-8'))

 *高级用法*

 对于一些更高级的操作（比如 Cookies 处理、代理设置等）就需要更强大的工具 Handler



**error** 模块，处理异常

 *URLError*

 打开一个不存在的页面，照理来说应该会报错，但是这时我们捕获了 URLError 这个异常

from urllib import request, error  

try:  

​    response = request.urlopen('<https://cuiqingcai.com/index.htm>')  

except error.URLError as e:  

​    print(e.reason)

 

*HTTPError*

它是 URLError 的子类，专门用来处理 HTTP 请求错误，比如认证请求失败等

from urllib import request,error  

try:  

​    response = request.urlopen('<https://cuiqingcai.com/index.htm>')  

except error.HTTPError as e:  

​    print(e.reason, e.code, e.headers, sep='\n')



**parse** 模块，解析链接

*urlparse*

该方法可以实现 URL 的识别和分段

from urllib.parse import urlparse  

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment')  

print(type(result), result)

运行结果如下

<class 'urllib.parse.ParseResult'>

ParseResult(scheme='http', netloc='[www.baidu.com](http://www.baidu.com)', path='/index.html', params='user', query='id=5',

fragment='comment')

返回结果 ParseResult 实际上是一个元组，我们可以用索引顺序来获取，也可以用属性名获取

print(result.scheme, result[0], result.netloc, result[1], sep='\n')

运行结果如下

http  

http  

[www.baidu.com](http://www.baidu.com)  

[www.baidu.com](http://www.baidu.com)



*urlunparse*

有了 urlparse 方法，相应地就有了它的对立方法 urlunparse。它接受的参数是一个可迭代对象，但是它的长度必须是 6，否则会抛出参数数量不足或者过多的问题

from urllib.parse import urlunparse  

data = ['http', '[www.baidu.com](http://www.baidu.com)', 'index.html', 'user', 'a=6', 'comment']  

print(urlunparse(data))

运行结果如下

http://www.baidu.com/index.html;user?a=6#comment



*urlsplit*

这个方法和 urlparse 方法非常相似，只不过它不再单独解析 params 这一部分，只返回 5 个结果。上面例子中的 params 会合并到 path 中

from urllib.parse import urlsplit  

result = urlsplit('http://www.baidu.com/index.html;user?id=5#comment')  

print(result)

运行结果如下

SplitResult(scheme='http', netloc='[www.baidu.com](http://www.baidu.com)', path='/index.html;user', query='id=5', fragment='comment')

返回结果是 SplitResult，它其实也是一个元组类型，既可以用属性获取值，也可以用索引来获取



*urlunsplit*

与 urlunparse 方法类似，它也是将链接各个部分组合成完整链接的方法，传入的参数也是一个可迭代对象，例如列表、元组等，唯一的区别是长度必须为 5

from urllib.parse import urlunsplit  

data = ['http', '[www.baidu.com](http://www.baidu.com)', 'index.html', 'a=6', 'comment']  

print(urlunsplit(data))

运行结果如下

<http://www.baidu.com/index.html?a=6#comment>



*urljoin*

提供一个 base_url（基础链接）作为第一个参数，将新的链接作为第二个参数，该方法会分析 base_url 的 scheme、netloc 和 path 这 3 个内容并对新链接缺失的部分进行补充，最后返回结果

from urllib.parse import urljoin  

print(urljoin('http://www.baidu.com', 'FAQ.html'))  

print(urljoin('http://www.baidu.com', 'https://cuiqingcai.com/FAQ.html'))  

print(urljoin('http://www.baidu.com/about.html', 'https://cuiqingcai.com/FAQ.html'))  

print(urljoin('http://www.baidu.com/about.html', 'https://cuiqingcai.com/FAQ.html?question=2'))  

print(urljoin('http://www.baidu.com?wd=abc', 'https://cuiqingcai.com/index.php'))  

print(urljoin('http://www.baidu.com', '?category=2#comment'))  

print(urljoin('[www.baidu.com](http://www.baidu.com)', '?category=2#comment'))  

print(urljoin('[www.baidu.com](http://www.baidu.com)#comment', '?category=2'))

运行结果如下

<http://www.baidu.com/FAQ.html>  

<https://cuiqingcai.com/FAQ.html>  

<https://cuiqingcai.com/FAQ.html>  

<https://cuiqingcai.com/FAQ.html?question=2>  

<https://cuiqingcai.com/index.php>  

<http://www.baidu.com?category=2#comment>  

[www.baidu.com](http://www.baidu.com)?category=2#comment  

[www.baidu.com](http://www.baidu.com)?category=2

base_url 提供了三项内容 scheme、netloc 和 path。如果这 3 项在新的链接里不存在，就不以补充；如果新的链接存在，就使用新的链接的部分，而 base_url 中的 params、query 和 fragment 是不起作用的。



*urlencode*

它在构造 GET 请求参数的时候非常有用，先声明了一个字典来将参数表示出来，然后调用 urlencode 方法将其序列化为 GET 请求参数

from urllib.parse import urlencode  

params = {  

​    'name': 'germey',  

​    'age': 22  

}  

base_url = '<http://www.baidu.com?>'  

url = base_url + urlencode(params)  

print(url)

运行结果如下

<http://www.baidu.com?name=germey&age=22>



*parse_qs*

利用 parse_qs 方法，就可以将它转回字典

from urllib.parse import parse_qs  

query = 'name=germey&amp;age=22'  

print(parse_qs(query))

运行结果如下

{'name': ['germey'], 'age': ['22']}



*parse_qsl*

它用于将参数转化为元组组成的列表

from urllib.parse import parse_qsl  

query = 'name=germey&amp;age=22'  

print(parse_qsl(query))

运行结果如下

[('name', 'germey'), ('age', '22')]



*quote*

该方法可以将内容转化为 URL 编码的格式。URL 中带有中文参数时，有时可能会导致乱码的问题，此时用这个方法可以将中文字符转化为 URL 编码

from urllib.parse import quote  

keyword = ' 壁纸 '  

url = '<https://www.baidu.com/s?wd=>' + quote(keyword)  

print(url)

运行结果如下

https://www.baidu.com/s?wd=% E5% A3%81% E7% BA% B8



*unquote*

unquote 方法，它可以进行 URL 解码

from urllib.parse import unquote  

url = 'https://www.baidu.com/s?wd=% E5% A3%81% E7% BA% B8'  

print(unquote(url))

结果如下

<https://www.baidu.com/s?wd> = 壁纸



**robotparser** 模块，分析 Robots 协议





## requests

**GET 请求**

HTTP 中最常见的请求之一就是 GET 请求，下面首先来详细了解一下利用 requests 构建 GET 请求的方法

import requests  

r = requests.get('<http://httpbin.org/get>')  

print(r.text)

运行结果如下：

{"args": {},   

  "headers": {  

​    "Accept": "*/*",   

​    "Accept-Encoding": "gzip, deflate",   

​    "Host": "[httpbin.org](http://httpbin.org)",   

​    "User-Agent": "python-requests/2.10.0"  

  },   

  "origin": "122.4.215.33",   

  "url": "<http://httpbin.org/get>"  

}

利用 params 这个参数

import requests  

data = {  

​    'name': 'germey',  

​    'age': 22  

}  

r = requests.get("<http://httpbin.org/get>", params=data)  

print(r.text)

获取二进制数据

import requests

r = requests.get("<https://github.com/favicon.ico>")

print(r.text)

print(r.content)  # 二进制数据

添加 headers

import requests

headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'

}

r = requests.get("<https://www.zhihu.com/explore>", headers=headers)

print(r.text)



**POST 请求**

import requests

data = {'name': 'germey', 'age': '22'}

r = requests.post("<http://httpbin.org/post>", data=data)

print(r.text)

除了使用 text 和 content 获取了响应的内容。此外，还有很多属性和方法可以用来获取其他信息，比如状态码、响应头、Cookies 等

import requests

r = requests.get('<http://www.jianshu.com>')

print(type(r.status_code), r.status_code)

print(type(r.headers), r.headers)

print(type(r.cookies), r.cookies)

print(type(r.url), r.url)

print(type(r.history), r.history)

运行结果如下

<class 'int'> 200

<class 'requests.structures.CaseInsensitiveDict'> {'X-Runtime': '0.006363', 'Connection': 'keep-alive', 'Content-Type': 'text/html; charset=utf-8', 'X-Content-Type-Options': 'nosniff', 'Date': 'Sat, 27 Aug 2016 17:18:51 GMT', 'Server': 'nginx', 'X-Frame-Options': 'DENY', 'Content-Encoding': 'gzip', 'Vary': 'Accept-Encoding', 'ETag': 'W/"3abda885e0e123bfde06d9b61e696159"', 'X-XSS-Protection': '1; mode=block', 'X-Request-Id': 'a8a3c4d5-f660-422f-8df9-49719dd9b5d4', 'Transfer-Encoding': 'chunked', 'Set-Cookie': 'read_mode=day; path=/, default_font=font2; path=/, _session_id=xxx; path=/; HttpOnly', 'Cache-Control': 'max-age=0, private, must-revalidate'}

<class 'requests.cookies.RequestsCookieJar'> <RequestsCookieJar[<Cookie _session_id=xxx for [www.jianshu.com/](http://www.jianshu.com/)>, <Cookie default_font=font2 for [www.jianshu.com/](http://www.jianshu.com/)>, <Cookie read_mode=day for [www.jianshu.com/](http://www.jianshu.com/)>]>

<class'str'> <http://www.jianshu.com/>

<class 'list'> []



**文件上传**

import requests

files = {'file': open('favicon.ico', 'rb')}

r = requests.post('<http://httpbin.org/post>', files=files)

print(r.text)



**Cookies**

获取 Cookies 的过程

import requests

r = requests.get('<https://www.baidu.com>')

print(r.cookies)

for key, value in r.cookies.items():

​    print(key + '=' + value)

这里首先调用 cookies 属性即可成功得到 Cookies，可以发现它是 RequestCookieJar 类型。然后用 items 方法将其转化为元组组成的列表，遍历输出每一个 Cookie 的名称和值，实现 Cookie 的遍历解析

直接用 Cookie 来维持登录状态

import requests

headers = {

​    'Cookie': 'q_c1=31653b264a074fc9a57816d1ea93ed8b|[1474273938000](tel:1474273938000)|[1474273938000](tel:1474273938000); d_c0="AGDAs254kAqPTr6NW1U3XTLFzKhMPQ6H_nc=|[1474273938](tel:1474273938)"; __utmv=51854390.100-1|2=registration_date=20130902=1^3=entry_date=20130902=1;a_t="2.0AACAfbwdAAAXAAAAso0QWAAAgH28HQAAAGDAs254kAoXAAAAYQJVTQ4FCVgA360us8BAklzLYNEHUd6kmHtRQX5a6hiZxKCynnycerLQ3gIkoJLOCQ==";z_c0=Mi4wQUFDQWZid2RBQUFBWU1DemJuaVFDaGNBQUFCaEFsVk5EZ1VKV0FEZnJTNnp3RUNTWE10ZzBRZFIzcVNZZTFGQmZn|[1474887858](tel:1474887858)|64b4d4234a21de774c42c837fe0b672fdb5763b0',

​    'Host': '[www.zhihu.com](http://www.zhihu.com)',

​    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36',

}

r = requests.get('<https://www.zhihu.com>', headers=headers)

print(r.text)

可以通过 cookies 参数来设置，不过这样就需要构造 RequestsCookieJar 对象，而且需要分割一下 cookies。这相对烦琐，不过效果是相同的，示例如下

import requests

cookies = 'q_c1=31653b264a074fc9a57816d1ea93ed8b|[1474273938000](tel:1474273938000)|[1474273938000](tel:1474273938000); d_c0="AGDAs254kAqPTr6NW1U3XTLFzKhMPQ6H_nc=|[1474273938](tel:1474273938)"; __utmv=51854390.100-1|2=registration_date=20130902=1^3=entry_date=20130902=1;a_t="2.0AACAfbwdAAAXAAAAso0QWAAAgH28HQAAAGDAs254kAoXAAAAYQJVTQ4FCVgA360us8BAklzLYNEHUd6kmHtRQX5a6hiZxKCynnycerLQ3gIkoJLOCQ==";z_c0=Mi4wQUFDQWZid2RBQUFBWU1DemJuaVFDaGNBQUFCaEFsVk5EZ1VKV0FEZnJTNnp3RUNTWE10ZzBRZFIzcVNZZTFGQmZn|[1474887858](tel:1474887858)|64b4d4234a21de774c42c837fe0b672fdb5763b0'

jar = requests.cookies.RequestsCookieJar()

headers = {

​    'Host': '[www.zhihu.com](http://www.zhihu.com)',

​    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36'

}

for cookie in cookies.split(';'):

​    key, value = cookie.split('=', 1)

​    jar.set(key, value)

r = requests.get('<http://www.zhihu.com>', cookies=jar, headers=headers)

print(r.text)



**会话维持**

import requests

s = requests.Session()

s.get('<http://httpbin.org/cookies/set/number/123456789>')

r = s.get('<http://httpbin.org/cookies>')

print(r.text)



**SSL 证书验证**

verify 参数控制是否检查此证书。其实如果不加 verify 参数的话，默认是 True，会自动验证

import requests

response = requests.get('<https://www.12306.cn>', verify=False)

print(response.status_code)



**代理设置**

import requests

proxies = {

  'http': '<http://10.10.1.10:3128>',

  'https': '<http://10.10.1.10:1080>',

}

requests.get('<https://www.taobao.com>', proxies=proxies)

import requests

proxies = {'https': '<http://user:password@10.10.1.10:3128/>',}

requests.get('<https://www.taobao.com>', proxies=proxies)



**超时设置**

import requests

r = requests.get('<https://www.taobao.com>', timeout=1)

print(r.status_code)



**身份认证**

import requests  

from requests.auth import HTTPBasicAuth  

r = requests.get('<http://localhost:5000>', auth=HTTPBasicAuth('username', 'password'))  

print(r.status_code)

可以直接简写如下

import requests

r = requests.get('<http://localhost:5000>', auth=('username', 'password'))

print(r.status_code)





## 正则表达式

匹配URL

[http|https]+://[^\s]*

匹配中文字符

[\u4e00-\u9fa5]



匹配规则

\w         匹配字母、数字及下划线

\W        匹配不是字母、数字及下划线的字符

\s          匹配任意空白字符，等价于 [\t\n\r\f]

\S          匹配任意非空字符

\d          匹配任意数字，等价于 [0-9]

\D          匹配任意非数字的字符

\A          匹配字符串开头

\Z          匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串

\z           匹配字符串结尾，如果存在换行，同时还会匹配换行符

\G          匹配最后匹配完成的位置

\n           匹配一个换行符

\t            匹配一个制表符

^            匹配一行字符串的开头

$             匹配一行字符串的结尾

.              匹配任意字符，除了换行符，当 re.DOTALL 标记被指定时，则可以匹配包括换行符的任意字符

[...]          用来表示一组字符，单独列出，比如 [amk] 匹配 a、m 或 k

[^...]        不在 [] 中的字符，比如 匹配除了 a、b、c 之外的字符

\*             匹配 0 个或多个表达式

\+            匹配 1 个或多个表达式

?             匹配 0 个或 1 个前面的正则表达式定义的片段，非贪婪方式

{n}           精确匹配 n 个前面的表达式

{n, m}      匹配 n 到 m 次由前面正则表达式定义的片段，贪婪方式

a|b          匹配 a 或 b

( )            匹配括号内的表达式，也表示一个组



**修饰符**

re.I             使匹配对大小写不敏感

re.L            做本地化识别（locale-aware）匹配

re.M           多行匹配，影响 ^ 和 $

re.S            使.匹配包括换行在内的所有字符

re.U            根据 Unicode 字符集解析字符。这个标志影响 \w、\W、\b 和 \B

re.X            该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解

result = re.match('^He.*?(\d+).*?Demo$', content, re.S)



**转义匹配**

当遇到用于正则匹配模式的特殊字符时，在前面加反斜线转义一下即可

import re

content = '(百度) [www.baidu.com](http://www.baidu.com)'

result = re.match('\(百度 \) www\.baidu\.com', content)

print(result)



**match**

match 方法会尝试从字符串的起始位置匹配正则表达式，如果匹配，就返回匹配成功的结果；如果不匹配，就返回 None。match 方法是从字符串的开头开始匹配的，一旦开头不匹配，那么整个匹配就失败了

import re

content = 'Hello [1234567](tel:1234567) World_This is a Regex Demo'

result = re.match('^Hello\s(\d+)\sWorld', content)

print(result)

print(result.group())

print(result.group(1))

print(result.span())

想把字符串中的 [1234567](tel:1234567) 提取出来，此时可以将数字部分的正则表达式用 () 括起来，然后调用了 group(1) 获取匹配结果

运行结果如下

<_sre.SRE_Match object; span=(0, 19), match='Hello [1234567](tel:1234567) World'>

Hello [1234567](tel:1234567) World

[1234567](tel:1234567)

(0, 19)

成功得到了 [1234567](tel:1234567)。这里用的是 group(1)，它与 group() 有所不同，后者会输出完整的匹配结果，而前者会输出第一个被 () 包围的匹配结果。假如正则表达式后面还有 () 包括的内容，那么可以依次用 group(2)、group(3) 等来获取



**search**

它在匹配时会扫描整个字符串，然后返回第一个成功匹配的结果



**findall**

取匹配正则表达式的所有内容



**sub**

修改字符串

想要把一串文本中的所有数字都去掉，如果只用字符串的 replace 方法，那就太烦琐了，这时可以借助 sub 方法

import re

content = '54aK54yr5oiR54ix5L2g'

content = re.sub('\d+', '', content)

print(content)

运行结果如下

aKyroiRixLg



**compile**

将正则字符串编译成正则表达式对象

import re

content1 = '2016-12-15 12:00'

content2 = '2016-12-17 12:55'

content3 = '2016-12-22 13:21'

pattern = re.compile('\d{2}:\d{2}')

result1 = re.sub(pattern, '', content1)

result2 = re.sub(pattern, '', content2)

result3 = re.sub(pattern, '', content3)

print(result1, result2, result3)

可以借助 compile 方法将正则表达式编译成一个正则表达式对象，以便复用





## XPath

XPath，全称 XML Path Language，即 XML 路径语言，它是一门在 XML 文档中查找信息的语言。它最初是用来搜寻 XML 文档的，但是它同样适用于 HTML 文档的搜索

**常用规则**

表　达　式                    描　　述

nodename                     选取此节点的所有子节点

/                                     从当前节点选取直接子节点

//                                    从当前节点选取子孙节点

.                                      选取当前节点

..                                     选取当前节点的父节点

@                                   选取属性



获取所有 li 节点，示例如下

from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())

result = html.xpath('//li')

print(result)

print(result[0])

通过 / 或 // 即可查找元素的子节点或子孙节点

from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())

result = html.xpath('//li/a')

print(result)

通过..查找元素的子节点

from lxml import etree  

html = etree.parse('./test.html', etree.HTMLParser())  

result = html.xpath('//a[@href="link4.html"]/../@class')  

print(result)

用 @符号进行属性过滤

from lxml import etree  

html = etree.parse('./test.html', etree.HTMLParser())  

result = html.xpath('//li[@class="item-0"]')  

print(result)

text 方法获取节点中的文本

from lxml import etree  

html = etree.parse('./test.html', etree.HTMLParser())  

result = html.xpath('//li[@class="item-0"]/text()')  

print(result)

@符号获取属性

from lxml import etree  

html = etree.parse('./test.html', etree.HTMLParser())  

result = html.xpath('//li/a/@href')  

print(result)

\```这里我们通过 @href 即可获取节点的 href 属性。注意，此处和属性匹配的方法不同，属性匹配是中括号加属性名和值来限定某个属性，如 [@href="link1.html"]，而此处的 @href 指的是获取节点的某个属性，二者需要做好区分。

运行结果如下：

\```python

['link1.html', 'link2.html', 'link3.html', 'link4.html', 'link5.html']

属性多值匹配

from lxml import etree  

text = '''  

<li class="li li-first"><a href="link.html">first item</a></li>  

'''  

html = etree.HTML(text)  

result = html.xpath('//li[contains(@class, "li")]/a/text()')  

print(result)

多属性匹配

可能遇到一种情况，那就是根据多个属性确定一个节点，这时就需要同时匹配多个属性。此时可以使用运算符 and 来连接

from lxml import etree  

text = '''  

<li class="li li-first" name="item"><a href="link.html">first item</a></li>

'''  

html = etree.HTML(text)  

result = html.xpath('//li[contains(@class, "li") and @name="item"]/a/text()')  

print(result)





## BeautifulSoup

BeautifulSoup 就是 Python 的一个 HTML 或 XML 的解析库

Beautiful Soup 在解析时实际上依赖解析器，它除了支持 Python 标准库中的 HTML 解析器外，还支持一些第三方解析器（比如 lxml）

Beautiful Soup 支持的解析器

| 解析器           | 使用方法                             | 优势                                                        | 劣势                                          |
| ---------------- | ------------------------------------ | ----------------------------------------------------------- | --------------------------------------------- |
| Python 标准库    | BeautifulSoup(markup, "html.parser") | Python 的内置标准库、执行速度适中 、文档容错能力强          | Python 2.7.3 or 3.2.2) 前的版本中文容错能力差 |
| LXML HTML 解析器 | BeautifulSoup(markup, "lxml")        | 速度快、文档容错能力强                                      | 需要安装 C 语言库                             |
| LXML XML 解析器  | BeautifulSoup(markup, "xml")         | 速度快、唯一支持 XML 的解析器                               | 需要安装 C 语言库                             |
| html5lib         | BeautifulSoup(markup, "html5lib")    | 最好的容错性、以浏览器的方式解析文档、生成 HTML5 格式的文档 | 速度慢、不依赖外部扩展                        |

如果使用 lxml，那么在初始化 Beautiful Soup 时，可以把第二个参数改为 lxml 即可：

from bs4 import BeautifulSoup

soup = BeautifulSoup('<p>Hello</p>', 'lxml')

print(soup.p.string)

基本用法

html = """

<html><head><title>The Dormouse's story</title></head>

<body>

<p class="title" name="dromouse"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were

<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,

<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and

<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;

and they lived at the bottom of a well.</p>

<p class="story">...</p>

"""

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.prettify())

print(soup.title.string)

prettify() 方法。这个方法可以把要解析的字符串以标准的缩进格式输出。这里需要注意的是，输出结果里面包含 body 和 html 节点，也就是说对于不标准的 HTML 字符串 BeautifulSoup，可以自动更正格式。这一步不是由 prettify() 方法做的，而是在初始化 BeautifulSoup 时就完成了

**节点选择器**

直接调用节点的名称就可以选择节点元素，再调用 string 属性就可以得到节点内的文本了

from bs4 import BeautifulSoup



soup = BeautifulSoup(html, 'lxml')

print(soup.title)

print(type(soup.title))

print(soup.title.string)

print(soup.head)

print(soup.p) #这种选择方式只会选择到第一个匹配的节点，其他的后面节点都会忽略

用 name 属性获取节点的名称

print(soup.title.name)

用 attrs 获取所有属性

print(soup.p.attrs)

print(soup.p.attrs['name'])



\# 运行结果如下

{'class': ['title'], 'name': 'dromouse'}

dromouse

用 string 属性获取节点元素包含的文本内容

print(soup.p.string)

嵌套选择

返回结果都是 bs4.element.Tag 类型，它同样可以继续调用节点进行下一步的选择

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.head.title)

print(type(soup.head.title))

print(soup.head.title.string)

关联选择

子节点和子孙节点

选取节点元素之后，如果想要获取它的直接子节点，可以调用 contents 属性，得到的结果是直接子节点的**列表**

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.p.contents)

也可以调用 children 属性得到相应的结果，返回结果是**生成器类型**。接下来，我们用 for 循环输出相应的内容

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.p.children)

for i, child in enumerate(soup.p.children):

​    print(i, child)

如果要得到所有的子孙节点的话，可以调用 descendants 属性，返回结果还是生成器

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.p.descendants)

for i, child in enumerate(soup.p.descendants):

​    print(i, child)



父节点和祖先节点

可以调用 parent 属性，输出的仅仅是节点的直接父节点

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.a.parent)

如果想获取所有的祖先节点，可以调用 parents 属性



兄弟节点

next_sibling 和 previous_sibling 分别获取节点的下一个和上一个兄弟元素

next_siblings 和 previous_siblings 则分别返回后面和前面的兄弟节点



**方法选择器**

find_all，就是查询所有符合条件的元素

它的 API 如下

find_all(name , attrs , recursive , text , **kwargs)

返回结果是列表类型

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.find_all(name='ul'))

print(soup.find_all(attrs={'id': 'list-1'}))

"""对于一些常用的属性，比如 id 和 class 等，我们可以不用 attrs 来传递,而对于 class 来说，由于 class 在 Python 里是一个关键字，所以后面需要加一个下划线"""

print(soup.find_all(id='list-1'))

print(soup.find_all(class_='element'))

\# text 参数可用来匹配节点的文本，传入的形式可以是字符串，可以是正则表达式对象

print(soup.find_all(text=re.compile('link')))



find 方法返回的是单个元素，类型依然是 **Tag 类型**

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.find(name='ul'))



另外还有许多的查询方法

find_parents 和 find_parent：前者返回所有祖先节点，后者返回直接父节点。

find_next_siblings 和 find_next_sibling：前者返回后面所有的兄弟节点，后者返回后面第一个兄弟节点。

find_previous_siblings 和 find_previous_sibling：前者返回前面所有的兄弟节点，后者返回前面第一个兄弟节点。

find_all_next 和 find_next：前者返回节点后所有符合条件的节点，后者返回第一个符合条件的节点。

find_all_previous 和 find_previous：前者返回节点前所有符合条件的节点，后者返回第一个符合条件的节点。



**CSS 选择器**

使用 CSS 选择器，只需要调用 select 方法，传入相应的 CSS 选择器即可，返回的结果均是符合 CSS 选择器的节点组成的**列表**

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

print(soup.select('.panel .panel-heading'))

print(soup.select('ul li'))

print(soup.select('#list-2 .element'))

print(type(soup.select('ul')[0]))



嵌套选择

select 方法同样支持嵌套选择，例如我们先选择所有 ul 节点，再遍历每个 ul 节点选择其 li 节点

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

for ul in soup.select('ul'):

​    print(ul.select('li'))



获取属性

节点类型是 Tag 类型，所以获取属性还可以用原来的方法

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

for ul in soup.select('ul'):

​    print(ul['id'])

​    print(ul.attrs['id'])



获取文本

要获取文本，也用 string 属性

from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

for li in soup.select('li'):

​    print('Get Text:', li.get_text())

​    print('String:', li.string)







## pyquery

字符串初始化

from pyquery import PyQuery as pq

doc = pq(html)

print(doc('li'))

首先引入 PyQuery 这个对象，取别名为 pq。然后声明了一个长 HTML 字符串，并将其当作参数传递给 PyQuery 类，这样就成功完成了初始化。接下来，将初始化的对象传入 CSS 选择器。在这个实例中，我们传入 li 节点，这样就可以选择所有的 li 节点



URL 初始化

初始化的参数不仅可以以字符串的形式传递，还可以传入网页的 URL，此时只需要指定参数为 url 即可

from pyquery import PyQuery as pq

doc = pq(url='<http://cuiqingcai.com>')

print(doc('title'))



\# 运行结果

<title> 静觅丨崔庆才的个人博客 </title>

PyQuery 对象会首先请求这个 URL，然后用得到的 HTML 内容完成初始化，这其实就相当于用网页的源代码以字符串的形式传递给 PyQuery 类来初始化。

它与下面的功能是相同的

from pyquery import PyQuery as pq

import requests

doc = pq(requests.get('<http://cuiqingcai.com>').text)

print(doc('title'))



文件初始化

当然除了传递一个 URL，还可以传递本地的文件名，参数指定为 filename

from pyquery import PyQuery as pq

doc = pq(filename='demo.html')

print(doc('li'))



pyquery 的 **CSS 选择器**

from pyquery import PyQuery as pq

doc = pq(html)

print(doc('#container .list li'))

初始化 PyQuery 对象之后，传入了一个 CSS 选择器 #container .list li，它的意思是先选取 id 为 container 的节点，然后再选取其内部的 class 为 list 的节点内部的所有 li 节点。然后，打印输出。它的类型依然是 PyQuery 类型



**查找节点**

这些函数和 jQuery 中的方法用法也完全相同

子节点

find 方法，传入的参数是 CSS 选择器，find() 方法会将符合条件的所有节点选择出来，结果的类型是 PyQuery 类型，find 的查找范围是节点的所有子孙节点，而如果我们只想查找子节点，那可以用 children 方法

from pyquery import PyQuery as pq

doc = pq(html)

items = doc('.list')

print(type(items))

print(items)

lis = items.find('li')

print(type(lis))

print(lis)

如果要筛选所有子节点中符合条件的节点，比如想筛选出子节点中 class 为 active 的节点，可以向 children() 方法传入 CSS 选择器.active

lis = items.children('.active')

print(lis)



父节点

用 parent 方法来获取某个节点的父节点，其类型依然是 PyQuery 类型

这里的父节点是该节点的直接父节点，也就是说，它不会再去查找父节点的父节点，即祖先节点

如果想获取某个祖先节点，这时可以用 parents 方法

from pyquery import PyQuery as pq

doc = pq(html)

items = doc('.list')

parents = items.parents()

print(type(parents))

print(parents)

parents() 方法会返回所有的祖先节点，如果想要筛选某个祖先节点的话，可以向 parents 方法传入 CSS 选择器，这样就会返回祖先节点中符合 CSS 选择器的节点：

parent = items.parents('.wrap')

print(parent)



兄弟节点

获取兄弟节点，可以使用 siblings() 方法

from pyquery import PyQuery as pq

doc = pq(html)

li = doc('.list .item-0.active')

print(li.siblings())



**遍历**

pyquery 的选择结果可能是多个节点，也可能是单个节点，类型都是 PyQuery 类型，并没有返回像 Beautiful Soup 那样的列表

对于多个节点的结果，我们就需要遍历来获取了。例如，这里把每一个 li 节点进行遍历，需要调用 items 方法：

from pyquery import PyQuery as pq

doc = pq(html)

lis = doc('li').items()

print(type(lis))

for li in lis:

​    print(li, type(li))

调用 items() 方法后，会得到一个生成器，遍历一下，就可以逐个得到 li 节点对象了，它的类型也是 PyQuery 类型。每个 li 节点还可以调用前面所说的方法进行选择，比如继续查询子节点，寻找某个祖先节点等，非常灵活



**获取信息**

获取属性

用 attr() 方法来获取属性

from pyquery import PyQuery as pq

doc = pq(html)

a = doc('.item-0.active a')

print(a.attr('href'))

当返回结果包含多个节点时，调用 attr 方法，只会得到第一个节点的属性。

那么，遇到这种情况时，如果想获取所有的 a 节点的属性，就要用到前面所说的遍历了



获取文本

用 text 方法来实现

from pyquery import PyQuery as pq

doc = pq(html)

a = doc('.item-0.active a')

print(a.text())

它会忽略掉节点内部包含的所有 HTML，只返回纯文字内容，但如果想要获取这个节点内部的 HTML 文本，就要用 html 方法了

from pyquery import PyQuery as pq

doc = pq(html)

li = doc('li')

print(li.html())

print(li.text())

print(type(li.text())

\# 运行结果如下

<a href="link2.html">second item</a>

second item third item fourth item fifth item

<class'str'>

html 方法返回的是第一个 li 节点的内部 HTML 文本，而 text 则返回了所有的 li 节点内部的纯文本，中间用一个空格分割开，即返回结果是一个字符串

如果得到的结果是多个节点，并且想要获取每个节点的内部 HTML 文本，则需要遍历每个节点。而 text() 方法不需要遍历就可以获取，它将所有节点取文本之后合并成一个字符串



**节点操作**

addClass 和 removeClass

from pyquery import PyQuery as pq

doc = pq(html)

li = doc('.item-0.active')

print(li)

li.removeClass('active')

print(li)

li.addClass('active')

print(li)

运行结果如下

<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>

<li class="item-0"><a href="link3.html"><span class="bold">third item</span></a></li>

<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>



用 attr 方法来修改属性，其中该方法的第一个参数为属性名，第二个参数为属性值，接着，调用 text 和 html 方法来改变节点内部的内容

html = '''

<ul class="list">

​     <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>

</ul>

'''

from pyquery import PyQuery as pq

doc = pq(html)

li = doc('.item-0.active')

print(li)

li.attr('name', 'link')

print(li)

li.text('changed item')

print(li)

li.html('<span>changed item</span>')

print(li)

运行结果如下

<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>

<li class="item-0 active" name="link"><a href="link3.html"><span class="bold">third item</span></a></li>

<li class="item-0 active" name="link">changed item</li>

<li class="item-0 active" name="link"><span>changed item</span></li>

如果 attr 方法只传入第一个参数的属性名，则是获取这个属性值；如果传入第二个参数，可以用来修改属性值。text 和 html 方法如果不传参数，则是获取节点内纯文本和 HTML 文本；如果传入参数，则进行赋值



remove方法就是移除

html = '''

<div class="wrap">

​    Hello, World

    <p>This is a paragraph.</p>

 </div>

'''

from pyquery import PyQuery as pq

doc = pq(html)

wrap = doc('.wrap')

print(wrap.text())

\# 运行结果：

Hello, World This is a paragraph.



wrap.find('p').remove()

print(wrap.text())

\# 运行结果：

Hello, World



**伪类选择器**

CSS 选择器之所以强大，还有一个很重要的原因，那就是它支持多种多样的伪类选择器，例如选择第一个节点、最后一个节点、奇偶数节点、包含某一文本的节点等

html = '''

<div class="wrap">

    <div id="container">

​        <ul class="list">

​             <li class="item-0">first item</li>

​             <li class="item-1"><a href="link2.html">second item</a></li>

​             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>

​             <li class="item-1 active"><a href="link4.html">fourth item</a></li>

​             <li class="item-0"><a href="link5.html">fifth item</a></li>

​         </ul>

​     </div>

 </div>

'''

from pyquery import PyQuery as pq

doc = pq(html)

li = doc('li:first-child')

print(li)

li = doc('li:last-child')

print(li)

li = doc('li:nth-child(2)')

print(li)

li = doc('li:gt(2)')

print(li)

li = doc('li:nth-child(2n)')

print(li)

li = doc('li:contains(second)')

print(li)







## 文件存储

**TXT 文本存储**

import requests

from pyquery import PyQuery as pq

url = '<https://www.zhihu.com/explore>'

headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'

}

html = requests.get(url, headers=headers).text

doc = pq(html)

items = doc('.explore-tab .feed-item').items()

for item in items:

​    question = item.find('h2').text()

​    author = item.find('.author-link-line').text()

​    answer = pq(item.find('.content').html()).text()

​    with open('explore.txt', 'a', encoding='utf-8') as file:

​        file.write('\n'.join([question, author, answer]))

​        file.write('\n' + '=' * 50 + '\n')

文件打开方式：

- r：以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
- rb：以二进制只读方式打开一个文件。文件指针将会放在文件的开头。
- r+：以读写方式打开一个文件。文件指针将会放在文件的开头。
- rb+：以二进制读写方式打开一个文件。文件指针将会放在文件的开头。
- w：以写入方式打开一个文件。如果该文件已存在，则将其覆盖。如果该文件不存在，则创建新文件。
- wb：以二进制写入方式打开一个文件。如果该文件已存在，则将其覆盖。如果该文件不存在，则创建新文件。
- w+：以读写方式打开一个文件。如果该文件已存在，则将其覆盖。如果该文件不存在，则创建新文件。
- wb+：以二进制读写格式打开一个文件。如果该文件已存在，则将其覆盖。如果该文件不存在，则创建新文件。
- a：以追加方式打开一个文件。如果该文件已存在，文件指针将会放在文件结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，则创建新文件来写入。
- ab：以二进制追加方式打开一个文件。如果该文件已存在，则文件指针将会放在文件结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，则创建新文件来写入。
- a+：以读写方式打开一个文件。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，则创建新文件来读写。
- ab+：以二进制追加方式打开一个文件。如果该文件已存在，则文件指针将会放在文件结尾。如果该文件不存在，则创建新文件用于读写。



**JSON 文件存储**

读取 JSON

import json

str = '''

[{

​    "name": "Bob",

​    "gender": "male",

​    "birthday": "1992-10-18"

}, {

​    "name": "Selina",

​    "gender": "female",

​    "birthday": "1995-10-18"

}]

'''

print(type(str))

data = json.loads(str)

print(data)

print(type(data))

运行结果如下

<class'str'>

[{'name': 'Bob', 'gender': 'male', 'birthday': '1992-10-18'}, {'name': 'Selina', 'gender': 'female', 'birthday': '1995-10-18'}]

<class 'list'>

loads 方法将字符串转为 JSON 对象。由于最外层是中括号，所以最终的类型是列表类型

千万注意 JSON 字符串的表示需要用双引号，否则 loads 方法会解析失败

str = '''

[{

​    'name': 'Bob',

​    'gender': 'male',

​    'birthday': '1992-10-18'

}]

'''

JSON 文本中读取内容，例如这里有一个 data.json 文本文件，其内容是刚才定义的 JSON 字符串，我们可以先将文本文件内容读出，然后再利用 loads 方法转化

import json

with open('data.json', 'r') as file:

​    str = file.read()

​    data = json.loads(str)

​    print(data)



输出 JSON

用 dumps 方法将 JSON 对象转化为字符串

import json

data = [{

​    'name': 'Bob',

​    'gender': 'male',

​    'birthday': '1992-10-18'

}]

with open('data.json', 'w') as file:

​    file.write(json.dumps(data, indent=2))

参数 indent，代表缩进字符个数

如果 JSON 中包含中文字符，按上面方法输出到文件，中文字符都变成了 Unicode 字符

还需要指定参数 ensure_ascii 为 False，另外还要规定文件输出的编码

with open('data.json', 'w', encoding='utf-8') as file:

​    file.write(json.dumps(data, indent=2, ensure_ascii=False))



**CSV 文件存储**

写入

import csv

with open('data.csv', 'w') as csvfile:

​    writer = csv.writer(csvfile)

​    writer.writerow(['id', 'name', 'age'])

​    writer.writerow(['10001', 'Mike', 20])

​    writer.writerow(['10002', 'Bob', 22])

​    writer.writerow(['10003', 'Jordan', 21])

直接以文本形式打开的话，其内容如下：

id,name,age

10001,Mike,20

10002,Bob,22

10003,Jordan,21

写入的文本默认以逗号分隔，调用一次 writerow 方法即可写入一行数据

如果想修改列与列之间的分隔符，可以传入 delimiter 参数，其代码如下

import csv

with open('data.csv', 'w') as csvfile:

​    writer = csv.writer(csvfile, delimiter=' ')

​    writer.writerow(['id', 'name', 'age'])

​    writer.writerow(['10001', 'Mike', 20])

​    writer.writerow(['10002', 'Bob', 22])

​    writer.writerow(['10003', 'Jordan', 21])

此时输出结果的每一列就是以空格分隔了，内容如下

id name age

10001 Mike 20

10002 Bob 22

10003 Jordan 21



也可以调用 writerows 方法同时写入多行，此时参数就需要为二维列表

import csv

with open('data.csv', 'w') as csvfile:

​    writer = csv.writer(csvfile)

​    writer.writerow(['id', 'name', 'age'])

​    writer.writerows([['10001', 'Mike', 20], ['10002', 'Bob', 22], ['10003', 'Jordan', 21]])

输出效果是相同的，内容如下

id,name,age

10001,Mike,20

10002,Bob,22

10003,Jordan,21



一般情况下，爬虫爬取的都是结构化数据，我们一般会用字典来表示。在 csv 库中也提供了字典的写入方式，示例如下

import csv

with open('data.csv', 'w') as csvfile:

​    fieldnames = ['id', 'name', 'age']

​    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

​    writer.writeheader()

​    writer.writerow({'id': '10001', 'name': 'Mike', 'age': 20})

​    writer.writerow({'id': '10002', 'name': 'Bob', 'age': 22})

​    writer.writerow({'id': '10003', 'name': 'Jordan', 'age': 21})

pandas 等库的话，可以调用 DataFrame 对象的 to_csv 方法来将数据写入 CSV 文件中



读取

import csv  

with open('data.csv', 'r', encoding='utf-8') as csvfile:  

​    reader = csv.reader(csvfile)  

​    for row in reader:  

​        print(row)

运行结果如下

['id', 'name', 'age']  

['10001', 'Mike', '20']  

['10002', 'Bob', '22']  

['10003', 'Jordan', '21']  

['10004', 'Durant', '22']  

['10005', ' 王伟 ', '22']

构造的是 Reader 对象，通过遍历输出了每行的内容，每一行都是一个列表形式。注意，如果 CSV 文件中包含中文的话，还需要指定文件编码

pandas 的话，可以利用 read_csv 方法将数据从 CSV 中读取出来

import pandas as pd  

df = pd.read_csv('data.csv')  

print(df)

运行结果如下

​      id    name  age  

0  10001    Mike   20  

1  10002     Bob   22  

2  10003  Jordan   21  

3  10004  Durant   22  

4  10005    王伟   22



## MySQL 的存储

插入数据

import pymysql

id = '[20120001](tel:20120001)'

user = 'Bob'

age = 20

db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')

cursor = db.cursor()

sql = 'INSERT INTO students(id, name, age) values(% s, % s, % s)'

try:

​    cursor.execute(sql, (id, user, age))

​    db.commit()

except:

​    db.rollback()

db.close()



对于爬虫的数据存储来说，一条数据可能存在某些字段提取失败而缺失的情况，而且数据可能随时调整。另外，数据之间还存在嵌套关系。如果使用关系型数据库存储，一是需要提前建表，二是如果存在数据嵌套关系的话，需要进行序列化操作才可以存储，这非常不方便



## MongoDB 存储

**连接 MongoDB**

import pymongo

client = pymongo.MongoClient(host='localhost', port=27017)

**指定数据库**

db = client.test

\# 或

db = client['test']

**指定集合**，MongoDB 的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表

collection = db.students

\# 或

collection = db['students']

**插入数据**，MongoDB 会自动产生一个 ObjectId 类型的_id 属性。insert() 方法会在执行后返回_id 值

student = {

​    'id': '20170101',

​    'name': 'Jordan',

​    'age': 20,

​    'gender': 'male'

}

result = collection.insert(student)

print(result)  # 5932a68615c2606814c91f3d

PyMongo 3.x 版本中，官方已经不推荐使用 insert() 方法了。当然，继续使用也没有什么问题。官方推荐使用 insert_one() 和 insert_many() 方法来分别插入单条记录和多条记录，与 insert() 方法不同，返回的是 InsertOneResult 对象和InsertManyResult对象，我们可以调用其 inserted_id 属性和 inserted_ids 获取_id

**查询**

用 find_one() 或 find() 方法进行查询，其中 find_one() 查询得到的是单个结果，find() 则返回一个生成器对象

result = collection.find_one({'name': 'Mike'})

print(type(result))

print(result)



\# 运行结果

<class 'dict'>

{'_id': ObjectId('5932a80115c2606a59e8a049'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}

**计数**

count = collection.find().count()

count = collection.find({'age': 20}).count()

**排序**

排序时，直接调用 sort() 方法，并在其中传入排序的字段及升降序标志即可

results = collection.find().sort('name', pymongo.ASCENDING)

print([result['name'] for result in results])

用 pymongo.ASCENDING 指定升序。如果要降序排列，可以传入 pymongo.DESCENDING

**更新**

使用 update() 方法，指定更新的条件和更新后的数据即可

condition = {'name': 'Kevin'}

student = collection.find_one(condition)

student['age'] = 25

result = collection.update(condition, student)

print(result)

这里我们要更新 name 为 Kevin 的数据的年龄：首先指定查询条件，然后将数据查询出来，修改年龄后调用 update() 方法将原条件和修改后的数据传入。

运行结果如下

{'ok': 1, 'nModified': 1, 'n': 1, 'updatedExisting': True}

返回结果是字典形式，ok 代表执行成功，nModified 代表影响的数据条数。

另外，我们也可以使用 $set 操作符对数据进行更新，代码如下

result = collection.update(condition, {'$set': student})

这样可以只更新 student 字典内存在的字段。如果原先还有其他字段，则不会更新，也不会删除。而如果不用 $set 的话，则会把之前的数据全部用 student 字典替换；如果原本存在其他字段，则会被删除。

另外，update() 方法其实也是官方不推荐使用的方法。这里也分为 update_one() 方法和 update_many() 方法，用法更加严格，它们的第二个参数需要使用 $ 类型操作符作为字典的键名，示例如下

condition = {'name': 'Kevin'}

student = collection.find_one(condition)

student['age'] = 26

result = collection.update_one(condition, {'$set': student})

print(result)

print(result.matched_count, result.modified_count)



\# 运行结果如下

<pymongo.results.UpdateResult object at 0x10d17b678>

1 0

**删除**

remove() 方法指定删除

result = collection.remove({'name': 'Kevin'})

print(result)

两个新的推荐方法 ——delete_one() 和 delete_many()





## Redis 存储

Redis 是一个基于内存的高效的键值型非关系型数据库，存取效率极高，而且支持多种存储数据结构

redis-py 库提供两个类 Redis 和 StrictRedis 来实现 Redis 的命令操作

**连接 Redis**

from redis import StrictRedis  

redis = StrictRedis(host='localhost', port=6379, db=0, password='foobared')  

redis.set('name', 'Bob')  

print(redis.get('name'))

连接成功，并可以执行 set 和 get 操作了



键操作

字符串操作

列表操作

集合操作

有序集合操作

散列操作



**RedisDump**

RedisDump 提供了强大的 Redis 数据的导入和导出功能

RedisDump 提供了两个可执行命令：redis-dump 用于导出数据，redis-load 用于导入数据







## Ajax数据爬取

利用 Chrome 开发者工具的筛选功能筛选出所有的 Ajax 请求。在请求的上方有一层筛选栏，直接点击 XHR，此时在下方显示的所有请求便都是 Ajax 请求了

主要是分析请求url的变化





## Selenium-动态渲染页面抓取

**声明浏览器对象**

from selenium import webdriver



browser = webdriver.Chrome()

browser = webdriver.Firefox()

browser = webdriver.Edge()

browser = webdriver.PhantomJS()

browser = webdriver.Safari()

**访问页面**

可以用 get() 方法来请求网页，参数传入链接 URL 即可

from selenium import webdriver

browser = webdriver.Chrome()

browser.get('<https://www.taobao.com>')

print(browser.page_source)

browser.close()

**查找节点**

from selenium import webdriver

browser = webdriver.Chrome()

browser.get('<https://www.taobao.com>')

input_first = browser.find_element_by_id('q')

input_second = browser.find_element_by_css_selector('#q')

input_third = browser.find_element_by_xpath('//*[@id="q"]')

print(input_first, input_second, input_third)

browser.close()

所有获取单个节点的方法

find_element_by_id

find_element_by_name

find_element_by_xpath

find_element_by_link_text

find_element_by_partial_link_text

find_element_by_tag_name

find_element_by_class_name

find_element_by_css_selector

Selenium 还提供了通用方法 find_element()，它需要传入两个参数：查找方式 By 和值。实际上，它就是 find_element_by_id() 这种方法的通用函数版本，比如 find_element_by_id(id) 就等价于 find_element(By.ID, id)，二者得到的结果完全一致

所有获取多个节点的方法

find_elements_by_id

find_elements_by_name

find_elements_by_xpath

find_elements_by_link_text

find_elements_by_partial_link_text

find_elements_by_tag_name

find_elements_by_class_name

find_elements_by_css_selector

**节点交互**

比较常见的用法有：输入文字时用 send_keys 方法，清空文字时用 clear 方法，点击按钮时用 click 方法

from selenium import webdriver

import time

browser = webdriver.Chrome()

browser.get('<https://www.taobao.com>')

input = browser.find_element_by_id('q')

input.send_keys('iPhone')

time.sleep(1)

input.clear()

input.send_keys('iPad')

button = browser.find_element_by_class_name('btn-search')

button.click()

**执行 JavaScript**

对于某些操作，Selenium API 并没有提供。比如，下拉进度条，它可以直接模拟运行 JavaScript，此时使用 execute_script() 方法即可实现

from selenium import webdriver

browser = webdriver.Chrome()

browser.get('<https://www.zhihu.com/explore>')

browser.execute_script('window.scrollTo(0, document.body.scrollHeight)')

browser.execute_script('alert("To Bottom")')

**获取节点信息**

获取属性

get_attribute() 方法来获取节点的属性

logo = browser.find_element_by_id('zh-top-link-logo')

print(logo.get_attribute('class'))

获取文本值

input = browser.find_element_by_class_name('zu-top-add-question')

print(input.text)

**延时等待**

隐式等待

超出设定时间后，则抛出找不到节点的异常

from selenium import webdriver

browser = webdriver.Chrome()

browser.implicitly_wait(10)

browser.get('<https://www.zhihu.com/explore>')

input = browser.find_element_by_class_name('zu-top-add-question')

print(input)

显式等待

指定要查找的节点，然后指定一个最长等待时间。如果在规定时间内加载出来了这个节点，就返回查找的节点；如果到了规定时间依然没有加载出该节点，则抛出超时异常

from selenium import webdriver

from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC



browser = webdriver.Chrome()

browser.get('<https://www.taobao.com/>')

wait = WebDriverWait(browser, 10)

input = wait.until(EC.presence_of_element_located((By.ID, 'q')))

button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, '.btn-search')))

print(input, button)

**Cookies**

使用 Selenium，还可以方便地对 Cookies 进行操作，例如获取、添加、删除 Cookies 等

from selenium import webdriver

browser = webdriver.Chrome()

browser.get('<https://www.zhihu.com/explore>')

print(browser.get_cookies())

browser.add_cookie({'name': 'name', 'domain': '[www.zhihu.com](http://www.zhihu.com)', 'value': 'germey'})

print(browser.get_cookies())

browser.delete_all_cookies()

print(browser.get_cookies())

**Chrome Headless 模式**

从 Chrome 59 版本开始，已经开始支持 Headless 模式，也就是无界面模式，这样爬取的时候就不会弹出浏览器了。如果要使用此模式，请把 Chrome 升级到 59 版本及以上。启用 Headless 模式的方式如下

chrome_options = webdriver.ChromeOptions()

chrome_options.add_argument('--headless')

browser = webdriver.Chrome(chrome_options=chrome_options)

首先，创建 ChromeOptions 对象，接着添加 headless 参数，然后在初始化 Chrome 对象的时候通过 chrome_options 传递这个 ChromeOptions 对象，这样我们就可以成功启用 Chrome 的 Headless 模式了。







## Splash

Splash 是一个 JavaScript 渲染服务，是一个带有 HTTP API 的轻量级浏览器，同时它对接了 Python 中的 Twisted 和 QT 库。利用它，我们同样可以实现动态渲染页面的抓取

Splash 可以实现如下功能：

- 异步方式处理多个网页渲染过程
- 获取渲染后的页面的源代码或截图
- 通过关闭图片渲染或者使用 Adblock 规则来加快页面渲染速度
- 可执行特定的 JavaScript 脚本
- 可通过 Lua 脚本来控制页面渲染过程
- 获取渲染的详细过程并通过 HAR（HTTP Archive）格式呈现



## Splash负载均衡配置







## 图形验证码

识别图形验证码需要库 tesserocr

import tesserocr

from PIL import Image

image = Image.open('code.jpg')

result = tesserocr.image_to_text(image)

print(result)

tesserocr 还有一个更加简单的方法，这个方法可直接将图片文件转为字符串，代码如下所示 

import tesserocr

print(tesserocr.file_to_text('image.png'))

此种方法的识别效果不如上一种方法好



识别带干扰的图形验证码，需要做一下额外的处理，如转灰度、二值化等操作

利用 Image 对象的 convert() 方法参数传入 L，即可将图片转化为灰度图像

image = image.convert('L')

image.show()

传入 1 即可将图片进行二值化处理

image = image.convert('1')

image.show()

还可以指定二值化的阈值。上面的方法采用的是默认阈值 127。不过我们不能直接转化原图，要将原图先转为灰度图像，然后再指定二值化阈值

import tesserocr

from PIL import Image

image = Image.open('code2.jpg')

image = image.convert('L')

threshold = 127

table = []

for i in range(256):

​    if i < threshold:

​        table.append(0)

​    else:

​        table.append(1)

image = image.point(table, '1')

result = tesserocr.image_to_text(image)

print(result)







## 滑动验证码

有原图：对比底图和原图，找到缺口位置

无原图：opencv 边缘检测





## 代理

urllib 代理的设置

from urllib.error import URLError

from urllib.request import ProxyHandler, build_opener

proxy = '127.0.0.1:9743'

proxy_handler = ProxyHandler({

​    'http': 'http://' + proxy,

​    'https': 'https://' + proxy

})

opener = build_opener(proxy_handler)

try:

​    response = opener.open('http://httpbin.org/get')

​    print(response.read().decode('utf-8'))

except URLError as e:

​    print(e.reason)

requests 的代理的设置

import requests

proxy = '127.0.0.1:9743'

proxies = {

​    'http': 'http://' + proxy,

​    'https': 'https://' + proxy,

}

try:

​    response = requests.get('http://httpbin.org/get', proxies=proxies)

​    print(response.text)

except requests.exceptions.ConnectionError as e:

​    print('Error', e.args)

Selenium 设置代理

from selenium import webdriver

proxy = '127.0.0.1:9743'

chrome_options = webdriver.ChromeOptions()

chrome_options.add_argument('--proxy-server=http://' + proxy)

browser = webdriver.Chrome(chrome_options=chrome_options)

browser.get('http://httpbin.org/get')



**代理池**

基本模块分为 4 块：获取模块、存储模块、检测模块、接口模块

- 存储模块：负责存储抓取下来的代理。首先要保证代理不重复，要标识代理的可用情况，还要动态实时处理每个代理，所以一种比较高效和方便的存储方式就是使用 Redis 的 Sorted Set，即有序集合。
- 获取模块：需要定时在各大代理网站抓取代理。代理可以是免费公开代理也可以是付费代理，代理的形式都是 IP 加端口，此模块尽量从不同来源获取，尽量抓取高匿代理，抓取成功之后将可用代理保存到数据库中。
- 检测模块：需要定时检测数据库中的代理。这里需要设置一个检测链接，最好是爬取哪个网站就检测哪个网站，这样更加有针对性，如果要做一个通用型的代理，那可以设置百度等链接来检测。另外，我们需要标识每一个代理的状态，如设置分数标识，100 分代表可用，分数越少代表越不可用。检测一次，如果代理可用，我们可以将分数标识立即设置为 100 满分，也可以在原基础上加 1 分；如果代理不可用，可以将分数标识减 1 分，当分数减到一定阈值后，代理就直接从数据库移除。通过这样的标识分数，我们就可以辨别代理的可用情况，选用的时候会更有针对性。
- 接口模块：需要用 API 来提供对外服务的接口。其实我们可以直接连接数据库来取对应的数据，但是这样就需要知道数据库的连接信息，并且要配置连接，而比较安全和方便的方式就是提供一个 Web API 接口，我们通过访问接口即可拿到可用代理。另外，由于可用代理可能有多个，那么我们可以设置一个随机返回某个可用代理的接口，这样就能保证每个可用代理都可以取到，实现负载均衡。

存储模块

使用 Redis 的有序集合，集合的每一个元素都是不重复的，序集合的每一个元素都有一个分数字段，分数是可以重复的，可以是浮点数类型，也可以是整数类型。该集合会根据每一个元素的分数对集合进行排序，数值小的排在前面，数值大的排在后面，这样就可以实现集合元素的排序了

分数是我们判断代理稳定性的重要标准，设置分数规则如下所示。

- 分数 100 为可用，检测器会定时循环检测每个代理可用情况，一旦检测到有可用的代理就立即置为 100，检测到不可用就将分数减 1，分数减至 0 后代理移除。
- 新获取的代理的分数为 10，如果测试可行，分数立即置为 100，不可行则分数减 1，分数减至 0 后代理移除。

这只是一种解决方案，当然可能还有更合理的方案。之所以设置此方案有如下几个原因。

- 在检测到代理可用时，分数立即置为 100，这样可以保证所有可用代理有更大的机会被获取到。你可能会问，为什么不将分数加 1 而是直接设为最高 100 呢？设想一下，有的代理是从各大免费公开代理网站获取的，常常一个代理并没有那么稳定，平均 5 次请求可能有两次成功，3 次失败，如果按照这种方式来设置分数，那么这个代理几乎不可能达到一个高的分数，也就是说即便它有时是可用的，但是筛选的分数最高，那这样的代理几乎不可能被取到。如果想追求代理稳定性，可以用上述方法，这种方法可确保分数最高的代理一定是最稳定可用的。所以，这里我们采取 “可用即设置 100” 的方法，确保只要可用的代理都可以被获取到。
- 在检测到代理不可用时，分数减 1，分数减至 0 后，代理移除。这样一个有效代理如果要被移除需要连续不断失败 100 次，也就是说当一个可用代理如果尝试了 100 次都失败了，就一直减分直到移除，一旦成功就重新置回 100。尝试机会越多，则这个代理拯救回来的机会越多，这样就不容易将曾经的一个可用代理丢弃，因为代理不可用的原因很可能是网络繁忙或者其他人用此代理请求太过频繁，所以在这里将分数为 100。
- 新获取的代理的分数设置为 10，代理如果不可用，分数就减 1，分数减到 0，代理就移除，如果代理可用，分数就置为 100。由于很多代理是从免费网站获取的，所以新获取的代理无效的比例非常高，可能可用的代理不足 10%。所以在这里我们将分数设置为 10，检测的机会没有可用代理的 100 次那么多，这也可以适当减少开销



获取模块

定义一个 Crawler 类来从各大网站抓取代理

import json

from .utils import get_page

from pyquery import PyQuery as pq

class ProxyMetaclass(type):

​    def __new__(cls, name, bases, attrs):

​        count = 0

​        attrs['__CrawlFunc__'] = []

​        for k, v in attrs.items():

​            if 'crawl_' in k:

​                attrs['__CrawlFunc__'].append(k)

​                count += 1

​        attrs['__CrawlFuncCount__'] = count

​        return type.__new__(cls, name, bases, attrs)

class Crawler(object, metaclass=ProxyMetaclass):

​    def get_proxies(self, callback):

​        proxies = []

​        for proxy in eval("self.{}()".format(callback)):

​            print(' 成功获取到代理 ', proxy)

​            proxies.append(proxy)

​        return proxies

​    def crawl_daili66(self, page_count=4):

​        """

​        获取代理 66

​        :param page_count: 页码

​        :return: 代理

​        """

​        start_url = 'http://www.66ip.cn/{}.html'

​        urls = [start_url.format(page) for page in range(1, page_count + 1)]

​        for url in urls:

​            print('Crawling', url)

​            html = get_page(url)

​            if html:

​                doc = pq(html)

​                trs = doc('.containerbox table tr:gt(0)').items()

​                for tr in trs:

​                    ip = tr.find('td:nth-child(1)').text()

​                    port = tr.find('td:nth-child(2)').text()

​                    yield ':'.join([ip, port])

​    def crawl_proxy360(self):

​        """

​        获取 Proxy360

​        :return: 代理

​        """

​        start_url = 'http://www.proxy360.cn/Region/China'

​        print('Crawling', start_url)

​        html = get_page(start_url)

​        if html:

​            doc = pq(html)

​            lines = doc('div[name="list_proxy_ip"]').items()

​            for line in lines:

​                ip = line.find('.tbBottomLine:nth-child(1)').text()

​                port = line.find('.tbBottomLine:nth-child(2)').text()

​                yield ':'.join([ip, port])

​    def crawl_goubanjia(self):

​        """

​        获取 Goubanjia

​        :return: 代理

​        """

​        start_url = 'http://www.goubanjia.com/free/gngn/index.shtml'

​        html = get_page(start_url)

​        if html:

​            doc = pq(html)

​            tds = doc('td.ip').items()

​            for td in tds:

​                td.find('p').remove()

​                yield td.text().replace(' ', '')

from db import RedisClient

from crawler import Crawler

POOL_UPPER_THRESHOLD = 10000

class Getter():

​    def __init__(self):

​        self.redis = RedisClient()

​        self.crawler = Crawler()

​    def is_over_threshold(self):

​        """判断是否达到了代理池限制"""

​        if self.redis.count()>= POOL_UPPER_THRESHOLD:

​            return True

​        else:

​            return False

​    def run(self):

​        print(' 获取器开始执行 ')

​        if not self.is_over_threshold():

​            for callback_label in range(self.crawler.__CrawlFuncCount__):

​                callback = self.crawler.__CrawlFunc__[callback_label]

​                proxies = self.crawler.get_proxies(callback)

​                for proxy in proxies:

​                    self.redis.add(proxy)



检测模块

由于代理的数量非常多，为了提高代理的检测效率，我们在这里使用异步请求库 aiohttp 来进行检测

requests 作为一个同步请求库，我们在发出一个请求之后，程序需要等待网页加载完成之后才能继续执行。也就是这个过程会阻塞等待响应，如果服务器响应非常慢，比如一个请求等待十几秒，那么我们使用 requests 完成一个请求就会需要十几秒的时间，程序也不会继续往下执行，而在这十几秒的时间里程序其实完全可以去做其他的事情，比如调度其他的请求或者进行网页解析等

对于响应速度比较快的网站来说，requests 同步请求和 aiohttp 异步请求的效果差距没那么大。可对于检测代理来说，检测一个代理一般需要十多秒甚至几十秒的时间，这时候使用 aiohttp 异步请求库的优势就大大体现出来了，效率可能会提高几十倍不止



接口模块

- 如果其他人使用这个代理池，他需要知道 Redis 连接的用户名和密码信息，这样很不安全。
- 如果代理池需要部署在远程服务器上运行，而远程服务器的 Redis 只允许本地连接，那么我们就不能远程直连 Redis 来获取代理。
- 如果爬虫所在的主机没有连接 Redis 模块，或者爬虫不是由 Python 语言编写的，那么我们就无法使用 RedisClient 来获取代理。
- 如果 RedisClient 类或者数据库结构有更新，那么爬虫端必须同步这些更新，这样非常麻烦。

综上考虑，为了使代理池可以作为一个独立服务运行，我们最好增加一个接口模块，并以 Web API 的形式暴露可用代理

from flask import Flask, g

from db import RedisClient

__all__ = ['app']

app = Flask(__name__)

def get_conn():

​    if not hasattr(g, 'redis'):

​        g.redis = RedisClient()

​    return g.redis

@app.route('/')

def index():

​    return '<h2>Welcome to Proxy Pool System</h2>'

@app.route('/random')

def get_proxy():

​    """

​    获取随机可用代理

​    :return: 随机代理

​    """

​    conn = get_conn()

​    return conn.random()

@app.route('/count')

def get_counts():

​    """

​    获取代理池总量

​    :return: 代理池总量

​    """

​    conn = get_conn()

​    return str(conn.count())

if __name__ == '__main__':

​    app.run()



调度模块

度模块就是调用以上所定义的 3 个模块，将这 3 个模块通过多进程的形式运行起来

TESTER_CYCLE = 20

GETTER_CYCLE = 20

TESTER_ENABLED = True

GETTER_ENABLED = True

API_ENABLED = True

from multiprocessing import Process

from api import app

from getter import Getter

from tester import Tester

class Scheduler():

​    def schedule_tester(self, cycle=TESTER_CYCLE):

​        """定时测试代理"""

​        tester = Tester()

​        while True:

​            print(' 测试器开始运行 ')

​            tester.run()

​            time.sleep(cycle)

​    def schedule_getter(self, cycle=GETTER_CYCLE):

​        """定时获取代理"""

​        getter = Getter()

​        while True:

​            print(' 开始抓取代理 ')

​            getter.run()

​            time.sleep(cycle)

​    def schedule_api(self):

​        """开启 API"""

​        app.run(API_HOST, API_PORT)

​    def run(self):

​        print(' 代理池开始运行 ')

​        if TESTER_ENABLED:

​            tester_process = Process(target=self.schedule_tester)

​            tester_process.start()

​        if GETTER_ENABLED:

​            getter_process = Process(target=self.schedule_getter)

​            getter_process.start()

​        if API_ENABLED:

​            api_process = Process(target=self.schedule_api)

​            api_process.start()







## Cookies池

Cookies 池架构的基本模块分为 4 块：存储模块、生成模块、检测模块、接口模块

- 存储模块负责存储每个账号的用户名密码以及每个账号对应的 Cookies 信息，同时还需要提供一些方法来实现方便的存取操作。
- 生成模块负责生成新的 Cookies。此模块会从存储模块逐个拿取账号的用户名和密码，然后模拟登录目标页面，判断登录成功，就将 Cookies 返回并交给存储模块存储。
- 检测模块需要定时检测数据库中的 Cookies。在这里我们需要设置一个检测链接，不同的站点检测链接不同，检测模块会逐个拿取账号对应的 Cookies 去请求链接，如果返回的状态是有效的，那么此 Cookies 没有失效，否则 Cookies 失效并移除。接下来等待生成模块重新生成即可。
- 接口模块需要用 API 来提供对外服务的接口。由于可用的 Cookies 可能有多个，我们可以随机返回 Cookies 的接口，这样保证每个 Cookies 都有可能被取到。Cookies 越多，每个 Cookies 被取到的概率就会越小，从而减少被封号的风险。



存储模块

需要存储的内容无非就是账号信息和 Cookies 信息。账号由用户名和密码两部分组成，我们可以存成用户名和密码的映射。Cookies 可以存成 JSON 字符串，但是我们后面得需要根据账号来生成 Cookies。生成的时候我们需要知道哪些账号已经生成了 Cookies，哪些没有生成，所以需要同时保存该 Cookies 对应的用户名信息，其实也是用户名和 Cookies 的映射。这里就是两组映射，我们自然而然想到 Redis 的 Hash，于是就建立两个 Hash

Hash 的 Key 就是账号，Value 对应着密码或者 Cookies。另外需要注意，由于 Cookies 池需要做到可扩展，所以这里 Hash 的名称可以做二级分类，例如存账号的 Hash 名称可以为 accounts:weibo，Cookies 的 Hash 名称可以为 cookies:weibo。如要扩展知乎的 Cookies 池，我们就可以使用 accounts:zhihu 和 cookies:zhihu，这样比较方便



生成模块

生成模块负责获取各个账号信息并模拟登录，随后生成 Cookies 并保存。我们首先获取两个 Hash 的信息，看看账户的 Hash 比 Cookies 的 Hash 多了哪些还没有生成 Cookies 的账号，然后将剩余的账号遍历，再去生成 Cookies 即可

在生成模块里我们可以根据不同的状态码做不同的处理。例如状态码为 1 的情况，表示成功获取 Cookies，我们只需要将 Cookies 保存到数据库即可。如状态码为 2 的情况，代表用户名或密码错误，那么我们就应该把当前数据库中存储的账号信息删除。如状态码为 3 的情况，则代表登录失败的一些错误，此时不能判断是否用户名或密码错误，也不能成功获取 Cookies，那么简单提示再进行下一个处理即可



检测模块

用生成模块来生成 Cookies，但还是免不了 Cookies 失效的问题，例如时间太长导致 Cookies 失效，或者 Cookies 使用太频繁导致无法正常请求网页。如果遇到这样的 Cookies，我们肯定不能让它继续保存在数据库里。

所以我们还需要增加一个定时检测模块，它负责遍历池中的所有 Cookies，同时设置好对应的检测链接，我们用一个个 Cookies 去请求这个链接。如果请求成功，或者状态码合法，那么该 Cookies 有效；如果请求失败，或者无法获取正常的数据，比如直接跳回登录页面或者跳到验证页面，那么此 Cookies 无效，我们需要将该 Cookies 从数据库中移除。

此 Cookies 移除之后，刚才所说的生成模块就会检测到 Cookies 的 Hash 和账号的 Hash 相比少了此账号的 Cookies，生成模块就会认为这个账号还没生成 Cookies，那么就会用此账号重新登录，此账号的 Cookies 又被重新更新



接口模块

一个 Cookies 池可供多个爬虫使用，所以我们还需要定义一个 Web 接口，爬虫访问此接口便可以取到随机的 Cookies。我们采用 Flask 来实现接口的搭建



调度模块

调度模块让这几个模块配合运行起来，主要的工作就是驱动几个模块定时运行，同时各个模块需要在不同进程上运行





## APP的爬取

常用的抓包软件有 WireShark、Filddler、Charles、mitmproxy、AnyProxy 等

通过设置代理的方式将手机处于抓包软件的监听之下，这样便可以看到 App 在运行过程中发生的所有请求和响应了，相当于分析 Ajax 一样。如果这些请求的 URL、参数等都是有规律的，那么总结出规律直接用程序模拟爬取即可，如果它们没有规律，那么我们可以利用另一个工具 mitmdump 对接 Python 脚本直接处理 Response

App 进行自动化控制，这里用到的库是 Appium



**Charles的使用**

请确保已经正确安装 Charles 并开启了代理服务，手机和 Charles 处于同一个局域网下，Charles 代理和 CharlesCA 证书设置好，另外需要开启 SSL 监听

原理：

首先 Charles 运行在自己的 PC 上，Charles 运行的时候会在 PC 的 8888 端口开启一个代理服务，这个服务实际上是一个 HTTP/HTTPS 的代理。

确保手机和 PC 在同一个局域网内，我们可以使用手机模拟器通过虚拟网络连接，也可以使用手机真机和 PC 通过无线网络连接。

设置手机代理为 Charles 的代理地址，这样手机访问互联网的数据包就会流经 Charles，Charles 再转发这些数据包到真实的服务器，服务器返回的数据包再由 Charles 转发回手机，Charles 就起到中间人的作用，所有流量包都可以捕捉到，因此所有 HTTP 请求和响应都可以捕获到。同时 Charles 还有权力对请求和响应进行修改。

重发，它可以将捕获到的请求加以修改并发送修改后的请求



**mitmproxy的使用**

mitmproxy 是一个支持 HTTP 和 HTTPS 的抓包程序，有类似 Fiddler、Charles 的功能，只不过它是一个控制台的形式操作。

mitmproxy 还有两个关联组件。一个是 mitmdump，它是 mitmproxy 的命令行接口，利用它我们可以对接 Python 脚本，用 Python 实现监听后的处理。另一个是 mitmweb，它是一个 Web 程序，通过它我们可以清楚观察 mitmproxy 捕获的请求。



mitmproxy 有如下几项功能。

- 拦截 HTTP 和 HTTPS 请求和响应
- 保存 HTTP 会话并进行分析
- 模拟客户端发起请求，模拟服务端返回响应
- 利用反向代理将流量转发给指定的服务器
- 支持 Mac 和 Linux 上的透明代理
- 利用 Python 对 HTTP 请求和响应进行实时处理

抓包原理：

和 Charles 一样，mitmproxy 运行于自己的 PC 上，mitmproxy 会在 PC 的 8080 端口运行，然后开启一个代理服务，这个服务实际上是一个 HTTP/HTTPS 的代理。

手机和 PC 在同一个局域网内，设置代理为 mitmproxy 的代理地址，这样手机在访问互联网的时候流量数据包就会流经 mitmproxy，mitmproxy 再去转发这些数据包到真实的服务器，服务器返回数据包时再由 mitmproxy 转发回手机，这样 mitmproxy 就相当于起了中间人的作用，抓取到所有 Request 和 Response，另外这个过程还可以对接 mitmdump，抓取到的 Request 和 Response 的具体内容都可以直接用 Python 来处理，比如得到 Response 之后我们可以直接进行解析，然后存入数据库，这样就完成了数据的解析和存储过程

设置代理

首先，我们需要运行 mitmproxy，启动 mitmproxy 的命令如下

mitmproxy

运行之后会在 8080 端口上运行一个代理服务

或者启动 mitmdump，它也会监听 8080 端口

mitmdump

将手机和 PC 连接在同一局域网下，设置代理为当前代理。首先看看 PC 的当前局域网 IP

手机局域网设置HTTP代理

运行 mitmproxy

mitmproxy

设置成功之后，我们只需要在手机浏览器上访问任意的网页或浏览任意的 App 即可。例如在手机上打开百度，mitmproxy 页面便会呈现出手机上的所有请求



mitmproxy 还提供了命令行式的编辑功能

**MitmDump 的使用**

mitmdump 是 mitmproxy 的命令行接口，同时还可以对接 Python 对请求进行处理，这是相比 Fiddler、Charles 等工具更加方便的地方。有了它我们可以不用手动截获和分析 HTTP 请求和响应，只需写好请求和响应的处理逻辑即可。它还可以实现数据的解析、存储等工作，这些过程都可以通过 Python 实现

通过 response() 方法获取每个请求的响应内容。接下来再进行响应的信息提取和存储，我们就可以成功完成爬取了



**Appium的使用**

Appium 是一个跨平台移动端自动化测试工具，可以非常便捷地为 iOS 和 Android 平台创建自动化测试用例。它可以模拟 App 内部的各种操作，如点击、滑动、文本输入等，只要我们手工操作的动作 Appium 都可以完成。在前面我们了解过 Selenium，它是一个网页端的自动化测试工具。Appium 实际上继承了 Selenium，Appium 也是利用 WebDriver 来实现 App 的自动化测试。对 iOS 设备来说，Appium 使用 UIAutomation 来实现驱动。对于 Android 来说，它使用 UiAutomator 和 Selendroid 来实现驱动。

Appium 相当于一个服务器，我们可以向 Appium 发送一些操作指令，Appium 就会根据不同的指令对移动设备进行驱动，完成不同的动作。

对于爬虫来说，我们用 Selenium 来抓取 JavaScript 渲染的页面，可见即可爬。Appium 同样也可以，用 Appium 来做 App 爬虫不失为一个好的选择



启动APP

Appium 启动 App 的方式有两种：一种是用 Appium 内置的驱动器来打开 App，另一种是利用 Python 程序实现此操作





直接点击 Start Server 按钮即可启动 Appium 的服务，相当于开启了一个 Appium 服务器

需要配置启动 App 时的 Desired Capabilities 参数，它们分别是 platformName、deviceName、appPackage、appActivity。

- platformName，平台名称，需要区分是 Android 还是 iOS，此处填写 Android。
- deviceName，设备名称，是手机的具体类型。
- appPackage，APP 程序包名。
- appActivity，入口 Activity 名，这里通常需要以.开头

desired_caps = {

​    'platformName': 'Android',

​    'deviceName': 'MI_NOTE_Pro',

​    'appPackage': 'com.tencent.mm',

​    'appActivity': '.ui.LauncherUI'

}

使用 Python 代码驱动 App 的方法。首先需要在代码中指定一个 Appium Server，而这个 Server 在刚才打开 Appium 的时候就已经开启了，是在 4723 端口上运行的，配置如下所示

server = 'http://localhost:4723/wd/hub'

新建一个 Session，这类似点击 Appium 内置驱动的 Start Session 按钮相同的功能，代码实现如下所示

from appium import webdriver

from selenium.webdriver.support.ui import WebDriverWait

driver = webdriver.Remote(server, desired_caps)

登录微信完整代码：

from appium import webdriver

from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC

server = 'http://localhost:4723/wd/hub'

desired_caps = {

​    'platformName': 'Android',

​    'deviceName': 'MI_NOTE_Pro',

​    'appPackage': 'com.tencent.mm',

​    'appActivity': '.ui.LauncherUI'

}

driver = webdriver.Remote(server, desired_caps)

wait = WebDriverWait(driver, 30)

login = wait.until(EC.presence_of_element_located((By.ID, 'com.tencent.mm:id/cjk')))

login.click()

phone = wait.until(EC.presence_of_element_located((By.ID, 'com.tencent.mm:id/h2')))

phone.set_text('18888888888')







## pyspider框架

pyspider 是由国人 binux 编写的强大的网络爬虫系统

pyspider 带有强大的 WebUI、脚本编辑器、任务监控器、项目管理器以及结果处理器，它支持多种数据库后端、多种消息队列、JavaScript 渲染页面的爬取，使用起来非常方便

PySpider 的功能有：

- 提供方便易用的 WebUI 系统，可以可视化地编写和调试爬虫。
- 提供爬取进度监控、爬取结果查看、爬虫项目管理等功能。
- 支持多种后端数据库，如 MySQL、MongoDB、Redis、SQLite、Elasticsearch、PostgreSQL。
- 支持多种消息队列，如 RabbitMQ、Beanstalk、Redis、Kombu。
- 提供优先级控制、失败重试、定时抓取等功能。
- 对接了 PhantomJS，可以抓取 JavaScript 渲染的页面。
- 支持单机和分布式部署，支持 Docker 部署。

pyspider 与 Scrapy 的区别。

- pyspider 提供了 WebUI，爬虫的编写、调试都是在 WebUI 中进行的，而 Scrapy 原生是不具备这个功能的，采用的是代码和命令行操作，但可以通过对接 Portia 实现可视化配置。
- pyspider 调试非常方便，WebUI 操作便捷直观，在 Scrapy 中则是使用 parse 命令进行调试，论方便程度不及 pyspider。
- pyspider 支持 PhantomJS 来进行 JavaScript 渲染页面的采集，在 Scrapy 中可以对接 ScrapySplash 组件，需要额外配置。
- PySpide r 中内置了 PyQuery 作为选择器，在 Scrapy 中对接了 XPath、CSS 选择器和正则匹配。
- pyspider 的可扩展程度不足，可配制化程度不高，在 Scrapy 中可以通过对接 Middleware、Pipeline、Extension 等组件实现非常强大的功能，模块之间的耦合程度低，可扩展程度极高

如果要快速实现一个页面的抓取，推荐使用 pyspider，开发更加便捷，如快速抓取某个普通新闻网站的新闻内容。如果要应对反爬程度很强、超大规模的抓取，推荐使用 Scrapy，如抓取封 IP、封账号、高频验证的网站的大规模数据采集

**pyspider 的架构**

pyspider 的架构主要分为 Scheduler（调度器）、Fetcher（抓取器）、Processer（处理器）三个部分，整个爬取过程受到 Monitor（监控器）的监控，抓取的结果被 Result Worker（结果处理器）处理



Scheduler 发起任务调度，Fetcher 负责抓取网页内容，Processer 负责解析网页内容，然后将新生成的 Request 发给 Scheduler 进行调度，将生成的提取结果输出保存



pyspider 的任务执行流程的逻辑很清晰，具体过程如下所示。

- 每个 pyspider 的项目对应一个 Python 脚本，该脚本中定义了一个 Handler 类，它有一个 on_start() 方法。爬取首先调用 on_start() 方法生成最初的抓取任务，然后发送给 Scheduler 进行调度。
- Scheduler 将抓取任务分发给 Fetcher 进行抓取，Fetcher 执行并得到响应，随后将响应发送给 Processer。
- Processer 处理响应并提取出新的 URL 生成新的抓取任务，然后通过消息队列的方式通知 Schduler 当前抓取任务执行情况，并将新生成的抓取任务发送给 Scheduler。如果生成了新的提取结果，则将其发送到结果队列等待 Result Worker 处理。
- Scheduler 接收到新的抓取任务，然后查询数据库，判断其如果是新的抓取任务或者是需要重试的任务就继续进行调度，然后将其发送回 Fetcher 进行抓取。
- 不断重复以上工作，直到所有的任务都执行完毕，抓取结束。
- 抓取结束后，程序会回调 on_finished() 方法，这里可以定义后处理过程。



启动 pyspider

pyspider all

可以打开浏览器，输入链接 http://localhost:5000，来管理项目、编写代码、在线调试、监控任务等



创建项目

点击右边的 Create 按钮，进入pyspider 的项目编辑和调试页面

右侧，pyspider 已经帮我们生成了一段代码，代码如下所示

from pyspider.libs.base_handler import *

class Handler(BaseHandler):

​    crawl_config = { }

​    @every(minutes=24 * 60)

​    def on_start(self):

​        self.crawl('http://travel.qunar.com/travelbook/list.htm', callback=self.index_page)

​    @config(age=10 * 24 * 60 * 60)

​    def index_page(self, response):

​        for each in response.doc('a[href^="http"]').items():

​            self.crawl(each.attr.href, callback=self.detail_page)

​    @config(priority=2)

​    def detail_page(self, response):

​        return {

​            "url": response.url,

​            "title": response.doc('title').text(),}

crawl_config，所有爬取配置统一定义到这里，如定义 Headers、设置代理等，配置之后全局生效

左侧on_start() 方法是爬取入口，初始的爬取请求会在这里产生，该方法通过调用 crawl() 方法即可新建一个爬取请求，第一个参数是爬取的 URL，这里自动替换成我们所定义的 URL。crawl() 方法还有一个参数 callback，它指定了这个页面爬取成功后用哪个方法进行解析



点击左栏右上角的 run 按钮，即可看到页面下方 follows 便会出现一个标注，其中包含数字 1，这代表有新的爬取请求产生

点击下方的 web 按钮，即可预览当前爬取结果的页面

点击下方的 html 按钮即可查看当前页面的源代码



切换到 Web 页面，找到攻想获取的元素，点击下方的 enable css selector helper，点击想获取的元素。这时候我们看到想获取的元素外多了一个红框，上方出现了一个 CSS 选择器，这就是当前待获取的元素对应的 CSS 选择器；

在右侧代码选中要更改的区域，点击左栏的右箭头，此时在上方出现的标题的 CSS 选择器就会被替换到右侧代码中



添加一个参数 fetch_type，获取js加载的图片，fetch_type 开启 PhantomJS 渲染。如果遇到 JavaScript 渲染的页面，指定此字段即可实现 PhantomJS 的对接，pyspider 将会使用 PhantomJS 进行网页的抓取

def index_page(self, response):

​    for each in response.doc('li> .tit > a').items():

​        self.crawl(each.attr.href, callback=self.detail_page, fetch_type='js')

​    next = response.doc('.next').attr.href

​    self.crawl(next, callback=self.index_page)



爬虫的主页面，将爬虫的 status 设置成 DEBUG 或 RUNNING，点击右侧的 Run 按钮即可开始爬取



通过在启动时指定配置文件来配置 pyspider WebUI 的访问认证，用户名为 root，密码为 123456，命令如下所示

pyspider -c pyspider.json all



定时爬取

我们可以通过 every 属性来设置爬取的时间间隔

@every(minutes=24 * 60)

def on_start(self):

​    for url in urllist:

​        self.crawl(url, callback=self.index_page)

项目状态

每个项目都有 6 个状态，分别是 TODO、STOP、CHECKING、DEBUG、RUNNING、PAUSE。

- TODO：它是项目刚刚被创建还未实现时的状态。
- STOP：如果想停止某项目的抓取，可以将项目的状态设置为 STOP。
- CHECKING：正在运行的项目被修改后就会变成 CHECKING 状态，项目在中途出错需要调整的时候会遇到这种情况。
- DEBUG/RUNNING：这两个状态对项目的运行没有影响，状态设置为任意一个，项目都可以运行，但是可以用二者来区分项目是否已经测试通过。
- PAUSE：当爬取过程中出现连续多次错误时，项目会自动设置为 PAUSE 状态，并等待一定时间后继续爬取。

抓取进度

在抓取时，可以看到抓取的进度，progress 部分会显示 4 个进度条

progress 中的 5m、1h、1d 指的是最近 5 分、1 小时、1 天内的请求情况，all 代表所有的请求情况。

蓝色的请求代表等待被执行的任务，绿色的代表成功的任务，黄色的代表请求失败后等待重试的任务，红色的代表失败次数过多而被忽略的任务，从这里我们可以直观看到爬取的进度和请求情况

删除项目

pyspider 中没有直接删除项目的选项。如要删除任务，那么将项目的状态设置为 STOP，将分组的名称设置为 delete，等待 24 小时，则项目会自动删除







## Scrapy框架

pyspider 框架的用法，我们可以利用它快速完成爬虫的编写。不过 pyspider 框架也有一些缺点，比如可配置化程度不高，异常处理能力有限等，它对于一些反爬程度非常强的网站的爬取显得力不从心

Scrapy 功能非常强大，爬取效率高，相关扩展组件多，可配置和可扩展程度非常高，它几乎可以应对所有反爬网站，是目前 Python 中使用最广泛的爬虫框架

Scrapy 是一个基于 Twisted 的异步处理框架，是纯 Python 实现的爬虫框架，其架构清晰，模块之间的耦合程度低，可扩展性极强，可以灵活完成各种需求

**Scrapy 框架的架构**





它可以分为如下的几个部分。

- Engine，引擎，用来处理整个系统的数据流处理，触发事务，是整个框架的核心。
- Item，项目，它定义了爬取结果的数据结构，爬取的数据会被赋值成该对象。
- Scheduler， 调度器，用来接受引擎发过来的请求并加入队列中，并在引擎再次请求的时候提供给引擎。
- Downloader，下载器，用于下载网页内容，并将网页内容返回给蜘蛛。
- Spiders，蜘蛛，其内定义了爬取的逻辑和网页的解析规则，它主要负责解析响应并生成提取结果和新的请求。
- Item Pipeline，项目管道，负责处理由蜘蛛从网页中抽取的项目，它的主要任务是清洗、验证和存储数据。
- Downloader Middlewares，下载器中间件，位于引擎和下载器之间的钩子框架，主要是处理引擎与下载器之间的请求及响应。
- Spider Middlewares， 蜘蛛中间件，位于引擎和蜘蛛之间的钩子框架，主要工作是处理蜘蛛输入的响应和输出的结果及新的请求。



Scrapy 中的数据流由引擎控制，其过程如下:

- Engine 首先打开一个网站，找到处理该网站的 Spider 并向该 Spider 请求第一个要爬取的 URL。
- Engine 从 Spider 中获取到第一个要爬取的 URL 并通过 Scheduler 以 Request 的形式调度。
- Engine 向 Scheduler 请求下一个要爬取的 URL。
- Scheduler 返回下一个要爬取的 URL 给 Engine，Engine 将 URL 通过 Downloader Middlewares 转发给 Downloader 下载。
- 一旦页面下载完毕， Downloader 生成一个该页面的 Response，并将其通过 Downloader Middlewares 发送给 Engine。
- Engine 从下载器中接收到 Response 并通过 Spider Middlewares 发送给 Spider 处理。
- Spider 处理 Response 并返回爬取到的 Item 及新的 Request 给 Engine。
- Engine 将 Spider 返回的 Item 给 Item Pipeline，将新的 Request 给 Scheduler。
- 重复第二步到最后一步，直到  Scheduler 中没有更多的 Request，Engine 关闭该网站，爬取结束。



项目结构

scrapy.cfg

project/

​    __init__.py

​    items.py

​    pipelines.py

​    settings.py

​    middlewares.py

​    spiders/

​        __init__.py

​        spider1.py

​        spider2.py

​        ...

- scrapy.cfg：它是 Scrapy 项目的配置文件，其内定义了项目的配置文件路径、部署相关信息等内容。
- items.py：它定义 Item 数据结构，所有的 Item 的定义都可以放这里。
- pipelines.py：它定义 Item Pipeline 的实现，所有的 Item Pipeline 的实现都可以放这里。
- settings.py：它定义项目的全局配置。
- middlewares.py：它定义 Spider Middlewares 和 Downloader Middlewares 的实现。
- spiders：其内包含一个个 Spider 的实现，每个 Spider 都有一个文件



创建项目

scrapy startproject tutorial

会生成如下文件：

scrapy.cfg     # Scrapy 部署时的配置文件

tutorial         # 项目的模块，引入的时候需要从这里引入

​    __init__.py    

​    items.py     # Items 的定义，定义爬取的数据结构

​    middlewares.py   # Middlewares 的定义，定义爬取时的中间件

​    pipelines.py       # Pipelines 的定义，定义数据管道

​    settings.py       # 配置文件

​    spiders         # 放置 Spiders 的文件夹

​    __init__.py



创建 Spider

cd tutorial

scrapy genspider quotes quotes.toscrape.com

进入刚才创建的 tutorial 文件夹，然后执行 genspider 命令。第一个参数是 Spider 的名称，第二个参数是网站域名。执行完毕之后，spiders 文件夹中多了一个 quotes.py，它就是刚刚创建的 Spider

import scrapy

class QuotesSpider(scrapy.Spider):

​    name = "quotes"

​    allowed_domains = ["quotes.toscrape.com"]

​    start_urls = ['http://quotes.toscrape.com/']

​    def parse(self, response):

​        pass

这里有三个属性 ——name、allowed_domains 和 start_urls，还有一个方法 parse。

- name，它是每个项目唯一的名字，用来区分不同的 Spider。
- allowed_domains，它是允许爬取的域名，如果初始或后续的请求链接不是这个域名下的，则请求链接会被过滤掉。
- start_urls，它包含了 Spider 在启动时爬取的 url 列表，初始请求是由它来定义的。
- parse，它是 Spider 的一个方法。默认情况下，被调用时 start_urls 里面的链接构成的请求完成下载执行后，返回的响应就会作为唯一的参数传递给这个函数。该方法负责解析返回的响应、提取数据或者进一步生成要处理的请求。



创建 Item

Item 是保存爬取数据的容器，它的使用方法和字典类似。不过，相比字典，Item 多了额外的保护机制，可以避免拼写错误或者定义字段错误。

创建 Item 需要继承 scrapy.Item 类，并且定义类型为 scrapy.Field 的字段

import scrapy

class QuoteItem(scrapy.Item):

​    text = scrapy.Field()

​    author = scrapy.Field()

​    tags = scrapy.Field()



运行

scrapy crawl quotes



保存到文件

scrapy crawl quotes -o quotes.json

每一个 Item 输出一行 JSON

scrapy crawl quotes -o quotes.jl

\# 或

scrapy crawl quotes -o quotes.jsonlines

scrapy crawl quotes -o quotes.csv

scrapy crawl quotes -o quotes.xml

scrapy crawl quotes -o quotes.pickle

scrapy crawl quotes -o quotes.marshal

scrapy crawl quotes -o ftp://user:pass@ftp.example.com/path/to/quotes.csv



使用 Item Pipeline

如果想进行更复杂的操作，如将结果保存到 MongoDB 数据库，或者筛选某些有用的 Item，则我们可以定义 Item Pipeline 来实现。

tem Pipeline 为项目管道。当 Item 生成后，它会自动被送到 Item Pipeline 进行处理，我们常用 Item Pipeline 来做如下操作。

- 清洗 HTML 数据
- 验证爬取数据，检查爬取字段
- 查重并丢弃重复内容
- 将爬取结果储存到数据库

只需要定义一个类并实现 process_item() 方法即可。启用 Item Pipeline 后，Item Pipeline 会自动调用这个方法。process_item() 方法必须返回包含数据的字典或 Item 对象，或者抛出 DropItem 异常。

process_item() 方法有两个参数。一个参数是 item，每次 Spider 生成的 Item 都会作为参数传递过来。另一个参数是 spider，就是 Spider 的实例。

import pymongo

class MongoPipeline(object):

​    def __init__(self, mongo_uri, mongo_db):

​        self.mongo_uri = mongo_uri

​        self.mongo_db = mongo_db

​    @classmethod

​    def from_crawler(cls, crawler):

​        return cls(mongo_uri=crawler.settings.get('MONGO_URI'),

​            mongo_db=crawler.settings.get('MONGO_DB')

​        )

​    def open_spider(self, spider):

​        self.client = pymongo.MongoClient(self.mongo_uri)

​        self.db = self.client[self.mongo_db]

​    def process_item(self, item, spider):

​        name = item.__class__.__name__

​        self.db[name].insert(dict(item))

​        return item

​    def close_spider(self, spider):

​        self.client.close()

MongoPipeline 类实现了 API 定义的另外几个方法。

- from_crawler，这是一个类方法，用 @classmethod 标识，是一种依赖注入的方式，方法的参数就是 crawler，通过 crawler 这个我们可以拿到全局配置的每个配置信息，在全局配置 settings.py 中我们可以定义 MONGO_URI 和 MONGO_DB 来指定 MongoDB 连接需要的地址和数据库名称，拿到配置信息之后返回类对象即可。所以这个方法的定义主要是用来获取 settings.py 中的配置的。
- open_spider，当 Spider 被开启时，这个方法被调用。在这里主要进行了一些初始化操作。
- close_spider，当 Spider 被关闭时，这个方法会调用，在这里将数据库连接关闭

在 settings.py 中加入如下内容

ITEM_PIPELINES = {

   'tutorial.pipelines.TextPipeline': 300,

   'tutorial.pipelines.MongoPipeline': 400,

}

MONGO_URI='localhost'

MONGO_DB='tutorial'

赋值 ITEM_PIPELINES 字典，键名是 Pipeline 的类名称，键值是调用优先级，是一个数字，数字越小则对应的 Pipeline 越先被调用



**Selector的用法**

Scrapy 还提供了自己的数据提取方法，即 Selector（选择器）。Selector 是基于 lxml 来构建的，支持 XPath 选择器、CSS 选择器以及正则表达式，功能全面，解析速度和准确度非常高

from scrapy import Selector

body = '<html><head><title>Hello World</title></head><body></body></html>'

selector = Selector(text=body)

title = selector.xpath('//title/text()').extract_first()

print(title)

XPath 选择器

response 有一个属性 selector，我们调用 response.selector 返回的内容就相当于用 response 的 text 构造了一个 Selector 对象。通过这个 Selector 对象我们可以调用解析方法如 xpath()、css() 等，通过向方法传入 XPath 或 CSS 选择器参数就可以实现信息的提取

\>>> result = response.selector.xpath('//a')

\>>> result

[<Selector xpath='//a' data='<a href="image1.html">Name: My image 1 <'>,

 <Selector xpath='//a' data='<a href="image2.html">Name: My image 2 <'>,

 <Selector xpath='//a' data='<a href="image3.html">Name: My image 3 <'>,

 <Selector xpath='//a' data='<a href="image4.html">Name: My image 4 <'>,

 <Selector xpath='//a' data='<a href="image5.html">Name: My image 5 <'>]

\>>> type(result)

scrapy.selector.unified.SelectorList

打印结果的形式是 Selector 组成的列表，其实它是 SelectorList 类型，SelectorList 和 Selector 都可以继续调用 xpath() 和 css() 等方法来进一步提取数据。但是获取的内容是 Selector 或者 SelectorList 类型，并不是真正的文本内容，就可以利用 extract() 方法

\>>> result.extract()

['<a href="image1.html">Name: My image 1 <br><img src="image1_thumb.jpg"></a>', '<a href="image2.html">Name: My image 2 <br><img src="image2_thumb.jpg"></a>', '<a href="image3.html">Name: My image 3 <br><img src="image3_thumb.jpg"></a>', '<a href="image4.html">Name: My image 4 <br><img src="image4_thumb.jpg"></a>', '<a href="image5.html">Name: My image 5 <br><img src="image5_thumb.jpg"></a>']

可以改写 XPath 表达式，来选取节点的内部文本和属性，如下所示

\>>> response.xpath('//a/text()').extract()

['Name: My image 1 ', 'Name: My image 2 ', 'Name: My image 3 ', 'Name: My image 4 ', 'Name: My image 5 ']

\>>> response.xpath('//a/@href').extract()

['image1.html', 'image2.html', 'image3.html', 'image4.html', 'image5.html']



CSS 选择器

Scrapy 的选择器同时还对接了 CSS 选择器，使用 response.css() 方法可以使用 CSS 选择器来选择对应的元素。

\>>> response.css('a[href="image1.html"]').extract()

['<a href="image1.html">Name: My image 1 <br><img src="image1_thumb.jpg"></a>']

\>>> response.css('a[href="image1.html"] img').extract()

['<img src="image1_thumb.jpg">']

接下来的两个用法不太一样。节点的内部文本和属性的获取是这样实现的，如下所示：

\>>> response.css('a[href="image1.html"]::text').extract_first()

'Name: My image 1 '

\>>> response.css('a[href="image1.html"] img::attr(src)').extract_first()

'image1_thumb.jpg'

获取文本和属性需要用::text 和::attr() 的写法。而其他库如 Beautiful Soup 或 pyquery 都有单独的方法。



CSS 选择器和 XPath 选择器一样可以嵌套选择。我们可以先用 XPath 选择器选中所有 a 节点，再利用 CSS 选择器选中 img 节点，再用 XPath 选择器获取属性

\>>> response.xpath('//a').css('img').xpath('@src').extract()

['image1_thumb.jpg', 'image2_thumb.jpg', 'image3_thumb.jpg', 'image4_thumb.jpg', 'image5_thumb.jpg']



正则匹配

Scrapy 的选择器还支持正则匹配

\>>> response.xpath('//a/text()').re('Name:\s(.*)')

['My image 1 ', 'My image 2 ', 'My image 3 ', 'My image 4 ', 'My image 5 ']

类似 extract_first() 方法，re_first() 方法可以选取列表的第一个元素，用法如下：

\>>> response.xpath('//a/text()').re_first('(.*?):\s(.*)')

'Name'

\>>> response.xpath('//a/text()').re_first('Name:\s(.*)')

'My image 1 '



**Spider 类分析**

这个类里提供了 start_requests() 方法的默认实现，读取并请求 start_urls 属性，并根据返回的结果调用 parse() 方法解析结果。另外它还有一些基础属性，下面对其进行讲解：

- name，爬虫名称，是定义 Spider 名字的字符串。Spider 的名字定义了 Scrapy 如何定位并初始化 Spider，所以其必须是唯一的。 不过我们可以生成多个相同的 Spider 实例，这没有任何限制。 name 是 Spider 最重要的属性，而且是必须的。如果该 Spider 爬取单个网站，一个常见的做法是以该网站的域名名称来命名 Spider。 例如，如果 Spider 爬取 mywebsite.com ，该 Spider 通常会被命名为 mywebsite 。
- allowed_domains，允许爬取的域名，是可选配置，不在此范围的链接不会被跟进爬取。
- start_urls，起始 URL 列表，当我们没有实现 start_requests() 方法时，默认会从这个列表开始抓取。
- custom_settings，这是一个字典，是专属于本 Spider 的配置，此设置会覆盖项目全局的设置，而且此设置必须在初始化前被更新，所以它必须定义成类变量。
- crawler，此属性是由 from_crawler() 方法设置的，代表的是本 Spider 类对应的 Crawler 对象，Crawler 对象中包含了很多项目组件，利用它我们可以获取项目的一些配置信息，如最常见的就是获取项目的设置信息，即 Settings。
- settings，是一个 Settings 对象，利用它我们可以直接获取项目的全局设置变量。

除了一些基础属性，Spider 还有一些常用的方法，在此介绍如下：

- start_requests()，此方法用于生成初始请求，它必须返回一个可迭代对象，此方法会默认使用 start_urls 里面的 URL 来构造 Request，而且 Request 是 GET 请求方式。如果我们想在启动时以 POST 方式访问某个站点，可以直接重写这个方法，发送 POST 请求时我们使用 FormRequest 即可。
- parse()，当 Response 没有指定回调函数时，该方法会默认被调用，它负责处理 Response，处理返回结果，并从中提取出想要的数据和下一步的请求，然后返回。该方法需要返回一个包含 Request 或 Item 的可迭代对象。
- closed()，当 Spider 关闭时，该方法会被调用，在这里一般会定义释放资源的一些操作或其他收尾操作。



**Downloader Middleware的用法**

Downloader Middleware 即下载中间件，它是处于 Scrapy 的 Request 和 Response 之间的处理模块。

Scheduler 从队列中拿出一个 Request 发送给 Downloader 执行下载，这个过程会经过 Downloader Middleware 的处理。另外，当 Downloader 将 Request 下载完成得到 Response 返回给 Spider 时会再次经过 Downloader Middleware 处理。

Downloader Middleware 在整个架构中起作用的位置是以下两个。

- 在 Scheduler 调度出队列的 Request 发送给 Downloader 下载之前，也就是我们可以在 Request 执行下载之前对其进行修改。
- 在下载后生成的 Response 发送给 Spider 之前，也就是我们可以在生成 Resposne 被 Spider 解析之前对其进行修改。

Downloader Middleware 的功能十分强大，修改 User-Agent、处理重定向、设置代理、失败重试、设置 Cookies 等功能都需要借助它来实现。



每个 Downloader Middleware 都定义了一个或多个方法的类，核心的方法有如下三个。

- process_request(request, spider)
- process_response(request, response, spider)
- process_exception(request, exception, spider)



**Spider Middleware的用法**

Spider Middleware 有如下三个作用。

- 我们可以在 Downloader 生成的 Response 发送给 Spider 之前，也就是在 Response 发送给 Spider 之前对 Response 进行处理。
- 我们可以在 Spider 生成的 Request 发送给 Scheduler 之前，也就是在 Request 发送给 Scheduler 之前对 Request 进行处理。
- 我们可以在 Spider 生成的 Item 发送给 Item Pipeline 之前，也就是在 Item 发送给 Item Pipeline 之前对 Item 进行处理。

每个 Spider Middleware 都定义了以下一个或多个方法的类，核心方法有如下 4 个。

- process_spider_input(response, spider)
- process_spider_output(response, result, spider)
- process_spider_exception(response, exception, spider)
- process_start_requests(start_requests, spider)



**Item Pipeline的用法**

它的主要功能有：

- 清洗 HTML 数据
- 验证爬取数据，检查爬取字段
- 查重并丢弃重复内容
- 将爬取结果储存到数据库

可以自定义 Item Pipeline，只需要实现指定的方法就好，其中必须要实现的一个方法是：

- process_item(item, spider)

另外还有几个比较实用的方法，它们分别是：

- open_spider(spider)
- close_spider(spider)
- from_crawler(cls, crawler)

open_spider() 方法是在 Spider 开启的时候被自动调用的，在这里我们可以做一些初始化操作，如开启数据库连接等。其中参数 spider 就是被开启的 Spider 对象。

close_spider() 方法是在 Spider 关闭的时候自动调用的，在这里我们可以做一些收尾工作，如关闭数据库连接等，其中参数 spider 就是被关闭的 Spider 对象。

from_crawler() 方法是一个类方法，用 @classmethod 标识，是一种依赖注入的方式。它的参数是 crawler，通过 crawler 对象，我们可以拿到 Scrapy 的所有核心组件，如全局配置的每个信息，然后创建一个 Pipeline 实例。参数 cls 就是 Class，最后返回一个 Class 实例。





**Scrapy对接Selenium**

Scrapy 也不能抓取 JavaScript 动态渲染的页面。在前文中抓取 JavaScript 渲染的页面有两种方式。一种是分析 Ajax 请求，找到其对应的接口抓取，Scrapy 同样可以用此种方式抓取。另一种是直接用 Selenium 或 Splash 模拟浏览器进行抓取

对接 Selenium 进行抓取，采用 Downloader Middleware 来实现。在 Middleware 里面的 process_request() 方法里对每个抓取请求进行处理，启动浏览器并进行页面渲染，再将渲染后的结果构造一个 HtmlResponse 对象返回。代码实现如下所示：

from selenium import webdriver

from selenium.common.exceptions import TimeoutException

from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC

from scrapy.http import HtmlResponse

from logging import getLogger

class SeleniumMiddleware():

​    def __init__(self, timeout=None, service_args=[]):

​        self.logger = getLogger(__name__)

​        self.timeout = timeout

​        self.browser = webdriver.PhantomJS(service_args=service_args)

​        self.browser.set_window_size(1400, 700)

​        self.browser.set_page_load_timeout(self.timeout)

​        self.wait = WebDriverWait(self.browser, self.timeout)

​    def __del__(self):

​        self.browser.close()

​    def process_request(self, request, spider):

​        """

​        用 PhantomJS 抓取页面

​        :param request: Request 对象

​        :param spider: Spider 对象

​        :return: HtmlResponse

​        """

​        self.logger.debug('PhantomJS is Starting')

​        page = request.meta.get('page', 1)

​        try:

​            self.browser.get(request.url)

​            if page > 1:

​                input = self.wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '#mainsrp-pager div.form> input')))

​                submit = self.wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#mainsrp-pager div.form> span.btn.J_Submit')))

​                input.clear()

​                input.send_keys(page)

​                submit.click()

​            self.wait.until(EC.text_to_be_present_in_element((By.CSS_SELECTOR, '#mainsrp-pager li.item.active> span'), str(page)))

​            self.wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.m-itemlist .items .item')))

​            return HtmlResponse(url=request.url, body=self.browser.page_source, request=request, encoding='utf-8', status=200)

​        except TimeoutException:

​            return HtmlResponse(url=request.url, status=500, request=request)

​    @classmethod

​    def from_crawler(cls, crawler):

​        return cls(timeout=crawler.settings.get('SELENIUM_TIMEOUT'),

​                   service_args=crawler.settings.get('PHANTOMJS_SERVICE_ARGS'))

首先我们在 __init__() 里对一些对象进行初始化，包括 PhantomJS、WebDriverWait 等对象，同时设置页面大小和页面加载超时时间。在 process_request() 方法中，我们通过 Request 的 meta 属性获取当前需要爬取的页码，调用 PhantomJS 对象的 get() 方法访问 Request 的对应的 URL。这就相当于从 Request 对象里获取请求链接，然后再用 PhantomJS 加载，而不再使用 Scrapy 里的 Downloader。最后，页面加载完成之后，我们调用 PhantomJS 的 page_source 属性即可获取当前页面的源代码，然后用它来直接构造并返回一个 HtmlResponse 对象。构造这个对象的时候需要传入多个参数，如 url、body 等，这些参数实际上就是它的基础属性。



为什么实现这么一个 Downloader Middleware 就可以了？之前的 Request 对象怎么办？Scrapy 不再处理了吗？Response 返回后又传递给了谁？

是的，Request 对象到这里就不会再处理了，也不会再像以前一样交给 Downloader 下载。Response 会直接传给 Spider 进行解析。



**Scrapy对接Splash**

除了 Selenium，Splash 也可以实现抓取 JavaScript 动态渲染页面

添加配置

修改 settings.py，配置 SPLASH_URL。在这里我们的 Splash 是在本地运行的，所以可以直接配置本地的地址：

SPLASH_URL = 'http://localhost:8050'

如果 Splash 是在远程服务器运行的，那此处就应该配置为远程的地址。

还需要配置几个 Middleware，代码如下所示：

DOWNLOADER_MIDDLEWARES = {

​    'scrapy_splash.SplashCookiesMiddleware': 723,

​    'scrapy_splash.SplashMiddleware': 725,

​    'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 810,

}

SPIDER_MIDDLEWARES = {'scrapy_splash.SplashDeduplicateArgsMiddleware': 100,}

这里配置了三个 Downloader Middleware 和一个 Spider Middleware，这是 Scrapy-Splash 的核心部分。我们不再需要像对接 Selenium 那样实现一个 Downloader Middleware，Scrapy-Splash 库都为我们准备好了，直接配置即可。

还需要配置一个去重的类 DUPEFILTER_CLASS，代码如下所示：

DUPEFILTER_CLASS = 'scrapy_splash.SplashAwareDupeFilter'

最后配置一个 Cache 存储 HTTPCACHE_STORAGE，代码如下所示：

HTTPCACHE_STORAGE = 'scrapy_splash.SplashAwareFSCacheStorage'

一个示例，如下所示

yield SplashRequest(url, self.parse_result,

​    args={

​        \# optional; parameters passed to Splash HTTP API

​        'wait': 0.5,

​        \# 'url' is prefilled from request url

​        \# 'http_method' is set to 'POST' for POST requests

​        \# 'body' is set to request body for POST requests

​    },

​    endpoint='render.json', # optional; default is render.html

​    splash_url='<url>',     # optional; overrides SPLASH_URL

)

在这里构造了一个 SplashRequest 对象，前两个参数依然是请求的 URL 和回调函数，另外还可以通过 args 传递一些渲染参数，例如等待时间 wait 等，还可以根据 endpoint 参数指定渲染接口，另外还有更多的参数

另外我们也可以生成 Request 对象，关于 Splash 的配置通过 meta 属性配置即可，代码如下：

yield scrapy.Request(url, self.parse_result, meta={

​    'splash': {

​        'args': {

​            \# set rendering arguments here

​            'html': 1,

​            'png': 1,

​            \# 'url' is prefilled from request url

​            \# 'http_method' is set to 'POST' for POST requests

​            \# 'body' is set to request body for POST requests

​        },

​        \# optional parameters

​        'endpoint': 'render.json',  # optional; default is render.json

​        'splash_url': '<url>',      # optional; overrides SPLASH_URL

​        'slot_policy': scrapy_splash.SlotPolicy.PER_DOMAIN,

​        'splash_headers': {},       # optional; a dict with headers sent to Splash

​        'dont_process_response': True, # optional, default is False

​        'dont_send_headers': True,  # optional, default is False

​        'magic_response': False,    # optional, default is True

​    }

})

SplashRequest 对象通过 args 来配置和 Request 对象通过 meta 来配置，两种方式达到的效果是相同的。



只需要在 Spider 里用 SplashRequest 对接 Lua 脚本就好了，如下所示：

from scrapy import Spider

from urllib.parse import quote

from scrapysplashtest.items import ProductItem

from scrapy_splash import SplashRequest

script = """

function main(splash, args)

  splash.images_enabled = false

  assert(splash:go(args.url))

  assert(splash:wait(args.wait))

  js = string.format("document.querySelector('#mainsrp-pager div.form> input').value=% d;document.querySelector('#mainsrp-pager div.form> span.btn.J_Submit').click()", args.page)

  splash:evaljs(js)

  assert(splash:wait(args.wait))

  return splash:html()

end

"""

class TaobaoSpider(Spider):

​    name = 'taobao'

​    allowed_domains = ['www.taobao.com']

​    base_url = 'https://s.taobao.com/search?q='

​    def start_requests(self):

​        for keyword in self.settings.get('KEYWORDS'):

​            for page in range(1, self.settings.get('MAX_PAGE') + 1):

​                url = self.base_url + quote(keyword)

​                yield SplashRequest(url, callback=self.parse, endpoint='execute', args={'lua_source': script, 'page': page, 'wait': 7})

我们把 Lua 脚本定义成长字符串，通过 SplashRequest 的 args 来传递参数，接口修改为 execute。另外，args 参数里还有一个 lua_source 字段用于指定 Lua 脚本内容。这样我们就成功构造了一个 SplashRequest，对接 Splash 的工作就完成了。其他的配置不需要更改

在 Scrapy 中，建议使用 Splash 处理 JavaScript 动态渲染的页面。这样不会破坏 Scrapy 中的异步处理过程，会大大提高爬取效率。而且 Splash 的安装和配置比较简单，通过 API 调用的方式实现了模块分离，大规模爬取的部署也更加方便。





**Scrapy通用爬虫**

通过 Scrapy，我们可以轻松地完成一个站点爬虫的编写。但如果抓取的站点量非常大，比如爬取各大媒体的新闻信息，多个 Spider 则可能包含很多重复代码。

如果我们将各个站点的 Spider 的公共部分保留下来，不同的部分提取出来作为单独的配置，如爬取规则、页面解析方式等抽离出来做成一个配置文件，那么我们在新增一个爬虫的时候，只需要实现这些网站的爬取规则和提取规则即可。

CrawlSpider

CrawlSpider 是 Scrapy 提供的一个通用 Spider。在 Spider 里，我们可以指定一些爬取规则来实现页面的提取，这些爬取规则由一个专门的数据结构 Rule 表示。Rule 里包含提取和跟进页面的配置，Spider 会根据 Rule 来确定当前页面中的哪些链接需要继续爬取、哪些页面的爬取结果需要用哪个方法解析等。

CrawlSpider 继承自 Spider 类。除了 Spider 类的所有方法和属性，它还提供了一个非常重要的属性和方法。

- rules，它是爬取规则属性，是包含一个或多个 Rule 对象的列表。每个 Rule 对爬取网站的动作都做了定义，CrawlSpider 会读取 rules 的每一个 Rule 并进行解析。
- parse_start_url()，它是一个可重写的方法。当 start_urls 里对应的 Request 得到 Response 时，该方法被调用，它会分析 Response 并必须返回 Item 对象或者 Request 对象。

Rule 的定义和参数如下所示：

class scrapy.contrib.spiders.Rule(link_extractor, callback=None, cb_kwargs=None, follow=None, process_links=None, process_request=None)

- link_extractor，是一个 Link Extractor 对象。通过它，Spider 可以知道从爬取的页面中提取哪些链接。提取出的链接会自动生成 Request。它又是一个数据结构，一般常用 LxmlLinkExtractor 对象作为参数，其定义和参数如下所示：

class scrapy.linkextractors.lxmlhtml.LxmlLinkExtractor(allow=(), deny=(), allow_domains=(), deny_domains=(), deny_extensions=None, restrict_xpaths=(), restrict_css=(), tags=('a', 'area'), attrs=('href',), canonicalize=False, unique=True, process_value=None, strip=True)

allow 是一个正则表达式或正则表达式列表，它定义了从当前页面提取出的链接哪些是符合要求的，只有符合要求的链接才会被跟进。

deny 则相反。

allow_domains 定义了符合要求的域名，只有此域名的链接才会被跟进生成新的 Request，它相当于域名白名单。

deny_domains 则相反，相当于域名黑名单。

restrict_xpaths 定义了从当前页面中 XPath 匹配的区域提取链接，其值是 XPath 表达式或 XPath 表达式列表。

restrict_css 定义了从当前页面中 CSS 选择器匹配的区域提取链接，其值是 CSS 选择器或 CSS 选择器列表。

还有一些其他参数代表了提取链接的标签、是否去重、链接的处理等内容，使用的频率不高。

- callback，即回调函数，和之前定义 Request 的 callback 有相同的意义。每次从 link_extractor 中获取到链接时，该函数将会调用。该回调函数接收一个 response 作为其第一个参数，并返回一个包含 Item 或 Request 对象的列表。注意，避免使用 parse() 作为回调函数。由于 CrawlSpider 使用 parse() 方法来实现其逻辑，如果 parse() 方法覆盖了，CrawlSpider 将会运行失败。
- cb_kwargs，字典，它包含传递给回调函数的参数。
- follow，布尔值，即 True 或 False，它指定根据该规则从 response 提取的链接是否需要跟进。如果 callback 参数为 None，follow 默认设置为 True，否则默认为 False。
- process_links，指定处理函数，从 link_extractor 中获取到链接列表时，该函数将会调用，它主要用于过滤。
- process_request，同样是指定处理函数，根据该 Rule 提取到每个 Request 时，该函数都会调用，对 Request 进行处理。该函数必须返回 Request 或者 None。

Item Loader

Rule 并没有对 Item 的提取方式做规则定义。对于 Item 的提取，我们需要借助另一个模块 Item Loader 来实现。

Item Loader 提供一种便捷的机制来帮助我们方便地提取 Item。它提供的一系列 API 可以分析原始数据对 Item 进行赋值。Item 提供的是保存抓取数据的容器，而 Item Loader 提供的是填充容器的机制。有了它，数据的提取会变得更加规则化。

Item Loader 的 API 如下所示：

class scrapy.loader.ItemLoader([item, selector, response,] **kwargs)

- item，Item 对象，可以调用 add_xpath()、add_css() 或 add_value() 等方法来填充 Item 对象。
- selector，Selector 对象，用来提取填充数据的选择器。
- response，Response 对象，用于使用构造选择器的 Response。

一个比较典型的 Item Loader 实例如下：

from scrapy.loader import ItemLoader

from project.items import Product

def parse(self, response):

​    loader = ItemLoader(item=Product(), response=response)

​    loader.add_xpath('name', '//div[@class="product_name"]')

​    loader.add_xpath('name', '//div[@class="product_title"]')

​    loader.add_xpath('price', '//p[@id="price"]')

​    loader.add_css('stock', 'p#stock]')

​    loader.add_value('last_updated', 'today')

​    return loader.load_item()

这里首先声明一个 Product Item，用该 Item 和 Response 对象实例化 ItemLoader，调用 add_xpath() 方法把来自两个不同位置的数据提取出来，分配给 name 属性，再用 add_xpath()、add_css()、add_value() 等方法对不同属性依次赋值，最后调用 load_item() 方法实现 Item 的解析。这种方式比较规则化，我们可以把一些参数和规则单独提取出来做成配置文件或存到数据库，即可实现可配置化。

另外，Item Loader 每个字段中都包含了一个 Input Processor（输入处理器）和一个 Output Processor（输出处理器）。Input Processor 收到数据时立刻提取数据，Input Processor 的结果被收集起来并且保存在 ItemLoader 内，但是不分配给 Item。收集到所有的数据后，load_item() 方法被调用来填充再生成 Item 对象。在调用时会先调用 Output Processor 来处理之前收集到的数据，然后再存入 Item 中，这样就生成了 Item。



新建项目

scrapy startproject scrapyuniversal

创建一个 CrawlSpider，需要先制定一个模板。我们可以先看看有哪些可用模板，命令如下所示：

scrapy genspider -l

\# 运行结果如下所示：

Available templates:

  basic

  crawl

  csvfeed

  xmlfeed

之前创建 Spider 的时候，我们默认使用了第一个模板 basic。这次要创建 CrawlSpider，就需要使用第二个模板 crawl，创建命令如下所示：

scrapy genspider -t crawl china tech.china.com

运行之后便会生成一个 CrawlSpider，其内容如下所示：

from scrapy.linkextractors import LinkExtractor

from scrapy.spiders import CrawlSpider, Rule

class ChinaSpider(CrawlSpider):

​    name = 'china'

​    allowed_domains = ['tech.china.com']

​    start_urls = ['http://tech.china.com/']

​    rules = (Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),

​    )

​    def parse_item(self, response):

​        i = {}

​        \#i['domain_id'] = response.xpath('//input[@id="sid"]/@value').extract()

​        \#i['name'] = response.xpath('//div[@id="name"]').extract()

​        \#i['description'] = response.xpath('//div[@id="description"]').extract()

​        return i

这次生成的 Spider 内容多了一个 rules 属性的定义。Rule 的第一个参数是 LinkExtractor，就是上文所说的 LxmlLinkExtractor，只是名称不同。同时，默认的回调函数也不再是 parse，而是 parse_item。



通用配置抽取

所有的变量都可以抽取，如 name、allowed_domains、start_urls、rules 等。这些变量在 CrawlSpider 初始化的时候赋值即可。我们就可以新建一个通用的 Spider 来实现这个功能，命令如下所示：

scrapy genspider -t crawl universal universal

这个全新的 Spider 名为 universal。接下来，我们将刚才所写的 Spider 内的属性抽离出来配置成一个 JSON，命名为 china.json，放到 configs 文件夹内，和 spiders 文件夹并列，代码如下所示：

{

  "spider": "universal",

  "website": "中华网科技",

  "type": "新闻",

  "index": "http://tech.china.com/",

  "settings": {"USER_AGENT": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36"

  },

  "start_urls": ["http://tech.china.com/articles/"],

  "allowed_domains": ["tech.china.com"],

  "rules": "china"

}

第一个字段 spider 即 Spider 的名称，在这里是 universal。后面是站点的描述，比如站点名称、类型、首页等。随后的 settings 是该 Spider 特有的 settings 配置，如果要覆盖全局项目，settings.py 内的配置可以单独为其配置。随后是 Spider 的一些属性，如 start_urls、allowed_domains、rules 等。rules 也可以单独定义成一个 rules.py 文件，做成配置文件，实现 Rule 的分离，如下所示：

from scrapy.linkextractors import LinkExtractor

from scrapy.spiders import Rule

rules = {

​    'china': (Rule(LinkExtractor(allow='article\/.*\.html', restrict_xpaths='//div[@id="left_side"]//div[@class="con_item"]'),

​             callback='parse_item'),

​        Rule(LinkExtractor(restrict_xpaths='//div[@id="pageStyle"]//a[contains(., "下一页")]'))

​    )

}

这样我们将基本的配置抽取出来。如果要启动爬虫，只需要从该配置文件中读取然后动态加载到 Spider 中即可。所以我们需要定义一个读取该 JSON 文件的方法，如下所示：

from os.path import realpath, dirname

import json

def get_config(name):

​    path = dirname(realpath(__file__)) + '/configs/' + name + '.json'

​    with open(path, 'r', encoding='utf-8') as f:

​        return json.loads(f.read())

定义了 get_config() 方法之后，我们只需要向其传入 JSON 配置文件的名称即可获取此 JSON 配置信息。随后我们定义入口文件 run.py，把它放在项目根目录下，它的作用是启动 Spider，如下所示：

import sys

from scrapy.utils.project import get_project_settings

from scrapyuniversal.spiders.universal import UniversalSpider

from scrapyuniversal.utils import get_config

from scrapy.crawler import CrawlerProcess

def run():

​    name = sys.argv[1]

​    custom_settings = get_config(name)

​    \# 爬取使用的 Spider 名称

​    spider = custom_settings.get('spider', 'universal')

​    project_settings = get_project_settings()

​    settings = dict(project_settings.copy())

​    \# 合并配置

​    settings.update(custom_settings.get('settings'))

​    process = CrawlerProcess(settings)

​    \# 启动爬虫

​    process.crawl(spider, **{'name': name})

​    process.start()

if __name__ == '__main__':

​    run()

运行入口为 run()。首先获取命令行的参数并赋值为 name，name 就是 JSON 文件的名称，其实就是要爬取的目标网站的名称。我们首先利用 get_config() 方法，传入该名称读取刚才定义的配置文件。获取爬取使用的 spider 的名称、配置文件中的 settings 配置，然后将获取到的 settings 配置和项目全局的 settings 配置做了合并。新建一个 CrawlerProcess，传入爬取使用的配置。调用 crawl() 和 start() 方法即可启动爬取。

在 universal 中，我们新建一个**init**() 方法，进行初始化配置，实现如下所示：

from scrapy.linkextractors import LinkExtractor

from scrapy.spiders import CrawlSpider, Rule

from scrapyuniversal.utils import get_config

from scrapyuniversal.rules import rules

class UniversalSpider(CrawlSpider):

​    name = 'universal'

​    def __init__(self, name, *args, **kwargs):

​        config = get_config(name)

​        self.config = config

​        self.rules = rules.get(config.get('rules'))

​        self.start_urls = config.get('start_urls')

​        self.allowed_domains = config.get('allowed_domains')

​        super(UniversalSpider, self).__init__(*args, **kwargs)

​    def parse_item(self, response):

​        i = {}

​        return i

在 __init__() 方法中，start_urls、allowed_domains、rules 等属性被赋值。其中，rules 属性另外读取了 rules.py 的配置，这样就成功实现爬虫的基础配置。

接下来，执行如下命令运行爬虫：

python3 run.py china





**Scrapyrt的使用**

Scrapyrt 为 Scrapy 提供了一个调度的 HTTP 接口，有了它，我们就不需要再执行 Scrapy 命令而是通过请求一个 HTTP 接口来调度 Scrapy 任务了。Scrapyrt 比 Scrapyd 更轻量，如果不需要分布式多任务的话，可以简单使用 Scrapyrt 实现远程 Scrapy 任务的调度。





**Scrapy对接Docker**

解决的就是环境的安装配置、环境的版本冲突解决等问题

对于 Python 来说，VirtualEnv 的确可以解决版本冲突的问题。但是，VirtualEnv 不太方便做项目部署，我们还是需要安装 Python 环境，

如何解决上述问题呢？答案是用 Docker。Docker 可以提供操作系统级别的虚拟环境



创建 Dockerfile

在项目的根目录下新建一个 requirements.txt 文件，将整个项目依赖的 Python 环境包都列出来，如下所示：

scrapy

pymongo

还可以指定版本号

scrapy>=1.4.0

pymongo>=3.4.0

在项目根目录下新建一个 Dockerfile 文件，文件不加任何后缀名，修改内容如下所示：

FROM python:3.6

ENV PATH /usr/local/bin:$PATH

ADD . /code

WORKDIR /code

RUN pip3 install -r requirements.txt

CMD scrapy crawl quotes

- 第一行的 FROM 代表使用的 Docker 基础镜像，在这里我们直接使用 python:3.6 的镜像，在此基础上运行 Scrapy 项目。
- 第二行 ENV 是环境变量设置，将 /usr/local/bin:$PATH 赋值给 PATH，即增加 /usr/local/bin 这个环境变量路径。
- 第三行 ADD 是将本地的代码放置到虚拟容器中。它有两个参数：第一个参数是.，代表本地当前路径；第二个参数是 /code，代表虚拟容器中的路径，也就是将本地项目所有内容放置到虚拟容器的 /code 目录下，以便于在虚拟容器中运行代码。
- 第四行 WORKDIR 是指定工作目录，这里将刚才添加的代码路径设成工作路径。这个路径下的目录结构和当前本地目录结构是相同的，所以我们可以直接执行库安装命令、爬虫运行命令等。
- 第五行 RUN 是执行某些命令来做一些环境准备工作。由于 Docker 虚拟容器内只有 Python 3 环境，而没有所需要的 Python 库，所以我们运行此命令来在虚拟容器中安装相应的 Python 库如 Scrapy，这样就可以在虚拟容器中执行 Scrapy 命令了。
- 第六行 CMD 是容器启动命令。在容器运行时，此命令会被执行。在这里我们直接用 scrapy crawl quotes 来启动爬虫。



构建镜像

docker build -t quotes:latest .

查看一下构建的镜像：

docker images

\# 返回结果中其中有一行就是：

quotes  latest  41c8499ce210    2 minutes ago   769 MB



运行

docker run quotes



推送至 Docker Hub

构建完成之后，我们可以将镜像 Push 到 Docker 镜像托管平台，如 Docker Hub 或者私有的 Docker Registry 等，这样我们就可以从远程服务器下拉镜像并运行了

为新建的镜像打一个标签，命令如下所示：

docker tag quotes:latest username/quotes:latest

推送镜像到 Docker Hub 即可，命令如下所示：

docker push germey/quotes

如果我们想在其他的主机上运行这个镜像，主机上装好 Docker 后，可以直接执行如下命令：

docker run germey/quotes

这样就会自动下载镜像，然后启动容器运行





## 分布式爬虫

Scrapy 爬虫是异步加多线程的，但是我们只能在一台主机上运行，所以爬取效率还是有限的，分布式爬虫则是将多台主机组合起来，共同完成一个爬取任务，这将大大提高爬取的效率。

Scrapy 单机爬虫中有一个本地爬取队列 Queue，这个队列是利用 deque 模块实现的。如果新的 Request 生成就会放到队列里面，随后 Request 被 Scheduler 调度。之后，Request 交给 Downloader 执行爬取



分布式架构

master-主机：维护爬虫队列。

slave-从机：数据爬取，数据处理，数据存储。



维护爬取队列

那么这个队列用什么维护来好呢？我们首先需要考虑的就是性能问题，什么数据库存取效率高？我们自然想到基于内存存储的 Redis，而且 Redis 还支持多种数据结构，例如列表 List、集合 Set、有序集合 Sorted Set 等等，存取的操作也非常简单，所以在这里我们采用 Redis 来维护爬取队列。

这几种数据结构存储实际各有千秋，分析如下：

- 列表数据结构有 lpush()、lpop()、rpush()、rpop() 方法，所以我们可以用它来实现一个先进先出式爬取队列，也可以实现一个先进后出栈式爬取队列。
- 集合的元素是无序的且不重复的，这样我们可以非常方便地实现一个随机排序的不重复的爬取队列。
- 有序集合带有分数表示，而 Scrapy 的 Request 也有优先级的控制，所以用有集合我们可以实现一个带优先级调度的队列。



怎样来去重

Scrapy 有自动去重，它的去重使用了 Python 中的集合。这个集合记录了 Scrapy 中每个 Request 的指纹，这个指纹实际上就是 Request 的散列值

request_fingerprint() 就是计算 Request 指纹的方法，其方法内部使用的是 hashlib 的 sha1() 方法。计算的字段包括 Request 的 Method、URL、Body、Headers 这几部分内容，这里只要有一点不同，那么计算的结果就不同。计算得到的结果是加密后的字符串，也就是指纹。每个 Request 都有独有的指纹，指纹就是一个字符串，判定字符串是否重复比判定 Request 对象是否重复容易得多，所以指纹可以作为判定 Request 是否重复的依据。

在去重的类 RFPDupeFilter 中，有一个 request_seen() 方法，这个方法有一个参数 request，它的作用就是检测该 Request 对象是否重复。这个方法调用 request_fingerprint() 获取该 Request 的指纹，检测这个指纹是否存在于 fingerprints 变量中，而 fingerprints 是一个集合，集合的元素都是不重复的。如果指纹存在，那么就返回 True，说明该 Request 是重复的，否则这个指纹加入到集合中。如果下次还有相同的 Request 传递过来，指纹也是相同的，那么这时指纹就已经存在于集合中，Request 对象就会直接判定为重复。这样去重的目的就实现了。

Scrapy 的去重过程就是，利用集合元素的不重复特性来实现 Request 的去重。



对于分布式爬虫来说，我们肯定不能再用每个爬虫各自的集合来去重了。因为这样还是每个主机单独维护自己的集合，不能做到共享。多台主机如果生成了相同的 Request，只能各自去重，各个主机之间就无法做到去重了。

那么要实现去重，这个指纹集合也需要是共享的，Redis 正好有集合的存储数据结构，我们可以利用 Redis 的集合作为指纹集合，那么这样去重集合也是利用 Redis 共享的。每台主机新生成 Request 之后，把该 Request 的指纹与集合比对，如果指纹已经存在，说明该 Request 是重复的，否则将 Request 的指纹加入到这个集合中即可。利用同样的原理不同的存储结构我们也实现了分布式 Reqeust 的去重。



防止中断

在 Scrapy 中，爬虫运行时的 Request 队列放在内存中。爬虫运行中断后，这个队列的空间就被释放，此队列就被销毁了。所以一旦爬虫运行中断，爬虫再次运行就相当于全新的爬取过程。

要做到中断后继续爬取，我们可以将队列中的 Request 保存起来，下次爬取直接读取保存数据即可获取上次爬取的队列。我们在 Scrapy 中指定一个爬取队列的存储路径即可，这个路径使用 JOB_DIR 变量来标识，我们可以用如下命令来实现：

scrapy crawl spider -s JOBDIR=crawls/spider

在 Scrapy 中，我们实际是把爬取队列保存到本地，第二次爬取直接读取并恢复队列即可。那么在分布式架构中我们还用担心这个问题吗？不需要。因为爬取队列本身就是用数据库保存的，如果爬虫中断了，数据库中的 Request 依然是存在的，下次启动就会接着上次中断的地方继续爬取。

所以，当 Redis 的队列为空时，爬虫会重新爬取；当 Redis 的队列不为空时，爬虫便会接着上次中断之处继续爬取。



三个分布式的问题解决了，总结如下：

- 爬取队列的实现，在这里提供了三种队列，使用了 Redis 的列表或有序集合来维护。
- 去重的实现，使用了 Redis 的集合来保存 Request 的指纹来提供重复过滤。
- 中断后重新爬取的实现，中断后 Redis 的队列没有清空，再次启动时调度器的 next_request() 会从队列中取到下一个 Request，继续爬取。



利用 Scrapy-Redis 来实现分布式的对接

配置 Scrapy-Redis

首先最主要的是，需要将调度器的类和去重的类替换为 Scrapy-Redis 提供的类，在 settings.py 里面添加如下配置即可：

SCHEDULER = "scrapy_redis.scheduler.Scheduler"

DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"

直接在 settings.py 里面配置为 REDIS_URL 变量即可：

REDIS_URL = 'redis://:foobared@120.27.34.25:6379'

分项单独配置。这个配置就更加直观明了，如根据 Redis 连接信息，可以在 settings.py 中配置如下代码：

REDIS_HOST = '120.27.34.25'

REDIS_PORT = 6379

REDIS_PASSWORD = 'foobared'

注意，如果配置了 REDIS_URL，那么 Scrapy-Redis 将优先使用 REDIS_URL 连接，会覆盖上面的三项配置。如果想要分项单独配置的话，请不要配置 REDIS_URL



配置调度队列

此项配置是可选的，默认使用 PriorityQueue。如果想要更改配置，可以配置 SCHEDULER_QUEUE_CLASS 变量，如下所示：

SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.PriorityQueue'

SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.FifoQueue'

SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.LifoQueue'

以上三行任选其一配置，即可切换爬取队列的存储方式。

配置持久化

此配置是可选的，默认是 False。Scrapy-Redis 默认会在爬取全部完成后清空爬取队列和去重指纹集合。

如果不想自动清空爬取队列和去重指纹集合，可以增加如下配置：

SCHEDULER_PERSIST = True

将 SCHEDULER_PERSIST 设置为 True 之后，爬取队列和去重指纹集合不会在爬取完成后自动清空，如果不配置，默认是 False，即自动清空。

值得注意的是，如果强制中断爬虫的运行，爬取队列和去重指纹集合是不会自动清空的。



配置重爬

此配置是可选的，默认是 False。如果配置了持久化或者强制中断了爬虫，那么爬取队列和指纹集合不会被清空，爬虫重新启动之后就会接着上次爬取。如果想重新爬取，我们可以配置重爬的选项：

SCHEDULER_FLUSH_ON_START = True

这样将 SCHEDULER_FLUSH_ON_START 设置为 True 之后，爬虫每次启动时，爬取队列和指纹集合都会清空。所以要做分布式爬取，我们必须保证只能清空一次，否则每个爬虫任务在启动时都清空一次，就会把之前的爬取队列清空，势必会影响分布式爬取。

注意，此配置在单机爬取的时候比较方便，分布式爬取不常用此配置。



Pipeline 配置

此配置是可选的，默认不启动 Pipeline。Scrapy-Redis 实现了一个存储到 Redis 的 Item Pipeline，启用了这个 Pipeline 的话，爬虫会把生成的 Item 存储到 Redis 数据库中。在数据量比较大的情况下，我们一般不会这么做。因为 Redis 是基于内存的，我们利用的是它处理速度快的特性，用它来做存储未免太浪费了，配置如下：

ITEM_PIPELINES = {'scrapy_redis.pipelines.RedisPipeline': 300}



运行

接下来将代码部署到各台主机上，每台主机启动了此命令之后，就会从配置的 Redis 数据库中调度 Request，做到爬取队列共享和指纹集合共享。同时每台主机占用各自的带宽和处理器，不会互相影响，爬取效率成倍提高。



随着时间的推移，指纹集合会不断增长，爬取队列会动态变化，爬取的数据也会被储存到 MongoDB 数据库中。



**Bloom Filter的对接**

当爬取数量达到上亿级别时，Redis 的占用的内存就会变得很大，而且这仅仅是指纹的存储。Redis 还存储了爬取队列，内存占用会进一步提高，更别说有多个 Scrapy 项目同时爬取的情况了。当爬取达到亿级别规模时，Scrapy-Redis 提供的集合去重已经不能满足我们的要求。所以我们需要使用一个更加节省内存的去重算法 Bloom Filter。

Bloom Filter，中文名称叫作布隆过滤器，Bloom Filter 的空间利用效率很高，使用它可以大大节省存储空间。Bloom Filter 使用位数组表示一个待检测集合，并可以快速地通过概率算法判断一个元素是否存在于这个集合中。利用这个算法我们可以实现去重效果。





## 分布式爬虫的部署

**Scrapyd分布式部署**

Scrapyd 是一个运行 Scrapy 爬虫的服务程序，它提供一系列 HTTP 接口来帮助我们部署、启动、停止、删除爬虫程序。Scrapyd 支持版本管理，同时还可以管理多个爬虫任务，利用它我们可以非常方便地完成 Scrapy 爬虫项目的部署任务调度。

访问 Scrapyd

安装并运行了 Scrapyd 之后，我们就可以访问服务器的 6800 端口看到一个 WebUI 页面了，例如我的服务器地址为 120.27.34.25，在上面安装好了 Scrapyd 并成功运行，那么我就可以在本地的浏览器中打开：http://120.27.34.25:6800，就可以看到 Scrapyd 的首页，这里请自行替换成你的服务器地址查看即可



daemonstatus.json

这个接口负责查看 Scrapyd 当前的服务和任务状态，我们可以用 curl 命令来请求这个接口，命令如下：

curl http://139.217.26.30:6800/daemonstatus.json

这样我们就会得到如下结果：

{"status": "ok", "finished": 90, "running": 9, "node_name": "datacrawl-vm", "pending": 0}

返回结果是 Json 字符串，status 是当前运行状态， finished 代表当前已经完成的 Scrapy 任务，running 代表正在运行的 Scrapy 任务，pending 代表等待被调度的 Scrapyd 任务，node_name 就是主机的名称。



addversion.json

这个接口主要是用来部署 Scrapy 项目用的，在部署的时候我们需要首先将项目打包成 Egg 文件，然后传入项目名称和部署版本。

我们可以用如下的方式实现项目部署：

curl http://120.27.34.25:6800/addversion.json -F project=wenbo -F version=first -F egg=@weibo.egg

在这里 -F 即代表添加一个参数，同时我们还需要将项目打包成 Egg 文件放到本地。

这样发出请求之后我们可以得到如下结果：

{"status": "ok", "spiders": 3}

这个结果表明部署成功，并且其中包含的 Spider 的数量为 3。



schedule.json

这个接口负责调度已部署好的 Scrapy 项目运行。

我们可以用如下接口实现任务调度：

curl http://120.27.34.25:6800/schedule.json -d project=weibo -d spider=weibocn

在这里需要传入两个参数，project 即 Scrapy 项目名称，spider 即 Spider 名称。

返回结果如下：

{"status": "ok", "jobid": "6487ec79947edab326d6db28a2d86511e8247444"}

status 代表 Scrapy 项目启动情况，jobid 代表当前正在运行的爬取任务代号。



cancel.json

这个接口可以用来取消某个爬取任务



listprojects.json

这个接口用来列出部署到 Scrapyd 服务上的所有项目描述。



listversions.json

这个接口用来获取某个项目的所有版本号，版本号是按序排列的，最后一个条目是最新的版本号。



listspiders.json

这个接口用来获取某个项目最新的一个版本的所有 Spider 名称。



listjobs.json

这个接口用来获取某个项目当前运行的所有任务详情。



delversion.json

这个接口用来删除项目的某个版本。



delproject.json

这个接口用来删除某个项目。



ScrapydAPI 的使用

ScrapydAPI 的使用方法，其实核心原理和 HTTP 接口请求方式并无二致，只不过用 Python 封装后使用更加便捷。



**Scrapyd-Client的使用**

Scrapyd-Client 为了方便 Scrapy 项目的部署，提供两个功能：

- 将项目打包成 Egg 文件。
- 将打包生成的 Egg 文件通过 addversion.json 接口部署到 Scrapyd 上。

也就是说，Scrapyd-Client 帮我们把部署全部实现了，我们不需要再去关心 Egg 文件是怎样生成的，也不需要再去读 Egg 文件并请求接口上传了，这一切的操作只需要执行一个命令即可一键部署。



Scrapyd-Client 部署

要部署 Scrapy 项目，我们首先需要修改一下项目的配置文件，例如我们之前写的 Scrapy 微博爬虫项目，在项目的第一层会有一个 scrapy.cfg 文件，它的内容如下：

[settings]

default = weibo.settings

[deploy]

\#url = http://localhost:6800/

project = weibo

在这里我们需要配置一下 deploy 部分，例如我们要将项目部署到 120.27.34.25 的 Scrapyd 上，就需要修改为如下内容：

[deploy]

url = http://120.27.34.25:6800/

project = weibo

这样我们再在 scrapy.cfg 文件所在路径执行如下命令：

scrapyd-deploy

运行结果如下：

Packing version 1501682277

Deploying to project "weibo" in http://120.27.34.25:6800/addversion.json

Server response (200):

{"status": "ok", "spiders": 1, "node_name": "datacrawl-vm", "project": "weibo", "version": "1501682277"}

返回这样的结果就代表部署成功了。

我们也可以指定项目版本，如果不指定的话默认为当前时间戳，指定的话通过 version 参数传递即可，例如：

scrapyd-deploy --version 201707131455

值得注意的是在 Python3 的 Scrapyd 1.2.0 版本中我们不要指定版本号为带字母的字符串，需要为纯数字，否则可能会出现报错。

另外如果我们有多台主机，我们可以配置各台主机的别名，例如可以修改配置文件为：

[deploy:vm1]

url = http://120.27.34.24:6800/

project = weibo

[deploy:vm2]

url = http://139.217.26.30:6800/

project = weibo

有多台主机的话就在此统一配置，一台主机对应一组配置，在 deploy 后面加上主机的别名即可，这样如果我们想将项目部署到 IP 为 139.217.26.30 的 vm2 主机，我们只需要执行如下命令：

scrapyd-deploy vm2

这样我们就可以将项目部署到名称为 vm2 的主机上了。

如此一来，如果我们有多台主机，我们只需要在 scrapy.cfg 文件中配置好各台主机的 Scrapyd 地址，然后调用 scrapyd-deploy 命令加主机名称即可实现部署，非常方便。

如果 Scrapyd 设置了访问限制的话，我们可以在配置文件中加入用户名和密码的配置，同时端口修改一下，修改成 Nginx 代理端口

[deploy:vm1]

url = http://120.27.34.24:6801/

project = weibo

username = admin

password = admin

[deploy:vm2]

url = http://139.217.26.30:6801/

project = weibo

username = germey

password = germey



**Scrapyd对接Docker**

使用了 Scrapyd-Client 成功将 Scrapy 项目部署到 Scrapyd 运行，前提是需要提前在服务器上安装好 Scrapyd 并运行 Scrapyd 服务，而这个过程比较麻烦。如果同时将一个 Scrapy 项目部署到 100 台服务器上，我们需要手动配置每台服务器的 Python 环境，更改 Scrapyd 配置吗？如果这些服务器的 Python 环境是不同版本，同时还运行其他的项目，而版本冲突又会造成不必要的麻烦。

所以，我们需要解决一个痛点，那就是 Python 环境配置问题和版本冲突解决问题。如果我们将 Scrapyd 直接打包成一个 Docker 镜像，那么在服务器上只需要执行 Docker 命令就可以启动 Scrapyd 服务，这样就不用再关心 Python 环境问题，也不需要担心版本冲突问题。



对接 Docker

接下来我们首先新建一个项目，然后新建一个 scrapyd.conf，即 Scrapyd 的配置文件，内容如下：

[scrapyd]

eggs_dir    = eggs

logs_dir    = logs

items_dir   =

jobs_to_keep = 5

dbs_dir     = dbs

max_proc    = 0

max_proc_per_cpu = 10

finished_to_keep = 100

poll_interval = 5.0

bind_address = 0.0.0.0

http_port   = 6800

debug       = off

runner      = scrapyd.runner

application = scrapyd.app.application

launcher    = scrapyd.launcher.Launcher

webroot     = scrapyd.website.Root

[services]

schedule.json     = scrapyd.webservice.Schedule

cancel.json       = scrapyd.webservice.Cancel

addversion.json   = scrapyd.webservice.AddVersion

listprojects.json = scrapyd.webservice.ListProjects

listversions.json = scrapyd.webservice.ListVersions

listspiders.json  = scrapyd.webservice.ListSpiders

delproject.json   = scrapyd.webservice.DeleteProject

delversion.json   = scrapyd.webservice.DeleteVersion

listjobs.json     = scrapyd.webservice.ListJobs

daemonstatus.json = scrapyd.webservice.DaemonStatus

在这里实际上是修改自官方文档的配置文件：

- max_proc_per_cpu = 10，原本是 4，即 CPU 单核最多运行 4 个 Scrapy 任务，也就是说 1 核的主机最多同时只能运行 4 个 Scrapy 任务，在这里设置上限为 10，也可以自行设置。
- bind_address = 0.0.0.0，原本是 127.0.0.1，不能公开访问，在这里修改为 0.0.0.0 即可解除此限制。

接下来新建一个 requirements.txt ，将一些 Scrapy 项目常用的库都列进去。

最后我们新建一个 Dockerfile，内容如下：

FROM python:3.6

ADD . /code

WORKDIR /code

COPY ./scrapyd.conf/etc/scrapyd/

EXPOSE 6800

RUN pip3 install -r requirements.txt

CMD scrapyd

到现在基本的工作就完成了，运行如下命令进行构建：

docker build -t scrapyd:latest .

构建成功后即可运行测试：

docker run -d -p 6800:6800 scrapyd





**Gerapy分布式管理**

当前可以优化的问题。

- 使用 Scrapyd-Client 部署时，需要在配置文件中配置好各台主机的地址，然后利用命令行执行部署过程。如果我们省去各台主机的地址配置，将命令行对接图形界面，只需要点击按钮即可实现批量部署，这样就更方便了。
- 使用 Scrapyd API 可以控制 Scrapy 任务的启动、终止等工作，但很多操作还是需要代码来实现，同时获取爬取日志还比较烦琐。如果我们有一个图形界面，只需要点击按钮即可启动和终止爬虫任务，同时还可以实时查看爬取日志报告，那这将大大节省我们的时间和精力。

所以我们的终极目标是如下内容。

- 更方便地控制爬虫运行
- 更直观地查看爬虫状态
- 更实时地查看爬取结果
- 更简单地实现项目部署
- 更统一地实现主机管理

而这所有的工作均可通过 Gerapy 来实现。

Gerapy 是一个基于 Scrapyd、Scrapyd API、Django、Vue.js 搭建的分布式爬虫管理框架