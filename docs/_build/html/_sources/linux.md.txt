# Linux（以 CentOS7.2 为主）

## 装机必备

- yum 源
- docker
- docker-compose
- python2.7 与 python3.6 兼容
- 防火墙
- jdk
- gcc
- sudoers 与 755

## 国内 yum 源

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
mv CentOS7-Base-163.repo /etc/yum.repos.d/
yum clean all
yum makecache
```

[https://www.runoob.com/linux/linux-yum.html](https://www.runoob.com/linux/linux-yum.html)

[http://mirrors.163.com/.help/centos.html](http://mirrors.163.com/.help/centos.html)

## 防火墙

1. centOS7 永久关闭防火墙(防火墙的基本使用)

```
# 查看状态
systemctl status firewalld.service

# 关闭防火墙
systemctl stop firewalld.service

# 开启防火墙
systemctl start firewalld.service

# 设置开机启动
systemctl enable firewalld.service

# 禁止开机启动
systemctl disable firewalld.service
```

[https://blog.csdn.net/ViJayThresh/article/details/81284007](https://blog.csdn.net/ViJayThresh/article/details/81284007)

2. CentOS7 查看和关闭防火墙

```
firewall-cmd --state
```

[https://blog.csdn.net/ytangdigl/article/details/79796961](https://blog.csdn.net/ytangdigl/article/details/79796961)

3. CentOS 防火墙添加例外端口

```
# 查看状态

firewall-cmd --state

# 添加 8080 例外端口
# –zone 作用域
# –add-port=8080/tcp 添加端口，格式为：端口/通讯协议
# –permanent 永久生效，没有此参数重启后失效
firewall-cmd --permanent --zone=public --add-port=8080/tcp

# 重新加载
firewall-cmd --reload
```

[https://blog.csdn.net/jiankunking/article/details/78794383](https://blog.csdn.net/jiankunking/article/details/78794383)

## jdk 安装

1. 解压 jdk-xxx-linux-x64.tar.gz，将解压的 jdk 目录移动到/opt 下

2. 编辑 `/etc/profile`

```
sudo vim /etc/profile

export JAVA_HOME=/opt/jdk1.7.0_55

# \$PATH 放在结束位置，否则会被系统自带的 openjdk 覆盖

export PATH=$JAVA_HOME/bin:$PATH

source /etc/profile
```

3. 运行 `java -version` 得到正确的版本

**注意**

如果 jdk 是 bin 文件，直接 `chmod 755`，然后 `./bin` 解压，接下来参考上面的步骤配置环境变量。

```
chmod 755 jdk-6u27-linux-x64.bin

./jdk-6u27-linux-x64.bin > /tmp/null
```

## tar 命令

```
# 解压

tar –zxvf xxx.tar.gz

# 压缩

tar –zcvf xxx.tar.gz
```

或者

```
# 压缩

zip -r kbase-converter.zip kbase-converter/_ -x "kbase-converter/DATAS_"
```

## 用户和组权限 sudoers

1. 基本用法

- 创建组 `groupadd g_normal`
- 创建用户 `useradd -g g_normal u_search`
- 修改密码 `passwd u_search`

2. 将用户 u_search 加入到 docker 组

```

groupadd docker
usermod -aG docker u_search
systemctl restart docker

# 查询当前用户所属的所有组
groups
```

3. 文件权限操作

```
# 改变 filename 的所有者为 xiaoming

chown xiaoming filename

# 改变 filename 所属的组为 root

chgrp root filename

# 改变 dirname 这个目录的所有者是 root

chown root ./dirname

# 改变 dirname 这个目录及其下面所的文件和目录的所有者是 root

chown ‐R root ./dirname
```

4. 设置 sudo

```
whereis sudoers

chmod -v u+w /etc/sudoers

vim /etc/sudoers

#这个是新用户
u_search ALL=(ALL) ALL

chmod -v u-w /etc/sudoers
```

## scp 传输文件

```
# 传输文件

scp -r -P 12598 lucy 172.16.9.55:/home/eko_0807/

# 拉取文件

scp -r -P 12598 ekozhan@172.16.9.55:/home/ekozhan/demo.tar.gz /home/eko_0807/
```

**注意**

`scp` 命令和 `ssh` 命令类似，记住一个，另一个妥妥的

## ssh & ssh 免密登录

当前服务器 ip：192.168.1.200

目标服务器 ip：192.168.1.210

```
ssh -l hadoop 172.16.9.55 -p 12598
```

或者

```
ssh -p 5060 eko.zhan@172.16.9.55
```

设置免密登录

```
ssh-keygen -t rsa
ssh-copy-id -i id_rsa.pub -p 12598 eko.zhan@192.168.1.210

# 注意修改 sshd_config 中的参数
/etc/ssh/sshd_config
RSAAuthentication yes
PubkeyAuthentication yes
```

## 查目录包含子目录大小

```
du -hsx \* | sort -rh | head -10

du -h --max-depth=1 .
```

## find 命令

查找文件名称包含 `.class` 的文件

```
find . -name '\*.class' -newermt '2017-06-09'
```

查找文件名称包含 `.class` 的文件，并将内容写入 /app/ekozhan 文件中

```
find . -name '\*.class' -newermt '2017-06-09' |xargs -i cp {} /app/ekozhan
```

统计 java 代码行数

```
find . -name \*.java |xargs cat|grep -v ^\$|wc -l

find . -type f |xargs cat|grep -v "^\$"|grep -v "^/"|wc -l

find . -name \*.java |xargs cat|grep -v "^\$"|grep -v "^/"|wc -l

find . -name _.htm_ -o -name _.js_ -o -name \*.java |xargs cat|grep -v "^\$"|grep -v "^/"|wc -l
```

查找所有包含 `.png.txt` 的文件并删除

```

find ./ -name '\*.png.txt' |xargs rm -rf
```

查找所有包含 `.png.txt` 的文件（忽略大小写）并删除

```
find ./ -iname '\*.png.txt' |xargs rm -rf
```

## cat 高级用法

查找日志中包含 `read time out` 的文本，不区分大小写

```
cat 2018_02_05.stdout.log |grep -i "read timed out"
```

查找日志中包含 `Exception`，不包含 `hibernate` `filter` `stream` 的文本

```

cat 2018_02_05.stdout.log |grep Exception|grep -vi hibernate|grep -vi filter|grep -vi stream
```

查看日志，从第 80780 行开始，显示 100 行

```
cat -n 2018_02_05.stdout.log|tail -n +80780|head -n 100
```

查看日志包含 `Exception` 的本文，上下 3 行

```
cat debug.log |grep -C 3 Exception
```

## 杀进程

```
kill -9 `ps -ef|grep java|grep -v grep|grep kbase-conference|awk '{print $2}'`

ps -ef|grep soffice|grep -v grep|awk '{print \$2}'|xargs kill -9
```

## 创建软连接

```
ln -s /mnt/disk0/xiaoi /xiaoi
```

## 重启关机记录排查

```
last|grep reboot
history|grep reboot
cat /var/log/messages
```

## CLOSE_WAIT

```
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

# 客户端连接数量，Connection Refused，最大数 65535

netstat -nat|grep -i 8080|wc -l
```

[https://blog.csdn.net/duan19056/article/details/51210110](https://blog.csdn.net/duan19056/article/details/51210110)

## crontab 定时任务

```
crontab -e 0 1 \* \* \* /usr/bin/python3 /home/yogurt/XinhuaSpider.py >> /home/yogurt/spider.out

crontab -e 59 23 \* \* _ rm -rf /tmp/lu_.tmp /tmp/webdav\*.log

crontab -e 30 0 \* \* _ find /tmp/lu_.tmp -ctime 0 -exec rm -rf {} \;

```

## 修改时间

```
date -s "2015-08-11 13:06:30"
clock -w
```

## 源码安装样例

```
wget http://memcached.org/latest
tar -zxvf memcached-1.x.x.tar.gz

cd memcached-1.x.x

./configure && make && make test && sudo make install
```

## Debian 与 CentOS 语法差异

```
Debian/Ubuntu: apt-get install libevent-dev

Redhat/Centos: yum install libevent-devel
```

## gcc

```
yum -y install gcc
```

## hostname 修改

1. hostname

2. hostnamectl set-hostname ibotplus

3. vim /etc/hosts

## python2.7 与 python3.6 兼容

CentOS7.2 安装 pip 以及升级

```
yum -y install epel-release

yum -y install python-pip

pip install --upgrade pip
```

python2.7 与 python3.6 兼容

[https://linuxize.com/post/how-to-install-python-3-on-centos-7/](https://linuxize.com/post/how-to-install-python-3-on-centos-7/)

推荐使用 conda

## Docker 安装

详见 [docker](./docker.html)

## ulimit

`/etc/security/limits.d/90-nproc.conf`

## 查看版本

```
uname -a
cat /etc/redhat-release
```
