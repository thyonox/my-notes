Git官方中文文档：[Git - Book](https://git-scm.com/book/zh/v2)

# 1. 概述

## 1.1 Git 简介

Git 是一个分布式版本控制工具，主要用于管理开发过程中的源代码文件。

Git 通过仓库存储和管理这些文件，而仓库又分为两种：

- 本地仓库：本地电脑上的仓库。
- 远程仓库：远程服务器上的仓库。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668640750404-e7c65165-c925-40cd-b7fb-a31f8459b4bf.png)

本地仓库到远程仓库提交、推送和拉取的不仅只是文件还有**版本信息**。

Git 的主要作用为：

- 代码回溯：Git 在管理文件过程中会记录日志，方便回退到历史版本。
- 版本切换：Git 存在**分支**的概念，一个项目可以有多个分支（版本），可以任意切换。
- 多人协作：Git 支持多人协作，共同开发一个项目。
- 远程备份：Git 通过远程仓库，上传、备份、拉取文件。

其他的版本可控制工具还有：

- SVN
- CVS
- VSS

## 1.2 Git 的下载安装

Git 官网：[Git](https://git-scm.com/)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678947761754-6c69b4d4-4f55-41d7-803f-0c4dfc4566aa.png)

安装完成后，打开 cmd 输入 `git -v`或者`git --version`，安装成功后打印正确版本信息。

桌面点击右键，有两个选项与 Git 有关：

- Git Gui here：图形界面（不常用）
- Git Bash here：命令行

# 2. 代码托管

Git 远程仓库一般放在代码托管服务器上。

比较有名的有：

- Github
- Gitee（国内使用）
- Gitlab

# 3. Git常用命令

## 3.1 全局设置

设置用户名和 email ，用于身份验证。邮箱用于验证github身份。

`git config --global user.name “your username”`

`git config --global user.email “your email”`

查看配置信息。

git config —list

## 3.2 获取 Git 仓库

要使用 Git 对项目进行版本管理，首先需要创建本地仓库。

**本地初始化仓库**

在需要作为本地仓库的空目录中，开打 Git bash，运行 `git init`

如果在当前目录中出现 `.git` 文件夹（默认隐藏），表示创建成功。

**从远程仓库克隆**

打开 Git base，运行 `git clone 远程仓库地址`

如果在当前目录中出现项目文件夹，表示克隆成功。对于github拉取不成功需要配置代理；


- 配置代理：
    
    ```jsx
    git config --global http.proxy http://proxy_address:port
    git config --global https.proxy https://proxy_address:port
    ```
    
- 查看代理：
    
    ```jsx
    git config --global --get http.proxy
    git config --global --get https.proxy
    ```
    
- 取消代理：
    
    ```jsx
    git config --global --unset http.proxy
    git config --global --unset https.proxy
    ```
    

## 3.3 Git 仓库相关概念

Git 仓库分为三个区，分别对应不同的文件状态。

- 工作区：包含 `.git` 文件夹的目录就是工作区，主要用于存放开发的代码。
- 暂存区：`.git` 文件夹中的 `index` 文件就是暂存区（stage），用于临时保存修改的文件。
- 版本库：`.git` 隐藏文件夹就是版本库，用于存储配置信息、日志信息以及文件版本等。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678950018038-c2ebf2e8-fdef-4343-a2a0-4792567c0eba.png)

工作区文件状态：

- `untracked` 未跟踪（未被纳入版本控制）
- `tracked` 已跟踪（被纳入版本控制）
	- 
- `Unmodified` 未修改状态（没有修改的文件）
- `Modified` 已修改状态（提交版本库）
- `Staged` 已暂存状态（提交暂存区）

## 3.4 本地仓库操作

|**常用命令**|**作用**|**示例**|
|---|---|---|
|`git status`|查看文件状态，没有保存更改的文件|`git status`|
|`git add`|将文件的修改加入暂存区|`git add fileName`（支持通配符）|
|`git reset`|将暂存区的文件取消暂存或者是切换到指定版本|`git reset 文件名` `git reset --hard 版本号`（版本号在日志信息中）|
|`git commit`|将暂存区的文件修改提交到版本库|`git commit -m msg 文件名`（支持通配符，-m 表示提交说明）|
|`git log`|将暂存区的文件修改提交到版本库|`git log`|

## 3.5 远程仓库操作

|**常用命令**|**作用**|**示例**|
|---|---|---|
|`git remote`|查看远程仓库|`git remote`返回远程仓库的简称（默认origin），`-v`参数可以查看更详细的信息|
|`git remote add`|添加远程仓库|`git remote add 远程仓库简称 远程仓库地址`|
|`git clone`|从远程仓库克隆|`git clone 远程仓库地址`|
|`git pull`|从远程仓库拉取|`git pull 远程仓库简称 分支名称`|
|`git push`|推送到远程仓库|`git push 远程仓库简称 分支名称`|

- 如果Idea推送不成功，检查是否代理，Git同样
- git的远程仓库是属于每个存储库而不是全局的。
- 如果提交被吞，可以使用修正功能。
- 一个仓库可以有多个分支，默认情况下在创建**本地仓库**后会自动创建一个 `master` 分支，而 **Github** 上创建一个远程仓库会自动创建一个 `main` 分支。
- 如果是通过 `init` 命令创建的仓库，并且仓库中存在文件，`pull` 命令会报错，加上`-allow-unrelated-histories`解决。

## 3.6 分支操作

分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。

分支对于 Git 尤为重要，不同的版本控制系统中也有分支，但他们的分支是创建一个副本，这是重量级的，尤其对于大型项目非常困难、繁杂。

但是 Git 中的分支，只是一个指针 head，创建、合并分支，都只是在移动 head 指针，这就是 Git 能脱颖而出的关键。

一开始的时候，master 分支是一条线，Git 用 master 指向最新的提交，再用 HEAD 指向 master，就能确定当前分支，以及当前分支的提交点：


![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678952620705-6280f46c-7f74-4305-94ea-401680b48c46.png)

每次提交，master 分支都会向前移动一步，这样，随着你不断提交，master 分支的线也越来越长。

当我们创建新的分支，例如 dev 时，Git 新建了一个指针叫 dev，指向 master 相同的提交，再把 HEAD 指向 dev，就表示当前分支在 dev 上：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678952651757-8ab9f435-83f0-4ad0-9017-157f983b35a2.png)

可以看到 Git 创建一个分支很快，因为除了增加一个 dev 指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对 dev 分支了，比如新提交一次后，dev 指针往前移动一步，而 master 指针不变：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678952681389-abdcc20f-3728-426d-b697-c4f9cb29b48f.png)

假如我们在 dev 上的工作完成了，就可以把 dev 合并到 master 上。Git 怎么合并呢？最简单的方法，就是直接把 master 指向 dev 的当前提交，就完成了合并：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678952716498-9262d0d6-8925-41d8-98e1-2fd135fad902.png)

合并完分支后，甚至可以删除 dev 分支。删除 dev 分支就是把 dev 指针给删掉，删掉后，我们就剩下了一条 master 分支：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678952735956-2fde696b-6a8c-4120-a817-e991cda9d7ea.png)

|**常用命令**|**作用**|**示例**|
|---|---|---|
|`git branch`|查看分支|`git branch`默认列出所以本地分支；`-r`参数列出所有远程分支；`-a`参数列出所有本地和远程分支|
|`git branch [name]`|创建分支|`git branch 分支名称`|
|`git checkout [name]`|切换（签出）分支|`git checkout 分支名称`|
|`git push [shortName] [name]`|推送至远程仓库分支|`git push 远程仓库简称 分支名称`|
|`git merge [name]`|合并分支|`git merge 分支名称`把目标分支合并到当前分支|
|`git push``gitbranch`|删除分支|`git branch -d 本地分支名``git push 远程仓库简称 -d 远程仓库分支名`|
|git branch -m <new-branch-name>|更改当前分支名称||
|git branch -m <old-branch-name> <new-branch-name>|更改指定分支名称||

如果你已经将分支推送到远程，需要更新远程分支的名称：

1. **删除远程旧分支**（这将从远程删除旧分支）：
    
    ```bash
    bash
    复制代码
    git push origin --delete <old-branch-name>
    
    ```
    
2. **推送新分支到远程**：
    
    ```bash
    bash
    复制代码
    git push origin <new-branch-name>
    
    ```
    
3. **重设上游分支**（如果需要）：
    
    ```bash
    bash
    复制代码
    git push --set-upstream origin <new-branch-name>
    
    ```
    

如果对不同分支都有提交，合并分支时就会产生冲突：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678953581679-4aa12953-fc05-4dde-b75c-14dd5f279937.png)

这时必须手动解决冲突，选择最终提交版本。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678953735977-389d845c-69f7-47d3-9af2-fb2cd5ebe23a.png)

## 3.7 标签操作

Git 中的标签，指的是某个分支某个特定时间点的状态。通过标签，可以很方便的切换到标记时的状态。

通常使用标签进行版本发布。

|**常用命令**|**作用**|**示例**|
|---|---|---|
|`git tag`|查看标签|`git tag`|
|`git tag [name]`|创建标签|`git tag 标签名`|
|`git push [shortName] [name]`|将标签推送至远程仓库|`git push 远程仓库简称 标签名`|
|`git checkout -b [branch] [name]`|检出标签|`git checkout -b 分支名 标签名`以标签指定的版本为基础版本，新建一个分支|

# 4. Idea 集成Git

使用命令行方式略显复杂，作为最强大的 IDE，Idea 可以简化我们操作 Git。

## 4.1 配置 Git

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678958827520-9a3e2b2f-0ea5-4ca5-a0d1-a8246f33edae.png)

## 4.2 Git 忽略文件

Git 工作区中有一个特殊文件 `.gitignore`，用于过滤不需要版本控制的文件。

使用 Git Bash 进入项目根目录，输入`touch .gitignore`创建忽略文件，也可以通过远程仓库创建。

忽略文件需要加入版本控制，并且对于已经提交到暂存区的文件不能忽略。

**.gitignore模板**

```bash
# 已编译的类文件*.class# 日志文件*.log# BlueJ files*.ctxt# Java移动工具 (J2ME).mtj.tmp/# 包文件 #*.jar*.war*.nar*.ear*.zip*.tar.gz*.rar# 虚拟机崩溃日志, see <http://www.java.com/en/download/help/error_hotspot.xmlhs_err_pid*replay_pid*>
```

在 Git 仓库的子文件夹中存在 `.gitignore` 文件是完全可以的，并且这种做法在实际开发中非常常见。以下是一些关于子文件夹中 `.gitignore` 文件的作用和注意事项：

### 作用

1. **局部忽略规则**：
    - 子文件夹中的 `.gitignore` 文件可以定义该特定文件夹下的忽略规则，而不影响上级目录或整个仓库的 `.gitignore` 设置。这样可以为不同模块或功能分别管理忽略文件。
2. **提高可读性**：
    - 在大型项目中，使用局部 `.gitignore` 文件可以使每个模块的忽略规则更清晰，便于团队成员理解和维护。
3. **特定环境配置**：
    - 可以根据项目的不同部分需要，配置不同的忽略规则，比如在测试文件夹中忽略生成的临时文件，而在源代码文件夹中则忽略编译生成的文件。

### 示例

假设你的项目结构如下：

```css
css
复制代码
my-project/
├── .gitignore
├── src/
│   ├── .gitignore
│   └── main.java
└── test/
    ├── .gitignore
    └── test.java

```

- 在根目录的 `.gitignore` 文件中，你可能会忽略全局的构建输出：
    
    ```
    plaintext
    复制代码
    /target/
    *.log
    
    ```
    
- 在 `src` 目录的 `.gitignore` 文件中，你可能想要忽略特定的 IDE 配置文件：
    
    ```
    plaintext
    复制代码
    .idea/
    *.iml
    
    ```
    
- 在 `test` 目录的 `.gitignore` 文件中，你可能会忽略测试生成的临时文件：
    
    ```
    plaintext
    复制代码
    /temp/
    *.class
    
    ```
    

### 注意事项

- **优先级**：子文件夹中的 `.gitignore` 文件会覆盖上级目录的设置，具体行为遵循 Git 忽略规则的优先级。
- **合并**：如果子文件夹中的 `.gitignore` 文件与根目录的设置存在冲突，确保团队成员了解每个目录中的规则。
- **清理已跟踪文件**：如果某个文件已经被 Git 跟踪，即使在 `.gitignore` 中添加了忽略规则，该文件仍然会继续被跟踪。需要使用 `git rm --cached <file>` 命令来停止跟踪。

### 总结

在 Git 仓库的子文件夹中使用 `.gitignore` 文件是管理不同模块或部分的忽略规则的有效方法。这种做法有助于提高项目的可维护性和可读性。如果你有更多关于 `.gitignore` 或 Git 的问题，欢迎继续交流！