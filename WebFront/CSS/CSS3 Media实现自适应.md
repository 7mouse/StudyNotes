# 利用@media 实现网页布局自适应

## 基础知识

- meta 标签viewport属性

  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

  - width = device-width：宽度等于当前设备的宽度
  - height = device-height：高度等于当前设备的高度
  - initial-scale：初始的缩放比例（默认设置为1.0）  
  - minimum-scale：允许用户缩放到的最小比例（默认设置为1.0）  
  - maximum-scale：允许用户缩放到的最大比例（默认设置为1.0）  
  - user-scalable：用户是否可以手动缩放（默认设置为no，因为我们不希望用户放大缩小页面） 

- ### IE8兼容

  因为IE8既不支持HTML5也不支持CSS3 Media，所以我们需要加载两个JS文件，来保证我们的代码实现兼容效果

  <!--[if lt IE 9]>

   <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>

   <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>

  <![endif]-->

  

- 现在有很多人的IE浏览器都升级到IE9以上了，所以这个时候就有又很多诡异的事情发生了，例如现在是IE9的浏览器，但是浏览器的文档模式却是IE8:

  为了防止这种情况，我们需要下面这段代码来让IE的文档模式永远都是最新的：

  1. <meta http-equiv="X-UA-Compatible" content="IE=edge">

   （如果想使用固定的IE版本，可写成：<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE9">）

   

  不过我最近又发现了一个更给力的写法：

  1. <meta http-equiv="X-UA-Compatible" content="IE=Edge，chrome=1">

  怎么这段代码后面加了一个chrome=1，这个[Google Chrome Frame（谷歌内嵌浏览器框架GCF）](http://zh.wikipedia.org/wiki/Google_Chrome_Frame)，如果有的用户电脑里面装了这个chrome的插件，就可以让电脑里面的IE不管是哪个版本的都可以使用Webkit引擎及V8引擎进行排版及运算，无比给力，不过如果用户没装这个插件，那这段代码就会让IE以最高的文档模式展现效果。这段代码我还是建议你们用上，不过不用也是可以的



- 进入CSS3 Media写法

    我们先来看下下面这段代码，估计很多人在响应式的网站CSS很经常看到类似下面的这段代码：

    1. @media screen and (max-width: 960px){
    2.   body{
    3. ​    background: #000;
    4.   }
    5. }

    这个应该算是一个media的一个标准写法，上面这段CSS代码意思是：当页面小于960px的时候执行它下面的CSS.这个应该没有太大疑问。

- CSS2 Media用法

    其实并不是只有CSS3才支持Media的用法，早在CSS2开始就已经支持Media，具体用法，就是在HTML页面的head标签中插入如下的一段代码：

    <link rel="stylesheet" type="text/css" media="screen" href="style.css">

    上面其实是CSS2实现的衬线用法，那CSS2的media难道就只能支持上面这一个功能吗？答案当然不是，他还有很多用法。


	例如我们想知道现在的移动设备是不是纵向放置的显示屏，可以这样写：
	<link rel="stylesheet" type="text/css" media="screen and (orientation:portrait)" href="style.css">
	我们把第一段的代码也用CSS2来实现，让它一样可以让页面宽度小于960的执行指定的样式文件：
	<link rel="stylesheet" type="text/css" media="screen and (max-width:960px)" href="style.css"> 
	既然CSS2可以实现CSS的这个效果为什么不用这个方法呢，很多人应该会问，但是上面这个方法，最大的弊端是他会增加页面http的请求次数，增加了页面负担，我们用CSS3把样式都写在一个文件里面才是最佳的方法。


## @media 查询属性



- 屏幕方向

  ```css
  /* 竖屏 */
  
  @media screen and (orientation: portrait) and (max-width: 720px) { 对应样式 }
  
  /* 横屏 */
  
  @media screen and (orientation: landscape) { 对应样式 }
  ```

- 屏幕大小限制

  min-width 最小屏幕 => 大于

  max-width 最大屏幕 => 小于

  

  @media (min-width: 1200){ //>=1200的设备 }

  @media (min-width: 992px){ //>=992的设备 }

  @media (min-width: 768px){ //>=768的设备 }

  因为如果是1440,由于1440>768那么你的1200就会失效。

  **所以我们用min-width时，小的放上面大的在下面，同理如果是用max-width那么就是大的在上面，小的在下面**

  @media (max-width: 1199){ //<=1199的设备 }

  @media (max-width: 991px){ //<=991的设备 }

  @media (max-width: 767px){ //<=768的设备 }



