# Ubuntu安装Docker
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


# 部署nextweb
$ sudo docker pull yidadaa/chatgpt-next-web
$ sudo docker run -d -p 3000:3000 \
	-e BASE_URL=https://api.openai-hub.com \
	-e OPENAI_API_KEY=sk-1Y91GtNHfVEdvJDJ15B83eF300A7426bA64a167e3c5f8b36 \
	-e CODE=a1234 \
	yidadaa/chatgpt-next-web

# 部署lobehub
$ sudo docker pull lobehub/lobe-chat
$ sudo docker run -d -p 3210:3210 \
	-e OPENAI_API_KEY=sk-1Y91GtNHfVEdvJDJ15B83eF300A7426bA64a167e3c5f8b36 \
	-e ACCESS_CODE=a1234 \
	--name lobe-chat \
	lobehub/lobe-chat

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
