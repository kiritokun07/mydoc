## Docker快速入门


## 安装Docker脚本：
```
curl -sSL https://get.daocloud.io/docker | sh
systemctl start docker
systemctl enable docker
```

## docker 安装redis
```
docker search redis
docker pull redis
docker run -d --name myredis -p6380:6379 -v /d/dockerdata/redis/data:/data redis --appendonly yes
```
-d —— 后台运行
–name —— 实例运行后的名字 myredis
-p6379:6379 —— 端口映射，冒号前面是windows下的端口，后面是虚拟机的端口
-v /d/dockerdata/redis/data:/data —— 保存数据的位置。
d:\dockerdata\redis\data 前面是windows下的实际保存数据目录
/data 虚拟机内的目录
redis-server –appendonly yes —— 在容器执行redis-server启动命令，并打开redis持久化配置

如果创建时未指定 --restart=always ,可通过update 命令

`docker update --restart=always xxx`

Docker容器开机自动启动
https://blog.csdn.net/lin521lh/article/details/78413631

## 安装Speedtest：
`docker run -d -p 6688:80 ilemonrain/html5-speedtest:alpine --restart=always`

## 安装Portainer：
`docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /home/docker/portainer:/data --name portainer --restart=always portainer/portainer`

## 安装SMB支持:
```
yum install cifs-utils -y
echo mount -t cifs //192.168.1.235/dockertest /home/smb -o username=docker,password=test1234  /etc/rc.local
chmod 755 /etc/rc.local
chmod -R 755 /etc/rc.d
```

## docker 安装minio
https://mp.weixin.qq.com/s/kvLZRqgm1lEITm1j6rzJ0A

https://www.jianshu.com/p/52dbc679094a
```
docker run -p 9090:9000 --name minio1 -d --restart=always -e MINIO_ACCESS_KEY=minio -e MINIO_SECRET_KEY=minio@321 -v /data/docker/minio/data:/data -v /data/docker/minio/config:/root/.minio minio/minio server /data --console-address ":9000" --address ":9090"
```

Docker minio 安装中的一些错误
https://blog.csdn.net/qq_44924407/article/details/119064437

## docker 安装jenkins
TODO

## docker 安装nexus
TODO







