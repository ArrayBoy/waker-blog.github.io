## git ci precommit

#### Commit 规范

> Commit 规范我们统一遵循使用 Angular 团队提议的《AngularJS Git Commit Message Conventions》格式如下

```js
<type>(<scope>): <subject> 必须填写
空一行
<body> 选填
空一行
<footer> 选填

```

- type 类型有如下，只能从一下列表选择
  - build：影响生成系统或外部依赖性的更改
  - ci: 更改 CI 配置文件和脚本
  - feat: 新功能（feature）
  - fix: 修补 bug
  - perf: 提高性能的代码更改
  - docs: 文档（documentation）
  - style: 不影响代码含义的更改（不影响代码运行的变动）
  - refactor: 代码修改既不修复错误，也不添加特征（即不是新增功能，也不是修改 bug 的代码变动）
  - test: 添加缺失测试或纠正现有测试
  - revert: 撤回
- scope 影响范围
- subject 是 commit 目的的简短描述，不超过 72 个字符
- body Body 部分是对本次 commit 的详细描述，可以分成多行
- footer 不兼容变动主要及一些额外说明
