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
  User xxx@163.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_github_rsa
  Port 443
```
>- 接下来生成密钥 ssh-keygen -t rsa -C "XXX@163.com" -f ../.ssh/id_gitlab_rsa
>- id_gitlab_rsa 为私钥
>- id_gitlab_rsa.pub 为公钥
>- 登录 https://gitlab.com/ 找到setting——>ssh keys 把公钥添加


### 二、关联远程仓库
>- git init
