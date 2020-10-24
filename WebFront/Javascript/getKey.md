在Javascript有多种方式获取键名, 但是每种方式之间都有一定的差异

1. 键名分类
   - 继承与非继承(自有属性)
   - Symbol与非Symbol
   - 可枚举和不可枚举(enumerable)

2. 方式分类

   枚举:

   - for in 和 for of:

     for in会获取遍历所有可枚举属性, 包括原型上的**继承属性**

     for of 等价于 Object.keys() 返回非继承键名

   方法:

   - Reflect.ownKeys 返回非继承的所有可枚举键名和Symbol

     等价于Object.getOwnPropertyNames(target).concat(Object.getOwnPropertySymbols(target))

   - Object.keys() 返回非继承所有键名

   - Object.getOwnPropertyDescriptors(a) 返回a所有自有的**属性描述符**(包括不可枚举)

1. ​				{value: 1, writable: true, enumerable: true, configurable: true}

2. - Object.getOwnPropertyNames() 返回自有非继承属性名(包括不可枚举)
   - Object.getOwnPropertySymbols() 返回自有非继承Symbol

