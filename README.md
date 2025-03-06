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
docker pull esphome/esphome

# 创建项目
docker run --rm -v "${PWD}":/config -it esphome/esphome wizard livingroom.yaml

switch:
  - platform: gpio
    name: "Living Room Dehumidifier"
    pin: 5

# 上传固件
docker run --rm --privileged -v "${PWD}":/config --device=/dev/ttyUSB0 -it esphome/esphome run livingroom.yaml

# 仪表板
docker run --rm --net=host -v "${PWD}":/config -it esphome/esphome
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
docker pull nodered/node-red:latest
chmod 0777 /usr/local/nodered
docker run -itd -p 1880:1880 -v /usr/local/nodered:/data -e TZ=Asia/Shanghai --name nodered nodered/node-red:latest
# 查看容器内node版本
docker exec -it a6d0731e22bb /bin/bash
node -v
```

# 部署home assistant
```bash
sudo docker pull homeassistant/home-assistant
sudo mkdir -p /data/homeassistant/config
sudo chmod -R 777 /data/homeassistant/
sudo docker run -d \
  --restart always \
  --name homeassistant  \
  -v /data/homeassistant/config:/config \
  -e TZ=Asia/Shanghai   \
  -p 8123:8123   \
  homeassistant/home-assistant:latest

# 安装hacs
sudo docker exec -it 845f349a2372 bash
845f349a2372:/config# wget -O - https://get.hacs.xyz | bash -
```

# 安装mosquitto
```bash
sudo docker pull eclipse-mosquitto
sudo mkdir -p /usr/local/mosquitto/config
sudo mkdir -p /usr/local/mosquitto/data
sudo mkdir -p /usr/local/mosquitto/log
sudo nano /usr/local/mosquitto/config/mosquitto.conf
> persistence true
> persistence_location /usr/local/mosquitto/data
> log_dest file /usr/local/mosquitto/log/mosquitto.log
> listener 9001
> port 1883
> allow_anonymous true

sudo chmod -R 755 /usr/local/mosquitto
sudo chmod -R 777 /usr/local/mosquitto/log

sudo docker run -it --name=mosquitto --privileged \
  -p 1883:1883 -p 9001:9001 \
  -v /usr/local/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf \
  -v /usr/local/mosquitto/data:/mosquitto/data \
  -v /usr/local/mosquitto/log:/mosquitto/log \
  -d eclipse-mosquitto
```


# 蒲公英访问端
```bash
sudo docker pull bestoray/pgyvpn
sudo docker run -d --device=/dev/net/tun --net=host --cap-add=NET_ADMIN \
  --env PGY_USERNAME="8687542:003" \
  --env PGY_PASSWORD="kr3PbHJTekr5kFj" \
  bestoray/pgyvpn
```

# 部署ROS2
```bash
sudo docker pull osrf/ros:humble-desktop-full-jammy
# 创建容器
sudo docker run -it --rm osrf/ros:humble-desktop-full-jammy
# 订阅者节点
ros2 run demo_nodes_cpp listener
# 再创建个容器
sudo docker run -it --rm osrf/ros:humble-desktop-full-jammy
# 发布者节点
ros2 run demo_nodes_cpp talker

xhost +local:docker  # 允许本地Docker容器访问
sudo docker run -it --rm \
  --env DISPLAY=$DISPLAY \
  --volume /tmp/.X11-unix:/tmp/.X11-unix \
  osrf/ros:humble-desktop-full-jammy

ros2 pkg executables turtlesim
ls -l /opt/ros/humble/lib/turtlesim/
ls -l /opt/ros/humble/setup.bash

source /opt/ros/humble/setup.bash
# 将图形输出发送Win10（可选）
export DISPLAY=192.168.1.130:0.0
ros2 run turtlesim turtlesim_node

# 新的终端使用docker exec命令连接已经运行的ROS2容器操作
sudo docker ps -a # 需要用NAMES
sudo docker exec -it romantic_ptolemy /bin/bash
source /opt/ros/humble/setup.bash
ros2 run turtlesim turtle_teleop_key

# 或直接新打开容器
# 创建 Docker 网络
sudo docker network create ros-net

# 启动第一个容器（可视化节点）
sudo docker run -it --rm \
  --net=ros-net \
  --name turtlesim-node \
  --env DISPLAY=$DISPLAY \
  --volume /tmp/.X11-unix:/tmp/.X11-unix \
  osrf/ros:humble-desktop-full-jammy \
  bash -c "source /opt/ros/humble/setup.bash && ros2 run turtlesim turtlesim_node"

# 启动第二个容器（控制节点）
sudo docker run -it --rm \
  --net=ros-net \
  osrf/ros:humble-desktop-full-jammy \
  bash -c "source /opt/ros/humble/setup.bash && ros2 run turtlesim turtle_teleop_key"

# 查看服务
ros2 service list
# 查看话题
ros2 topic list

# 启动时挂载脚本目录
sudo docker run -it --rm \
  --net=ros-net \
  -v $(pwd):/app \
  osrf/ros:humble-desktop-full-jammy
source /opt/ros/humble/setup.bash
# 小龟复位
ros2 service call /reset std_srvs/srv/Empty "{}"
python3 turtle_teleop.py
```

```python
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class TurtleTeleop(Node):
	def __init__(self):
		super().__init__('turtle_teleop')
		self.publisher_ = self.create_publisher(Twist, '/turtle1/cmd_vel', 10)
		self.timer = self.create_timer(0.1, self.timer_callback) # 100 Hz

	def timer_callback(self):
		msg = Twist()
		msg.linear.x = 0.5 # 2 m/s forward
		msg.angular.z = 0.25 # No rotation
		self.publisher_.publish(msg)
		self.get_logger().info('Published velocity command')
		
def main(args=None):
	rclpy.init(args=args)
	turtle_teleop = TurtleTeleop()
	rclpy.spin(turtle_teleop)
	turtle_teleop.destroy_node()
	rclpy.shutdown()

if __name__ == '__main__':
	main()
```

# 部署microros代理
```bash
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1

sudo docker pull microros/micro-ros-agent:humble
# 串口模式
sudo docker run -it --rm --device=/dev/ttyUSB0 --net=host microros/micro-ros-agent:humble serial --dev /dev/ttyUSB0 -b 115200 -v4
sudo docker run -it --rm -v /dev:/dev -v /dev/shm:/dev/shm --privileged --net=host microros/micro-ros-agent:humble serial --dev /dev/ttyUSB0 -v6
# WiFi模式
sudo docker run -it --rm --net=host microros/micro-ros-agent:humble udp4 --port 8888 -v6
sudo docker run -it --rm -v /dev:/dev -v /dev/shm:/dev/shm --privileged --net=host microros/micro-ros-agent:humble udp4 --port 8888 -v6

# 启动容器
sudo docker run -it --rm \
  --net=host \
  -v $(pwd):/app \
  osrf/ros:humble-desktop-full-jammy
source /opt/ros/humble/setup.bash
ros2 topic list

# 订阅话题
ros2 topic echo /esp32_s3_chatter
data: Hello from ESP32-S3
---
data: Hello from ESP32-S3
---
data: Hello from ESP32-S3
---
data: Hello from ESP32-S3
---
data: Hello from ESP32-S3
---

export ROS_DOMAIN_ID=1
echo $ROS_DOMAIN_ID
ros2 topic pub /from_ubuntu std_msgs/msg/String "{data: 'Hello from Ubuntu'}"

# ROS2创建功能包
ros2 pkg create my_chatter --build-type ament_python --dependencies rclpy std_msgs
# 构建并运行节点
colcon build --packages-select my_chatter
source install/setup.bash
ros2 run my_chatter ubuntu_publisher
ros2 run my_chatter esp32_subscriber
# 手动发送消息
ros2 topic pub /from_esp32 std_msgs/msg/String "data: 'hello from esp32'" --once
# 查看活跃话题
ros2 topic list
ros2 topic echo /from_ubuntu

```
