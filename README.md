## git 学习

1. 版本控制
   版本控制在软件开发中,可以帮助程序员进行代码的追踪,维护,控制等等一系列的操作

2. 集中式版本管理

   - CVS 和 SVN 都是是属于集中式版本控制系统（Centralized Version Control Systems，简称 CVCS）
   -  它们的主要特点是单一的集中管理的服务器，保存所有文件的修订版本；
   -  协同开发人员通过客户端连接到这台服务器，取出最新的文件或者提交更新；
   - 2.1 但是集中式版本控制也有一个核心的问题：中央服务器不能出现故障：
     - 如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作；
     - 如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，毫无疑问你将丢失所有数据；

3. 分布式版本管理

   - git 就是分布式版本管理

4. git 命令

   - 4.1 常见
     - git clone "http://gitee....." 克隆仓库,做了这步可以不做 git init
     - git init 创建一个空的本地仓库
     - git add . 所有文件都监听,更新提交暂存区
     - git commit -m "描述更新了什么" 将暂存区的提交
     - git commit -a -m "描述更新了什么" 是上两条的合并简写
     - git log 打印日志
     - git log --pretty=oneline 让每条日志一行显示,显示得好看一点
     - git status 状态
     - q 结束,相当于 cmd 的 Ctrl+C
     - git reset 版本回退 上一个版本就是 HEAD^，上 100 个版本就是 HEAD~100
       - git reset --hard HEAD^
       - git reset --hard HEAD~1000
       - git reset --hard f1d1d2d4 (这个码是 git log 那个很长的 commit id,也叫校验和) 指定某一个特定版本,不用写完,只取前八位也可以
     - git reflog 包含回退的版本,因为 git log 会把回退的版本校验和删掉.这样就可以用 git reset 回到 log 里面没有的版本
     - git fetch 拉取代码,获取到代码后默认并没有合并到本地仓库,我们需要通 merge 进行合并
     - git pull 相当于 git fetch 加上 git merge
     - git branch 查看分支

5. 远程仓库
   - 5.1 如何创建远程仓库
     - 使用第三方的 git 服务器:如 GitHub,gitee,gitlab 等
     - 在自己服务器搭建一个 git 服务
   - 5.2 目前 git 服务器验证手段主要有两种(对于私有仓库)
   - 方式一:基于 HTTP 的凭证存储(因为 HTTP 协议是无状态的连接,所以每次连接都需要用户名和密码,凭证解决这个问题)
   - 方式二:基于 SSH 的秘钥
     - 1. 用户在电脑命令行输入指令,生成一对公钥和私钥,公钥放到 gitee 或者 GitHub 那个网站上的一个位置,私钥留在电脑,就可以直接传输了
     - 2. 在 git bash 终端输入指令 ssh-keygen -t ed25519 -C “your email" 或者 ssh-keygen -t rsa -b 2048 -C “your email" (上面是公钥加密类型不同而已,都可以用)
     - 3. 在 c 盘的用户文件夹,有一个文件夹下有.ssh 文件夹,打开.ssh 文件夹里的 id_rsa.pub 文件(记事本打开),存的是公钥,打开 gitee 的设置,把里面的公钥复制粘贴
   - 5.3 让本地仓库和远程仓库建立连接(组长没建仓库,自己建本地和远程仓库的情况)
     - 方案一:
       - git remote add <shortname> <url> // shortname 是仓库名,一般叫 origin,url 是远程仓库地址
       - 例子:git remote add gitlab http://152.136.185.210:7888/coderwhy/gitremotedemo.git
       - 然后 git pull,如果拉不下来,那 git pull origin master --allow-unrelated-histories(
         这是因为 git 不知道远程仓库的哪个分支和本地仓库的哪个分支建立连接,这个命令是让本地 master 跟踪远程 origin 的 master 分支,允许不相关历史)
       - git push
     - 方案二:
       - 1. git remote add origin <url>
       - 2. git branch --set-upstream-to=origin/master
       - 3. gi fetch
       - 4. git merge --allow-urlrelated-histories
       - 5. git push
