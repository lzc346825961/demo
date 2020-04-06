# webpack面试题

## 1. webpack3和webpack4的区别
1.1. mode/–mode参数
新增了mode/--mode参数来表示是开发还是生产（development/production）
production 侧重于打包后的文件大小，development侧重于goujiansud
1.2. 移除loaders，必须使用rules（在3版本的时候loaders和rules 是共存的但是到4的时候只允许使用rules）
1.3. 移除了CommonsChunkPlugin (提取公共代码)，用optimization.splitChunks和optimization.runtimeChunk来代替
1.4. 支持es6的方式导入JSON文件，并且可以过滤无用的代码

```javascript
let jsonData = require('./data.json')
import jsonData from './data.json'
import { first } from './data.json' // 打包时只会把first相关的打进去
```
1.5. 升级happypack插件（happypack可以进行多线程加速打包）
1.6. ExtractTextWebpackPlugin调整，建议选用新的CSS文件提取kiii插件mini-css-extract-plugin，production模式，增加 minimizer

## 2. loader 和 plugin 不同
2.1. loader是使wenbpack拥有加载和解析非js文件的能力
2.2. plugin 可以扩展webpack的功能，使得webpack更加灵活。可以在构建的过程中通过webpack的api改变输出的结果

## 3. webpack构建流程
3.1. 初始化参数，从配置文件和shell语句中读到的参数合并，得到最后的参数
3.2. 开始编译：用合并得到的参数初始化complier对象，加载是所有配置的插件，执行run方法开始编译
3.3. 确定入口，通过entry找到入口文件
3.4. 编译模块，从入口文件出发，调用所有配置的loader对模块进行解析翻译，在找到该模块依赖的模块进行处理
3.5. 完成模块编译，得到每个模块被翻译之后的最终的内容和依赖关系
3.6. 输出资源，根据入口和模块之间的依赖关系，组装成一个个包含多个模块的chunk，在把每个chunk转换成一个单独的文件加载到输出列表
3.7. 输出完成，确定输出的路径和文件名，把内容写到文件系统中
在以上过程中，webpack会在特定的时间点广播出特定的事件，插件在舰艇感兴趣的事件后会执行特定的逻辑，改变webpack的运行结果

## 4. webpack 热加载执行原理
？？？？

## 5. 如何利用webpack来优化前端性能
5.1. 压缩代码。uglifyJsPlugin 压缩js代码， mini-css-extract-plugin 压缩css代码
5.2. 利用CDN加速，将引用的静态资源修改为CDN上对应的路径，可以利用webpack对于output参数和loader的publicpath参数来修改资源路径
5.3. 删除死代码（tree shaking），css需要使用Purify-CSS
5.4. 提取公共代码。webpack4移除了CommonsChunkPlugin (提取公共代码)，用optimization.splitChunks和optimization.runtimeChunk来代替

## 6. 什么是bundle,什么是chunk，什么是module?
bundle:有webpack打包出来的文件
chunk：webpack在进行模块的依赖分析的时候，代码分割出来的代码块
module:开发中的单个模块

## 7. webpack-dev-server和http服务器如nginx有什么区别?
webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，他比传统的http服务对开发更加简单高效。

## 8. DefinePlugin
DefinePlugin ：允许创建一个在编译时可以配置的全局变量

## 9. DllPlugin
使用DllPlugin可以减少基础模块编译次数，动态链接库插件，其原理是吧网页依赖的基础模块抽离出来打包到dll文件中，当需要导入的模块存在于某个dll中时，这个模块不再被打包，而是去dll中获取。在dll中大多包含的时常用的第三方模块，只要这些模块版本不升级，就只需要被编译一次。
注意
DllPlugin参数中的name必须要和output.library值保持一致，并且生成的mainfest文件中会引用output.library值

## 10. happyPack开启多线程loader转换
运行在node.js之上的webpack时单线程模型，也就是只能一个一个文件进行处理，不能并行处理，happypack可以将任务分解给多个子进程，最后将结果发给主进程，js是单线程模型，只能通过这种多线程的方式提高性能
