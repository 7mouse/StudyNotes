![img](https://www.wangbase.com/blogimg/asset/202008/bg2020080719.jpg)

# 空中布局

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080703.jpg)

使用grid布局 place-items 定位: <align-items> <justify-content> 分别控制垂直和水平方向(主轴和交叉轴)



# 并列式布局

并列式布局就是多个项目并列。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080706.jpg)

如果宽度不够，放不下的项目就自动折行。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080707.jpg)

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080708.jpg)

它的实现也很简单。首先，容器设置成 Flex 布局，内容居中（`justify-content`）可换行（`flex-wrap`）。

```css
.container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}
```

然后，项目上面只用一行`flex`属性就够了

```css
.item{
   flex: 0 1 150px;
   margin: 5px;
}
```

`flex`属性是`flex-grow`、`flex-shrink`、`flex-basis`这三个属性的简写形式。

```css
flex: <flex-grow> <flex-shrink> <flex-basis>;
```

- `flex-basis`：项目的初始宽度。
- `flex-grow`：指定如果有多余宽度，项目是否可以扩大。
- `flex-shrink`：指定如果宽度不足，项目是否可以缩小。

`flex: 0 1 150px;`的意思就是，项目的初始宽度是150px，且不可以扩大，但是当容器宽度不足150px时，项目可以缩小。

如果写成`flex: 1 1 150px;`，就表示项目始终会占满所有宽度。

# 双栏式布局

两栏式布局就是一个边栏，一个主栏。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080712.jpg)

下面的实现是，边栏始终存在，主栏根据设备宽度，变宽或者变窄。如果希望主栏自动换到下一行，可以参考上面的"并列式布局"。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080714.jpg)

使用 Grid，实现很容易（[CodePen 示例](https://codepen.io/una/pen/gOaNeWL)）。

> ```css
> .container {
>     display: grid;
>     grid-template-columns: minmax(150px, 25%) 1fr;
> }
> ```

上面代码中，`grid-template-columns`指定页面分成两列。第一列的宽度是`minmax(150px, 25%)`，即最小宽度为`150px`，最大宽度为总宽度的25%；第二列为`1fr`，即所有剩余宽度。

同等的还有min-content / max-content

# 三明治布局

三明治布局指的是，页面在垂直方向上，分成三部分：页眉、内容区、页脚。
![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080715.jpg)

这个布局会根据设备宽度，自动适应，并且不管内容区有多少内容，页脚始终在容器底部（粘性页脚）。也就是说，这个布局总是会占满整个页面高度。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080716.jpg)

```css
.container {
    display: grid;
    grid-template-rows: auto 1fr auto;
}
```

上面代码写在容器上面，指定采用 Grid 布局。核心代码是`grid-template-rows`那一行，指定垂直高度怎么划分，这里是从上到下分成三部分。第一部分（页眉）和第三部分（页脚）的高度都为`auto`，即本来的内容高度；第二部分（内容区）的高度为`1fr`，即剩余的所有高度，这可以保证页脚始终在容器的底部。

# 圣杯布局

圣杯布局是最常用的布局，所以被比喻为圣杯。它将页面分成五个部分，除了页眉和页脚，内容区分成左边栏、主栏、右边栏。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080717.jpg)

这里的实现是，不管页面宽度，内容区始终分成三栏。如果宽度太窄，主栏和右边栏会看不到。如果想将这三栏改成小屏幕自动堆叠，可以参考并列式布局。

![img](F:\Notes\WebFront\CSS\flex和grid实现五大布局.assets\bg2020080718.jpg)

HTML 代码如下。

> ```markup
> <div class="container">
>     <header/>
>     <div/>
>     <main/>
>     <div/>
>     <footer/>
> </div>
> ```

CSS 代码如下（[CodePen 示例](https://codepen.io/una/pen/mdVbdBy)）。

> ```css
> .container {
>     display: grid;
>     grid-template: auto 1fr auto / auto 1fr auto;
> }
> ```

上面代码要写在容器上面，指定采用 Grid 布局。核心代码是`grid-template`属性那一行，它是两个属性`grid-template-rows`（垂直方向）和`grid-template-columns`（水平方向）的简写形式。

> ```css
> grid-template: <grid-template-rows> / <grid-template-columns>
> ```

`grid-template-rows`和`grid-template-columns`都是`auto 1fr auto`，就表示页面在垂直方向和水平方向上，都分成三个部分。第一部分（页眉和左边栏）和第三部分（页脚和右边栏）都是本来的内容高度（或宽度），第二部分（内容区和主栏）占满剩余的高度（或宽度）。

## 六、参考链接

- [Ten modern layouts in one line of CSS](https://web.dev/one-line-layouts/), Una Kravets
- [Flex 布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
- [Grid 布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
- [grid-template 属性](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-template), MDN