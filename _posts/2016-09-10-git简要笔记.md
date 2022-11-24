### 1，配置账户

```
git help #帮助
git version #git版本
```

姓名
```
git config --global user.name "name"
```
邮箱
```
git config --global user.email "email"
```
全部信息
```
git config --list 
```
输出如下类似信息
```
user.email=hbxglong@163.com
user.name=hloong
filter.media.required=true
filter.media.clean=git media clean %f
filter.media.smudge=git media smudge %f
filter.hawser.clean=git hawser clean %f
filter.hawser.smudge=git hawser smudge %f
filter.hawser.required=true
filter.lfs.clean=git lfs clean %f
filter.lfs.smudge=git lfs smudge %f
filter.lfs.required=true
core.autocrlf=input
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
```
### 2，创建提交项目
在项目根目录执行，表示创建一个git目录

```
git init
```
检查项目
```
git status
```
添加项目 ==注意：修改后的文件提交也用add==
```
git add abc.java #添加单个文件
git add java/ #添加当前java文件下的所有文件
git add . #添加当前目录的所有文件
```
移除项目
```
git rm --cached abc.java #移除单个文件，如此就不会提交到服务器了
```
提交项目

```
git commit -m "提交说明文字" #直接提交
git commit #会跳转到vi编辑器
```
假如跳转到vi编辑器的时候：

![image](http://note.youdao.com/yws/public/resource/5ab8e1106e3cd4394d112de02a7f6309/xmlnote/98CAADABE0E944879209D5DFA4774811/18985)

编辑完后，按esc键退出编辑，然后:x退出编辑器并保存
执行`git log`可以查看具体内容，如下图

![image](http://note.youdao.com/yws/public/resource/5ab8e1106e3cd4394d112de02a7f6309/xmlnote/8424417E62F34C649FB841FC90CDCB17/19015)

### 3，下载编辑项目
```
git clone git@…… #下载某项目
git branch test #表示新建一个test分支
git branch #列出不同的分支，带*表示当前的分支
git checkout test #签出分支 test

git merge test #切换到master分支后执行，表示合并test分支的代码

git branch -d test #删除分支test，注意：此方法只能用于已经合并了的分支，如果想删除没合并的就用-D
git branch -D test #强行删除分支test

git tag  #显示所有的tag标记
git tag -a v1.0 -m "创建一个v1.0版本标记" #为一个版本打标签
git tag -d v1.0 #删除这个v1.0版本标记
```
### 4，github相关
- `注册`，`本机生成SSH key`，`新建项目`，`提交到github`，`更新代码`，`忽略文件`，`for`

##### 注册基本人人都会了
##### 本机生成SSH key

```
ssh-keygen -t rsa -b 4096 -C "hbxglong@163.com"
```
输入后会显示

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/hl/.ssh/id_rsa): 
```
然后连续2次enter回车，表示不设置密码

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/hl/.ssh/id_rsa.
Your public key has been saved in /Users/hl/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:7KuMZ5PppSGyaYO8t3V2kP4VYmAcoU1VQx2U8AQMVYc hbxglong@163.com
```
这就成了
![image](http://note.youdao.com/yws/public/resource/5ab8e1106e3cd4394d112de02a7f6309/xmlnote/WEBRESOURCE60894b42ece9a450fa02a31c8085292f/19086)

SSH key 添加成功之后，输入 ssh -T git@github.com 进行测试

Clone自己的项目 我们以我在 GitHub 上创建的 test 项目为例，执行如下命令：
>git clone git@github.com:hloong/java.git

如果本地项目已经有了，想把本地项目直接提交到github，那就用下面的命令；
但是用之前要先去github新建一个repository，再来用下面的命令关联

>git remote add origin git@github.com:hloong/java.git

之后可以直接用提交到github了：

>git push origin master


Git常用操作命令收集：
>1) 远程仓库相关命令
```
检出仓库：$ git clone git://github.com/jquery/jquery.git
查看远程仓库：$ git remote -v
添加远程仓库：$ git remote add [name] [url]
删除远程仓库：$ git remote rm [name]
修改远程仓库：$ git remote set-url --push[name][newUrl]
拉取远程仓库：$ git pull [remoteName] [localBranchName]
推送远程仓库：$ git push [remoteName] [localBranchName]
 
2）分支(branch)操作相关命令
查看本地分支：$ git branch
查看远程分支：$ git branch -r
创建本地分支：$ git branch [name] ----注意新分支创建后不会自动切换为当前分支
切换分支：$ git checkout [name]
创建新分支并立即切换到新分支：$ git checkout -b [name]
删除分支：$ git branch -d [name] ---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项
合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并
创建远程分支(本地分支push到远程)：$ git push origin [name]
删除远程分支：$ git push origin :heads/[name]
我从master分支创建了一个issue5560分支，做了一些修改后，使用git push origin master提交，但是显示的结果却是'Everything up-to-date'，发生问题的原因是git push origin master 在没有track远程分支的本地分支中默认提交的master分支，因为master分支默认指向了origin master 分支，这里要使用git push origin issue5560：master 就可以把issue5560推送到远程的master分支了。

    如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，那么可以这么做。

$ git push origin test:master         // 提交本地test分支作为远程的master分支 //好像只写这一句，远程的github就会自动创建一个test分支
$ git push origin test:test              // 提交本地test分支作为远程的test分支

如果想删除远程的分支呢？类似于上面，如果:左边的分支为空，那么将删除:右边的远程的分支。

$ git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的，不用担心
3）版本(tag)操作相关命令
查看版本：$ git tag
创建版本：$ git tag [name]
删除版本：$ git tag -d [name]
查看远程版本：$ git tag -r
创建远程版本(本地版本push到远程)：$ git push origin [name]
删除远程版本：$ git push origin :refs/tags/[name]
 
4) 子模块(submodule)相关操作命令
添加子模块：$ git submodule add [url] [path]
如：$ git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs
初始化子模块：$ git submodule init ----只在首次检出仓库时运行一次就行
更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下
删除子模块：（分4步走哦）
1)$ git rm --cached [path]
2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉
3) 编辑“.git/config”文件，将子模块的相关配置节点删除掉
4) 手动删除子模块残留的目录
```