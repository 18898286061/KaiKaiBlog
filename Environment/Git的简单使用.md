# 使用Git
使用Git有三种方式，请按照需求选择

1. 只在本地使用
2. 将本地仓库上传到GitHub
3. 下载GitHub上的仓库

# 使用Git

## 只在本地使用
### 1.1 初始化
1. 创建我们的项目目录： <code>mkdir demo1</code>
2. 进入目录  <code>cd demo1</code>
3. <code>git init</code> ，这句命令会在demo1里创建一个 .git目录
4. <code>ls -la</code> 你会看到.git目录，它就是一个【仓库】
5. git-demo-1 目录里面添加任意文件：分别是index.html 和 css/style.css
  - <code>touch index.html</code>
  - <code>makir css</code>
  - <code>touch css/stye.css</code>
6. 运行<code>git satus -sb</code>
```
 ## Initial commit on master
 ?? css/
 ?? index.html
```
?? 表示了Git不知道怎么去对待这些变动

7. git add 将文件添加到【暂存区】
  - 1.  你可以一个一个的add
    - 2. git add index.html
    - 3. git add css.style.css
  - 2. 你也可以一次性add
    - 1. git add. （add.的意思是把当前目录里面呢的变动都加到【暂存区】）
    
8. 再次运行`git status -sb`，可以看到?? 变成了A
```
 ## Initial commit on master
 A  css/style.css
 A  index.html
```
A 的意思就是添加，也就是说你告诉了 git   ：“这些文件我要加到仓库里”

9. 使用`git commit -m “注释信息”` 将你add过的内容【正式提交】到本地仓库（.git就是本地仓库），并添加一些注释信息，方便日后查阅
  - 你可以一个一个的commit
    - `git commit index.html -m "添加index.html"`
    - `git commit css/style.css -m "添加css/style.css"`
  - 你也可以一次性commit
    - `git commit . -m "添加了几个文件"`
 
10. 再次运行`git status -sb`，发现没有文件变动了，这是因为文件的变动已经记录在仓库里了。
11. 这时你使用git log 就可以看到历史上的变动：
```
 commit f0d95058cd32a332b98967f6c0a701c64a00810a
 Author: frankfang <frankfang1990@gmail.com>
 Date:   Thu Sep 28 22:30:43 2017 +0800

     添加几个文件
```

12. 以上。    就是git add / git commit 的一次完整过程。

### 1.2文件内容变动
如果文件的内容修改了，应该怎么做呢？

1. `start css/style.css` 会使用默认的编辑器打开 css/style.css（macOS 上对应的命令是 open css/style.css）
2. 然后我们在 css/style.css 里写入 body {background: red}，保存退出
3. 运行 git status -sb 发现提示中有一个 M

 ```
 ## master
 M css/style.css
 ```
 
这个 M 的意思就是 Modified，表示这个文件被修改了
4. 此时你如果想让改动保存到仓库里，你需要先 `git add css/style.css` 或者也可以 `git add .`
  注意，由于这个 css/style.css 以前被我们 add 过，你往文章上面看，我们是 add 过 css/style.css 的，所以此处的 `git add` 操作可以省略，但我建议你使用 git 的前一个月，不要省略 git add。
  换句话说，每一次改动，都要经过 `git add` 和 `git commit` 两个命令，才能被添加到 .git 本地仓库里。

5. 再次运行 `git status -sb` 发现 M 有红色变成了绿色
6. 运行 `git commit -m` "更新 css/style.css"，这个改动就被提交到 .git 本地仓库了。
7. 再再次运行 `git status -sb`，会发现没有变更了，这说明所有变动都被本地仓库记录在案了。
这里来透露一下 git status -sb 是什么意思：git status 是用来显示当前的文件状态的，哪个文件变动了，方便你进行 git add 操作。-sb 选项的意思就是，-s 的意思是显示总结（summary），-b 的意思是显示分支（branch），所以 -sb 的意思是显示总结和分支。


### 1.3 总结
1. `git init`，初始化本地仓库 .git
2. `git status -sb`，显示当前所有文件的状态
3. `git add` 文件路径，用来将变动加到暂存区
4. `git commit -m "信息"`，用来正式提交变动，提交至 .git 仓库
5. 如果有新的变动，我们只需要依次执行 `git add xxx` 和 `git commit -m 'xxx'` 两个命令即可。
6. `git log` 查看变更历史



## 将本地仓库上传到Github
如何将我们这个 demo1 上传到 GitHub 呢？

1. 在 GitHub 上新建一个空仓库，名称随意，一般可以跟本地目录名一致，也叫做 demo1
  
  除了仓库名，其他的什么都别改，**其他的什么都别改!!** 这样你才能创建一个空仓库
  
2. 点击创建按钮之后，GitHub 就会把后续的操作全告诉你

3. **点击 SSH 按钮!!**，如果不点击这个按钮，你就会使用默认的 HTTPS 地址。但是千万不要使用 HTTPS 地址，因为 HTTPS 地址使用起来特别麻烦，每次都要输入密码，而 SSH 不用输入用户名密码。
为什么 SSH 不用密码呢，因为你已经上传了 SSH public key。（如果没有请去配置自己的SSH Key）

3. 进去之后又两块操作
  - 第一块为「…or create a new repository on the command line」
  - 第二块为「…or push an existing repository from the command line」

4. 由于我们已经有本地仓库了，所以看第二块，而第二块下面的命令就是我们需要的，我们一行一行拷贝过来执行
找到图中的「…or push an existing repository from the command line」这一行，你会看到 `git remote add origin https://github.com/xxxxxxxxxx/git-demo-1.git`， 如果你发现这个地址是 https 开头的，那你就做错了，还记得吗，我们要使用 SSH 地址，GitHub 的 SSH 地址是以 git@github.com 开头的。「点击 SSH按钮」

得到新的命令 `git remote add origin git@github.com:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/git-demo-1.git`，复制并运行它
复制第二行 `git push -u origin maste`r，运行它
刷新当前页面，你的仓库就上传到 GitHub 了！


## 直接在Github创建一个仓库，然后下载到本地

这里我们将讲解第三种用法，那就是直接在Github创建仓库，然后下载到本地

1. 在Github 上新建一个仓库 domo2，这次不创建空仓库了，而是自带 README 和 Lisence 的仓库，创建的选项如下：
  - Repository name：   （填写你自己的库名）
  - Description(optional)： （形容一下你这个库）
  - Public （库的私密性）
  - Iniyialize this repository with a README （使用一个README初始化这个库）
  - Add .gitignore: Node （忽略的上传文件）
  - Add a license: MIT License （许可证）
  - 最后点击绿色按钮的创建（Create respository）
2. 这样一来，这个仓库就会自动拥有三个文件：.gitignore、LICENSE、README.md
3. 这三个文件的作用链接：
  - README 自行搜索啦~
  - 在项目目录创建 .gitignore 文件就可以指定「哪些文件不上传到远程仓库」，比如
  .gitignroe
  ```
  /node_modules/
  /.vscode/
  //（这句是注释别写上去）这样就可以避免 node_modules/ 和 .vscode/ 目录被上传到 github 了。
  ```
  - [.gitignore的作用](http://gitbook.liuhui998.com/4_1.html)
  - [LISENCE的作用](http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html)

4. 好了，现在远程仓库已经创建好了，接下来使用 git clone 命令把我们库里的东西下载下来
5. 点击页面中唯一的绿色按钮「clone or download」，会看到一个弹出层
6. 请确保弹出层里的地址是 SSH 地址，也就是 git@github.com 开头的地址，如果不是，就点击 Use SSH 按钮。然后复制这个地址。
7. 打开 Git Bash，找一个安全的目录：`cd ~/Desktop`运行。
8. 运行 `git clone 你刚才得到的以git@github.com开头的地址`，运行完了你就会发现，桌面上多出一个 demo2 目录。
9. `cd demo2`进入这个多出来的目录
10. 运行 `ls -la` 你会看到，远程目录的所有文件都在这里出现了，另外你还看到了 .git 本地仓库。这是你就可以添加文件，git add，然后 git commit 了。

### 回顾一下
我们再回顾一遍已经学到的命令：（这次只多了一个 git clone 命令）

1. git clone git@github.com:xxxx，下载仓库
2. git init，初始化本地仓库 .git
3. git status -sb，显示当前所有文件的状态
4. git add 文件路径，用来将变动加到暂存区
5. git commit -m "信息"，用来正式提交变动，提交至 .git 仓库
6. 如果有新的变动，我们只需要依次执行 git add xxx 和 git commit -m 'xxx' 两个命令即可。

## 如何上传更新
你在本地目录有任何变动，只需按照以下顺序就能上传：

1.git add 文件路径
2. git commit -m "信息"
3. git pull 
4. git push

下面是例子

1. `cd git-demo-1`
2. `touch index2.html`
3. `git add index2.html`
4. `git commit -m "新建 index2.html"`
5. `git pull`
6. `git push`

## 永远都不要上传 node_modules 到 github。
如果你想防止自己手贱上传 node_modules 到 github ，可以：

1. 在项目根目录 `touch .gitignore`
2. 在 .gitignore 里添加一行 `/node_modules/`
3. `git add .gitignore`
4. `git commit -m 'ignore'`

## 还有一些有用的命令
- `git remote add origin git@github.com:xxxxxxx.git` 将本地仓库与远程仓库关联
- `git remote set-url origin git@github.com:xxxxx.git` 上一步手抖了，可以用这个命令来挽回
- `git branch` 新建分支
- `git merge` 合并分支
- `git stash` 通灵术
- `git stash pop` 反转通灵术
- `git revert` 后悔了
- `git reset` 另一种后悔了
- `git diff` 查看详细变化

学 Git 不是一两天就能学会的，还需要长期的使用，所以别妄想现在就掌握它.。
