

## git

## git工作分区

+ 工作区

+ 暂存区

+ 本地仓库

   ```javascript
   git add .  把所有文件添加到暂存区
   git commit -m "备注信息" 把暂存区的文件提交到本地仓库种，生成一条commit提交记录 
   
   git reset 哈希值  可以回到指定提交版本的代码
   ```

   

 ![image-20231218185119843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218185119843.png)



## git中文件的状态 

+ Untrack 未被跟踪
+ Unmodified  未修改
+ Modified  修改过
+ staged  已经提交到暂存区

```javascript
新建了一个gitDemo的文件 
git init 初始化git仓库

新建了demo01.html 

git status  可以查看仓库的状态  如图   
```



![image-20231218190618504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218190618504.png)



```javascript
git add demo1.html  就把文件添加到了暂存区 
git status 查看状态
```

![image-20231218191452040](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218191452040.png)

```javascript
如果发现代码不对，在还没有提交到本地仓库的时候 ，可以通过

git rm --cached demo1.html   把demo1.html 从暂存区删除掉。

重新编写代码，在重新 git add demo1.html
```

```javascript
git commit -m "注释"   把暂存区的文件提交到本地仓库 ，其他新建的，未被跟踪的文件是不会提交到本地仓库的

git log 可以查看提交日志
```

![image-20231218192019363](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218192019363.png)

###  版本回退--git reset

```javascript
git reset --soft 版本号   #工作区和暂存区的代码都会保存

git reset --hard 版本号   #工作区和暂存区的代码都不会保存 回退到指定版本时候的代码  慎用

git reset --mixed 版本号  #工作区的代码保存  暂存区的丢弃  提交前需要先 git add .

git reflog 和 git log --oneline 都可以查看到当前所在版本
```



![image-20231218201304000](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218201304000.png)



### git diff 对比文件的不同

```javascript
 git diff #不加任何参数  比较工作区和暂存区的所有文件差异
 git diff 文件名    #比较工作区和暂存区的指定文件差异
```

![image-20231218202043118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218202043118.png)



```javascript
git add .  #把修改后的文件提交到暂存区

git diff HEAD 或者  git diff --cached  #比较暂存区 和上一个版本库里的对比 按 q退出

git diff HEAD 文件名  #比较暂存区和上一个版本指定文件的差异

git diff 版本id  版本id  #可以比较2个版本的所有文件差异很少使用

git diff 版本id  版本id 文件名 #可以比较2个版本的指定文件差异很少使用

git diff HEAD~ HEAD    比较上一个版本 和 当前版本的差异   HEAD 代表当前最新提交版本    HEAD~ 上一个版本 HEAD^
```

![image-20231218202214682](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218202214682.png)



### HEAD

```javascript
 HEAD               #代表当前最新提交版本    
 HEAD~ 或  HEAD^    #上一个版本
 HEAD~2 		   #前2个版本
```



### git 删除文件

```javascript
1 git rm 文件名称  #推荐 

2 rm 文件名   //操作繁琐
或者直接删除

删除后需要 git add .  #更新暂存区 因为本地删除了，暂存区还存在同名文件， 

git ls-files       #查看暂存区的文件
```



### 配置git账户邮箱 公钥

```javascript
 git config user.name    查看用户名
 
 gt config user.email    查看邮箱
 
 git config global user.name "用户名"   设置用户名
 
 git config global user.email "邮箱地址" 设置邮箱
 
 ssh-keygen -t rsa -C "woaijuanxueas@163.com"    生成sshkey

 cat ~/.ssh/id_rsa.pub							 查看 ssh public Key
 
 然后复制这个key 到gitHub个人中心点击 settings  创建sshKey
```



![image-20231218171254299](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231218171254299.png)





### clone 项目，提交本地代码到git仓库

```javascript
进入对应的磁盘  比如我是 D:vue-project
cd vue-project
git clone git@github.com:zhipeng-wu/next-my-blog.git

克隆的时候会生成 对应的仓库名称的目录 

cd next-my-blog

yarn create next@last .   //构建项目  记得加. 意思就是用当前目录作为项目名称的目录

git status 查看文件状态 

git add .  添加本地文件到暂存区

git commit -m "项目初始化完成"  把暂存区的文件提交到本地仓库

git push -u origin master  把本地仓库的文件提交到远程仓库

```



## 创建/切换/合并 分支

```javascript
#查看本地分支
git branch
#切换分支
git checkout 分支名称

#创建并切换到新分支
  git switch -c feature-x  ||  git checkout -b feature-x   

#合并分支 假如我在feacture-x  分支开发完成后， 先切换到master 主分支上在合并 合并后分支还在
   git checkout master
   git merge feature-x
#删除分支
git branch -d 分支名称
```



### git rebase 合并分支

**git merge 和git rebase 都能合并分支  git rebase( 变基)**    

+ 区别是 merge 会产生额外的一条 commit 提交记录

   ![image-20231219163313124](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219163313124.png)

```javascript
如图 master分支有5次提交节点， dev 分支有 2条提交节点。
在master main:3 这个节点的时候 创建了dev分支，   dev分支上有2次提交。
然后切换回了master分支， 又分别提交了main4, main5 2次提交。  

如果在dev 分支上执行就会把 dev1, dev2 合并到main5后面   左图

如果在master主分支上 执行 rebase操作， 就会把main4， main5 嫁接到 dev 分支上   右侧图

#main:3 这个节点可以看出 main4,main5   dev分支 它们的共同祖先节点。 因为是在main3 提交后，创建并切换了dev分支
```





![image-20231219144756424](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219144756424.png)



###  reabse例子   

###  分支 rebase 主分支

```javascript
master 主分支提交了 master1 然后 切换到 dev分支 做了 2次提交  接着切换回了master主分支，又做了2次提交

现在就在dev 分支上使用 git rebase master

把dev1 ,dev2 拼接在了master3后面 

master 分支 还是 master1, master2,master3 3个文件

dev分支上 多了master2,master3 文件
```



![image-20231219153956405](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219153956405.png)

![image-20231219160539672](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219160539672.png)



![image-20231219160953550](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219160953550.png)

![image-20231219161041965](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219161041965.png)



###  例子2 主分支 rebase 分支

```javascript
和 例子1相反  dev分支  就 master1,dev1,dev2 个文件

master 分支 多了 dev1.html,dev2.html



```



![image-20231219162127424](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219162127424.png)



### 针对以存在的项目

```javascript
1. git init 初始化本地仓库
2  git remote add origin  远程仓库的地址 (origin 是个别名可以任意取，通常使用origin) 这使本地仓库和远程仓库进行关联
3. git status 查看文件状态
4. git add .
5. git commit -m '注释'
6. git push -u origin master  把本地文件同步到远程仓库

```



### 其他常用命令- 日志命令

```javascript
git remote -v #查看项目关联的远程仓库名称
git pull #拉取远程仓库所有分支合并到本地

#日志命令
git reflog  和 git  log 都能查看 日志

它们的区别:
git reflog 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作），git reflog常用于恢复本地的错误操作。

git log 命令可以显示所有提交过的版本信息，不包括已经被删除的 commit 记录和 reset 的操作。

```



### git tag

```javascript
给指定commit 加上一个标签 方便查看 回滚
git tag  版本名称  commit哈希值

```



![image-20231219170921886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219170921886.png)



![image-20231219170939572](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231219170939572.png)
