[官网地址](https://webpack.docschina.org/plugins/compression-webpack-plugin/)

```java
yarn add compression-webpack-plugin --save-dev

const CompressionPlugin = require("compression-webpack-plugin");

module.exports = {
  plugins: [new CompressionPlugin(
    test: /\.js(\?.*)?$/i,
    //小于20kb不打包gz文件
  	threshold:1024* 20
  )],
};
```

## 配置项

- test：匹配[string|RegExp|Array<String|RegExp];  //要压缩的文件格式，默认所有

- include: 匹配[string|RegExp|Array<String|RegExp];

- exclude: 不匹配[String|RegExp|Array<String|RegExp]

- algorithm: 压缩方式(算法) 'gzip’或自定义function

- compressionOptions：例子compressionOptions: { level: 1 },具体参数详情参考https://nodejs.org/api/zlib.html#zlib_class_options

- threshold：指定压缩文件最小size 单位bytes（只压缩比该参数值大得文件）

- minRatio:最小压缩比（压缩后小于该比例不处理）Number，设定为Infinity表示全部文件都处理包括size为0得文件；设定为MAX_SAFE_INTEGER表示全部文件都处理，除了size为0得文件

- filename：输出文件名，默认"[path][base].gz" String|function

- deleteOriginalAssets：是否删除源文件 默认false