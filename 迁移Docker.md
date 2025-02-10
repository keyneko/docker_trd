# 迁移 Docker 到 /mnt/sdb



##### 停止 Docker
```bash
sudo systemctl stop docker.socket
sudo systemctl stop docker
sudo systemctl status docker docker.socket
```

##### 复制现有 Docker 数据
```bash
sudo rsync -aH --info=progress2 /var/lib/docker/ /mnt/sdb/docker/
```

##### 修改 Docker 配置
```bash
sudo nano /etc/docker/daemon.json

{
  "data-root": "/mnt/sdb/docker"
}
```

##### 重启 Docker
```bash
sudo systemctl daemon-reload
sudo systemctl start docker.socket
sudo systemctl start docker

docker info | grep "Docker Root Dir"
```





#### 查看 Gitea 容器的挂载点配置
```bash
sudo docker inspect gitea
"Mounts": [
    {
        "Type": "bind",
        "Source": "/某个路径/在/宿主机上的目录",
        "Destination": "/data",
        ...
    }
]
```

##### 在容器内检查空间情况
```bash
sudo docker exec -it gitea df -h
```

##### 停止 Gitea 容器
```bash
sudo docker stop gitea
```

##### 在 HDD 上创建新目录
```bash
sudo mkdir -p /mnt/sdb/gitea
sudo chown -R $(whoami):$(whoami) /mnt/sdb/gitea
```

##### 迁移数据
```bash
sudo rsync -a /usr/local/gitea/ /mnt/sdb/gitea/
```

##### 更新 Docker 配置
```bash
sudo docker rm gitea
sudo docker run -d --name gitea -v /mnt/sdb/gitea:/data -p 13000:3000 -p 10022:22 gitea/gitea:latest
sudo docker start gitea
sudo docker exec -it gitea df -h
```





#### 如果 HA 运行缓慢，可以单独把 HA 数据库 放在 SSD (/var/lib/docker)，只把容器本体放在 HDD
```bash
sudo mv /mnt/sdb/docker/volumes/homeassistant_db /var/lib/docker/volumes/
```

##### 创建软链接
```bash
sudo ln -s /var/lib/docker/volumes/homeassistant_db /mnt/sdb/docker/volumes/
```

