# Python

## 基础语法知识

- [菜鸟教程](https://www.runoob.com/python/python-tutorial.html)
- python中的所有变量都是类引用，数据类型是类的属性之一
- 数字、字符串、元组为不可变类型，具有相同不可变类型值的变量会指向同一内存位置

```python
>>> a = 1
>>> b = 1
>>> print(a is b)
True
>>> print(a.index == b.index)
True
>>> print(id(a) == id(b))
True
``` 

- 关于字符串变量`__name__`：运行时某个module时该module中`__name__ = '__main__'`，而在其`import`的其他module中`__name__ = '__对应的modele名__'`


## 对比MATLAB

使用`numpy`做矩阵处理

||python|MATLAB|
|:---:|:---:|:---:|
|索引|`a[i]`，从0开始|`a(i)`，从1开始|
|矩阵运算|`a*b`/`a.dot(b)`|`a.*b`/`a*b`|
|一维数组|只有一个维度`L`|二维`(L, 1)`|

## 面向对象

语法：

```python
class classname：
    'document' # 对应内置属性__doc__的值
    static_attr1 = 1
    static_attr2 = '2' # 类所有实例共用的属性


    def __init__(self, attr1_value, attr2_value): # 构造函数
        self.attr1 = attr1_value
        self.attr2 = attr2_value


    def __del__(self): # 析构函数
        print('销毁')


    def function1(self, para1, para2): # 实际调用时无需输入第一个参数self
        print('this is function1 with' + str(para1) + str(para2))

    @staticmethod # 静态函数
    def staticfunction(self)
        print('this is a static function')

inst = classname(1, 2) # 类实例化，调用的是__init__函数
``` 

- 双下划线开头的属性、方法为私有
- 特殊函数：`__init__()`、`__del__()`、`__str__()`（返回字符串作为`print()`结果）、`__repr__()`、`__len__()`（返回数字作为`len()`结果）等
- 类内置属性：`__doc__`、`__dict__`、`__module__`等，返回类的相关信息
- 特殊的属性`__slot__`：`__slot__ = [tuple]`，限定类的属性（不过对子类不起作用）
- 属性操作：`hasattr()`、`setattr()`、`delattr()`等（只对对象生效）
- 抽象类，需要`abc`模块的抽象基类
```python
    @abstructmethod # 抽象类函数
    def abstructfunction(self)
        print('this is a abstruct function')
```

继承： 

```python
class classname(parentclass):
    ...
```  

- `super().function()`调用父类函数
- 优先查找子类方法，找不到再在父类中查找

## 文件操作

比较简单，[菜鸟教程](https://www.runoob.com/python/file-methods.html)

## 图形界面

默认GUI开发模块：`tkinter` 

```python
import tkinter
def command():
    print('hello')
top = tkinter.Tk() # 创建窗口
top.geometry('1024x512') # 设置窗口大小
top.title('gui') # 设置窗口标题
panel = tkinter.Frame(top)  # 创建panel
button = tkinter.Button(panel, text='button', command=command) # 创建按钮
button.pack(side='left') # 设置按钮位置
panel.pack(side='bottom') # 设置panel位置
tkinter.mainloop() # 开启事件循环
``` 

`tkinter`功能并不强大，gui开发有许多其他module可供使用

## 正则表达式

[菜鸟教程](https://www.runoob.com/regexp/regexp-syntax.html) 

使用`re`模块 

`flag`

- re.I：忽略大小写
- re.M：多行模式
- ...
- 按位叠加，如`re.I | re.M`

函数

- `pattern = re.compile(string, flag)`，生成正则表达式对象（`RegexObject`），其中`string`常用原始字符串（形如`r'string'`）来避免转义字符带来的麻烦
- `re.match(pattern, string, flag=0)`，匹配字符与正则表达式，失败返回`None`，成功返回一个特定的匹配对象
- `re.search(pattern, string, flag=0)`，查找并返回字符串中与正则表达式的第一处成功匹配
- `re.split(pattern, string, maxsplit=0, flag=0)`拆分长字符串
- ...

## 多线程

使用`threading`模块

```python
import threading
def thread_func(para1, para2):
    print 'this is thread function with input' + str(para1) + str(para2)
t = threading.Thread(target=thread_func, args=('1', 2)) # Thread类
t.start() # 开启线程
...
t.join() # 线程t回到主程序后再运行后面的代码
...
print('the end of the main program')
```
## 数据交换

- json

```python
import json
json.dumps()
json.loads()
``` 

- xml

## 网络编程

模块

- `socket`
- `urllib`
- `requests`

### urllib模块的使用 

[九种HTTP请求方法](https://www.runoob.com/http/http-methods.html)

`request.urlopen`的基本使用 

```python
from urllib import request

# ---使用请求方法：GET---
resp = request.urlopen('http://www.baidu.com') # 返回HTTPResponse类的数据
content = resp.read()
print(resp.status) # 成功响应状态码：200
print(resp.headers) # 数据头部
print(content.decode('utf-8')) # read()返回二进制数据（bytes类型），需解码为字符串

# 下载图片1
ikun = request.urlopen('https://i0.hdslb.com/bfs/archive/90f39c20bd64341d22023598d7f44491a01c193f.jpg')
f = open('realman.jpg', 'wb')
f.write(ikun.read())
f.close()
# 下载图片2
request.urlretrieve('https://www.somode.com/uploadimg/image/20230129/20230129150835_59024.jpg', 'little_black_spot.jpg')
``` 

\*关于url 

```python
# https://          协议
# search.bilibili.com          域名
# /all          路径
# ?keyword=%E5%B0%8F%E9%BB%91%E5%AD%90&search_source=1          参数
``` 

\*关于字符编码 

```python
# urllib中的url按ascii码解析
# 对于中文字符则需先获得中文字符的Unicode码，再按ascii解析Unicode码
from urllib import parse
print(parse.quote('小黑子'))
# 打印结果：%E5%B0%8F%E9%BB%91%E5%AD%90

# 如果需要url参数形式则有更方便的编码转换方法urlencode
data = {
    'keyword': '小黑子',
    'search_source': '1'
}
print(parse.urlencode(data))
# 打印结果：keyword=%E5%B0%8F%E9%BB%91%E5%AD%90&search_source=1
``` 

请求对象定制 

```python
from urllib import request
# 请求头，提供系统与浏览器信息，让对方服务器认为这是用户通过浏览器发起的请求
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/538.37 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/538.37 Edg/116.0.1902.200',
    'Cookie': 'buvid3=05102CC6-6AA1-43CD-AFB1-56B2D3028ABD167644infoc; LIVE_BUVID=AUTO4316367178967911; i-wanna-go-back=-1; blackside_state=0; buvid4=2782EEE5-D4E7-5021-A3FC-4138F394BC7700399-022012715-9TO+hWhxQHMv66RhJsMsnXdTm5lM0AjVd9K0RePMX6Nru+jhqr5A/A%3D%3D; CURRENT_BLACKGAP=0; buvid_fp_plain=undefined; DedeUserID=1431764197; DedeUserID__ckMd5=8ed918b233cf061a; is-2022-channel=1; Hm_lvt_d7c703709393831235e160fc28becc542=1663443604; _uuid=107FF2BC4-1648-5992-9D9B-C6B5382FAEC912726infoc; b_nut=100; hit-new-style-dyn=0; rpdid=|(JJmY)YRuuJ0J\'uYY)Yu||YJ; b_ut=5; header_theme_version=CLOSE; nostalgia_conf=-1; CURRENT_FNVAL=4042; CURRENT_PID=34a026d0-cd1d-223e-b4a6-e96ffa252a74; SESSDATA=we9df72e%2C1698665309%2C4962d%2A52; bili_jct=23b9579ca4268b9c54e7697sdf6fcfe8; FEED_LIVE_VERSION=V8; home_feed_column=4; fingerprint=0fd268dec0325ede634f525a664ac159; bili_ticket=eyJhbGciOiJFUzM4NCIsImtpED4Rq6ImVjMDIiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2OTIwMTQSEWwlhdCI6MTY5MTc1NTQ5MSwicGx0IjotMX0.KKJe3BvASOQLHi0Zy3r94p9dffsr-NckBsxn0VrCEZQbMqDkPEOHujf-SuOah64ONlhWSnLe8rzyvVgd4nH9PQ6Tq8H7Aq_LZhq27pGX3IDKufHGvdlB50QQ6batRqt5; bili_ticket_expires=1692334691; CURRENT_QUALITY=50; PVID=1; buvid_fp=26801ecd7d9b2b725d06b5d8d562abd1; bp_video_offset_1431764197=829189912341258848; innersign=0; browser_resolution=828-803; sid=4rtbd9kl; b_lsid=D979768D_189EDA76233'
} 

# ---使用请求方法：GET---
requ = request.Request(url='http://www.bilibili.com',headers=headers) # 生成请求类，函数Request(url, data=None, headers={}, ...)
resp = request.urlopen(requ) # 发送请求
content = resp.read()
print(resp.status)
print(resp.headers)
f = open('bilibili.html', 'wb')
f.write(content)
f.close()

# ---使用请求方法：POST---
# （即附加了数据）
data = {
    'keyword': '小黑子',
    'search_source': '1'
}
data = parse.urlencode(data).encode('utf-8') # 这里需由str编码为byte
requ = request.Request('', data, headers)
resp = request.urlopen(requ)
``` 

### requests模块的使用

```python
# requests

import requests
resp = requests.get('https://i0.hdslb.com/bfs/archive/90f39c20bd64341d22023598d7f44491a01c193f.jpg')
print(resp.status_code) # 成功响应状态码：200
f = open('real_man.jpg', 'wb') # 打开/创建只写二进制文件
f.write(resp.content) # content为二进制内容（unicode可用resp.text）
f.close()
```

## 浏览器操作

使用`selenium`模块

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
driver = webdriver.Chrome()
driver.get('http://www.bilibili.com')
driver.find_element(By.CLASS_NAME, 'nav-search-input').send_keys('只因你太美')
driver.find_element(By.CLASS_NAME, 'nav-search-btn').click()
```