# Docker

## nginx

```
	docker run --name nginx -p 80:80 -v /Users/lizhixin/dev/docker/nginx:/usr/share/nginx/html:ro -d nginx

```

## mysql
osx中使用mysql镜像创建容器会出现volumes无权限加载的情况
解决方法：http://stackoverflow.com/questions/34442831/permission-denied-when-mounting-docker-volume-in-osx

创建自己的镜像，修改权限 	

```
	FROM mysql

	RUN usermod -u 1000 mysql
	RUN mkdir -p /var/run/mysqld
	RUN chmod -R 777 /var/run/mysqld

```

```
	docker build -t mysql2 .

```
生成容器

```
	docker run -it --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /Users/lizhixin/dev/edx/mysql:/var/lib/mysql mysql2

```

## docker-machine 使用代理

```
	docker-machine create -d virtualbox \
    --engine-env HTTP_PROXY=http://192.168.1.1:8080 \
    --engine-env HTTPS_PROXY=http://192.168.1.1:8080 \
    --engine-env NO_PROXY=localhost \
    --EXTRA_ARGS="--registry-mirror=http://de1b1668.m.daocloud.io" \
    default

```


## 容器中设置时间区间
```
	cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

```