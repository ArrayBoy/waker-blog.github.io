## 基本概念

- Entry：入口，Webpack 执行构建的第一步将从 Entry 开始。

- Module：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。

- Chunk：代码块，一个 chunk 由多个模块组合而成，用于代码合并与分割。

- Loader：模块转换器，用于将模块的原内容按照需求转换成新内容。loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 模块，以供应用程序使用，以及被添加到依赖图中。

```js
const path = require("path");

module.exports = {
  output: {
    filename: "my-first-webpack.bundle.js",
  },
  module: {
    //当你碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先 use(使用) raw-loader 转换一下。
    rules: [{ test: /\.txt$/, use: "raw-loader" }],
  },
};
```

- Plugin：loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const webpack = require("webpack"); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: "raw-loader" }],
  },
  //html-webpack-plugin 为应用程序生成一个 HTML 文件，并自动将生成的所有 bundle 注入到此文件中。
  plugins: [new HtmlWebpackPlugin({ template: "./src/index.html" })],
};
```

> 综上，loader 是处理过程中，局部的，插件是处理一个完整的，全局的的方法，类似于 vue2 计算属性和公共方法。

## 流程概括

```javascript
初始化参数 ——> 开始编译 ——> 确定入口 ——> 编译模块 ——> 完成模块编译 ——> 输出资源 ——> 输出完成
```

- 初始化参数：从配置文件(默认 webpack.config.js)和 shell 语句中读取与合并参数，得出最终的参数

- 开始编译(compile)：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件 Plugin，通过执行对象的 run 方法开始执行编译

- 确定入口：根据配置中的 entry 找出所有的入口文件

- 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行编译,再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过处理

- 完成编译模块：经过第四步之后，得到了每个模块被翻译之后的最终内容以及他们之间的依赖关系

- 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 chunk，再将每个 chunk 转换成一个单独的文件加入输出列表中，这是可以修改输出内容的最后机会

- 输出完成：在确定好输出内容后，根据配置(webpack.config.js && shell)确定输出的路径和文件名，将文件的内容写入文件系统中(fs)

**总结一下，Webpack 的构建流程可以分为以下三大阶段：**

1. 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler。

2. 编译：从 Entry 发出，针对每个 Module 串行调用对应的 Loader 去翻译文件内容，再找到该 Module 依赖的 Module，递归地进行编译处理。

3. 输出：对编译后的 Module 组合成 Chunk，把 Chunk 转换成文件，输出到文件系统。

## bundle.js

> bundle.js 其实是一个立即执行函数,bundle.js 能直接运行在浏览器中的原因在于输出的文件中通过**webpack_require**函数定义了一个可以在浏览器中执行的加载函数来模拟 Node.js 中的 require 语句。

而且 Webpack 做了缓存优化，执行加载过的模块不会再执行第二次，执行结果会缓存在内存中，当某个模块第二次被访问时会直接去内存中读取被缓存的返回值。

```javascript
(function (modules) {
  //模拟require语句
  function __webpack_require__() {}
  //执行存放所有模块数组中的第0个模块(main.js)
  __webpack_require_[0];
})([
  /*存放所有模块的数组*/
]);
```

## 性能优化

#### 减少 Webpack 打包时间

- 优化 Loader：优化 Loader 的文件搜索范围(exclude 掉 node_modules)、将 Babel 编译过的文件缓存起来(loader: 'babel-loader?cacheDirectory=true')。对于 Loader 来说，影响打包效率首当其冲必属 Babel 了。因为 Babel 会将代码转为字符串生成 AST，然后对 AST 继续进行转变最后再生成新的代码，项目越大，转换代码越多，效率就越低。

- HappyPack: 可以将 Loader 的同步执行转换为并行的。

```javascript
module: {
  loaders: [
    {
      test: /\.js$/,
      include: [resolve('src')],
      exclude: /node_modules/,
      // id 后面的内容对应下面
      loader: 'happypack/loader?id=happybabel'
    }
  ]
},
plugins: [
  new HappyPack({
    id: 'happybabel',
    loaders: ['babel-loader?cacheDirectory'],
    // 开启 4 个线程
    threads: 4
  })
]
```

- DllPlugin: 可以将特定的类库提前打包然后引入。极大的减少打包类库的次数，只有当类库更新版本才有需要重新打包。

```javascript
// 单独配置在一个文件中
// webpack.dll.conf.js
const path = require("path");
const webpack = require("webpack");
module.exports = {
  entry: {
    // 想统一打包的类库
    vendor: ["react"],
  },
  output: {
    path: path.join(__dirname, "dist"),
    filename: "[name].dll.js",
    library: "[name]-[hash]",
  },
  plugins: [
    new webpack.DllPlugin({
      // name 必须和 output.library 一致
      name: "[name]-[hash]",
      // 该属性需要与 DllReferencePlugin 中一致
      context: __dirname,
      path: path.join(__dirname, "dist", "[name]-manifest.json"),
    }),
  ],
};

// 使用 DllReferencePlugin 将依赖文件引入项目中
// webpack.conf.js
module.exports = {
  // ...省略其他配置
  plugins: [
    new webpack.DllReferencePlugin({
      context: __dirname,
      // manifest 就是之前打包出来的 json 文件
      manifest: require("./dist/vendor-manifest.json"),
    }),
  ],
};
```

- 代码压缩：webpack3 中使用 webpack-parallel-uglify-plugin 来并行运行 UglifyJS(单线程)，webpack4 中将 mode 设置为 production 则默认开启压缩。

#### 减少 Webpack 打包后的文件体积

- 按需加载：每个路由页面单独打包为一个文件、loadash 这种大型类库同样可以使用这个功能。

- Scope Hoisting: 会分析出模块之间的依赖关系，尽可能的把打包出来的模块合并到一个函数中去。Webpack4 中开启这个功能，只需要启用 optimization.concatenateModules。

```javascript
// test.js
export const a = 1;
// index.js
import { a } from "./test.js";
```

打包上面两个文件后，生成代码类似这样：

```javascript
[
  /* 0 */
  function (module, exports, require) {
    //...
  },
  /* 1 */
  function (module, exports, require) {
    //...
  },
];
```

如果使用 Scope Hositing,会生成这样的类似代码：

```javascript
[
  /* 0 */
  function (module, exports, require) {
    //...
  },
];
```

- Tree Shaking：可以实现删除项目中未被引用的代码。Webpack4 的生产环境默认开启这个功能。

```javascript
// test.js
export const a = 1;
export const b = 2;
// index.js
import { a } from "./test.js";
```

test 文件中的变量 b 如果没有在项目中使用到的话，就不会被打包到文件中。

## 面试相关

#### 1. 谈谈你对 WebPack 的认识?

WebPack 是一个模块打包工具，可以使用 WebPack 管理模块依赖，并编译输岀模块所需的静态文件。它能够很好地管理与打包 Web 开发中所用到的 HTML、 JavaScript 、CSS 以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源， WebPack 有对应的模块加载器。Web Pack 模块打包器会分析模块间的依赖关系，最后生成优化且合并后的静态资源。

WebPack 的两大特色如下。

（1）代码切割（ code splitting）

（2） loader 可以处理各种类型的静态文件，并且支持串行操作 WebPack 以 CommonJS 规范来书写代码，但对 AMD/CMD 的支持也很全面，方便对项目进行代码迁移。

WebPack 具有 require.js 和 browserify 的功能，但也有很多自己的新特性，

- 对 CommonJS、AMD、ES6 的语法实现了兼容。

- 对 JavaScript、CSS、图片等资源文件都支持打包

- 串联式模块加载器和插件机制，让其具有更好的灵活性和扩展性，例如提供对 CoffeeScript、 EMAScript 6 的支持

- 有独立的配置文件 webpack.config. js。

- 可以将代码切割成不同的块，实现按需加载，缩短了初始化时间。

- 支持 SourceUrls 和 SourceMaps，易于调试。

- 具有强大的 Plugin 接口，大多是内部插件，使用起来比较灵活

- 使用异步 I/O，并具有多级缓存，这使得 WebPack 速度很快且在增量编译上更加快。

#### 2. 在使用 WebPack 时，你都做些什么？

用来压缩合并 CSS 和 JavaScript 代码，压缩图片，对小图生成 base64 编码，对大图进行压缩，使用 Babel 把 EMAScript 6 编译成 EMAScript 5，热重载，局部刷新等。在 output 中配置出口文件，在 entry 中配置入口文件。

使用各种 loader 对各种资源做处理，并解析成浏览器可运行的代码。

#### 3. 说说 WabPack 打包的流程？

具体流程如下。

（1）通过 entry 配置入口文件。

（2）通过 output 指定输出的文件。

（3）使用各种 loader 处理 CSS、 JavaScript、 image 等资源，并将它们编译与打包成浏览器可以解析的内容等。

#### 4. WebPack 的核心原理是什么？

（1）一切皆模块。

正如 JavaScript 文件可以是一个“模块”（ module）一样，其他的（如 CSS、 image 或 HTML）文件也可视作模块。因此，可以执行 require（'myJSfile js'），亦可以执行 require（ 'myCSSfile.css'）。这意味着我们可以将事务（业务）分割成更小的、易于管理的片段，从而达到重复利用的目的。

（2）按需加载。

传统的模块打包工具（ module bundler）最终将所有的模块编译并生成一个庞大的 bundle. js 文件。但是，在真实的 App 里， bundle. js 文件的大小在 10MB 到 15MB 之间，这可能会导致应用一直处于加载状态。因此， WebPack 使用许多特性来分割代码，然后生成多个 bundle js 文件，而且异步加载部分代码用于实现按需加载。

#### 5. WebPack 中 loader 的作用是什么？说说你工作中几个常用的 loader。

```javascript
babel- loader：将下一代的 JavaScript语法规范转换成现代浏览器能够攴持的语法规范。因为 babel有些复杂，所以大多数开发者都会新建一个. babelrc进行配置。

css-loader、 style- loader：这两个建议配合使用，用来解析CSS文件依赖。

less- loader：解析less文件。

file- loader：生成的文件名就是文件内容的MD5散列值，并会保留所引用资源的原始扩展名。

url- loader：功能类似于file-loader，实现图片文字等资源的打包，limit选项定义大小限制，如果小于该限制，则打包成base64编码格式；如果大于该限制，就使用file- loader去打包成图片。
```

#### WebPack 工具中常用到的插件有哪些？

```javascript
HtmlWebpackPlugin：依据一个HTML模板，生成HTML文件，并将打包后的资源文件自动引入。

commonsChunkPlugin：抽取公共模块，减小包占用的内存空间，例如vue的源码、 jQuery的源码等。

extract-text-webpack- plugin：将样式抽取成单独的文件。

hostess: 实现浏览器兼容。

babel：将 JavaScript未来版本（ EMAScript6、 EMAScript2016等）转换成当前浏览器支持的版本。

hot module replacement：修改代码后，自动刷新、实时预览修改后的效果

ugliifyJsPlugin：压缩 JavaScript代码。
```

#### 6. plugins 和 loader 有什么区别？

> 它们是两个完全不同的东西。loader 负责处理源文件，如 CSS、jsx 文件，一次处理一个文件。而 plugins 并不直接操作单个文件，它直接对整个构建过程起作用。

#### 7. 说说 HtmlWebpackPlugin 插件的作用?

> 依据一个简单的 index .html 模板，生成一个自动引用你打包后的 JavaScript 文件的、新的 index.html 文件。

#### 8. 说说 WebPack 支持的脚本模块规范?

> 不同项目在定义脚本模块时使用的规范不同。有的项目会使用 Commonjs 规范（参考 Node. js）；有的项目会使用 EMAScript 6 模块规范；有的还会使用 AMD 规范（参考 Require. js）。WebPack 支持这 3 种规范，还支持混合使用。

#### 9. 如何为项目创建 package. json 文件？

> 将命令行切换至根目录下，运行 npm init，命令行就会一步一步引导你建立 package. json 文件。手动在根目录下创建一个空文件，并命名为 package. json，在文件中填充 JSON 格式的常规内容。例如初期只需要 name 和 version 字段。

```javascript
{
"name"："Project"，
"version"：" 0.0.1"
}
```

#### 10. WebPack 和 gulp/grunt 相比有什么特性？

> gulp/ grunt 是一种能够优化前端的流程开发工具，而 Web Pack 是一种模块化的解决方案，由于 WebPack 提供的功能越来越丰富，使得 WebPack 可以代替 gulp/grunt 类的工具。

就一句： **WebPack 能够按照模块的依赖关系构建文件组织结构。**

#### 11. WebPack 的工作方式是什么？

> 把项目当作一个整体，通过一个给定的主文件（如 index. js）， WebPack 将从这个文件开始找到你项目的所有依赖，并使用 loader（加载器）来处理它们，最后打包为个浏览器可识别的 JavaScript 文件。

#### 12. 如何用 webpack-dev- server 监控文件编译？

> 打开多个控制台，用 webpack--watch 实时监控文件变动，并随时编译。

#### 13. publicPath 是什么？

在 WebPack 自动生成资源路径时，比如由于 WebPack 异步加载分包而需要独立出来的块，或者打包 CSS 时， WebPack 自动替换掉的图片、字体文件，又或者使用 html-webpack-plugin 后 WebPack 自动加载的入口文件等，这些 WebPack 生成的路径都会参考 publicPath 参数。不需要关注 CDN，需要关注的是，文件发布出来后，应该部署到哪里。如果文件是与页面放到一起的，那么可以按相对路径来设置，比如'./'之类的；而如果 JavaScript、CSS 文件用于存放 CDN，当然就要填写 CDN 的域名和路径。

#### 14. 当使用 Babel 直接打包的 JavaScript 文件中含有 jsx 语法的时候会报错，如何解决这个问题？

修改 package. json 并添加 react，如以下代码所示。

```javascript

"babel"：{
    "presets"：[
          "es2015",
             "react",
           "stage-o"
],
 "plugins" ：[
"add-module-exports"
]
}
```

#### 15.当使用 html- webpack- plugin 时找不到指定的 template 文件怎么办？

通过以下代码进行解决。

```javascript
{
test:/ \.html ？$/,
loader : 'html-loader '
}
```

#### 16.WebPack 如何切换开发环境和生产环境？

> 生产环境与开发环境的区别无非就是调用的接口地址、资源存放路径、线上的资源是否需要压缩等方面。目前的做法是通过在 package. json 中设置 node 的一个全局变量，然后在 webpack. config. js 文件里面进行生产环境与开发环境的配置切换。

#### 17. WebPack 的优势是什么？

（1） WebPack 以 CommonJS 的形式书写脚本，对 AMD/CMD 的支持也很全面，方便对旧项目进行代码迁移

（2）绝大部分前端资源都可以模块化。

（3）开发便捷，能替代 grunt/gulp 的部分工作，如程序打包、压缩混淆、图片转 base64 编码等。

（4）扩展性强，插件机制完善，特别是支持 React 热插拔功能。

#### 18.图片处理常见的加载器有几种？

（1）file- loader，默认情况下会根据图片生成对应的 MD5 散列的文件格式。

（2）url- loader，它类似于 file- loader，但是 url- loader 可以根据自身文件的大小，来决定是否把转化为 base64 格式的 DataUrl 单独作为文件，也可以自定义对应的散列文件名。

（3） image- webpack- loader，提供压缩图片的功能。

#### 19. WebPack 命令的-- config 选项有什么作用？

> -- config 用来指定一个配置文件，代替命令行中的选项，从而简化命令。如果直接执行 WebPack, WebPack 会在当前目录下查找名为 webpack. config. js 的文件。
