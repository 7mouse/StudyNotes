# JSX 语法

## 渲染 JSX

```
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```

这种模板语法在React中被称为JSX, 以上将h1标签渲染至 #example 中

这种js代码必须放置在` <script type="text/babel">`中, 以此来标注JSX代码, 并且必须包含babel.min.js文件进行编译

0.14版本之前使用 text/jsx, JSTransform.js运行, 已经被废弃

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <script src="../build/react.development.js"></script>
    <script src="../build/react-dom.development.js"></script>
    <script src="../build/babel.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">

      // ** Our code goes here! **

    </script>
  </body>
</html>
```

## 使用JS 在JSX中

JSX中 < 开头表示 HTML 语法, { 表示 JS 语法的开头

```
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

## JSX 数组使用

使用JSX数组

```
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

## render 函数 中的JSX

```
// correct
class HelloMessage extends React.Component {
  render() {
    return <div>
      <h1>Hello {this.props.name}</h1>
      <p>some text</p>
    </div>;
  }
}
```

return 只能返回一个标签, 需要用div 或者 Fragment



## this.props.children 访问一个组件的子节点

```
class NotesList extends React.Component {
  render() {
    return (
      <ol>
      {
        React.Children.map(this.props.children, function (child) {
          return <li>{child}</li>;
        })
      }
      </ol>
    );
  }
}

ReactDOM.render(
  <NotesList>
    <span>hello</span>
    <span>world</span>
  </NotesList>,
  document.getElementById('example')
);
```



this.props.children 不同的值

无子节点: undefined

一个节点: object

多个节点: array



我们可以用 `React.Children.map` 来遍历子节点，而不用担心 `this.props.children` 的数据类型是 `undefined` 还是 `object`。

`React.Children.forEach` 类似于 React.Children.map()，但是不返回对象。

`React.Children.count `返回 children 当中的组件总数，和传递给 map 或者 forEach 的回调函数的调用次数一致。

`React.Children.only` 返回 `children` 中 **仅有的子级**。否则抛出异常。

这里**仅有的子级**，指的是, 参数只能是一个对象，不能是多个对象（数组）~