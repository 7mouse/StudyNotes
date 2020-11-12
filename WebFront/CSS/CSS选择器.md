# CSS 选择器

## 类选择器

.className {}

## id 选择器

\#idName {}

## 元素选择器

tagName {}

## 统配选择器

\* {}

## 后代选择器

tagName childTagName {}

## 子选择器

e1 > e2 {} 仅第一后代, 即子代

## 属性选择器

- [foo|="bar"] 选择foo属性值以"bar-"     (U+002D)开头 或者 "bar"的元素
- [foo~="bar"]     包含"bar"的一组词 (词句)
- [foo*="bar"]     包含字串"bar"
- [foo^="bar"]     "bar"开头
- [foo$="bar"]     "bar"结尾
- [foo$=".PDF"     i] **不区分大小写**

## 兄弟选择器

a +b {} 紧接着a的b

## 全兄弟选择器

a~b{} a后的所有同级b

## 交集选择器

.a.b {}

## 并集选择器

div, p {}

## 伪类选择器

### 动态伪类

LVF HA => love hate

- a:link

- a:visited
- a:focus

- a:hover

- a:active

### 目标伪类

- :target

  a href="#title"

  :target {color:red;}

  点击a, #title变色

- :empty 空元素

  包括置换元素img, input等

- a:root 根元素 => HTML
- :only-child 作为唯一子元素的节点

- a:first-child

- :only-of-type :first-of-type :last-of-type

- :nth-child(n) :nth-last-child

  n从0开始, 但是HTML从1开始算

  odd奇数 even偶数

- :nth-of-type :nth-last-of-type

### 状态伪类

- :enabled :disabled 接受和不接受输入

- :checked 选中的

- :indeterminate 选中但未选的, 有JS DOM

- :default 默认选中的

- :valid 有效的 :invalid 无效的

- :in-range :out-of-range

  input type="number" min="0" max="1000"

  step="10" value="23"

- :required 必须输入的 :optional可选的

- :read-write 可写 :read-only 不能由用户编辑的

### 否定伪类

- :not()

### 其他伪类

- :lang 伪类

## 伪元素选择器

### 文本选择

- ::first-letter

  字体属性, 背景, 文本修饰, 行内排布, 行内布局, 边框, box-shadow, color, opacity

- ::first-line

  字体, 背景, 内外边距, 边框, 文本修饰, 行内排版, color, opacity

### 前置和后置内容元素

- ::after

- ::before

  <img src="F:\Notes\WebFront\CSS\CSS选择器.assets\clip_image001.png" alt="计算机生成了可选文字: 1 2 3 4 5 5 7 8 9 content content content content content content content content content content content content none pr、efix url(http:／/““．example．com/test.html) chapter、counter attr、(valuestring) open-quote close-quote no-open-quote no—close—quote open-quotechaptercounter inherit" style="zoom:75%;" />

  content时必要属性, 可以attr引用元素属性, 并转化为string

  可以用来制作计数器(ol)

  <img src="F:\Notes\WebFront\CSS\CSS选择器.assets\clip_image001-1605187203433.png" alt="计算机生成了可选文字: CSS 2 4 5 6 8 9 14 body{ COUnter-到 hl{ COUnter-到 h短：befO「e{ sectlOn; content： &quot;Sectlon h2：:befo「e{ COI_ante「(section) content：counter(section) 见一下效果 Section1．HTML教程： 1．1HTML教程 1．2CSS教程 Section2．Scripting教程： 2．1JavaScript 2．2VBScript counter(subsection)" style="zoom:75%;" />

  