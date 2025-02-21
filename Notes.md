# MIT Missing Semester Notes
---
## 1. Shell
常用命令：
- `ls`：列出目录内容
  - `-l`：显示详细信息
- `cd`：切换目录
- `pwd`：显示当前目录
- `echo`：输出
- `cat`：查看文件内容
- `which`：查看命令位置
- `man`：查看命令手册
- `mkdir`：创建目录
- `$PATH`：(查看)环境变量
- `mv`：移动或重命名文件
- `cp`：复制文件

Shell中使用`/`表示目录，`~`表示家目录，`.`表示当前目录，`..`表示上级目录。
例如：
  - `cd ~/Dev`表示切换到`home`目录下的`Dev`文件夹。
  - `cd ../..`表示切换到上上级目录。
  - `cd -`表示切换到上次所在目录。

如果某个路径以`/`开头，表示绝对路径，否则表示相对路径。
如果不指定路径，程序默认执行当前路径，如`ls`，直接使用会输出当前目录下的文件。
通常情况下使用`-h`或`--help`可以查看命令（程序）的帮助信息。
```shell
lightcone@Lightcone:~$ ls /
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
lightcone@Lightcone:~$ ls / -l
total 2152
lrwxrwxrwx   1 root root       7 May  2  2023 bin -> usr/bin
drwxr-xr-x   2 root root    4096 Mar 10 17:31 boot
drwxr-xr-x  16 root root    3260 Jul 14 09:19 dev
drwxr-xr-x 137 root root   12288 Jul 14 09:20 etc
drwxr-xr-x   3 root root    4096 Dec  3  2023 home
-rwxrwxrwx   1 root root 2127224 Apr 26 02:17 init
lrwxrwxrwx   1 root root       7 May  2  2023 lib -> usr/lib
lrwxrwxrwx   1 root root       9 May  2  2023 lib32 -> usr/lib32
lrwxrwxrwx   1 root root       9 May  2  2023 lib64 -> usr/lib64
lrwxrwxrwx   1 root root      10 May  2  2023 libx32 -> usr/libx32
drwx------   2 root root   16384 Apr 11  2019 lost+found
drwxr-xr-x   2 root root    4096 May  2  2023 media
drwxr-xr-x   7 root root    4096 Jul 14 09:19 mnt
drwxr-xr-x   2 root root    4096 May  2  2023 opt
dr-xr-xr-x 361 root root       0 Jul 14 09:19 proc
drwx------   4 root root    4096 Mar 10 18:18 root
drwxr-xr-x  24 root root     640 Jul 14 09:20 run
lrwxrwxrwx   1 root root       8 May  2  2023 sbin -> usr/sbin
drwxr-xr-x   8 root root    4096 May  2  2023 snap
drwxr-xr-x   2 root root    4096 May  2  2023 srv
dr-xr-xr-x  11 root root       0 Jul 14 09:19 sys
drwxrwxrwt   9 root root    4096 Jul 14 09:19 tmp
drwxr-xr-x  14 root root    4096 May  2  2023 usr
drwxr-xr-x  14 root root    4096 Mar 10 17:32 var
lightcone@Lightcone:~$
```
上方代码中`d`代表是目录，`rwx`分别代表文件所有者、文件所属组、其他用户的权限。其中`r`表示读权限，`w`表示写权限，`x`表示执行权限。

**在程序间创建连接：**
在程序中主要有两个"流"，输入流和输出流。输入流是程序从外部获取数据，输出流是程序向外部输出数据。最简单的定向是使用`>`和`<`，`>`表示将输出流重定向到文件，`<`表示将输入流重定向到文件。
```shell
lightcone@Lightcone:~$ echo "Hello, World!" > hello.txt
lightcone@Lightcone:~$ cat hello.txt
Hello, World!
lightcone@Lightcone:~$ cat < hello.txt
Hello, World!
lightcone@Lightcone:~$
```
`>>`表示将输出流追加到文件，`2>`表示将错误流重定向到文件。
```shell
lightcone@Lightcone:~$ echo "Hello, World!" > hello.txt
lightcone@Lightcone:~$ echo "Hello, World!" >> hello.txt
lightcone@Lightcone:~$ cat hello.txt
Hello, World!
Hello, World!
lightcone@Lightcone:~$ ls /notexist 2> error.txt
lightcone@Lightcone:~$ cat error.txt
ls: cannot access '/notexist': No such file or directory
lightcone@Lightcone:~$
```
使用`|`可以将一个程序的输出流重定向到另一个程序的输入流。
```shell
lightcone@Lightcone:~/ShellLearning$ ls / | grep bin
bin
sbin
lightcone@Lightcone:~/ShellLearning$
```
`grep`是一个强大的文本搜索工具，可以使用正则表达式搜索文本。
上面这一句的意思是列出`/`目录下的文件，并将结果通过管道传递给`grep`，`grep`搜索包含`bin`的行。
还可以这样：
```shell
lightcone@Lightcone:~/ShellLearning$ ls / -l | tail -n1
drwxr-xr-x  14 root root    4096 Mar 10 17:32 var
lightcone@Lightcone:~/ShellLearning$
```
`tail`默认显示文件的最后10行，`-n`参数可以指定显示的行数。

root用户是Linux系统中的超级用户，拥有最高权限。但我们不应该总是以root用户的身份运行程序，因为这样会增加系统被攻击的风险。我们可以使用`sudo`命令来以root用户的身份运行程序。`sudo`是`do as superuser`的缩写，可以让普通用户以root用户的身份运行程序。
```shell
lightcone@Lightcone:~$ sudo ls /root
[sudo] password for lightcone:
ls: cannot open directory '/root': Permission denied
lightcone@Lightcone:~$
```

`tee`命令可以将输入流输出到文件，并将输入流传递给下一个程序。如果没有定向下一个程序，`tee`会将输入流输出到标准输出。
```shell
lightcone@Lightcone:~$ echo "Hello, World!" | tee hello.txt | cat
Hello, World!
lightcone@Lightcone:~$
```

---
## 课后习题·Shell
1. 在 /tmp 下新建一个名为 missing 的文件夹。
```shell
lightcone@Lightcone:~/ShellLearning$ mkdir tmp; cd ./tmp; mkdir missing
lightcone@Lightcone:~/ShellLearning/tmp$ ls
missing
lightcone@Lightcone:~/ShellLearning/tmp$
```
2. 用 man 查看程序 touch 的使用手册。
```shell
lightcone@Lightcone:~/ShellLearning man touch
```
3. 用 touch 在 missing 文件夹中新建一个叫 semester 的文件。
```shell
lightcone@Lightcone:~/ShellLearning/tmp$ touch missing/semester
lightcone@Lightcone:~/ShellLearning/tmp$ ls missing
semester
lightcone@Lightcone:~/ShellLearning/tmp$
```
4. 将以下内容一行一行地写入 semester 文件：
    ```shell
    #!/bin/sh
    curl --head --silent https://missing.csail.mit.edu
    ```
```shell
lightcone@Lightcone:~/ShellLearning/tmp/missing$ echo '#!/bin/sh' | tee semester
#!/bin/sh
lightcone@Lightcone:~/ShellLearning/tmp/missing$ echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
lightcone@Lightcone:~/ShellLearning/tmp/missing$ cat semester
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
lightcone@Lightcone:~/ShellLearning/tmp/missing$
```

---
使用`$`可以访问变量，例如
```shell
lightcone@Lightcone:~$ a=Hello
lightcone@Lightcone:~$ echo $a
Hello
lightcone@Lightcone:~$
```
`$0`表示程序名，`$1`表示第一个参数，`$2`表示第二个参数，以此类推。`$#`表示参数个数，`$@`表示所有参数，`$*`表示所有参数，但是会将所有参数当作一个参数。

## 2. Git
Git是一个分布式版本控制系统，可以追踪文件的变化。
##### Git的三个区域：
- **工作区**：处理工作的区域
- **暂存区**：临时存放文件的区域
- **本地git仓库**：最终存放文件的区域

##### 在文件夹的体现如下：
- **工作区**：电脑中看到的目录
- **暂存区**：`.git`目录下的`index`文件（二进制文件）
- **版本库**：整个`.git`目录

##### Git配置：
- `git config --global user.name "Your Name"`：设置用户名
- `git config --global user.email "Your Email"`：设置邮箱
每一个Git提交都会记录这两个信息。

可以使用`git config --list`查看配置信息。

##### 文件右侧标记
使用VsCode打开包含Git仓库的文件夹，会有如下标记：
- `M`：文件被修改但还没有添加到暂存区
- `A`：文件已经添加到暂存区，但还没有提交
- 'D'：文件被删除但还没有提交
- 'R'：文件被重命名但还没有添加到暂存区
- 'C'：文件被复制但还没有添加到暂存区
- 'U'：文件有冲突，需要解决

#### Git基础命令
- `git init`：初始化一个Git仓库，会在当前目录下生成一个`.git`目录
##### 记录更新到Git仓库：
每当完成一个阶段的目标，想要记录下来，就可以将其提交到Git仓库。
核心操作:`工作区开发`-->`将修改后的文件添加到暂存区`-->`将暂存区的文件提交到本地Git仓库`
- `git add <file>`：将文件添加到暂存区
```shell
# 将 index.html 添加到暂存区
git add index.html
# 将css目录下的所有文件添加到暂存区
git add css/
# 将所有文件添加到暂存区
git add .
```
将暂存区内容提交到版本库：
- `git commit -m "message"`, `-m`后面的内容是提交的内容说明
过程如下：
![alt text](image.png)
以上命令相当于存档了一次，在版本库中产生一次提交记录并生成一个版本号。
存档不耽误工作，可以继续修改文件，然后再次存档。

##### Git日志及状态查看
查看所有提交的日志记录：
- `git log`
运行命令效果如下：
![alt text](image-1.png)
如果想查看最近一次提交的内容：
- `git show`
日志很多，想要简化查看：
- `git log --oneline`
![alt text](image-2.png)

##### Git版本回退
- `git reset --hard HEAD^`：回退到上一个版本
- `git reset --hard 版本号`：回退到指定版本
- `git reflog`：查看命令历史，可以找到版本号
```shell
(base) PS E:\Education\课程项目\Asset\MIT-Missing-Semester> git reflog
bf8187a (HEAD -> master) HEAD@{0}: commit: 添加Git部分命令
a6b3cc4 HEAD@{1}: commit (initial): 添加Git学习部分
(base) PS E:\Education\课程项目\Asset\MIT-Missing-Semester>
```
其中`(HEAD -> master)`表示当前版本，`HEAD@{0}`表示当前版本，`HEAD@{1}`表示上一个版本。
`commit(initial)`表示初始提交，`commit`表示普通提交。

果然，刚刚那句话没有出现在这里了，因为我回退到了上一个版本。哈哈哈哈哈哈。

##### Git忽略文件
在Git仓库中，有些文件或文件夹不需要被Git管理，这时可以在仓库根目录下创建一个`.gitignore`文件，将不需要被Git管理的文件或文件夹写入其中。
例如：
```txt
# .gitignore 文件：
# 忽略文件夹
css
# 忽略文件
css/style.css
# 忽略所有 .txt 文件
*.txt
```

##### Git分支
分支是Git的一个重要概念，可以将开发分支和主分支分开，这样可以在开发分支上进行开发，不影响主分支。
分支本质上是一个指向某个提交的一个叫做`HEAD`的指针。每次提交，`HEAD`指针都会向前移动。`HEAD`指针指向的分支叫做当前分支，会影响工作区里的代码。
例如，当我们输入命令`git reset --hard a6b3cc4`时，`HEAD`指针会指向`a6b3cc4`，当前分支会指向`a6b3cc4`，工作区里的代码也会变成`a6b3cc4`的代码。

###### 创建分支
- `git branch <branchname>`：创建一个新的分支
该命令创建了一个新的分支，但是并没有切换到新的分支。我们可以查看所有分支：
- `git branch`
查看分支的效果如下：
![alt text](image-3.png)
其中`*`表示当前分支。
- `git checkout <branchname>`：切换到指定分支
第一次创建并切换到新的分支，可以发现工作区里的代码和新的分支的代码一样。我们可以在此基础上进行开发，而不影响主分支。

这里就是新的分支开发了。哈哈哈哈哈哈哈。
如果我们查看`.git`目录，可以发现其版本树结构：
```
object/master
  / commit1 a6b3cc4
  / commit2 bf8187a
  ...
object/branch
  / commit1 a8b3do9
  / commit2 ut8r34a
  ...
```
![alt text](image-4.png)

###### 合并分支
当我们在新的分支上开发完成后，我们可以将新的分支合并到主分支上。
首先，我们需要切换到目标分支上：
- `git checkout master`
切换分支后，`HEAD`指针位置如下：
![alt text](image-5.png)
合并分支：
- `git merge <branchname>`
- 合并后效果如图：
![alt text](image-6.png)
可见，`master`分支已经包含了`branch`分支下最新一次提交的代码。且版本号与`branch`分支下最新一次提交的版本号一样。

##### 分支删除
加入某个分支开发完毕，代码已经合并到主分支，那么可以删除该分支：
- `git branch -d <branchname>`
如果分支没有合并到主分支，Git会提示无法删除分支。如果需要强行删除分支，可以使用：
- `git branch -D <branchname>`

##### 合并分支时的冲突问题
当创建新分支后右在主分支上进行了修改并提交（例如修改了`login.html`文件），并在新分支上也修改了`login.html`文件并提交，那么在合并分支时就会出现冲突。
我们需要进行抉择：采用主分支的、新分支的、或者两者的结合。

##### Git分支流程图详解
不管在什么分支下，版本号是通用货币，每开辟一个分支，相当于在版本树上新增一个记录节点。
现在只有`master`分支，`HEAD`指向`master`分支，`master`分支指向最新的提交。
![alt text](image-7.png)
为当前版本创建一个新的分支`reg`
![alt text](image-8.png)
切换到`reg`分支进行开发，`HEAD`指向`reg`分支，`reg`分支指向最新的提交。
![alt text](image-9.png)
在`reg`分支上提交了新的代码，`reg`分支指向最新的提交。
![alt text](image-10.png)
又在`reg`分支上提交了新的代码，`reg`分支指向最新的提交。
![alt text](image-11.png)
切换到主分支上，`HEAD`指向`master`分支：
![alt text](image-12.png)
合并`reg`分支到`master`分支，`master`分支指向`reg`最新的一次提交：
![alt text](image-13.png)
从某一版本号开始，`master`分支修改并提交了新的代码，而`reg`分支也修改并提交了新的代码：
![alt text](image-14.png)
此时想要将`reg`分支合并到`master`分支，就会出现冲突：
![alt text](image-15.png)
手动解决了冲突后，在`master`分支上提交了新的代码（产生了新的版本号）：
![alt text](image-16.png)
删除了`reg`分支，开发过程结束：
![alt text](image-17.png)

##### Git远程仓库
远程仓库是指存放在网络服务器上的Git仓库，可以在多台电脑上协同开发。
![alt text](image-18.png)
由于每次`push`都需要带上远程仓库的地址，非常麻烦，可以使用`git remote add 仓库别名 仓库地址`命令来简化操作。
例如：
- `git remote add origin https://github.com/FrankLightcone/Miss-Semester-Notes.git`
如果要删除这个别名，可以使用：
- `git remote remove origin`

想要将本地仓库的内容推送到远程仓库，可以使用：
- `git push origin master`，`origin`是远程仓库的别名，`master`是要推送的分支。
- `git push origin --all`：推送所有分支

如果想要将远程仓库的内容拉取到本地仓库，可以使用：
- `git pull origin master`，`origin`是远程仓库的别名，`master`是要拉取的分支。
- `git pull origin --all`：拉取所有分支
- `git pull`：拉取当前分支
- `git pull --rebase`：拉取并合并
- `git pull --rebase origin master`：拉取并合并指定分支
- `git pull --rebase origin --all`：拉取并合并所有分支

##### 克隆仓库
如果想要将远程仓库的内容克隆到本地，可以使用：
- `git clone 仓库地址`，`仓库地址`是远程仓库的地址。


### Git命令大全
![alt text](image-19.png)