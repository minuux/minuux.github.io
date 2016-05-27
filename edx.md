# 	Edx

## 背景
edX是哈佛大学和麻省理工学院共同创立的非营利网络教育项目，旨在为全球提供来自哈佛大学、麻省理工学院、加州大学伯克利分校、清华大学、北京大学、香港大学、香港科技大学等全球顶尖高校及组织的慕课（大规模在线公开课）。课程主题涵盖生物、数学、统计、物理、化学、电子、工程、计算机、经济、金融、文学、历史、音乐、哲学、法学、人类学、商业、医学、营养学等多个学科。

## 介绍
http://blog.just4fun.site/about-Open-edX.html

## 5种扩展edx的方式
https://github.com/edx/edx-platform/wiki/Five-ways-to-extend-edX

## 浏览器支持，必须IE11或chrome,firefox,safari
http://edx.readthedocs.io/projects/edx-guide-for-students/en/latest/front_matter/browsers.html

## 第三方登陆

集成公司的登录方式，但EDX现在只提供了OAUTH2的方式
http://blog.just4fun.site/Open-edX-login-explore.html

## 案例
http://www.xuetangx.com/

## 主题修改
http://blog.just4fun.site/edx-front-dev.html
https://github.com/edx/edx-platform/blob/named-release/dogwood.rc/themes/README.rst

## Open edX开发技能与入门资料
http://blog.just4fun.site/open-edx-dev-skill-and-resource.html

## 使用ansible 安装失败
```
cd /var/tmp/configuration/playbooks
sudo ansible-playbook -c local ./edx_sandbox.yml -i "localhost," --limit @/home/vagrant/edx_sandbox.retry

```

## 翻译成本地语言
localization and development in china
https://github.com/edx/edx-platform/wiki/localization-and-development-in-china 
```
/*
http://www.ichenfei.com/open-edx-nationalization-i18.html
*/

/*删除原有.po翻译文件*/
cd /edx/app/edxapp/edx-platform/conf/locale/zh_CN/LC_MESSAGES/
sudo rm *

/*下载我们翻译做好的.po文件,并更改所有权,通过浏览器下载就不需要更改所有权了*/
	
wget http://mirrors.edustack.org/LC_MESSAGES/dogwood/django.po http://mirrors.edustack.org/LC_MESSAGES/dogwood/djangojs.po

chown edxapp:edxapp django.po djangojs.po

/*切换到edxapp用户加载环境执行翻译*/
sudo -u edxapp bash
source /edx/app/edxapp/edxapp_env
cd /edx/app/edxapp/edx-platform
paver i18n_fastgenerate

/*退出edxapp用户并重启edxapp，最后的:是必要的*/
exit
sudo /edx/bin/supervisorctl restart edxapp:

```
## Docker
edxops/edxapp docker image执行 supervisorctl
```
/edx/app/supervisor/venvs/supervisor/bin/supervisorctl
```
日志 
/edx/var/log

## 脚本执行时间

```
PLAY RECAP ******************************************************************** 
INFO:ansible.callback_plugins.datadog_tasks_timing:edxapp | checkout edx-platform repo into {{ edxapp_code_dir }} ----------------- 544.58s
INFO:ansible.callback_plugins.datadog_tasks_timing:mongo | install mongo server and recommends ------------------------------------ 317.94s
INFO:ansible.callback_plugins.datadog_tasks_timing:edxapp_common | Install system packages ----------------------------------------- 96.93s
INFO:ansible.callback_plugins.datadog_tasks_timing:mysql | Install mysql-5.6 and dependencies -------------------------------------- 58.14s
INFO:ansible.callback_plugins.datadog_tasks_timing:rabbitmq | install rabbit package using gdebi ----------------------------------- 44.68s
INFO:ansible.callback_plugins.datadog_tasks_timing:edxapp | install system packages on which LMS and CMS rely ---------------------- 44.03s
INFO:ansible.callback_plugins.datadog_tasks_timing:rabbitmq | install python-software-properties if debian ------------------------- 31.26s
INFO:ansible.callback_plugins.datadog_tasks_timing:edxapp | add ppas for current versions of nodejs -------------------------------- 30.02s
INFO:ansible.callback_plugins.datadog_tasks_timing:mongo | add the mongodb repo to the sources list -------------------------------- 28.09s
INFO:ansible.callback_plugins.datadog_tasks_timing:mysql | Install MySQL community apt repositories -------------------------------- 21.41s
INFO:ansible.callback_plugins.datadog_tasks_timing:
Playbook edx_sandbox finished: Thu May 19 14:48:45 2016, 208 total tasks.  0:22:18 elapsed.

```

## 配置远程服务

### mysql
数据库
- edxapp
- edxapp_csmh

用户 edxapp001:password,root:password

### mongo
数据库 edxapp
用户 edxapp:password

```
use edxapp
db.createUser(
   {
     user: "edxapp",
     pwd: "password",
     roles: [ "readWrite"]
   }
)

use cs_comments_service
db.createUser(
   {
     user: "cs_comments_service",
     pwd: "password",
     roles: [ "readWrite"]
   }
)
```

### memcahce


### edx-platform
修改代码，不要从github上复制代码，直接从本地复制
修改 checkout edx-platform repo into {{ edxapp_code_dir }}
```
- name: checkout edx-platform repo into {{ edxapp_code_dir }}
  shell: cp -R /vagrant/edx-platform {{ edxapp_code_dir }}
  sudo_user: "{{ edxapp_user }}"
  register: edxapp_platform_checkout
  tags:
    - install
    - install:code
```


### 调整源地址
修改palybooks/roles/common_vars/default/main.yml
```
COMMON_PYPI_MIRROR_URL: 'http://mirrors.aliyun.com/pypi/simple'
COMMON_NPM_MIRROR_URL: 'https://registry.npm.taobao.org'

```