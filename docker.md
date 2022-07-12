---
title: docker
post: 2021-12-19
tags: [docker, linux, container]
---

## 安装

### 快捷安装

不要忘了添加当前用户到 docker 组。

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 官方的安装教程

```shell
# 卸载旧版
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get install ca-certificates curl gnupg lsb-release

# 秘钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 添加源
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 安装
sudo apt-get install docker-ce docker-ce-cli containerd.io
# 安装 docker-compose
sudo apt-get install docker-compose

# 测试
sudo docker run hello-world

# 卸载
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

## 添加当前用户到 docker 组

```sh
# 查看 docker 组中用户列表
sudo cat /etc/group | grep docker
# 添加当前用户到 docker 组
sudo gpasswd -a ${USER} docker
# 重启 docker 服务
sudo systemctl restart docker
# socket 权限
sudo chmod 666 /var/run/docker.sock
```

## 常用命令

```bash
docker ps -aq # 列出所有容器的 ID
docker stop $(docker ps -aq) # 停止所有的容器
docker rm $(docker ps -aq) # 删除所有的容器
docker rm $(docker images -q) # 删除所有的镜像
docker cp mycontainer:/opt/file.txt /opt/local/
docker cp /opt/local/file.txt mycontainer:/opt/
```

```bash
docker ps -a # 查看所有容器状态
docker start|restart|stop [ID] # 启动已停止的容器
docker pull ubuntu
docker attach [ID] # 进入容器，退出时容器停止
docker exec [ID] # 进入容器，退出时容器不会停止
docker exec -it ubuntu_bash bash
```

### image

```bash
docker image ls
docker image rm [IMAGE]
```

### containers

```bash
docker container inspect [containerID] | grep IP # 查容器 ip

docker container ls #查看当前运行容器
docker container ls -a #查看已停止的容器
docker container stop [containerID]
docker container rm [containerID]
```

## run

```bash
docker run [options] mysql:5.7.3
-rm
-p 127.0.0.1:80:80
-it /bin/bash # 允许 stdin 交互，指定终端
-d # 后台
--name [container_name] # 指定名字
--volume $PWD/:/var/www/html # 目录映射
--env MYSQL_ROOT_PASSWORD=123456 # 可指定多个环境变量
--link wordpressdb:mysql
-privileged=true # 给 root 权限
```

## docker-compose

```bash
# 下载
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
# 添加可执行权限
sudo chmod +x /usr/local/bin/docker-compose
# 测试安装结果
docker-compose --version
```

### docker-compose 常用命令

```bash
docker-compose build web
docker-compose up -d database_default
docker-compose run web python manage.py migrate
docker-compose run web python manage.py createsuperuser
docker-compose up -d
```
