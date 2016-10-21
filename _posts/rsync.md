# cygwin 部署 rsync

在windows下备份手机相册，通过简单的 [手机APP](https://itunes.apple.com/cn/app/zhao-pian-shi-pin-bei-fen/id945026388) 直接备份，可以每次备份不重复。
这个APP支持linux,mac。但windows需要通过cygwin部署才行

## 安装cygwin
  下载cygwin后安装rsync,反正我选择后就可以了，或许还需要安装ssh

## 配置rsync
gist:[rsyncd.conf](https://gist.github.com/minuux/02e28a90d87a58db3492c548b2200e55)
```
# 写入之后需要使用到的rsync密码文件，文件名可以选择自己喜欢的名称，和配置文件中一致即可
# echo "minuux:123456" >> /etc/rsync_server.passwd

use chroot = false
strict modes = false
lock file = rsyncd.lock 
hosts allow = *
max connections = 5
# 设置端口后，便于client配置
port = 28953
pid file = /var/run/rsyncd.pid
# use cygwin in windows,so set windows's user
# windows登录的用户名称是Administrator,所以cygwin中也有该用户，用该用户的uid
uid = Administrator
gid = None
# 设置日志文件路径
log file = /cygdrive/g/rsync/rsyncd.log

# Module definitions
# Remember cygwin naming conventions : c:\work becomes /cygdrive/c/work
# 设置模块，便于client链接的时候选择，类似于alias，客户端连接的时候可以设置为home/other，将会备份在/cygdrive/g/myhome/photos/other
[home]
path = /cygdrive/g/myhome/photos
auth users = minuux
secrets file =/etc/rsync_server.passwd
read only = no
list = yes
transfer logging = yes


```

## 启动rsync

```
rsync --daemon
```
查看rsync运行情况,如果存在rsync.pid类似的名称就说明已经启动了

```
cd /var/run
ls
```

## 其他
有的时候会出现备份完成后，从windows访问已备份的文件夹，结果发现没有权限。。。，需要到cygwin下为该目录使用 chmod授权，如果只是自己备份可以执行
```
chmod 644 文件夹名称
```
为什么是644？请点击:https://www.v2ex.com/t/299246#reply70
