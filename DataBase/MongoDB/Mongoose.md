# Mongoose 基础总结

## 1. 基础知识

- 基础概念

  document 基本存储单位=> record

  collection 存储document的集合 => table

  schema document模板 => 字段名集合

  model 通过schema 生成模板, 通过模板进行增删改查

  

- ### 安装配置

  `npm install mongoose --save`

  

- ### 引入模块

  `const mongoose = require('mongoose');`

  `mongoose.connect('mongodb://localhost/test');`

  如果由用户名和密码:

  `mongoose.connect('mongodb://username:password@localhost:port/dbname')`

  

- ### 定义Schema

  MongoDB中的Schema类似SQL中数据库字段, 映射到MongoDB的一个Collection, 可以理解为一种表的格式。
  
  ```javascript
  let UserSchema = mongoose.Schema({
      name:String,
      age:Number,
      status:Number
  })
  ```
  
  以上就定义了一个名为UserSchema的Schema数据类型, 但是有类型还不够, 还需要根据Schema创建Model实例
  
  
  
- ### 定义和创建数据模型Model

  **Model本质上是Schema编译过来的构造函数**

  mongoose接受1~2个参数
  
  ```javascript
  // model里的第一个参数modelName: 首字母大写, 和Collection名称对应的 单数! 名称
  // 模型会与模型名称相同的多个数据库集合建立连接
let User = mongoose.model('User', UserSchema);
  ```

  接下来可以用Model创建实例进行增删改查了。
  
  **注意, mongoDB会根据model名自动创建collection, 单数名词变复数, 字母变小写, 有些词的复数会被同义替换(peason, people)**
  
  
  

## 2. 增删改查

此节仅作简单介绍

- ### 增加元素

  创建Document

  在mongoDB中, document类似SQL中的一行数据(记录), 我们使用刚生成的model来创建document

  ```javascript
  let user = new User({
  	name: 'Mouse',
  	age: 99,
  	state : 1
  });
  ```

  现在我们拥有了一个文档

  ```javascript
  user.save(function(err) {
      if (err) {
          return console.error(err);
      }
      console.log('saved');
  })
  ```

  mongoDB容错性很高, 如果键名错误或者忽略, 则忽略

  还可以这么创建

  ```javascript
  User.create({name:'mouse', age:11}, function(err, user){
      if (err) return handleError(err);
      console.log('saved');
  })
  ```

  

- ### 查找元素:

  `User.find({xxx}).where().gt().exec(callback)`
  
  ```javascript
  User.find({}, function(err, doc) {
      if (err) {
          console.log(err);
      }
      console.log(doc)
  });
  ```
  
- ### 修改数据

  User.updateOne({}, function(){

  

  })

  

- ### 删除数据

  删除所有匹配的

  User.remove({xxx}, function(err) {

  ​	
  
  })
  
  删除第一个
  
  User.deleteOne({参数}, ..., function(err, result){
  
  ​	
  
  })

# 设计模式

用户信息存储 样例 https://www.cnblogs.com/gdragon/p/11994966.html

100万用户 模型设计 性能分析https://blog.csdn.net/ywh147/article/details/8916307

# SchemaType 模式类型

## 1. 类型

- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array
- Decimal128

## 2. SchemaType选项

使用预定义修饰符对schema数据类型进行一定的格式化操作

```javascript
let UserSchema = mongoose.Schema({
    name: {
        type: String,
        trim: true
    },
    age: Number,
    status: {
        type: Number,
        default: 1
    }
})
```

所有类型可用的选项:

- `required`: 布尔值或函数 如果值为真，为此属性添加 [required 验证器](http://www.mongoosejs.net/docs/validation.html#built-in-validators)
- `default`: 任何值或函数 设置此路径默认值。如果是函数，函数返回值为默认值
- `select`: 布尔值 指定 query 的默认 [projections](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/)
- `validate`: 函数 adds a [validator function](http://www.mongoosejs.net/docs/validation.html#built-in-validators) for this property
- `get`: 函数 使用 [`Object.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 定义自定义 getter
- `set`: 函数 使用 [`Object.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 定义自定义 setter
- `alias`: 字符串 仅mongoose >= 4.10.0。 为该字段路径定义[虚拟值](http://www.mongoosejs.net/docs/guide.html#virtuals) gets/sets

索引相关的选项:

- `index`: 布尔值 是否对这个属性创建[索引](https://docs.mongodb.com/manual/indexes/)
- `unique`: 布尔值 是否对这个属性创建[唯一索引](https://docs.mongodb.com/manual/core/index-unique/)
- `sparse`: 布尔值 是否对这个属性创建[稀疏索引](https://docs.mongodb.com/manual/core/index-sparse/)

字符串

- `lowercase`: 布尔值 是否在保存前对此值调用 `.toLowerCase()`
- `uppercase`: 布尔值 是否在保存前对此值调用 `.toUpperCase()`
- `trim`: 布尔值 是否在保存前对此值调用 `.trim()`
- `match`: 正则表达式 创建[验证器](http://www.mongoosejs.net/docs/validation.html)检查这个值是否匹配给定正则表达式
- `enum`: 数组 创建[验证器](http://www.mongoosejs.net/docs/validation.html)检查这个值是否包含于给定数组

数字

- `min`: 数值 创建[验证器](http://www.mongoosejs.net/docs/validation.html)检查属性是否大于或等于该值
- `max`: 数值 创建[验证器](http://www.mongoosejs.net/docs/validation.html)检查属性是否小于或等于该值

日期

- `min`: Date
- `max`: Date

```javascript
var Assignment = mongoose.model('Assignment', { dueDate: Date });
Assignment.findOne(function (err, doc) {
  doc.dueDate.setMonth(3);
  doc.save(callback); // THIS DOES NOT SAVE YOUR CHANGE

  doc.markModified('dueDate');
  doc.save(callback); // works
})
```



内建Date方法不会触发mongoose修改跟踪逻辑, 如果使用doc.setMonth()类似的内建方法修改, 需要使用doc.markModified('var_name')追踪修改





# Conections 连接

## 1. 建立连接

`mongoose.connect('mongodb://username:password@host:port/database?options...');`

## 2. 缓存操作

​		我们使用mongoose的时候不需要等待连接建立, mongoose会对我们的命令进行缓存, 包括创建model

​		如果要禁用缓存, 需要修改bufferCOmmands设置. 全局可以使用 `momgoose.set('bufferCommands', false)`

## 3. 连接选项

`mongoose.connect(uri, option)`

- bufferCommands

- user/pass 用于认证用户名和密码, mongoose特有, 等价于mongoDB的auth.user和auth.password

- autoIndex 默认情况下true, mongoose连接时会自动创建schema索引

- dbName 指定连接的数据库名称. 覆盖连接字符串

  如果使用`mongodb+srv`语法连接MongoDB Atlas需要使用dbName指定数据库

- autoReconnect 默认true, 断开自动连接
- poolSize 最大socket连接数, 默认5

## 4. 连接回调

mongoose.connect(uri, options. callback)

callback含有一个参数 err



# Models 模型

