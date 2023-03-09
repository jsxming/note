## 什么是babel？

是一个工具链，主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中

(文档地址)[https://www.babeljs.cn/docs/]

它可以

- 语法转换
- 通过 Polyfill 方式在目标环境中添加缺失的特性 （通过引入第三方 polyfill 模块，例如 [core-js](https://github.com/zloirock/core-js)）
- 源码转换等
- **Source map**，因此你可以轻松调试编译后的代码。

```javascript
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

```json
//v7.8.0或更高的版本适用 babel.config.json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          //根据具体需要配置 浏览器的兼容性
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        //只包含你所需要的 polyfill
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

```javascript
//运行此命令将 src 目录下的所有代码编译到 lib 目录：
./node_modules/.bin/babel src --out-dir lib 
//或
npx babel src --out-dir lib
```

### @babel/preset-env、 @babel/core 

preset-env是一组预先设定的语法转换插件
Babel 的核心功能包含在 @babel/core 模块中
