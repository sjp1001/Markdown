# 安装request库

- `pip install requests`注意这里是requests
- `import requsets`引入requests库

# requests库的方法

|方法（7个）|说明|
|-|-|
|`requsets.request()`|构造一个请求，以支撑以下各种方法（下面所有方法在使用时都会返回Requst对象）|
|`requsets.get()`    |获取HTML网页的主要方法，对应于HTTP的GET|
|`requsets.head()`   |获取HTML网页头信息的方法，对应于HTTP的HEAD|
|`requsets.post()`   |向HTML网页提交POST请求的方法，对应于HTTP的POST|
|`requsets.put()`    |向HTML网页提交PUT请求的方法，对应于HTTP的PUT|
|`requsets.path()`   |向HTML网页提交局部修改请求，对应于HTTP的PATH|
|`requsets.delete()` |向HTML网页提交删除请求，对应于HTTP的DELETE|

### request方法（`requests.request(metho, url, **kwargs)`）
>其他方法都封装在这个方法里

- `r=requests.request('GET',url,**kwargs)`
- `r=requests.request('HEAD',url,**kwargs)`
- `r=requests.request('POST',url,**kwargs)` 
- `r=requests.request('PUT',url,**kwargs)`
- `r=requests.request('PATCH',url,**kwargs)`
- `r=requests.request('delete',url,**kwargs)`
- `r=requests.request('OPTIONS',url,**kwargs)`
- **url**:网页链接
- **\*\*kwargs**：13个控制访问参数（其他六个方法也有这13个）
	1. params：url中的额外参数，字典或字节流格式，可选
	2. data：字典、字节序列或文件对象
	3. json：json格式数据
	4. headers：字典，HTTP头字段（eg.我们可以模拟我们是什么浏览器等）
	5. cookies：字典或CookieJar，Request中的cookie
	6. auth：元组，支持HTTP认证功能
	7. files：字典类型，传输文件
	8. timeout：设定超时时间，秒为单位
	9. proxies：字典类型，设定访问代理服务器，可增加登录认证（可隐藏自己ip地址）
	10. allow_redirects：默认为True，重定向开关
	11. stream：默认为True，获取内容立即下载
	12. verify：默认为True，认证SSL证书开关
	13. cert：本地SSL证书路径
- 我们可以通过`r.request.url`，看到我们提交的数据（其他参数也可以）

### get方法（`r = requests.get(url)`）
- 构造一个向服务器请求资源的Request对象（上边的七个）
- 返回一个包含服务器资源的Response对象，r（返回的Response对象 r 有五个属性） 

|属性|说明|
|-|-|
|`r.status_code`        |HTTP请求的返回状态，200表示链接成功，可以提取下面四个属性，404或其他表示失败|
|`r.raise_for_status()` |如果不是200，产生异常requests.HTTPError(各种异常见下表)|
|`r.text`               |HTTP响应内容的字符串形式，即，url对应的页面内容|
|`r.encoding`           |从HTTP的header中猜测的响应内容编码方式|
|`apparent_encoding`    |从内容中分析出的响应的内容的编码方式（备选编码方式）|
|`r.content`            |从HTTP响应内容的二进制形式（比如图片）|

|各种异常|说明|
|-|-|
|`requests.ConnectionError`   |  网络连接错误异常，如DNS查询失败、拒绝连接等|
|`requests.HTTPError`         |  HTTP错误异常|
|`requests.URLRequired`       |  URL缺失异常|
|`requests.TooManyRedirects`  |  超过最大重定向次数，产生重定向异常|
|`requests.ConnectTimeout`    |  连接远程服务器超时异常requests.Timeout 请求URL超时，产生超时异常|

### head方法（`r = requests.head(url)`）
- 例子
```
r = requests.head(url)
print(r.headers)
```

### post方法（`r = requests.post(url, data)`）
- 例子
```python
payload = {'key1':'value1', 'key2':'value2'}
r = r = requests.post(url, data=payload)
print(r.text)

# 如果内容是字典则存在 form 中，如果是字符串则存在data中
```

### put方法（跟post方法类似，只不过会将原有的数据覆盖掉）

### 理解patch和put的区别
假设URL位置有一组数据UserInfo，包括UserID、UserName等20个字段。
需求：用户修改了UserName，其他不变。
采用patch方法，仅向URL提交UserName的局部更新请求。
采用put方法，必须将所有20个字段一并提交到URL，未提交字段被删除。
patch方法的最主要好处：节省网络带宽


# HTTP协议（超文本传输协议）
- URL格式 http://host[:port]/[path]
	- host：合法的Internet主机域名或IP地址
	- port：端口号，缺省端口为80
	- path：主机路径

# 通常代码框架（更有效可靠）：

- 正常网页爬取
	```python
	import requests
	
	def getHTMLText(url):
	    try:
	        r = requests.get(url)
	        # 查看链接状态
	        # print(r.status_code)
	        r.raise_for_status() # 如过状态不是200，则引发HTTPError异常
	        # 查看网页可能编码方式
	        # print(r.apparent_encoding)  # 输出为 'utf-8'
	        # 修改编码方式为想要的
	        r.encoding = 'utf-8'  # 默认为ISO-8859-1
	        # 打印内容
	        # print(r.text)
	        return r.text # 返回内容
	    except:
	        return '产生异常'
	
	if __name__ == '__main__':
	    url = 'http://www.baidu.com'
	    getHTMLText(url)
	```
- 图片爬取下载
	```python
	import requests
	import os
	
	# 定义一个图片的url
	url = "http://image.nationalgeographic.com.cn/2017/0211/20170211061910157.jpg"
	# 定义存储的文件位置
	root = "D://pics//"
	# 将图片名字截取下来
	path = root + url.split('/')[-1]
	try:
	    if not os.path.exists(root):
	        os.mkdir(root)
	    if not os.path.exists(path):
	        r = requests.get(url)
	        with open(path,'wb')as f:
	            f.write(r.content)
	            f.close()
	            print("文件保存成功")
	    else:
	        print("文件已存在")
	except:
	    print("爬取失败")
	```
	
	

# re库
