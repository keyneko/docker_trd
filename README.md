# 安装Docker
```bash
# 卸载ubuntu自带的旧版本docker
$ apt-get remove docker docker-engine docker.io containerd runc

sudo apt update
sudo apt upgrade
# 依赖软件包
sudo apt-get install ca-certificates curl gnupg lsb-release
# 添加Docker官方GPG密钥
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# 添加Docker的软件源
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# 安装Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io
# 将当前用户添加到docker组
sudo usermod -aG docker $USER

# 启动docker来验证是否安装成功
systemctl start docker
# 安装工具
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# 重启docker
service docker restart
# 验证是否成功
sudo docker run hello-world
# 查看docker的版本
sudo docker version
# 查看镜像
sudo docker images
# 删除镜像
sudo docker rmi hello-world

# 列出所有容器
sudo docker ps -a 
# 删除容器
sudo docker rm 8321e07c0ff9
# 运行已停止的容器
sudo docker start 4b2dfddad1f5
# 连接到终端
sudo docker attach 4b2dfddad1f5

# 如果TLS handshake timeout（Docker镜像拉取错误）
sudo vim /etc/docker/daemon.json
{
	"registry-mirrors":[
		"https://6kx4zyno.mirror.aliyuncs.com",
		"https://docker.mirrors.ustc.edu.cn/"
	]
}
systemctl daemon-reload
systemctl restart docker
```

# 部署nextweb
```bash
$ sudo docker pull yidadaa/chatgpt-next-web
$ sudo docker run -d -p 3000:3000 \
	-e BASE_URL=https://api.openai-hub.com \
	-e OPENAI_API_KEY=sk-xxxxx \
	-e CODE=a1234 \
	yidadaa/chatgpt-next-web
```

# 部署lobehub
```bash
$ sudo docker pull lobehub/lobe-chat
$ sudo docker run -d -p 3210:3210 \
	-e OPENAI_API_KEY=sk-xxxxx \
	-e OPENAI_PROXY_URL=https://api.openai-proxy.com/v1 \
	-e ACCESS_CODE=a1234 \
	--name lobe-chat \
	lobehub/lobe-chat
```

# 安装clash-verge 
```bash
# Ubuntu 18.04安装clash-verge 
sudo dpkg -i clash-verge_1.5.2_amd64.deb
# 提示错误 version `GLIBC_2.28' not found (required by clash-verge)
sudo dpkg -r clash-verge

# TODO: 只能本地编译打包了, 基于tauri框架
https://tauri.app/v1/guides/getting-started/prerequisites/#setting-up-linux
git clone https://github.com/clash-verge-rev/clash-verge-rev.git
```

# 安装ShellCrash
```bash
git clone https://github.com/juewuy/ShellCrash.git
cd ShellCrash
# root用户安装
sudo su
bash ./install.sh
source /etc/profile &> /dev/null 
crash
crash -h
crash -u
```

# 部署webdav
```bash
sudo docker pull bytemark/webdav
sudo docker run --restart always \
    -e AUTH_TYPE=Digest \
    -e USERNAME=keyneko \
    -e PASSWORD=a1234 \
    -p 8080:80 \
    -v /srv/dav:/var/lib/dav \
    -d bytemark/webdav
```

# 部署esphome
```bash
docker pull ghcr.io/esphome/esphome

# 创建项目
docker run --rm -v "${PWD}":/config -it ghcr.io/esphome/esphome wizard livingroom.yaml

switch:
  - platform: gpio
    name: "Living Room Dehumidifier"
    pin: 5

# 上传固件
docker run --rm --privileged -v "${PWD}":/config --device=/dev/ttyUSB0 -it ghcr.io/esphome/esphome run livingroom.yaml

# 仪表板
docker run --rm --net=host -v "${PWD}":/config -it ghcr.io/esphome/esphome
```

# 部署gitea
```
sudo docker pull gitea/gitea
docker run -d --privileged=true \
	--restart=always \
	--name=gitea \
	-p 10022:22 \
	-p 13000:3000 \
	-v /usr/local/gitea:/data gitea/gitea:latest
```

# 部署Node-RED
```bash
docker pull nodered/node-red
chmod 0777 /usr/local/nodered
docker run -itd -p 1880:1880 -v /usr/local/nodered:/data -e TZ=Asia/Shanghai --name nodered nodered/node-red:latest
```
