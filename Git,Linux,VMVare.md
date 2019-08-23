# Git,Linux

## Git

### 远程仓库

`Git`是一款分布式版本控制系统，也即同一个`Git`仓库，可以分布到不同的机器上。最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。这里的别的机器就可以是：GitHub，这是一个非常nice的网站，可以作为你个人的远程仓库，如此一来你就能将你的代码托管到GitHub上。

#### 建立本地机器和远程仓库的连接

由于本地的仓库和远程的仓库之间的数据传输是通过shh加密的，所以需要进行一点设置，将本地家目录的ssh的```id_rsa_pub``` 公钥保存到GitHub远程仓库，这样做的目的是确保每一次提交到代码都是你本人提交，而不是别人冒充你提交的。

如果你的机器上暂时没有```.ssh``` 目录的话，表名你还没有进行过shh的秘钥生成，所以需要到家目录执行一下的命令：

~~~bash
ssh-keygen -t rsa -C "ifseayou@gmail.com"
~~~

一路回车，然后在进入```./ssh```  中复制公钥。添加到GitHub中（打开“Account settings”，“SSH Keys”页面）GitHub支持添加多个`SSHkey`，这表示你可以从多个终端向GitHub服务器提交代码。

### 提交

```shell
git init   # 初始化为本地仓库

git add . # 将该新增的文件添加到缓冲区（暂存区）

git status  # 查看文件有没有提交到缓冲区

git commit -m 'desc' # 将暂存区的文件提交到本地

git remote add origin git@github.com:*** # 将本地仓库和远端关联

git push origin master # 将暂存区的文件提交到远端
```

#### `git status`

该命令用来查看当前仓库的状态，`git status` 来显示一下是添加了新的文件，是否修改了文件，是否删除了文件，如下：

~~~shell 
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   MySQL.md  # 红色
# 上面表示修改了MySQL.md文件，可以将该文件通过git add 的方式将这一改变提交到缓冲区。

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   MySQL.md  # 绿色

# 我们已经将修改的文件提交到了缓冲区，两次modify的颜色是不一样的，接下来需要使用git commit提交到仓库。
~~~

如上，我们对文件进行了修改，如果此时想要看看哪里做了修改，我们可以使用

~~~shell
git diff MySQL.md # 可以查看该文件被修改的位置
~~~

### 克隆

~~~shell
# 首先进入某个文件目录，然后输入以下的命令：
git clone git@github.com:ifseayou/tech-doc.git 

# 如此一来，当前clone下来的项目会自己成为一个本地的仓库并且和远端关联。
~~~

### 建仓姿势

#### method one

平时建笔记的时候，只需要一个分支即可，搞那么多分支根本没有必要。我认为比较好的创建方式为：

① 先在`github`上建好项目，该项目名要想好

② 在本地的任意一个目录，clone `github`的项目，clone下来的项目自带`.git`文件。

如：

~~~shell
# 先在github建立infra-doc

# 在markdowns文件夹下clone
~~~

![](img/md/1.png)

#### method two

如果本地已经有了项目`tech-doc`，想要备份一下，需要在`github`建立好项目名`tech-doc`

~~~shell
# 在tech-doc下执行
git init 
git remote add origin url

# 然后就可以提交代码了
~~~

![](img/md/2.png)

### `gitignore`文件

有些时候，你必须把某些文件放到`Git`工作目录中，但又不能提交它们忽略文件的原则是：

* 忽略操作系统自动生成的文件，比如缩略图等；
* 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
* 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

### 分支

~~~ shell
# 查看分支
git branch

# 创建分支
git branch dev（分支名）

# 切换分支
git checkout dev

# 创建分支并切换
git checkout -b dev

# 合并某分支到当前的分支
git merge dev

# git push

# 分支删除
git branch -d dev
~~~

### 拉取

~~~ shell 
# 拉取某分支最新代码 
git pull origin master
~~~

### 多人协作开发：`commit + pull + push`

多人协作开发来讲，基本上多数人是从远程`clone` 一个项目到本地，各自开发各自的模块，如果现在自己开发完成之后，就把自己代码提交**`commit`**到本地，然后在从远程拉取**`pull`**最新的代码（因为可能有其他的开发小伙伴提交了新的代码），此时`gitbash`会提示请求合并，填写备注信息之后，在将代同步到远程仓库**`push`**

总结起来就是：

~~~shell
git commit
git pull # 如果远程仓库有变化，会有编辑提示merge信息要填写
git push
~~~



### dev和master的协同操作

就我个人的习惯来说，我会一直在dev上进行提交，在提交之后，回到master分支，然后在master分支合并dev分支，然后在master分支上提交到远程。

## Linux

### top

``Linux `` 环境下的任务管理器，实时显示各个进程的资源占用情况。

   该命令的详情查看： [top命令详解](https://www.cnblogs.com/taobataoma/archive/2007/12/26/1015167.html)

### 以root身份修改isea的用户名

~~~shell
passwd isea
# 连续两次输入密码即可
~~~

### 免密配置

**原理：公钥加密私钥解**

* 假设A想要远程B，在A上生成RSA密钥对，并将公钥发送到B，B机保存一份
* 如果A远程B，会询问B是否可以不要你的密码等你B，B发送一个用A的公钥加密的code
* A收到后用自己的私钥解开，证明我A是在你B上认证过的，并且我就是A本人。
* 由此，A可以远程B了

[步骤教程](<https://askubuntu.com/questions/46930/how-can-i-set-up-password-less-ssh-login>)

~~~shell
# 在A上执行：
ssh-keygen

# 在A上执行，也可以支持免密登录自己
ssh-copy-id -i id_rsa.pub root@192.168.1.31

# 在A上可以登录B
ssh 192.168.1.31
~~~

### 配置命令的随处访问

~~~shell
# 例如，root用户在/root下写了一个xcall脚本，需要在任何位置root可以执行该脚本

#方法：修改/etc/profile文件添加：
export PATH="$PATH:/root"

# 修改完成之后，
source /etc/profile
~~~

## `VMVare`

以独占文件的方式配置文件失败，另一个正在运行的VMware进程可能正在使用配置文件：

解决的办法是：直接去任务管理器中将所有的和`VMVare`相关的进程都杀掉。