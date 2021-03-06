# pytoch-docker
- 安装好 docker， docker-compose

- 写好 docker-compose.yml

```bash
# 构建容器
docker-compose up -d
# 查看容器id
docker container ls
# 进入容器
docker exec -it {container_id} bash
# 执行初始化安装脚本
sh system_init.sh
# 将 PermitRootLogin 的值从 withoutPassword 改为yes 允许root登陆
vi /etc/ssh/sshd_config
# 重启动ssh服务
service ssh restart
# 配置root密码
passwd root
# 若执行conda activate base 报错CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.，可以执行以下命令，进入base环境
source activate
```

- 若 ssh 连接失败，可以删除 .ssh 中 known_hosts 中关于这个ip的记录

- .ssh config 配置

```bash
Host ubuntu_zc
  HostName 192.168.31.3
  Port 1122
  User root
```

- system_init.sh 内容

```bash
#!/bin/bash

# 网络支持
apt-get update
apt install net-tools -y

# ssh 支持
apt install openssh-server -y
apt-get install openssh-client -y
service ssh start
# vi /etc/ssh/sshd_config # 将 PermitRootLogin 的值从 withoutPassword 改为yes 允许root登陆
# service ssh restart # 重启动ssh服务 
# 设置root密码（容器内运行）
# passwd root

# 常用软件支持
apt install vim -y
```

