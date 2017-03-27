---
title: 使用markdown书写PPT
date: 2017-03-27 17:24:36
categories: [工具]
tags: [markdown,ppt]
---

>使用[LandSlide](https://github.com/adamzap/landslide)将MarkDown生成为HTML文件，用来分享。


## LandSlide安装
### 使用`pip`安装
```shell
//先安装pip，如果已经安装，跳过此步
$ sudo easy_install pip

//安装landslide
$ pip install landslide
```

### 源码安装
```shell
$ git clone https://github.com/adamzap/landslide.git
$ cd landslide
$ python setup.py build
$ sudo python setup.py install
```

## 编写md文件
官方测试用例

```markdown
# Landslide

 ---

 # Overview

 Generate HTML5 slideshows from markdown, ReST, or textile.

 ![](http://i.imgur.com/bc2xk.png){ImgCap}python{/ImgCap}

 Landslide is primarily written in Python, but it's themes use:

 - HTML5
 - Javascript
 - CSS

 ---

 # Code Sample

 Landslide supports code snippets

     !python
     def log(self, message, level='notice'):
         if self.logger and not callable(self.logger):
             raise ValueError(u"Invalid logger set, must be a callable")

         if self.verbose and self.logger:
             self.logger(message, level)
```

**简单说明：**

- `---`用来表示当前页面结束
- 代码段只支持`tab`符号方式

## 生成HTML文件
`$ landslide test.md -i -o > test.html`

## 快捷键
```
 h:        展示帮助
 ← →:      上/下一张幻灯片
 ESC:      展示目录
 n:        显示当前是第几张幻灯片
 b:        屏幕全黑
 e:        使当前幻灯片最大化
 3:        展示伪3D效果
 c:        取消显示前后幻灯片预览，只显示当前幻灯片
```

---
## 参考链接
- [用Markdown写一个极客范儿的PPT](http://www.jianshu.com/p/e063303317cb)
- [官网](https://github.com/adamzap/landslide)
