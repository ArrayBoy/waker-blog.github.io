<!--
 * @Author: your name
 * @Date: 2021-09-02 14:58:52
 * @LastEditTime: 2023-02-10 15:08:21
 * @LastEditors: yanglilong yanglilong@uino.com
 * @Description: In User Settings Edit
 * @FilePath: /blog/docs/frontEndBuild/vite.md
-->

#### [官网](https://vitejs.cn/)

- index.html?

> 在开发期间 Vite 是一个服务器，而 index.html 是该 Vite 项目的入口文件。Vite 将 index.html 视为源码和模块图的一部分。

Vite 解析 &lt;script type="module" src="..."&gt; ，这个标签指向你的 JavaScript 源码。甚至内联引入 JavaScript 的 &lt;script type="module"&gt; 和引用 CSS 的 &lt;link href&gt; 也能利用 Vite 特有的功能被解析。另外，index.html 中的 URL 将被自动转换，因此不再需要 %PUBLIC_URL% 占位符了。

- 项目根目录?

> 与静态 HTTP 服务器类似，Vite 也有 “根目录” 的概念，即服务文件的位置，在接下来的文档中你将看到它会以 &lt;root&gt; 代称。源码中的绝对 URL 路径将以项目的 “根” 作为基础来解析，因此你可以像在普通的静态文件服务器上一样编写代码（并且功能更强大！）。Vite 还能够处理依赖关系，解析处于根目录外的文件位置，这使得它即使在基于 monorepo 的方案中也十分有用。

Vite 也支持多个 .html 作入口点的 [多页面应用模式](https://vitejs.cn/guide/build.html#multi-page-app)

vite 以当前工作目录作为根目录启动开发服务器。你也可以通过 vite serve some/sub/dir 来指定一个替代的根目录。
