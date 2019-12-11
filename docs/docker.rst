========================
Docker
========================

centos7.2安装docker
=======================
.. code:: sh

  yum install docker

https://www.jianshu.com/p/232bc2c1e95d

在docker里禁用selinux
>>>>>>>>>>>>>>>>>>>>>>>>

.. code:: sh

  vim /etc/sysconfig/docker
  --selinux-enabled=false

https://www.cnblogs.com/hongdada/p/8886893.html

centos7安装docker并设置开机启动
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.. code:: sh

  systemctl enable docker

https://www.cnblogs.com/rwxwsblog/p/5436445.html

docker启动，重启，关闭命令
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.. code:: sh

  sudo service docker start

https://blog.csdn.net/EasternUnbeaten/article/details/80463837

docker-compose 安装
>>>>>>>>>>>>>>>>>>>>>>

https://github.com/docker/compose/releases


Docker 镜像与容器
==================

拉取镜像
>>>>>>>>>
1. docker search nginx
2. docker pull nginx
3. docker run -p 80:80 --name ng_container -d nginx

Docker 删除 <none> 镜像
>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.. code:: sh

  docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

Docker 运行容器
>>>>>>>>>>>>>>>>>>>>>>>>>>>>
- -d: 后台运行容器，并返回容器ID
- -v: 主机的目录/app/tomcat/webapps映射到容器的/app/tomcat/webapps
- -t：进入终端
- -i：获得一个交互式的连接，通过获取container的输入

.. code:: sh

  docker run -d -p 8080:8080 -p 8009:8009 -v /app/tomcat/webapps:/app/tomcat/webapps tomcat

  docker run -p 80:80 --name ng_container -v $PWD/www:/www -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/logs:/www/logs  -d nginx 

Docker 进入容器
>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.. code:: sh

  docker exec -it container_id bash
  docker exec -it container_name /sh

Docker 退出容器
>>>>>>>>>>>>>>>>>>>>>>>>>>>>
- exit
- CTRL+D

Docker 删除容器
>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.. code:: sh

  docker ps -a

  docker stop container_id
  docker rm container_id

  # docker同时删除停止的容器
  docker rm $(docker container ls -f "status=exited" -q)


Docker 容器内安装 ifconfig netstat ping vim 等测试工具的方法
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
https://blog.csdn.net/weixin_42350212/article/details/84973320

.. code:: sh

  apt-get install iputils-ping

  apt-get install vim

查看容器详情
>>>>>>>>>>>>>>
.. code:: sh

  docker inspect container_id

Dockerfile 编写样例
=====================
https://github.com/ekoz/docker-kbase/blob/master/Dockerfile

Docker save 与 Docker export 的区别
==========================================
https://blog.csdn.net/liukuan73/article/details/78089138

Docker: 限制容器可用的内存
============================
https://www.cnblogs.com/sparkdev/p/8032330.html

Demo
================
.. code:: sh

  # mysql
  docker run -d -p 3306:3306 --restart always --privileged=true --name mysql3306 -e MYSQL_USER="u_teach" -e MYSQL_PASSWORD="u_teach" -e MYSQL_ROOT_PASSWORD="rOOt" -v /home/ekozhan/mysql/data:/var/lib/mysql -v /home/ekozhan/mysql/logs:/var/log/mysql 1e4405fe1ea9

  # mongodb
  docker run --name mongo_container -p 27017:27017 -e TZ="Asia/Shanghai" --restart always --privileged=true -v /opt/docker/mongodb/data/db:/data/db -v /opt/docker/mongodb/data/configdb:/data/configdb -v /opt/docker/mongodb/data/logs:/data/logs -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=rOOt -d 57c2f7e05108  --config /opt/docker/mongodb/data/mongod.conf

docker-compose
==================
**注意**

项目中使用最多的就是 docker-compose

.. code:: sh

  docker-compose down
  docker-compose build
  docker-compose up -d

相关资料
==========
https://yeasy.gitbooks.io/docker_practice/