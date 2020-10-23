# Mongoose

## 1. 基础知识 

- 安装配置

  `npm install mongoose --save`

- 引入模块

  `const mongoose = require('mongoose');`

  `mongoose.connect('mongodb://localhost/test');`

  如果由用户名和密码:

  `mongoose.connect('mongodb://username:password@localhost:port/dbname')`

- 定义Schema

  数据库中的Schema, 为数据库对象