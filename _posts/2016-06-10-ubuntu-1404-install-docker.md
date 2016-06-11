---
layout: post
title:  "ubuntu install docker"
date:   2016-06-10 01:02
categories:  notes
---

## 使用ubuntu14.04安装docker

[docker官方给出的安装方法](https://docs.docker.com/engine/installation/linux/ubuntulinux/)


### Update your apt sources

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
### Add an entry for your Ubuntu operating system.

```
sudo su
sudo echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
exit
```

### 检查docker的安装源是否正确

```
sudo apt-get update
sudo apt-get purge lxc-docker
apt-cache policy docker-engine

```

### 安装在ubuntu上安装docker所需要的依赖程序

```
sudo apt-get -Y install linux-image-extra-$(uname -r)
sudo apt-get -Y install apparmor
sudo reboot
```

### 最后安装docker

```
sudo apt-get update
sudo apt-get -Y install docker-engine
```

### 使用docker的时候不需要每次都输入**sudo**

下面代码中的vagrant是你使用的用户名，以后当你使用该用户名登录系统使用docker,就不需要每次输入sudo,
执行后，退出 vagrant,然后重新登录就可以不需要输入 sudo，正常使用docker.

```
sudo groupadd docker
sudo usermod -aG docker vagrant
```
### test docker

```
sudo docker run hello-world
```

