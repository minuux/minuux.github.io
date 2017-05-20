---
layout: post
title:  "梅林小宝-ddnspod"
date:   2017-05-21 00:08:31
categories: notes
---


## 为子域名*设置动态ip

安装好梅林小宝固件后，在soft center中可以安装ddnspod，通过脚本来更新域名ip。
但是，我需要设置的子域名是 \*.test.com,直接保存后，提示 record line invailid。
参考 <http://www.anrip.com/ddnspod> ,将脚本保存为utf-8格式，设置后的确可以保存成功，但是我在dnspod中设置的\*子域名就变成了@前缀了

所以最后只能通过修改梅林小宝固件中的文件来处理了（梅林小宝版本：380.63_X7.2）

复制文件到本地


```
	scp -r admin@router.asus.com:/jffs/.koolshare/init.d/S99ddnspod.sh .
```


修改文件


```
parseDomain() {
    mainDomain=${ddnspod_config_domain#*.}
    local tmp=${ddnspod_config_domain%$mainDomain}
    #subDomain=${tmp%.}
    subDomain="*"
}
```

将文件修改后复制到路由器


```
	scp -r S99ddnspod.sh admin@router.asus.com:/jffs/.koolshare/init.d/
```


