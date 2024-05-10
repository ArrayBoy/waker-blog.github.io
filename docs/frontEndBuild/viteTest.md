<!--
 * @Author: yanglilong yanglilong@uino.com
 * @Date: 2023-02-10 15:11:47
 * @LastEditors: yanglilong yanglilong@uino.com
 * @LastEditTime: 2023-02-10 16:01:08
 * @FilePath: /blog/docs/frontEndBuild/viteTest.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->

## 一、vue2 + webpack 老项目升级 vite

> 早期使用 vue2+webpack 的项目很多，老项目在开发过程中比较痛苦的事情就是项目启动和热更新很慢,特别是在一些大型项目中重新启动或者,改动代码等待热更新的时候都要等半天,为了更好的开发体验，决定把 webpack 换成 vite,在升级到 vite 的时候可能会出现一些问题，这里主要是记录升级 vite 遇到的一些问题

1. 升级 vite 前后对比

电脑配置

内存: 16g

CPU 主频: 3.20 GHz

核心数: 10

|              | vue-cli |    vite |
| ------------ | :-----: | ------: |
| 项目启动时间 | 57995ms |   222ms |
| 项目打包时间 | 67000ms | 35000ms |
| 首次访问     |  瞬时   |  4500ms |

> vite 打包的时候用的是 rollup，如果 javascript 文件很多并且打包很慢的话可以把默认的 babel 换成 swc 或 esbuild

2. 注意事项

   - vite 基于原生 es module，项目上的一些 commonjs 写法可能不再支持

   - 引入非 es 模块的依赖需要在 vite.config.js 里面处理一下, 一般情况下加到 optimizeDeps 里面就可以了

   - vite 支持的 vue 版本最低为 2.7

   - vite 支持的 eslint 最低为 7.x

   - 导入 worker 文件需要在 url 最后加上 '...?worker'

   - 如果项目使用到了环境变量，变量名必须改成 VITE\_开头，否则项目代码里是访问不到这个变量的

   - vite 对外暴露的环境变量是 import.meta.env

   - vite 不支持 require, 可以换成 import 的方式，比较常见的是使用 require 引入图片

   - 代码里是用到 process 环境的地方需要改成 import.meta.env

   - 在安装的时候可能有一些依赖因为版本问题安装不上，建议把 node_modules 目录删除了再进行安装

3. 准备工作

由于一些第三方库可能依赖比较新的 nodejs 版本, 这里建议使用 node v16 版

1). 把 vue 升级到 2.7.x, eslint 升级到 7.x

```js
npm install vue eslint
```

2). 安装 vite 相关依赖

这里我使用的的是@vitejs/plugin-vue2 这个插件，是 vue 作者尤雨溪开发的，还有一个使用比较多的是 <b>vite-plugin-vue2</b>

目前使用比较多的三个 vue 解析器

@vitejs/plugin-vue2 (vue 作者尤雨溪开发的，兼容上应该不会有啥大问题)

vite-plugin-vue2（社区提供的插件）

@vue/compat (使用的是 vue3，同时兼容 vue2 语法) 需要把 vue 升级到 3.x, 使用方式可以参考https://github.com/vuejs/core/tree/main/packages/vue-compat

由于我们项目暂时没有计划升级到 vue3，就直接用了@vitejs/plugin-vue2，如果你想同时使用 vue3 或者打算升级到 vue3 的话可以使用@vue/compat

```js
npm install vite vite-plugin-eslint @vitejs/plugin-vue2 @rollup/plugin-node-resolve
```

3). 添加 vite 配置文件（默认是 vite.config.{js,ts}）
当前项目并没有用到 ts，所以就直接加了个 vite.config.js 文件就可以了

```js
import vue from "@vitejs/plugin-vue2";
import resolve from "@rollup/plugin-node-resolve";
export default defineConfig(({ command }) => {
  return {
    server: {
      port: 8080,
      proxy: {
        // 配置代理
        api: {
          target: "",
        },
      },
    },
    preview: {
      port: 3000,
    },
    css: {
      preprocessorOptions: {
        css: {
          javascriptEnabled: true,
        },
        stylus: {
          javascriptEnabled: true,
        },
        less: {
          javascriptEnabled: true,
        },
      },
    },
    plugins: [resolve(), vue()],
  };
});
```

4. 修改 vite 配置文件

1). 配置别名

在使用 webpack 的时候经常用到 import '@/' 这样的引入路径，vite 要加一下配置才能正常使用

```js
// vite.config.js
import tsconfigPaths from 'vite-tsconfig-paths'
const aliases = [
  {
    find: /@\//,
    replacement: `${__dirname}/src/`
  }
]

...
{
 resolve: {
    alias: aliases
 },
 plugins: [
 	// tsconfig里面配置的别名可以直接在代码里面使用,如果不需要的话可以去掉
 	tsconfigPaths()
 ]
}
...
```

2). 处理 stylus 引入问题
在 vue2 项目里面 stylus 引入文件的路径写法可能是@import '~@/...' 这么写的

升级成 vite 后可能会出现报错

![](../../media/vite1.png)

更新 vue 编译器版本后不支持 ~@ 这种路径写法了

具体问题可以参考 https://github.com/vuejs/component-compiler-utils/issues/49

这里我们需要把带 ~ 的 url 给转换一下

```js
// vite.config.js
import { Evaluator } from 'stylus'

const visitImport = Evaluator.prototype.visitImport
Evaluator.prototype.visitImport = function (imported) {
  const path = imported.path.first
  if (path.string && path.string.startsWith && path.string.startsWith('~')) {
    const alias = aliases.find((entry) => {
      const reg = entry.find
      if (reg instanceof RegExp) {
        return reg.test(path.string)
      } else {
        return path.string.startsWith(`~${reg}`)
      }
    })

    if (alias) {
      path.string = path.string
        .substr(1)
        .replace(alias.find, alias.replacement)
    }
  }

  return visitImport.call(this, imported)
}

...
{
 plugins: [
 	// 需要把当前Evaluator传给vue解析器
	vue({
		style: {
          preprocessOptions: {
            stylus: { Evaluator }
          }
        }
   }),
 ]
}
...
```

3). 最终的 vite 配置

```js
// vite.config.js
import { loadEnv, defineConfig } from "vite";
import vue from "@vitejs/plugin-vue2";
import { Evaluator } from "stylus";
import { visualizer } from "rollup-plugin-visualizer";
import resolve from "@rollup/plugin-node-resolve";
import tsconfigPaths from "vite-tsconfig-paths";

// vite别名配置
const aliases = [
  {
    find: /^@\/topo-static/,
    // eslint-disable-next-line node/no-path-concat
    replacement: `${__dirname}/public/topo-static`,
  },
  {
    find: /^topo-static/,
    // eslint-disable-next-line node/no-path-concat
    replacement: `${__dirname}/public/topo-static`,
  },
  {
    find: /@\//,
    // eslint-disable-next-line node/no-path-concat
    replacement: `${__dirname}/src/`,
  },
];

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  // 加载环境变量
  const devenv = loadEnv(mode, process.cwd(), "");
  // 转发代理配置
  const proxy = {
    "/topo-api": {
      target: devenv.VITE_APP_URL,
    },
  };

  return {
    // 启动服务参数
    server: {
      port: 8080,
      proxy,
    },
    preview: {
      port: 3000,
      proxy,
    },
    css: {
      preprocessorOptions: {
        css: {
          javascriptEnabled: true,
        },
        stylus: {
          javascriptEnabled: true,
        },
        less: {
          javascriptEnabled: true,
        },
      },
    },
    resolve: {
      alias: aliases,
    },
    plugins: [
      vue({
        style: {
          preprocessOptions: {
            stylus: { Evaluator },
          },
        },
      }),
      resolve(),
      tsconfigPaths(),
      visualizer(),
    ],
  };
});

// 修改Evaluator.prototype.visitImport方法
// 把所有带 ~的路径转成绝对路径
const visitImport = Evaluator.prototype.visitImport;
Evaluator.prototype.visitImport = function (imported) {
  const path = imported.path.first;
  if (path.string && path.string.startsWith && path.string.startsWith("~")) {
    const alias = aliases.find((entry) => {
      const reg = entry.find;
      if (reg instanceof RegExp) {
        return reg.test(path.string);
      } else {
        return path.string.startsWith(`~${reg}`);
      }
    });

    if (alias) {
      path.string = path.string
        .substr(1)
        .replace(alias.find, alias.replacement);
    }
  }

  return visitImport.call(this, imported);
};
```

5. 修改代码让项目正常运行

1). vue 文件引入问题
在升级到 vite 后 vue 文件的引入需要加上后缀

不加的话可能会解析文件失败

![](../../media/vite2.png)

有两种解决方式, 我是直接把所有引入 vue 的地方都加上后缀了

一. 把所有的 vue 文件引入都加上后缀 (比较推荐的方式)

二. 改 vite 配置文件 (也可以解决问题，但不推荐)

```js
// vite.config.js
import resolve from '@rollup/plugin-node-resolve'
...
plugins: [
 	resolve({ extensions: ['.vue', ...] }),
	...
]
...
```

2). vue 深度选择器语法修复

在老项目里面可能想改嵌套组件样式的时候经常用到 /deep/ 或 >>>

升级后可能会出现一个警告，新版本的 vue 解析器已经不支持以前这种语法了

![](../../media/vite3.png)

这里要把所有用到/deep/ 或 >>>的地方都改成 :deep() 的语法，不改的话对应的样式将不生效，可能会导致页面布局混乱，样式错乱等问题

由于使用的地方很多，一个一个改太慢了，这里我是直接用正则匹配一下然后全局替换就可以了

![](../../media/vite4.png)

3). 修改环境变量引用问题
升级后代码就不能通过 process 来访问环境变量了，需要改成 import.meta.env

4). 修改 eslint 配置文件，这是升级后的 eslint 配置

```js
module.exports = {
  root: true,
  env: {
    browser: true,
    jest: true,
    es2021: true,
    node: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:vue/essential",
    "@vue/standard",
    "@vue/eslint-config-typescript",
  ],
  parser: "vue-eslint-parser",
  parserOptions: {
    ecmaVersion: 2021,
    sourceType: "module",
  },
};
```

5). 修改 tsconfig.json 配置文件

需要把 jsx 打开，指定 vue 编译器版本

```js
{
  "compilerOptions": {
    "jsx": "preserve",
    "baseUrl": "."
  },
  "vueCompilerOptions": {
  	// 指定vue编译器版本
    "target": 2.7
  }
}
```

6).添加 HTML 入口文件

vite 默认入口文件是 <rootDir>/index.html， 在项目根目录下新建一个 index.html 文件

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>森拓扑</title>
    <link rel="icon" href="/favicon-site.ico">
  </head>
  <body>
    <noscript>
      <strong>We're sorry but
        <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

7). 导入 worker 文件

在老项目上可以直接使用 new Worker(new URL('./worker.js', import.meta.url));这种方式导入一个 worker 文件

升级 vite 后就不支持这种导入方式了

需要改成 import Worker from './worker.js?worker' 这种方式导入

6. 单元测试相关配置
   之前 vue-cli 里面做了一些单元测试的处理，直接用脚手架就跑 jest

删掉 vue-cli 后我们需要加一些单元测试的配置

这里还是使用 jest 去运行我们的单元测试代码

1). 安装 jest 相关依赖

可以把用不到的依赖去掉

```js
npm i babel-jest jest jest-canvas-mock jest-environment-jsdom workerloader-jest-transformer vue-jest
```

2). jest vue 文件处理

jest 默认情况下解析不了 vue 文件, 我们需要在配置文件里面改一下

![](../../media/vite5.png)

```js
// jest.config.cjs
module.exports = {
  moduleFileExtensions: ["vue", "js", "ts"],
  transform: {
    // 指定vue文件转换器, 这里我用了vue-jest，也可以换成其它的
    "^.+\\.vue$": "vue-jest",
  },
};
```

3). jest JavaScript 文件处理
升级 vite 后导入 worker 是在 url 后面加上?worker 表示这是一个 worker 文件

jest 默认使用的是 babel-jest 转换器并不知道这是一个 worker 文件

我们需要指定 worker 文件的规则和转换器

比如我们项目里面的 worker 文件名末尾必须加上 .worker.js

这里 worker 文件我用的是 workerloader-jest-transformer 转换器

非 worker 文件我还是用默认的 babel-jest

如果单元测试转换很慢的话可以考虑换其它效率转换效率比较高的转换器

目前其它效率比较高的转换器有

@swc/jest (使用 rust language 编写，号称比 babel 单核快 20 倍，四核快 70 倍)

![](../../media/vite6.png)

esbuild-jest (使用 go language 编写，速度上比@swc/jest 更快，但没有类型检查，如果需要 ts 类型检查的话可能不好支持)

![](../../media/vite7.png)

指定 jascript 和 worker 文件转换器

```js
// jest.config.cjs
module.exports = {
  transform: {
    // 指定非worker文件转换器
    "^((?!worker).)+\\.js$": "babel-jest",
    // 指定worker文件转换器
    "^.+\\.worker.[t|j]sx?$": "workerloader-jest-transformer",
  },
};
```

4). jest 导入文件处理

这部分改动不大,主要加了'^@/(._)$': '&lt;rootDir&gt;/src/$1', 和 '(._\\.js)\\?worker$': '$1' 两行配置

```js
// jest.config.cjs
module.exports = {
  moduleNameMapper: {
    "^@/(.*)$": "<rootDir>/src/$1",
    "(.*\\.js)\\?worker$": "$1",

    "\\.(css|less|scss|sass|styl)$": "<rootDir>/tests/unit/mocks/styleMock.js",
    "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga|md)$":
      "<rootDir>/tests/unit/mocks/fileMock.js",
  },
};
```

5). jest 环境变量配置
默认情况下 jest 是不会加载环境变量的，代码里面用到环境变量的地方可能会出现异常

我们需要先加载一下环境变量

```js
// jest.config.cjs
module.exports = {
  // 在运行单元测试的时候读取配置文件
  globalSetup: "<rootDir>/tests/unit/dotenv-test.js",
};
```

```js
// <rootDir>/tests/unit/dotenv-test.js
const path = require("path");
const dotenv = require("dotenv");

module.exports = async () => {
  dotenv.config({ path: path.resolve(__dirname, "../../.env.test") });
};
```

6). vue-jest 配置
如果我们用到了 babel 转换器，需要在配置文件里面告诉 vue-jest 我们使用了 babel 转换器，否则解析 vue 文件可能会出现异常

```js
// jest.config.cjs
module.exports = {
  globals: {
    "vue-jest": {
      babelConfig: true,
    },
  },
};
```

7). 最终的 jest 配置文件

```js
// jest.config.cjs
module.exports = {
  verbose: true,
  moduleFileExtensions: ["vue", "js", "ts"],
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.vue$": "vue-jest", // vue文件转换
    "^((?!worker).)+\\.js$": "babel-jest", // 非worker文件解析器
    "^.+\\.worker.[t|j]sx?$": "workerloader-jest-transformer", // worker文件转换器
  },
  testRegex: "(/__tests__/.*|(\\.|/)(test|spec))\\.(js|ts)$",
  moduleNameMapper: {
    "^@/(.*)$": "<rootDir>/src/$1",
    "(.*\\.js)\\?worker$": "$1",

    "\\.(css|less|scss|sass|styl)$": "<rootDir>/tests/unit/mocks/styleMock.js",
    "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga|md)$":
      "<rootDir>/tests/unit/mocks/fileMock.js",
    gojs: "<rootDir>/packages/gojs/release/go.js",
  },
  setupFiles: ["jest-canvas-mock", "<rootDir>/tests/unit/setup"],
  // 运行单元测试的时候读取环境变量
  globalSetup: "<rootDir>/tests/unit/dotenv-test.js",
  globals: {
    "vue-jest": {
      babelConfig: true,
    },
  },
  transformIgnorePatterns: ["/node_modules/(?!(uuid))", "/packages/(?!(mqtt))"],
  coverageDirectory: "<rootDir>/tests/unit/coverage",
  coverageReporters: ["text-summary"],
  collectCoverageFrom: ["src/**/*.js"],
};
```

8). babel 配置文件
babel 目前只有 jest 用到了，所以不用考虑浏览器环境

babel 默认跑的是浏览器环境，但 jest 是在 node 环境运行的，所以我们需要指定 babel 的运行环境为 node 环境

在升级 vite 后我们的环境变量都是通过 import.meta.env 的方式访问, jest 是跑在 node 环境的所以没法通过 import.meta.env 的方式来访问环境变量

我们除了指定 node 环境还需要把所有的 import.meta.env 替换成 process

```js
// babel.config.cjs
module.exports = {
  // 指定jest运行环境
  presets: [
    [
      "@babel/preset-env",
      { useBuiltIns: "entry", corejs: "2", targets: { node: "current" } },
    ],
    "@babel/preset-typescript",
  ],
  // node环境还是需要通过process来访问环境变量
  // 需要把所有 import.meta.env 替换成 process
  plugins: [
    function () {
      return {
        visitor: {
          MetaProperty(path) {
            path.replaceWithSourceString("process");
          },
        },
      };
    },
  ],
};
```

7. 保留 webpack 配置
   升级 vite 后原先的 webpack 可能就不能正常使用了

我们项目保留的 webpack 只编译 js 代码，没有涉及到 vue 文件

1). 导入文件别名配置

```js
// webpack.config.js
module.exports = {
  resolve: {
    extensions: [".js"],
    // 配置别名跟vite一致
    alias: {
      "@": resolve("src"),
    },
  },
};
```

2). 解析 worker 文件和非 worker 文件

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.worker\.js(\?worker)?$/,
        use: { loader: "worker-loader" }, // webpack可以用worker-loader解析我们的worker文件
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader", // 其它js文件还是使用babel-loader解析
          options: {
            // babel配置文件里面指定了node环境
            // 这里改成默认的配置就可以了
            presets: [["@babel/preset-env", { targets: "defaults" }]],
            // webpack可以直接用@babel/plugin-proposal-optional-chaining处理我们的环境变量
            plugins: ["@babel/plugin-proposal-optional-chaining"],
          },
        },
      },
    ],
  },
};
```

8. package.json 文件修改
   如果项目用到了流水线，runner 环境给 node 分配的内存可能有点小，可能会有个堆内存不足的错误

我们只需要把 node 的内存调大一点就可以了, 内存给到 4096 就可以正常运行了

![](../../media/vite8.png)

```js
{
	"scripts": {
		"start": "vite",
		"preview": "vite preview",
		"test": "jest",
    	"dev": "npm start",
		"unit": "npm test -- --coverage",
		"build": "cross-env NODE_OPTIONS=--max-old-space-size=4096 vite build",
		"pre-commit": "lint-staged"
	}
}
```

到这里整个升级过程都完成了

运行 npm start 应该可以启动项目了

## 二、vue3+vite
