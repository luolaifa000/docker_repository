# docker_repository
镜像仓库

```
单独起一个mysql容器，不用自己构建，直接用官方的
docker pull mysql:5.7
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
–name：容器名，此处命名为mysql
-e：配置信息，此处配置mysql的root用户的登陆密码
-p：端口映射，此处映射 主机3306端口 到 容器的3306端口
-d：源镜像名，此处为 mysql:5.7


php_nginx_redis_base 
博客用的PHP环境的基础镜像 (里面有php-fpm redis 没有mysql，要单独起一个mysql容器, 腾讯云的机器只能用腾讯云的镜像，本来放阿里云的仓库的,发现不能用)
这里的基础镜像是放在腾讯云容器服务的镜像仓库里
FROM hkccr.ccs.tencentyun.com/yumancang/php_nginx_redis_base
业务容器启动后，要手动改下.env里面的DB配置和手动起redis守护进程
#docker build -t yumancang/php_nginx_redis_base .
#docker tag yumancang/php_nginx_redis_base hkccr.ccs.tencentyun.com/yumancang/php_nginx_redis_base
#docker login --username=100003270371 hkccr.ccs.tencentyun.com
#docker push hkccr.ccs.tencentyun.com/yumancang/php_nginx_redis_base

node_nginx_vue_base
这里的基础镜像是放在腾讯云容器服务的镜像仓库里
FROM hkccr.ccs.tencentyun.com/yumancang/node_nginx_vue_base
业务容器启动后，要手动改下.env里面的DB配置和手动起redis守护进程
#docker build -t yumancang/node_nginx_vue_base .
#docker tag yumancang/node_nginx_vue_base hkccr.ccs.tencentyun.com/yumancang/node_nginx_vue_base
#docker login --username=100003270371 hkccr.ccs.tencentyun.com
#docker push hkccr.ccs.tencentyun.com/yumancang/node_nginx_vue_base
```