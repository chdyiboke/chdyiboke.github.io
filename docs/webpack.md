# 前端工程化

https://webpack.docschina.org

## 从0开始--前端工程搭建

## 性能优化--各个方面，记录时间

## 总结

  [JavaScript Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html):
```
  <!--  生产环境定位bug: 出错的时候，除错工具将直接显示原始代码  -->
  devtool: 'source-map',  // 最高质量(完整)的选择，使其对生产有价值。but,慢

```

```
// 安装  http://npm.taobao.org/
cnpm install [name]

webpack
// 目标 默认编译目标文件  /src/index.js  

webpack --config webpack.config.dev.js  //指定文件

webpack --progress --colors --watch   // 参数

配置文档：https://webpack.js.org/configuration/entry-context/



```

