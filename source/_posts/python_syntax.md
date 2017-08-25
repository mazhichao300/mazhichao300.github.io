---
layout: post
title:  Python2的一些小语法
date:   2017-08-25 15:13:28 +0800
categories: [Python]
tags: [Python]
---


### Python保留N位小数点
```python
a = 1
b = 3
n = 2

# 默认两个Int除，结果也是int，需要转为float

# 方法一：用round
round(float(a) / b, n)

# 方法二：用format
format(float(a) / b, '.2f')

# 方法三：用 % 格式化
'%.2f' %(float(a) / b)

#方法三扩展，展现百分比， %%表示输出%
'%.2f%%' %(100.0 * a / b)
```

### 多线程
`threading`

```python
import threading

def func1(name):
    print name

def func2(name):
    print "hello, %s" % name

threads = []
t1 = threading.Thread(target=func1, args=('ma'))
t2 = threading.Thread(target=func2, args=('world'))
threads.append(t1)
threads.append(t2)

for t in threads:
    t.setDaemon(True)
    t.start()

```

### 判断变量类型
`isinstance(x, type)`

- 判断int： isinstance(x,int)
- 判断string: isinstance(s, basestring) or isinstance(s, (str, unicode))

### urllib2网络请求
```python
# GET请求
import urllib,urllib2 

uri = 'https://www.baidu.com/'  

ret = urllib2.urlopen(uri).read()

#POST请求
url = 'http://www.someserver.com/cgi-bin/register.cgi'   
  
values = {'name' : 'Michael Foord',   
          'location' : 'Northampton',   
          'language' : 'Python' }   
  
data = urllib.urlencode(values)   # 具体data格式视服务端而定，比如json，data = json.dumps(values)
  
req = urllib2.Request(url, data)   
  
response = urllib2.urlopen(req)   
  
the_page = response.read()   

```

**为urlopen设置可选参数:** `timeout`

```python
urllib2.urlopen(r, data=None, timeout=3)
```

### 模块路径设置
- 代码中动态添加： `sys.path.append('/home/xxx/xxx')`
- 设置环境变量`PYTHONPATH`

### JSON处理
```
json.dumps()

json.loads()
```
[详情参考](http://www.runoob.com/python/python-json.html)

### 时间处理
常用的时间戳： `int(time.time())`
[更多用法可参考](http://www.cnblogs.com/snow-backup/p/5063665.html)

### python函数是引用还是传值
python不允许程序员选择采用传值还是传引用。Python参数传递采用的肯定是“传对象引用”的方式。这种方式相当于传值和传引用的一种综合。如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值－－相当于通过“传引用”来传递对象。如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用，就不能直接修改原始对象－－相当于通过“传值'来传递对象。

[python函数传参是传值还是传引用？](http://www.cnblogs.com/loleina/p/5276918.html)

### 连接mysql
```python
config = {
    'host': '127.0.0.1',
    'port': 3306,
    'user': 'root',
    'passwd': 'root',
    'db': 'test',
    'charset': 'utf8'
}
conn = mdb.connect(**config)

# 如果使用事务引擎，可以设置自动提交事务，或者在每次操作完成后手动提交事务conn.commit()
conn.autocommit(1)    # conn.autocommit(True) 

# 使用cursor()方法获取操作游标
cursor = conn.cursor()
cursor.execute(sql)

# 关闭游标连接
cursor.close()
# 关闭数据库连接
conn.close()
```

[Python连接MySQL数据库](http://www.cnblogs.com/conanwang/p/6028110.html)

### python遍历dict
```python

dict.items()  # 获取(k, v)数组

dict.keys()   #获取key数组

dict.values() # 获取value数组

for i in dict:
    key = i
    value = dict[key]

for (k, v) int dict.items():
    key = k
    value = v
```

### 连接MongoDB
先安装`pymongo`模块, [pymongo官方文档](http://api.mongodb.com/python/current/tutorial.html)

```python
from pymongo import MongoClient

client = MongoClient("10.85.139.145", 8088)
db = client.test
collection = db.user

res = collection.find_one({"name":"python"})
```

### AttributeError: ‘module’ object has no attribute’xxx’
本来import自己写的一个脚本模块，正常运行；但是在模块中加了一个函数后，再运行就报错如标题。
排查发现是`.pyc`文件导致，删除后正常。

.pyc文件是由.py文件经过编译后生成的字节码文件，其加载速度相对于之前的.py文件有所提高，而且还可以实现源码隐藏，以及一定程度上的反编译。可能pyc有时更新不即时，导致错误。

[When are .pyc files refreshed?](https://stackoverflow.com/questions/15839555/when-are-pyc-files-refreshed)


----------------

## 参考
- [%格式化和format格式化](http://blog.csdn.net/wo_renfanzi/article/details/51477796)
- [python 多线程就这么简单](http://www.cnblogs.com/fnng/p/3670789.html)
- [How to check if type of a variable is string?](https://stackoverflow.com/questions/4843173/how-to-check-if-type-of-a-variable-is-string)
- [python通过get方式,post方式发送http请求和接收http响应-urllib urllib2](http://blog.csdn.net/mack415858775/article/details/39696107)


