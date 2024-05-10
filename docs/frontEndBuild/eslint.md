## links

- [eslint](https://eslint.org/)、[eslint 中文](https://eslint.bootcss.com/docs/user-guide/configuring)
- [@vue/cli-plugin-eslint](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint)
  > vue-cli 的 eslint 插件,如果使用 vue-cli 构建项目时选择了 eslint 选项，则 cli 会安装@vue/cli-plugin-eslint 这个包，这个包向 vue-cli-service 注入命令 lint，可在 vue.config.js 中配置 lintOnSave 字段决定是否在保存修改时执行规则检测并修复部分检测不通过的代码；也可手动执行 npm/yarn run lint 执行 eslint 检测。
- [@vue/cli-plugin-babel](https://github.com/vuejs/vue-cli/tree/dev/packages/@vue/cli-plugin-babel#readme)
  > vue-cli 的 babel 插件（集成于 vue-cli，作为单独包发布 npm）
- [eslint-config-airbnb-base](https://github.com/airbnb/javascript)
  > Airbnb 的基础 JS .eslintrc(没有 React 插件)作为一个可扩展的共享配置
- [babel-eslint]()
  > 自定义的 eslint 解析器,已更改为@babel/eslint-parser，原包不再更新（不建议使用）

## 使用

### 校验代码风格有三种方式：

1. 借助 vue-cli 生成项目自动配置好 eslint；

   如果是使用 vue-cli 生成的项目，则已经配置好如下的 eslint 配置项。可在 package.json

   ```js
    "eslintConfig": {
        "root": true,
        "env": {
        "node": true,
        "es6": true
        },
        "extends": [
        "plugin:vue/essential",
        "eslint:recommended"
        ],
        "parserOptions": {
        "parser": "babel-eslint"
        },
        "rules": {}
    },
   ```

   **在 package.json 文件里的 eslintConfig 字段指定配置或者创建.eslintrc.js 文件，ESLint 会查找和自动读取它们，再者，你可以在命令行运行时指定一个任意的配置文件。**

2. 自己配置，本文是确认使用 airbnb 风格的规则后，按照 eslint 官网步骤为 vue 项目添加 eslint。

   #### 全局

   ```js
   //全局安装 ESLint
   npm i eslint -g

   //生成配置文件,根据自己的项目需求进行设置
   eslint --init
   ```

   #### 项目中

   ```js
   //在项目中安装 ESLint
   npm install eslint --save-dev

   //使用eslint初始化一个配置文件(因为是局部安装，所以需要使用相对路径定位eslint)
   ./node_modules/.bin/eslint --init

   //这时已经在项目根目录中生成文件.eslintrc，包含一份默认的eslint配置，配置详解。也可以不采用eslint --init指令生成一下配置文件，自己创建.eslintrc文件，配置相关选项。
   module.exports = {
       //规则运行环境
       "env": {
           "browser": true,
           "es2021": true
       },
       //使用的规则集
       "extends": [
           "eslint:recommended",
           "plugin:vue/essential"
       ],
       //parseOptions不是指定解析器，是传给解析器的配置项，告诉解析器需要支持的规则， 如：es2018，和项目是使用ecmaScript模块编写
       "parserOptions": {
           "ecmaVersion": 12,
           "sourceType": "module"
       },
       "plugins": [
           "vue"
       ],
       //自定义规则
       "rules": {
       }
   };
   ```

   #### 除 eslint 外还需要安装

   - babel-eslint
   - eslint-config-airbnb-base：airbnb 风格的规则
   - eslint-plugin-import：支持检测 es6+的 import/export 语法，防止文件路径引入错误或拼写错误。
   - eslint-plugin-vue：

   不同框架或环境可添加的 eslint 插件列表：[keywords:eslintplugin - npm search](https://www.npmjs.com/search?q=keywords:eslintplugin),配置完成后，在每次编译时不通过的 warn 和 error 信息会提示在终端里。

3. 借助 vscode 集成插件

## 坑点

- vue 项目使用 eslint 版本^8.3.0 之后报错"eslint.CLIEngine is not a constructor"?

  > airbnb 支持需要 eslint 8 以上版本，但升级后由于 vue-eslint-parser 的不支持造成上面报错

  解决方案：

  ```javascript
  //vue.config.js 里面关闭语法检测
  module.exports = {
    lintOnSave: false,
  };
  ```
