# Git



### 安装

1.    windows 下载安装后。能够运行git bash 就是成功了。

2.    需要配置git config 变量。

      这个config 是git 自己的，在cmd 命令行中输入：

      1.    git config --global user.name ""
      2.    git config --global user.email ""

3.    之后可以用git config -l 来list所有的配置

4.    ![image-20210801113245996](https://github.com/Rainsong-fire/gitnote/blob/master/git/Git.assets/image-20210801113245996.png)

5.    因为git 是分布式版本控制系统，因此必须配置个人的信息。所以

      git config --global user.name 和email 是必须要有的。

### 创建版本库

1.    版本库又叫仓库，repository。就是一个可以被git 管理的目录，目录下所有的文件的变化，都会被git 跟踪。

2.    创建方式：

      先创建一个空目录，进入空目录后，用命令行键入命令：

      git init就可以了。

      在windows 下，可以借助power shell，更方便。

      可以看到出现了一个隐藏文件.git，这个文件是git 用来跟踪管理这个目录的。

      ![image-20210801115342148](https://i.loli.net/2021/08/02/tIGTiqlLcz9NrWX.png)

      在一个非空的文件夹内建立git repository也是可以的。



### 把文件添加到版本库

1.    所有的版本控制系统，都只能跟踪文本文件的改动。txt，网页，程序代码等等。

      这里的跟踪改动是指，在某一行加了什么，删除了什么这种细节都能够跟踪记录。

      而非文本文件，也能管理，但是不能跟踪。

2.    注意，不能用windows的记事本来编辑文本文件，因为编码问题。用visual studio code。

3.    添加文件：

      1.    要把想添加的文件放到被git 管理的那个目录下，不然git 找不到文件。

      2.    添加：

            在被管理的目录启动powershell：

            git add ......txt

            用tab键可以一个一个向下翻文件，就不用输入了。

            这一步没有返回，无返回，就是好事

      3.    提交：

            git commit -m "写入想写入的comment 的信息"

            -m 是写入提交的说明，这样方便在历史记录中寻找。手动输入时，在文件名子前要有 .\并且中间没有空格直接是文件名，文件名要有拓展名。

            add 和commit 是不同的行为。add 是添加文件到commit 区，暂存区，

            ```
              (use "git add <file>..." to update what will be committed)
              (use "git restore <file>..." to discard changes in working directory)
            ```

            而commit 是提交到库，也就是提交到当前的分支。可以认为，commit 是将所有被修改了的文件进行了一次整体的updata。

      总结：初始化一个目录成为git 目录：git init

      ​			添加一个文件 git add 然后用tab

      ​			提交一个文件 git commit -m ""

### git status

1.    当一个新文件被add和commit 后，他就算是正式成为了仓库中文件中的一个，并会被git 跟踪管理。

2.    当继续修改文件

      1.    git status 用来查看所有的变化。

            git status 是一个核心命令，他将显示：

            1.    changes not staged for commit

            2.    changes to be committed

            3.    被修改的文件是哪些，也可以用 git restore  来丢弃掉修改，只要未add都可以改回来。

                  这里的restore 有两重，第一重是从stage区撤出：

                  要加 --stage + filename。如：
            
                  ![image-20210801152526242](https://i.loli.net/2021/08/02/YFvMCOj3aNRg1lw.png)
            
                  
            
                  第二重是撤出工作区文件的所有的修改。
            
            4.    git diff 用来看某一个文件的变化。
            
                  绿色的为改变后的样子，白色的为没有改变的部分，红色为修改前的样子。
            
            5.    在add 之后，文件也可以用git store 再取消add。

3.    git status 可以查看整个仓库的状态，查看某个文件被修改了，而git diff 可以查看文件具体是如何被修改的。git diff 本质上看的是，工作区的文件和版本库中最新提交的那个文件之间的区别。

### 版本回退

1.    commit 文件进行了存档，可以进行版本的回退。

2.    用 git log 来查看版本存档，git log --pretty= oneline

      这样可以一行显示，信息省略。这里的存档，是提交后，commit后的存档。

      命令参数 --pretty=oneline，中间是没有空格的。

3.    git用head来表示当前版本。版本回退，可以理解为将head指针重新指向另外的commit 的版本。

      使用git reset --hard HEAD^ 

      或者是git reset --hard HEAD~2

      或者是git reset --hard 1083sdegtds（某个commit版本的sha，也就是commit id）

4.    如果还是后悔了，可以用git reflog来按照时间线来查看每一次提交的commit id。再配合git reset --hard commit id就可以将HEAD 指针指向相应位置了。

      reflog 可以显示所有的reset相关命令和所有出现过的版本。

### 删除文件

1.    先手动将某文件从工作区删除。

2.    再git rm 文件名。

3.    git commit即可。

4.    如果是误删文件，只要版本库没有提交，那么就可以从版本库中取回：

      ![image-20210801154127915](https://i.loli.net/2021/08/02/SKyCDPm6BtZpV5q.png)

      checkout 命令用于从版本库中提取某文件到工作区。

      git checkout -- 文件名

### 远程仓库

1.    github 作为远程托管代码的服务器。

2.    本地git 仓库和远程github 之间通信用ssh 进行加密。

3.    创建一个ssh密钥对：

      1.    打开git bash，创建ssh key：

            ssh-keygen -t rsa -C"email"

            这个email要填写github的email。

      2.    注意，-C与email之间没有空格。

            生成的ssh文件夹在windows 的用户目录下，为隐藏文件.ssh

      3.    将ssh中的公钥，粘贴给网站。

4.    添加远程库：

      1.    添加远程仓库的本质是，在远程代码库中让两个库进行同步。

            也可以叫做关联远程库，是指localgit文件夹和远程git文件夹同步。是两个文件夹之间的行为，而不是账户之间的行为。

      2.    先在github上创建一个同名repo

      3.    在本地仓库中运行：

            git remote add origin git@github.com：自己的账户名/刚创建的repo.git

            注意冒号的位置。

            即可。如图：

            ![image-20210801163415682](https://i.loli.net/2021/08/02/qkgAnXtCM2vNr5x.png)

            这样，就添加了远程的仓库。远程库的名字为origin，这个是默认的叫法。

5.    把本地库推送给远程库：

      1.    用命令git push，把当前本地的分支，master推给远程。

      2.    如果远程库是空的，那么用命令：

            git push -u origin master

            其中origin是远程库的名字，master 是当前的分支，-u参数是指，将本地master和远程master进行关联。

            同时，第一次推送会有密钥的warning，直接yes就可以。

      3.    第一次推送后，接下来的推送可以用简化命令。

            当想进行推送的时候：

            git push origin master即可。

6.    删除远程库：

      1.    删除远程库，并不是把远程代码库删除，而是删除origin，也就是本地库和远程库的关联。

      2.    首先，查看本地库关联的信息：

            git remote -v

            即可。

      3.    如果确定删除：

            git remote rm origin。

            origin为通常的名字。

7.    从远程库克隆：

      1.    首先建立一个空的文件夹

      2.    进行git init，将该文件夹被git 管理

      3.    进行clone：

            命令为：git clone git@github.com:用户名/仓库名.git

            注意：冒号后没有空格。

            ![image-20210801165152596](https://i.loli.net/2021/08/02/d3vsG2aSZLTe1EK.png)

8.    git 进行clone 的地址有两种，ssh，或者是https

### 分支管理

1.    分支管理的意义在于，我可以创建一个自己的分支，将不断未完工的代码进行上传而又不影响别人的工作。这些分支对别人并不可见，直到合并。

2.    每次提交，时间线就向前推进一步。而如果只有一个分支，那么就只是一条线。就是master，主分支。

      HEAD 并不是指向提交的，HEAD指向的是master，如版本回退

      git reset --hard HEAD^, HEAD的指向变了。

3.    最开始时候，master是一条线，git 用master 指向最新的提交，在用HEAD 指向master。

      创建一个新的分支，dev，这个dev也是一个指针，指向master 的提交，也就是最新的提交。

      然后，HEAD 指向dev，而不是master了。

      然而，从现在开始，每一次提交，master不再动了，动的是dev。

      当所有的工作完成后，可以将dev 和master 进行合并。如何合并呢，就是将master 指向dev的最新提交。

4.    创建并切换分支：

      ​		git checkout -b dev

      ​		这个命令是两个命令的合并：

      ​		git branch dev

      ​		git checkout dev

      ​		查看当前分支用命令：

      ​		git branch

      所有提交的内容，都是dev分支上的，原分支master 并没有变化。

      切换分支，也可以用switch：

      git switch -c dev, 创建一个新的分支

      git switch dev，切换已有分支。

5.    合并分支：

      1.    合并分支，是把某个分支，合并到当前分支。因此，如果要合并到master，先要切换到master分支：

            git checkout master

            git merge dev

            ![image-20210801172834117](https://i.loli.net/2021/08/02/Gnb46Elc5yBHT9N.png)

            即可。

            fast forward 是指，master 的指针指向了dev 的最新提交。

      2.    删除dev 分支：

            git branch  -d dev

            即可。

      3.    使用参数 --no-ff -m ：

             git merge --no-ff -m "merge with no-ff" dev

            可以将分支信息保留，在git log 中可以体现出来。因为这种merge，是要有commit的，所以有参数-m。

            --no-ff是可以看出来曾经合并过的，而不带这个参数则看不出来曾经合并。

      4.    真正工作时，分三层，一层为master，为发布新的版本

            二层为dev，进行不断的与员工的修改进行merge

            三层为个别员工的分支。

### 解决冲突

1.    两个分支合并时，如果两个分支分别都进行了修改，并都提交了，而修改不同，那么合并时就会发生冲突。

2.    需要将待合并的分支进行修改，跟上master 的修改变化，然后再增添一些修改，才能合并。

3.    查看不同，用cat + 文件名即可。

4.    ![image-20210801175635773](https://i.loli.net/2021/08/02/zKXiCBRbInpeUq2.png)


### 用分支处理bug

1.    如果是在dev 上进行的工作，那如果没有完成是不可以提交的。而如果此时要进行别的工作，就需要将现场保存起来。

2.    使用stash功能：

      git stash 即进行了现场保存。

3.    恢复现场：

      git stash list
      
      git stash pop
      
      或者是，git stash apply，然后git stash drop。
      
      ![image-20210801182945062](https://i.loli.net/2021/08/02/EsaQwOP2DUIVAH4.png)
      
4.    如何单独的复制一次提交：

      单独复制一次提交的意思就是，在不同的分支上，将某一个分支上的某一次修改的提交，复制到本分支上。这样来节省时间。

      1.    到某个分支下，复制下某次提交的commit id。

      2.    运行git cherry-pick commit id

            如图：

            ![image-20210801184111306](https://i.loli.net/2021/08/02/h961iN54OfFWqwS.png)

            在log 模式下，退出按q即可。

注意：当进行分支切换的时候，如果本分支有不能提交的修改，那么需要进行stash，不然，这些修改会被默认带到新的分支中。只有stash之后，新打开的分支才是最后一次提交的分支。

### 多人协作

1.    推送分支：

      推送分支，就是把该分支上的所有本地提交推送到远程库上。

      推送时要指定本地分支，这样git 会把本地分支推送到远程库的对应的分支上。

      git push origin dev

      git push origin master

      就是进行推送

2.    抓取分支：

      当clone 时，默认只clone 一个master 分支。

      开发时，创建本地dev 与远程的dev相对应：

      git checkout -b dev origin/dev

      这个命令需要远程已经有了dev 分支，因此才能够建立本地的dev分支与远程分支对应。

      所以，先由某个人将他的 dev 推送给远端。远端就相当于有了dev。

      其他人再git pull

      然后git checkout -b dev origin/dev即可。

3.    git pull

      如果出现git pull 的失败，是因为没有指定本地分支与远端分支的链接。

      git branch --set-upstream-to=origin/dev dev

      这样，就让远端的dev 和本地的dev 对应了。

总结： git pull 用于远端库与本地库的合并，用于更新本地库。



### 标签

1.    标签永远与某一次的提交，commit 相关：

      默认为本branch的最后一次commit

      git tag name即可。

2.    给某一个特殊的commit 进行标签：

      git tag name commit id

3.    git tag，按字母顺序列出标签。

      git show tagname，显示信息。

4.    给标签做说明：

      git tag -a name -m "blalblalbls" commit id

      ![image-20210801194608381](https://i.loli.net/2021/08/02/9qG7gbE3N1WBFn6.png)

5.    删除标签：

      git tag -d tagname

      ![image-20210801195236932](https://i.loli.net/2021/08/02/cC5ya9SPtIGDQho.png)

      推送标签：

      git push origin tagname   git push origin --tags(全部未推送tag)

      因为tagname是标记给commit 的，因此不存在分支的问题。
      
      git push origin :refs/tags/tagname删除标签。

![image-20210801195150429](https://i.loli.net/2021/08/02/Vyviw1HUBsd5pfA.png)

### rebase

1.    简单理解，rebase是把某个分支的一系列的commit ，从某个分支的HEAD 处，重新进行一遍提交。

      因此有两个结果，一是时间线变成了直线，也可以用来合并commit。二是可以将某个分支上做的所有的改动，都合并到master 分支上。

2.    rebase 和merge 是同样的目的，就是合并分支。integrate changes of one branch into another branch但是方式不同。

3.    基本流程：

      1.    产生history fork。历史分叉：

            ![image-20210802112410380](https://i.loli.net/2021/08/02/VZqKPoGIEd4CbzF.png)

            

            b. 使用merge：

            merge 是git pull 的一个步骤。本质就是两个分支如果没有冲突，直接update，如果有冲突，那么就解决冲突，然后update。这时，main分支没有变化，但是feature 分支已经是建立在main分支的基础上了。![image-20210802113630041](https://i.loli.net/2021/08/02/I4Vjs5WKtQrXgoa.png)

            可以认为，是把main分支上的所有的变化，合并到了feature 分支中。

            这样做的优点是，main没有变化，很安全。但是如果main经常变化，那么就经常需要这种merge。

            

      3.    使用rebase：

            另外一种合并的方式，就是把feature branch 的开端，从原来的开端移植当前的开端。

            这样做很方便，但是他将所有的历史记录，都改变了。原来feature 的历史记录，被抹除了，因此整体呈现一个线性。main 分支并没有改变。feature 分支建立在了新的分支的头部。

            This moves the entire `feature` branch to begin on the tip of the `main` branch, effectively incorporating all of the new commits in `main`. But, instead of using a merge commit, rebasing *re-writes* the project history by creating brand new commits for each commit in the original branch.每一个feature中的commit，都在rebase 的过程中重写了。project history 不见了。

            ![image-20210802115118539](https://i.loli.net/2021/08/02/5yilhT46E1vpx9z.png)

      4.    rebase 的优点首先是，没有了菱形的历史记录，变成了线形。

      ##### rebase -i

      1.    interacting rebase 很强大，因为它允许在进行移植的过程中，对每一个commit 进行操作。

      2.    git rebase -i main

            打开一个text editor。用fixup语句，将多个commit合并。

            这是merge 不能做到的。merge 的历史完全保留了。

      

      #### golden rule

      1.    The golden rule of `git rebase` is to never use it on *public* branches.

            golden rule 就是，永远不要用在公共分支。

            ![image-20210802122419087](https://i.loli.net/2021/08/02/jHAgha5UBWtClLo.png)

            也就是，把公共的分支，rebase 到自己的feature 分支之上。这样做，只有自己的本地库改变了，其他人的没有变，远端库也没有变化。git 会认为这个本地库的main 和远端库的main 发生了分叉。

            

      2.    如果想把这个被rebased 了的main push 回远端，那么git 会阻止这么做。因为，这其中存在冲突。因为rebase 的commit ，全都是基于原project 的commit 的新的commit，因此存在冲突。

            如果想强行push，就要用：

            force pushing,        git push --force

            这会把远端强行写成和本地端一样的内容。但是其他人可能会非常疑惑。

      3.    force push 的一个为数不多的用处是，你已经将本feature push 给了远端，用正常的方式，也就是merge，但是呢，这个feature写的很乱，你想重新把它clean up之后再进行push。就可以用rebase，先继续宁clean up，然后强推给远端。

      #### local clean up

      1.    通过rebase，可以进行local clean up，也就是将各种繁琐的，复杂的log，变成精简的log。

      2.    rebase 有两种选择，一个是本feature 的parent branch，另一个是feature 自己的先前的某次commit，也就是在某次commit 之后，进行rebase操作。相当于是重写。

            git rebase  -i HEAD~3

            就是最后三步进行重写。

            ![image-20210802124905217](https://i.loli.net/2021/08/02/Uz7plHfsxoWMbE6.png)

      #### private branch can perform rebase

      1.    比如当两个员工，都同时在某个feature工作，他们都同时对某个feature 进行了新的branch，当最后提交的时候，可以不用merge，而用rebase，将两个人的工作合并，进行提交。

      

      

      

      

















