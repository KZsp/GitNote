##### 1、安装

git官网可下载



##### 2、配置

> Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。
>
> git安装目录下的`/etc/gitconfig` 文件是所有用户的配置，使用`git config --system`设置时修改该文件
>
> C盘用户目录的`~/.gitconfig`是当前用户的配置，使用 `git config --global` 设置时修改该文件

1. 配置用户信息

   ```bash
   git config --global user.name "lyl"
   git config --global user.email 2061999999@qq.com
   ```

2. 查看所有配置信息

   ```bash
   git config --list
   ```



##### 3、概念

**指令和流程：**

![img](git.assets/git-command.jpg?raw=true)

workspace：工作区，一般通过git checkout切换分支达到跳转工作区（分支）

staging area：暂存区

local repository：本地仓库

remote repository：远程仓库



**Git和Github的关系：**

git是分支管理工具，github是代码托管平台，相当于上图中的远程仓库，远程仓库可以是github，也可以是自己的电脑（通过ssh协议访问）



**注意点：**

github现在默认主分支为main，而不是master



##### 4、快速上手

1. 初始化本地仓库，这里两种方式：从远程仓库克隆，本地新建。

   ```bash
   # 初始化当前文件夹为git仓库，其实就是创建了.git文件夹，里面是git的一堆配置
   git init
   
   # 初始化指定目录为git仓库，并在指定目录里创建.git文件夹
   git init .\Downloads\hh
   ```

   ```bash
   # 从目标远程仓库地址克隆到本地，克隆下来的此时就是本地仓库
   git clone [url]
   ```

2. 查看文件状态。（文件在工作区还是暂存区）

   红色说明文件未添加到暂存区，绿色说明文件添加到暂存区了。并且文件前面的数字表示了发生了什么状态改变，如果是A，说明是新增(add)，如果是M，说明是修改(modif)

   ```bash
   # 会显示新增、删除、修改过的文件或目录
   git status -s
   ```

3. 查看工作区文件和暂存区文件的区别

   ```bash
   # 一般在git add后就没有区别了，在git add前，文件修改后，使用此指令能看出哪里改变
   git diff
   ```

4. 将文件从工作区添加到暂存区

   ```bash
   # 添加当前目录的所有修改文件到暂存区
   git add .
   
   # 添加多个修改的文件到暂存区
   git add [file1] [file2] ...
   
   # 添加目录里所有增加的文件到暂存区，包括子目录
   git add [dir]
   ```

5. 提交到本地仓库

   ```bash
   # 提交到本地仓库，-m是说明这次提交了什么
   git commit -m "在xxx的基础上增加了xxx"
   ```

6. 版本回退。（从本地仓库回退commit版本，一般不会使用回退，谨慎使用）

   ```bash
   # 格式：
   git reset [--soft | --mixed | --hard] [HEAD]
   
   # --soft参数是表示回退到指定版本，但是已经修改了的不会改变，文件依然存在
   # --mixed表示重置暂存区，于上次commit保持一致，不需要后面的HEAD参数
   # --hard回退到指定版本，并且修改的内容也会回退，谨慎使用
   
   # HEAD表示当前版本
   # HEAD^1表示上一个版本,HEAD^2表示上上一个版本
   # HEAD^表示上一个版本，HEAD^^表示上上一个版本
   # HEAD~1表示上一个版本，HEAD~2表示上上一个版本
   ```

   案例：

   ```bash
   # 清空暂存区，也就是add了但是没有commit的内容
   git reset --mixed
   
   # 回退到上一版本，修改的内容不会发生回退，只是commit版本号回退
   git reset --soft HEAD^1
   
   # 回退到上一版本，修改的内容也会回退，慎用
   git reset --hard HEAD^1
   ```

7. 删除文件。直接删除还需要手动add，然后commit，才能被git记录，这里直接使用git命令删除

   ```bash
   # 从工作区删除a.txt文件
   git rm a.txt
   
   # 从工作区和暂存区删除a.txt文件（如果文件被add到暂存区，想要删除需要带-f参数，强制）
   git rm -f a.txt
   
   # 从暂存区删除a.txt文件，但是工作区的a.txt保留
   git rm --cached a.txt
   ```

8. 移动文件

   ```bash
   # 把当前目录的a文件移动为b文件，简单说是重命名
   git mv a b
   
   # 如果b文件存在，使用-f可以强制重命名并覆盖存在的文件
   git mv -f a b
   ```

9. 查看日志

   ```bash
   # 查看历史提交记录
   git log
   
   # 查看简洁版历史
   git log --oneline
   
   # 查看简洁版历史，并且查看什么时候出现了分支合并
   git log --oneline --graph
   
   # 逆向查看简洁版历史
   git log --oneline --reverse
   
   # 查看简洁版历史中，提交作者是lyl的最新5条记录
   git log --author=lyl --oneline -5
   
   # 查看简洁版历史中，当前时间的三周前到2021-02-20之间的提交记录，不显示合并提交
   git log --oneline --after=3.weeks.ago --before=2021-02-20 --no-merges
   ```

10. 查看指定文件的修改记录

    ```bash
    # 查看a.txt的修改记录
    git blame a.txt
    ```

11. 本地仓库设置远程仓库

    ```bash
    # 显示当前本地仓库配置的远程仓库信息
    git remote -v
    
    # 本地仓库配置添加远程仓库，别名为origin
    # 这里的链接使用https协议，ssh协议需要密钥，详情看ssh，github创建远程仓库详情看后面章节
     git remote add origin https://github.com/xx/xx
    
    # 查看远程仓库的信息
    git remote show https://xxx.xxx
    
    # 修改本地仓库配置的远程仓库名origin为test
    git remote rename origin test
    
    # 删除远程仓库test
    git remote rm test  
    ```

12. pull、push、fetch。

    使用fetch从远程获取别人提交的更新

    ```bash
    # 告诉git去别名为origin的远程仓库获取新增的数据
    git fetch origin
    ```

    如果返回空，则说明远程仓库的内容在我pull或clone下来之后就没有更改过，不用合并远程仓库分支到本地仓库分支；否则执行如下：

    ```bash
    # 合并别名为origin的远程仓库的master分支到本地仓库的master分支
    git merge origin/master
    ```

    确认同步完成后，使用push将本地仓库分支合并到远程仓库分支

    ```bash
    # git push 远程仓库地址别名 本地分支:远程分支
    git push origin master:master
    
    # 如果需要提交的本地分支和远程分支相同，则可以简写
    git push origin master
    
    # 如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：
    git push --force origin master
    
    # 删除远程仓库的master分支
    git push origin --delete master
    ```

    pull的作用是让远程分支合并到本地分支，相当于fetch后的merge简写

    ```bash
    # 将别名为origin的远程仓库的master分支合并到本地的brantest分支
    git pull origin master:brantest
    
    # 将别名为origin的远程仓库的master分支合并到当前本地分支
    git pull origin master
    ```

    

13. 打标签。标签一般用来打版本号，因为版本号比commit号好记忆，类似于域名和ip的关系。

    ```bash
    # 查看全部标签
    git tag
     
    #增加一个标签
    git tag v1.0
     
    # 增加一个需要输入内容的标签，记录日志，-a是注解，使用-a会自动调用安装git时的选项，我选的vscode，需要输入一些内容，标签才会创建成功
    git tag -a v1.0
     
    # 不小心提交后，对目标commit版本85fc7e7xxxxx进行追加标签
    git tag -a v0.9 85fc7e7
     
    # 通过日志查看标签，--decorate让标签显现
    git log --oneline --decorate --graph
    
    # 指定标签描述信息
    git tag -a v1.0 -m "该版本增加了很多东西"
     
    # PGP签名标签
    git tag -s v1.0 -m "PGP签名"
     
    # 删除标签
    git tag -d v1.1
     
    # 显示指定标签的信息
    git show v1.0
    ```



##### 5、分支

1. 查看分支。查看当前本地仓库的所有分支

   ```bash
   # 查看所有分支，默认存在master分支（git init创建的）
   git branch
   ```

2. 创建分支。创建一个新的分支，新分支内容从主分支复制的

   ```bash
   # 创建一个叫test的分支
   git branch test
   ```

3. 删除分支

   ```bash
   # 删除test分支
   git branch -d test
   ```

4. 切换分支，切换分支会删除未提交的修改，需要注意

   ```bash
   # 切换到test分支
   git checkout test
   
   # 切换到test分支，不存在就创建
   git checkout -b test
   ```

5. 合并分支，将test分支合并到主分支，合并后test分支的版本就是旧的，master分支就是新的，此时即使修改了master分支里面的文件，合并test分支过来也会说主分支已经是最新的，什么也没有合并过来，所以如果想让test分支继续使用，可以选择先把master分支合并到test分支，然后就同步了，或者删除test分支，新建test分支。

   ```bash
   # 假设当前是master分支，下面的命令是合并test分支到master分支
   git merge test
   ```

6. 合并冲突。假设主分支修改Z.txt文件内容是

   ```txt
   <hello>
   你好，ZA
   </hello>
   ```

   test分支修改Z.txt文件内容是

   ```txt
   <hello>
   你好，Z
   </hello>
   ```

   此时合并就会说冲突了

   ```bash
   Auto-merging hah/Z.txt
   # conflict是冲突的意思，这里指出了冲突的文件在哪里，in hah/Z.txt里面
   CONFLICT (content): Merge conflict in hah/Z.txt
   Automatic merge failed; fix conflicts and then commit the result.
   ```

   打开Z.txt后显示：

   ```text
   <hello>
   <<<<<<< HEAD
   你好，ZA
   =======
   你好，Z
   >>>>>>> master
   </hello>
   ```

   意思是说两文件不同的位置，需要手动修改，这里我选择保留前面的。

   ```txt
   <hello>
   你好，ZA
   </hello>
   ```

   保存后，通过add和commit提交后，就解决了分支冲突问题



##### 6、Git使用SSH协议

1. 使用SSH密钥工具能生成公钥和私钥

   ```bash
   #          -t是密钥类型  -C是备注  -o说是更强的防暴力破解
   ssh-keygen -t rsa -C "2061@qq.com" -o
   ```

2. 提示输入密钥保存路径，默认`~/.ssh/id_rsa`，这里回车默认

3. 提示输入密码或空，输入密码则还会出现确认密码，这里我选择输入密码

4. 创建成功后会在默认路径`~/.ssh/id_rsa`生成`id_rsa`、`id_rsa.pub`和`known_hosts`文件，pub后缀为公钥，id_rsa为私钥，其中known_hosts是记录最近认证的地址

5. 将公钥以记事本打开，复制到github的账户设置里面的ssh密钥，私钥保存在本机就ok

6. 此时将ssh链接添加到本地仓库后，显示远程仓库信息时会出现提示

   ```bash
   # 将ssh链接添加到本地仓库
   git remote add origin git@github.com:KZsp/KZsp.git
   # 显示远程仓库信息
   git remote show git@github.com:KZsp/KZsp.git
   ```

   提示目标ip是第一次，需要输入密钥的密码，输入完密码后，就认证成功了，之后每次链接都需要输入密码

   ```bash
   Warning: Permanently added the RSA host key for IP address '52.74.223.119' to the list of known hosts.
   Enter passphrase for key '/c/Users/LYL/.ssh/id_rsa':
   * remote git@github.com:KZsp/KZsp.git
     Fetch URL: git@github.com:KZsp/KZsp.git
     Push  URL: git@github.com:KZsp/KZsp.git
     HEAD branch: main
   ```

   

##### 7、Github创建远程仓库

有手就行



##### 8、Git遇到的问题

github上的远程仓库默认分支是main，而我本地初始化的仓库是master，此时我提交上去，会导致新建一个分支，所以先删除了远程仓库的master分支，然后我尝试在本地仓库创建main分支，切换到main分支后，删除master分支，此时，pull后再push会出现如下异常：

```bash
To github.com:KZsp/KZsp.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:KZsp/KZsp.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

解决办法：

```bash
# 先使用如下方式把远程仓库分支和本地分支合并，参数是允许不相关的历史记录。
git pull origin main --allow-unrelated-histories

# 然后就可以push了
git push origin main:main
```


