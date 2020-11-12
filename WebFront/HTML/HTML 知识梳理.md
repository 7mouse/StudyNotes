# 基础知识

## 标签基础

### 行内元素(内联元素) / 块级元素

| 块级元素                         | 行内元素                                              |
| -------------------------------- | ----------------------------------------------------- |
| 元素占据一行, 默认100%父元素宽度 | 元素紧挨着(div内空格会留白), 挤满换行, 宽度为内容宽度 |
| 可以设置高度, 宽度, 内外边距     | 不能设置宽高, 上下外边距                              |
|                                  |                                                       |

### 常见标签

- 常见块级元素 

  div、p、h1~h6、ul、li、ol、dd、dt、dl、table、hr、blockquote、address、pre、menu

  HTML5中新增的：header、footer、aside、section

  ![img](F:\Notes\WebFront\HTML\HTML 知识梳理.assets\20180811205647487)

- 常见行内元素 

  span、img、input、a、label、button、select、textarea、sup、sub、abbr、s、i、em、u、strong、small

  ![img](F:\Notes\WebFront\HTML\HTML 知识梳理.assets\20180811210812645)

  **注意! <input>和<img>都是行内元素，但是它们是可以设置宽和高的。这里就涉及到可替换元素和不可替换元素。 **

### 可替换元素 / 不可替换元素

- 替换元素

  就是浏览器根据元素的标签和属性，来决定元素的具体显示内容。

   例如浏览器会根据<img>标签的src属性的值来读取图片信息并显示出来，而如果查看html代码，则看不到图片的实际内容；又例如根据<input>标签的type属性来决定是显示输入框，还是单选按钮等。

  

   html中的<img>、<input>、<textarea>、<select>、<object>都是替换元素。这些元素往往没有实际的内容，即是一个空元素，例如： `<img src="cat.jpg" />`, `<input type="submit" name="submit" value="提交" /> `浏览器会根据元素的标签类型和属性来显示这些元素。可替换元素也在其显示中生成了框。 

- 不可替换元素

  html 的大多数元素是不可替换元素，即其内容直接表现给用户端（例如浏览器）。



### meta标签 => 字集优化, 搜索引擎优化, 手机自适应

```HTML
<meta charset="UTF-8">
   	采用utf-8编码(推荐), 规定使用的字符集(解决乱码)
    GBK 新华字典 gb2312
<meta name="keyword" content="后盾人,后盾人教程,php"/>
    设置关键字, 在搜索时, 方便获得更高的搜索排名
<meta name="descripition" content="后盾人专注网站开发 1828231646" />
    设置网页描述, 搜索引擎中展示时的介绍, 建议敲上电话

```
## 图片使用

- 图片差异

  图片的压缩很重要, 大图片的重复利用会导致带宽下降

  GIF 表示有限, 有透明背景, 大, 颜色范围小

  JPEG 尺寸压缩, 颜色范围广, 有损压缩, 不支持背景透明

  PNG-8 PNG-24 颜色范围不同, 尺寸大, 有噪点, 颜色范围广, 无损压缩, 支持背景透明

  **大照片应该转化为jpeg , 可以优化带宽**

- 加载不同

  JPEG至上而下加载, PNG很快, 只需要下载1/64就可以显示模糊的缩略图

- 建议静态文件都存储到云服务器 或者使用cdn 或者使用icon等, 加快带宽

## 文件后缀名

文件后缀 早期计算机文件后缀只能有三位

所以html htm jpeg jpg 都是合法后缀名



## 表单

form: 

​	action 发送支持 / method 发送的方式

fieldset>legend 设置表单 外框+标题

input: name表示键名 value是键值 发送表单必须设置name, 多个选项可以name为同一个

​	readonly 只能提交 不能改

​	disable 不能发送, 只能看

​	patten 正则验证

label for => id 点击label 聚焦id所在元素

select name > option value

optgroup label => option



## 一些细节

- 标签属性

```html
<html lang="en"> 
    简体中文 zh-CN
	表示语言, 可以不设置, 不影响显示
<title> 设置标题 </title>
    如果没有标题, 则为文件名, 则引擎搜索的时候, 会以权限搞得标签, 比如h1作为标题

```

修饰字符, 下标sub 上标sup 删除线del 下划线ins 不准确中划线 s

body > header / footer / main / aslide

article 主要内容 > section * n 模块

p 段落, 删除内容前后空格

pre 显示源代码, 保留格式

blockquote 标记引用

br 换行 hr 下划线



# 标签语义化

### 语义标签

语义是指对一个词或者句子含义的正确解释。很多html标签也具有语义的意义，也就是说元素本身传达了关于标签所包含内容类型的一些信息。

例如，当浏览器解析到<h1></h1>标签时，它将该标签解释为包含这一块内容的最重要的标题。

h1标签的语义就是用它来标识特定网页或部分最重要的标题。

![image-20201109165125022](F:\Notes\WebFront\HTML\HTML 知识梳理.assets\image-20201109165125022.png)

![• header  • hi  h2  h3  nav  • footer  • article  • section  •  •  o/  • blockquote  • strong  • em  abbr  •  • small ](F:\Notes\WebFront\HTML\HTML 知识梳理.assets\clip_image001.png)

### 为什么要语义化

- 代码结构: 使页面没有css的情况下，也能够呈现出很好的内容结构
- 有利于SEO:     爬虫依赖标签来确定关键字的权重，因此可以和搜索引擎建立良好的沟通，帮助爬虫抓取更多的有效信息
- 提升用户体验：     例如title、alt可以用于解释名称或者解释图片信息，以及label标签的灵活运用。
- 便于团队开发和维护: 语义化使得网站代码分块, 更具有可读性，让其他开发人员更加理解你的html结构，减少差异化。
- 方便其他设备解析:     如屏幕阅读器、盲人阅读器、移动设备等，以有意义的方式来渲染网页。

### 为什么不使用语义化

- 规范不明确, 存在废除风险, 难以向下兼容

- p 里面放 div 会导致渲染问题，而且非常隐蔽

  p > article section

- 在不需要兼容老 IE     的情况下我都会用语义化标签，语义化标签对于屏幕阅读器还是比较重要的。在需要兼容老 IE 的情况下，我一般用 role 属性来代替语义化标签。
- 存在框架可以更改 Vue=> Material组件框架Vuestify

### 常用的语义化

-  SEO Search Engine Optimization 搜索引擎优化

  促进关键词排名=> h1标签, 尽量只有一个 不能过多



# HTML新标签

新特性:

- 新元素
- 新属性
- 完全支持 CSS3
- Video 和 Audio
- 2D/3D 制图
- 本地存储
- 本地 SQL 数据
- Web 应用

## 进度条progress

```html
<progress value="22" max="100"></progress> 
```

progress DOM对象有三个属性, value, labels(进度条标记列表), max, position 当前位置



## 表单新元素

- form 属性

  input list="datalistName" + datalist name="datalistName" > option value

  keygen: 标签规定用于表单的密钥对生成器字段。当提交表单时，私钥存储在本地，公钥发送到服务器。

  output

- 输入框Input Type

  HTML type

  ​	submit, button, text, password, checkbox, hidden, image, radio, reset

  HTML5 新type

  - color
  - date
  - datetime
  - datetime-local
  - email
  - month
  - number
  - range
  - search
  - tel
  - time
  - url
  - week

  

## 多媒体

video, audio

# 参考文档

## html中内联元素和块级元素的区别（超级详细）https://blog.csdn.net/xuanfuhuo4769/article/details/81326457