[官网](https://angular.cn/docs)

Angular 是一个应用设计框架与开发平台，用于创建高效、复杂、精致的单页面应用。

大多数 Angular 代码都只能用最新的 JavaScript 编写，它会用 类型 实现依赖注入，还会用装饰器来提供元数据。

## 特性

#### 先来快速看看angular的特性

* 横跨所有平台

> 学会用 Angular 构建应用，然后把这些代码和能力复用在多种多种不同平台的应用上 —— Web、移动 Web、移动应用、原生应用和桌面原生应用。

* 速度与性能

> 通过 Web Worker 和服务端渲染，达到在如今(以及未来）的 Web 平台上所能达到的最高速度。
> Angular 让你有效掌控可伸缩性。基于 RxJS、Immutable.js 和其它推送模型，能适应海量数据需求。

* 美妙的工具

> IDE支持好

##  Angular CLI

```javascript
npm install -g @angular/cli

ng new my-app

cd my-app

ng serve --open
```

## Angular CLI 命令

```javascript
ng generate component hero   //+ New Component

ng generate service hero  //+ New service

ng generate module app-routing --flat --module=app  //+  AppRoutingModule
// --flat 把这个文件放进了 src/app 中，而不是单独的目录中。
// --module=app 告诉 CLI 把它注册到 AppModule 的 imports 数组中。

ng add @angular/material   //+ Angular Material

ng add @angular/pwa   //+ Add PWA Support

ng add _____   //+ Add Dependency

Run and Watch Tests   //+ ng test

ng build --prod   //+ Build for Production
```

## 项目文件结构

[参考](https://angular.cn/guide/file-structure)

#### 根目录

|  配置文件   | 用途  |
|  ----  | ----  |
| .editorconfig  | 代码编辑器的配置。 |
| .gitignore  | 指定 Git 应忽略的不必追踪的文件。 |
| README.md  | 根应用的简介文档. |
| angular.json  | 为工作区中的所有项目指定 CLI 的默认配置，包括 CLI 要用到的构建、启动开发服务器和测试工具的配置项，比如 TSLint，Karma 和 Protractor。欲知详情，请参阅 Angular 工作空间配置 部分。 |
| tsconfig.json  | 工作空间中所有项目的基本 TypeScript 配置。所有其它配置文件都继承自这个基本配置。 |
| tslint.json  | 工作空间中所有项目的默认的 TSLint 配置。 |

#### 顶层文件 src/ 为测试并运行你的应用提供支持。其子文件夹中包含应用源代码和应用的专属配置。

|  应用支持文件 | 目的  |
|  ----  | ----  |
| app/  | 包含定义应用逻辑和数据的组件文件 |
| environments/ | 包含特定目标环境的构建配置选项。默认情况下，有一个无名的标准开发环境和一个生产（“prod”）环境。你还可以定义其它的目标环境配置。 |
| index.html  | 当有人访问你的站点时，提供服务的主要 HTML 页面。CLI 会在构建你的应用时自动添加所有的 JavaScript 和 CSS 文件 |
| main.ts  | 应用的主要切入点。用 JIT 编译器编译应用，然后引导应用的根模块（AppModule）在浏览器中运行。你也可以在不改变任何代码的情况下改用 AOT 编译器，只要在 CLI 的 build 和 serve 命令中加上 --aot 标志就可以了。 |
| polyfills.ts  | 为浏览器支持提供了腻子（polyfill）脚本。 |
| styles.sass  | 列出为项目提供样式的 CSS 文件。该扩展还反映了你为该项目配置的样式预处理器。 |
| test.ts  | 单元测试的主入口点，带有一些 Angular 特有的配置。你通常不需要编辑这个文件。 |

#### 在 src/ 文件夹里面，app/ 文件夹中包含此项目的逻辑和数据。Angular 组件、模板和样式也都在这里。

|  SRC/APP/ 文件 | 用途  |
|  ----  | ----  |
| app/app.component.ts  | 为应用的根组件定义逻辑，名为 AppComponent。当你向应用中添加组件和服务时，与这个根组件相关联的视图就会成为视图树的根。 |
| app/app.component.html  | 定义与根组件 AppComponent 关联的 HTML 模板。 |
| app/app.component.css  | 为根组件 AppComponent 定义了基本的 CSS 样式表。 |
| app/app.component.spec.ts | 为根组件 AppComponent 定义了一个单元测试。 |
| app/app.module.ts  | 定义了名为 AppModule 的根模块，它会告诉 Angular 如何组装应用。这里最初只声明一个 AppComponent。当你向应用中添加更多组件时，它们也必须在这里声明。 |