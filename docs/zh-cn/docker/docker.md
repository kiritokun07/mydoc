## Docker快速入门


## 安装Docker脚本：
```
curl -sSL https://get.daocloud.io/docker | sh
systemctl start docker
systemctl enable docker
```

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
