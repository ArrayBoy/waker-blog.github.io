## 一、代码提交规范 husky + lint-staged

> 前置条件：vue-cl 初始化的项目，项目配置已包含 eslint 设置，项目经过 git 初始化，即项目包含.git 文件。

- 配置依赖

  1. "husky": "^7.0.0" [husky](https://typicode.github.io/husky/#/?id=monorepo)
     > husky 是一个 git 的 npm 包，为 git 扩展了一些 git hooks,husky 会安装一系列的 git hook 到项目的.git/hook 目录中。
     > husky 主用功能是为 git 添加 git 钩子，它允许我们在使用 git 中在一些重要动作发生时触发自定义脚本(npm script), 比如我们可以在 git push 之前执行特定的自定义脚本对代码进行单元测试、又或者在 git commit 之前执行 eslint 校验，当然本文主要介绍如何借用 husky 为 git 添加 commit-msg 钩子并对 commit 进行校验。

  ```js
    npx husky-init && npm install

    npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'

    //跳过检测
    git commit -m "msg" --no-verify

    //在CI/Docker中禁用husky
    npm ci --omit=dev --ignore-scripts

    //卸载
    npm uninstall husky && git config --unset core.hooksPath

  ```

  > npx 是一个工具，npm v5.2.0 引入的一条命令（npx）,简化 npm 的命令和操作

  2. "lint-staged": "^12.1.2" [lint-staged](https://github.com/okonet/lint-staged)

  > 获取所有被提交的文件并执行配置好的任务命令,各种 lint 校验工具可以配置在 lint-staged 任务中。用于对 Git 暂存区中的文件执行代码检测

  ```js
    // 安装并同时修改配置package.json、pre-commit里面的配置
    npx mrm@2 lint-staged
    // mrm是一个命令行npm包，保持配置统一

    // 执行后package.json里面配置
    {
      "devDependencies": {
        "lint-staged": "^12.1.2",
      },
      "lint-staged": {
        "*.js": "eslint --cache --fix"
      }
    }
    // 可修改为
    {
      "lint-staged": {
        "src/**/*.{js,vue}": "eslint --fix"
      }
    }

    // pre-commit里面
    npx lint-staged
  ```

  3. "commitlint": [commitlint 官网](https://commitlint.js.org/#/)、[commitlintNPM](https://www.npmjs.com/package/@commitlint/cli)

  > 格式化提交信息，包含@commitlint/cli @commitlint/config-conventional 或者 @commitlint/config-angular

  ```js
    // 安装
    npm install --save-dev @commitlint/cli @commitlint/config-conventional

    // 配置commitlint.config.js
    echo "module.exports = {extends: ['@commitlint/config-conventional']};" > commitlint.config.js

    npm install --save-dev @commitlint/cli @commitlint/config-angular

    // 配置commitlint.config.js
    echo "module.exports = {extends: ['@commitlint/config-angular']};" > commitlint.config.js

    // 配置commit-msg
    npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
  ```

  4. "conventional-changelog-cli": "^2.1.1" [conventional-changelog-cli](https://www.npmjs.com/package/conventional-changelog-cli)，[可参考](https://blog.csdn.net/weixin_34326179/article/details/91382865)

  > 从 git 元数据中生成一个变更日志，可以全局安装不影响 package.json

  ```js
  npm install -g conventional-changelog-cli
  ```

  然后修改 package.json

  ```js
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "prepare": "husky install",
    "log": "conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md"
  }

  // 执行npm run log 可在本地生成CHANGELOG.md文件
  ```
