# webpack模块路径解析

```javascript
    resolve: {
        extensions: ['.js', '.vue'], //当 import 或者 require 导入模块是 omit了后缀名，按照数组中指定后缀的寻找模块
        alias: {
            vue: 'vue/dist/vue.js',
            echarts:"echarts/dist/echarts.js",
            'vue$': 'vue/dist/vue.esm.js' //全功能的vue
        }
    },
    externals: {//打包的时候不会将externals内的依赖包打包进去 
        ol: "ol",
        echarts: "echarts"
    },
```

## 绝对路径
## 相对路径
## 模块路径

如果路径指向一个**文件夹**，则采取以下步骤找到具有正确扩展名的正确文件：

1. 如果文件夹中包含 package.json 文件，则按照顺序查找 resolve.mainFields 配置选项中指定的字段(默认是 main字段)。并且 package.json 中的第一个这样的字段确定文件路径。
2. 如果 package.json 文件不存在或者 package.json 文件中的 main 字段没有返回一个有效路径，则按照顺序查找 **resolve.mainFiles** 配置选项中指定的文件名，看是否能在 import/require 目录下匹配到一个存在的文件名。文件扩展名通过 resolve.extensions 选项采用类似的方法进行解析。

## 模块的查找路径 resolve.modules
逐级查找 node_modules目录

## 当不存在package.json时 使用resolve.mainFields(默认值是index)
当从 npm 包中导入模块时（例如，import * as D3 from "d3"），此选项将决定在 **package.json** 中使用哪个字段导入模块。根据 webpack 配置中指定的 target 不同，默认值也会有所不同。

当 target 属性设置为 webworker, web 或者没有指定，默认值为：

mainFields: ["browser", "module", "main"]
对于其他任意的 target（包括 node），默认值为：

mainFields: ["module", "main"]
例如，D3 的 package.json 含有这些字段：
```bash
{
  ...
  main: 'build/d3.Node.js',
  browser: 'build/d3.js',
  module: 'index',
  ...
}
```
这意味着当我们 import * as D3 from "d3"，实际从 browser 属性解析文件。在这里 browser 属性是最优先选择的，因为它是 mainFields 的第一项。同时，由 webpack 打包的 Node.js 应用程序默认会从 module 字段中解析文件。



## 参考目录
- https://doc.webpack-china.org/configuration/resolve/#resolve-modules webpack模块解析
- https://doc.webpack-china.org/concepts/module-resolution/ 路径解析