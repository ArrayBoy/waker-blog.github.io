## npm 相关

```js
查看当前仓库源：npm config get registry

修改为淘宝镜像：npm config set registry https://registry.npm.taobao.org  //修改为淘宝npm镜像

修改为阿里镜像：npm config set registry https://registry.npmmirror.com  //修改为阿里npm镜像

修改为cnpmjs镜像：npm config set registry https://r.cnpmjs.org  //修改为cnpmjs镜像

修改为原始地址：npm config set registry https://registry.npmjs.org  //修改为原始地址

查看当前目录下安装了哪些node包： npm ls

登陆npm： npm login

查看当前npm用户：npm whoami  //此命令需要登陆npm

发布本地包：npm publish

查看node安装路径：npm get prefix

查看全局node包：npm root -g

清理npm缓存：npm cache clean -f
```

## 用户配置相关

```javascript
初始化仓库：git init

查看隐藏文件:ls -al

查看git配置：git config

修改全局用户名：git config --global user.name 'CloudBai'
修改仓库改用户名：git config --local user.name 'CloudBai'

修改全局用户邮箱： git config --global user.email 'tets@qq.com'
修改仓库级用户邮箱： git config --local user.email 'tets@qq.com'

仓库级(全局)删除用户名：git config --local(global) --unset user.name

仓库级(全局)删除用户邮箱：git config --local(global) --unset user.email

更新所有文件的用户名和邮箱：git commit --amend --reset-author

查看所有配置信息：git config --list

查看用户名配置：git config user.name

查看用户邮箱配置：git config user.email
```

## 退出 vim

```javascript
esc → shift + ';' → q!
```

## 提交

```javascript
git add filename1 filename2 // add 单个或者多个文件 git add /Applications/my-project/my/blog/docs/git/index.md

git add . //添加当前目录下的所有文件到暂存区

git commit -m "msg"

git pull

git push

git push origin //分支名
```

## 更新

```js
// 更新冲突：Please commit your changes or stash them before you merge.
git stash  //备份当前的工作区，从最近一次提交中读取相关内容，让工作区保持和上一次提交的内容一致。同时，将工作区的内容保存到git栈中。

git pull

git stash pop  //从git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个stash的内容，所以用栈来管理，pop会从最近一个stash中读取内容并恢复到工作区。

git stash apply // 使用apply命令恢复，stash列表中的信息是会继续保留的,而使用pop命令进行恢复，会将stash列表中的信息进行删除。

git stash list  //显示git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。

git stash clear // 清空git栈

```

## 修改

```js
git status  //查看有修改过的文件(标红色的文件表示未提交到缓存区，绿色字表示已经添加到了缓存区)

git diff  //查看文件改动

git diff filename  //查看具体文件改动,git diff /Applications/my-project/my/blog/docs/git/index.md
```

## 撤销

```js
// 如果你想要撤回上一次的push操作，你可以使用几种不同的方法，具体取决于你想要实现的结果。以下是几种常见的方法：

1. 使用 git reset 和 git push --force
如果你想要完全撤销上一次的push，并且不介意潜在的数据丢失，可以使用git reset将HEAD指针移动到你想要的状态，然后强制推送到远程仓库。

git reset --hard HEAD~1
git push --force origin branch_name
这里的branch_name是你想要推送的分支名称，比如master或main。

警告：使用--force选项会覆盖远程仓库的历史，这可能会影响其他协作者的工作，因此请谨慎使用。

2. 使用 git revert
如果你希望以一种不会影响项目历史记录的方式来撤销更改，可以使用git revert命令。这个命令会为你撤销的更改创建一个新的提交。

git revert HEAD
git push origin branch_name
git revert会创建一个新的提交，它是上一个提交的逆操作，从而撤销了上一个提交所做的更改。

3. 使用 git push 与 --force-with-lease 选项
--force-with-lease 是一种更安全的强制推送选项，它在远程仓库没有新的更改时才允许强制推送。

git push --force-with-lease origin branch_name
如果远程仓库有新的更改，这个命令会失败，从而避免覆盖其他人的工作。


在团队协作中，通常推荐使用第二种方式git revert，因为它是一种更安全、更可逆的操作。


```

## 创建

```js

git branch -a  //查看仓库所有分支

// 方法一

git branch branch_name  //新建本地分支

git checkout branch_name  //切换本地分支

git push origin branch_name  // 推送到远程仓库

git branch --set-upstream-to=origin/branch_name  // 将本地仓库与远程仓库进行关联

// 方法二

git checkout -b branch_name  //新建本地分支并切换到该分支

git push origin branch_name:branch_name  //将本地分支推送到远程仓库并与本地分支同名

git branch --set-upstream-to=origin/branch_name  // 将本地仓库与远程仓库进行关联

// 其他操作

git branch -d branch_name  //删除本地分支

git branch -D branch_name  //删除本地未合并分支

git branch -m <原分支名> <更改的分支名>  //分支改名

git push origin branch_name:master  //将本地分支推送到远程仓库的master上面
```

## 删除

```js
git push origin -d branch_name //删除远程分支

git branch -d branch_name  //删除本地分支, 需要先切到其他分支

git branch -D branch_name  //删除本地未合并分支
```

## 分支操作

```javascript
git commit --amend -m '提交注释' //修正上一次消息提交的注释

```

## 合并分支

```javascript
//比如把dev合并到master

git checkout master  //先切到master分支

git pull  //确保master为最新代码

git merge dev //再合并，有冲突解决冲突

git merge dev --squash //把分支所有提交当成一次commit来合并

git commit -m 'merge msg' //没冲突提交

git push origin //push

//这样合并完成，但是master可能存在其他提交，这时候dev和master并不是同步的，那么再来一次master合并到dev就可以（俗称反向合并）
```

## 回退

```javascript
git reset --hard commit_id  回退到上一个版本（谨慎操作）

git reset --hard HEAD^  回退到上一个版本

git reset --hard HEAD~1  回退到上一个版本

// error: Pulling is not possible because you have unmerged files.
//hint: Fix them up in the work tree, and then use 'git add/rm <file>'
//hint: as appropriate to mark resolution and make a commit.
//fatal: Exiting because of an unresolved conflict.

git reset --hard FETCH_HEAD

git reset --soft HEAD^  // 仅仅是撤回commit操作，改动的代码仍然保留然后执行

git reset //撤销未提交已添加的修改

git reflog   命令查看HEAD指针曾经指向过的提交记录
```

## 解决冲突--特指 commit 后不能 push 也不能 pull

```javascript
git reset --soft HEAD^  // 仅仅是撤回commit操作，改动的代码仍然保留然后执行
git stash  //备份当前的工作区，从最近一次提交中读取相关内容，让工作区保持和上一次提交的内容一致。同时，将工作区的内容保存到git栈中。

git pull

//pop恢复
git stash pop  //从git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个stash的内容，所以用栈来管理，pop会从最近一个stash中读取内容并恢复到工作区。

//apply恢复
git stash apply // 使用apply命令恢复，stash列表中的信息是会继续保留的,而使用pop命令进行恢复，会将stash列表中的信息进行删除。

git stash list  //显示git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。

git stash clear // 清空git栈
```

## 标签

```javascript
git tag  所有标签

git tag tagName 打一个标签
git tag -a tagName -m "描述"  打一个带描述的标签

git tag -d tagName  删除本地标签

git push origin :refs/tags/标签名   删除远程标签

git tag -l 'tagName*'  搜索符合条件的标签

git push origin tagName    将本地tag推送到远端服务器

git push --tags / git push origin --tags   将本地所有tag推送到远端服务器

git fetch origin tag tagName   拉取远程标签
```

给一个分支创建 tag 有 2 种方式，第一种是上面通过命令行创建后推到远程，另一种是直接在远程仓库 create new tag，输入 tag 名、选择分支（一般为 master），填写描述(message),填写发布细节（一般为 changelog.md 里面的更新内容）

## changelog

```javascript
Commitizen & Change Log  git提交记录格式化生成

npm install -g commitizen  全局安装

commitizen init cz-conventional-changelog --save --save-exact  项目里执行使其支持 Angular 的 Commit message 格式

npm install -g conventional-changelog-cli  全局安装

conventional-changelog -p angular -i CHANGELOG.md -s   项目里执行生成changelog.md（中台只需执行此行）
```

## 提交记录

```javascript
git log --stat  每次提交的简略统计信息

git log -p -2  最近的两次提交所引入的差异（按 补丁 的格式输出）

git log --pretty=oneline   一行显示提交统计信息

git log --pretty=oneline <filename> 仅查看这个文件的所有历史记录

git log --pretty=format:"%H - %an, %ar : %s"  完整哈希值-姓名-时间-提交说明

git show  查看最新的commit

git show commitId  查看指定commit hashID的所有修改

git show commitId fileName  查看某次commit中具体某个文件的修改
```

git log --pretty=format 常用的选项

%H 提交的完整哈希值

%h 提交的简写哈希值

%T 树的完整哈希值

%t 树的简写哈希值

%P 父提交的完整哈希值

%p 父提交的简写哈希值

%an 作者名字

%ae 作者的电子邮件地址

%ad 作者修订日期（可以用 --date=选项 来定制格式）

%ar 作者修订日期，按多久以前的方式显示

%cn 提交者的名字

%ce 提交者的电子邮件地址

%cd 提交日期

%cr 提交日期（距今多长时间）

%s 提交说明

## diff

```javascript
git diff <hashcode-before-right> <hashcode> <filename>   查看目标文件两个版本之间的差异(不带<>)
```

## git 答疑

1. git pull 和 git fetch 的区别

```javascript
git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并。

git pull 会将远程仓库的变化下载下来，并和当前分支合并。
```

2. git rebase 和 git merge 的区别?

```javascript
git merge 和 git rebase 都是用于分支合并，关键在 commit 记录的处理上不同。

git merge 会新建一个新的 commit 对象，然后两个分支以前的 commit 记录都指向这个新 commit 记录。这种方法会保留之前每个分支的 commit 历史。

git rebase 会先找到两个分支的第一个共同的 commit 祖先记录，然后将提取当前分支这之后的所有 commit 记录，然后将这个 commit 记录添加到目标分支的最新提交后面。经过这个合并后，两个分支合并后的 commit 记录就变为了线性的记
录了。
```

3. hint: Please, commit your changes before merging.

```javascript
git merge --abort
```

## vscode 实用插件

- GitLens git 扩展

- Bracket Pair Colorizer 括号颜色及缩进指示

- Indent Rainbow 缩进颜色提示

- Import Cost 注导入包的大小对于项目打包后体积掌握很有帮助

- REST Client 直接在 VSCode 中来测试我们的 API

- Auto Close Tag 和 Auto Rename Tag 自动补全标签和联动重名标签

- quokkaJS 直接运行 JS Ctrl+ Shift + P 选择 toggle (stop/start) on current file 或者选择当前文件

- Open-In-Browser 在浏览器打开

- CSS Peek 追踪至样式表中 CSS 类和 ids 定义的地方。当你在 HTML 文件中右键单击选择器时，选择“ Go to Definition 和 Peek definition ”选项，它便会给你发送样式设置的 CSS 代码。

- Color Info 在颜色上悬停光标，就可以预览色块中色彩模型的（HEX、 RGB、HSL 和 CMYK）相关信息了。

- HTML CSS Support 智能提示 CSS 类名以及 id

- vscode-icons 另一套 目录树图标主题

- Path Intellisense (必备)　　自动提示文件路径，支持各种快速引入文件

- Vue TypeScript Snippets vue 的 typescript 代码片段

- React/Redux/react-router Snippets (推荐)(react 必备) React/Redux/react-router 语法智能提示

- koroFileHeader 多行注释插件： ctrl + Alt + i(头部) ctrl + Alt + t(光标处)

- Add jsdoc comments 多行注释插件： ctrl + shift + P 然后 Add doc comments （强推荐）

- Prettier - Code formatter 格式化代码，设置保存和粘贴自动格式化（设置——>搜索 format）

- 修改多行注释快捷键：File>>Preferences>>Keyboard Shortcuts>>搜索 comment>>Toggle Block Comment>>编辑>>按下键盘快捷键>>“回车”确定

[详情参见](https://zhuanlan.zhihu.com/p/113222681)
