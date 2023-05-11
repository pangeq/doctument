### 一、配置github 和gitlab 的ssh
>- 在.ssh文件夹下创建一个config 文件（注意没有后缀）文件中大致可配置如下字段：
```
Host：用来替代将要连接的服务器地址。当ssh的时候如果服务器地址能匹配上这里Host指定的值，
	则Host下面指定的HostName将被作为最终的服务器地址使用，并且将使用该Host字段下面配
	置的所有自定义配置来覆盖默认的`ssh_config`配置信息。
Port：自定义端口
User：用户名
HostName：真正连接的服务器地址
PreferredAuthentications：指定优先使用哪种方式验证，支持密码和秘钥验证方式
IdentityFile：指定本次连接使用的密钥文件 
该文件的主要作用就是指明各个git帐号对应的User以及IdentityFile的文件位置 
```

``` 
示例：
# gitlab
Host gitlab
  HostName ssh.gitlab.com
  IdentityFile ~/.ssh/id_rsa

# github
Host github.com
  HostName ssh.github.com
  User XXX@163.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_github_rsa
  Port 443
```
>- 接下来生成github密钥 ssh-keygen -t ed25519 -C "XXX@163.com"
>回车后输入密钥文件名字  id_github_rsa
>- id_github_rsa 为私钥
>- id_github_rsa.pub 为公钥
>- 登录 https://github.com/ 找到setting——>ssh keys 添加公钥


### 二、关联远程仓库
>- git init  （初始化git）
>- git status （查看当前代码状态）
>- git add .
>- git commit -m 'init'
>- git checkout -b uat（创建本地分支）
>- git remote add origin 'git@github.com:XXX/autonomy.git' (设置远程分支)
>- git push origin uat （把uat分支推送到远程）
>- git branch --set-upstream-to=origin/uat（关联本地代码和远程仓库分支）
>- git branch -a （查看所有分支）
>- git remote remove origin (解除关联，也可以直接删除.git 重新设置)
>- git remote -v（查看远程关联仓库）
