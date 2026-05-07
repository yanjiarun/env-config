# Ubuntu 22.04 + ROS2 + PX4 + Gazebo SITL 仿真环境配置

## 1. 系统准备与基础依赖安装
（默认已安装好Ubuntu 22.04.5 LTS）

### 1.1 中文输入法安装
```bash
sudo apt update
sudo apt install fcitx5 fcitx5-chinese-addons fcitx5-frontend-gtk4 fcitx5-frontend-gtk3 fcitx5-frontend-qt5
fcitx5 -d
```

### 1.2 系统语言环境配置
```bash
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

### 1.3 软件仓库配置
```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
```

### 1.4 基础软件安装（部分可选）

提供网络工具如ifconfig、netstat等，用于网络接口管理：
```bash
sudo apt install net-tools
```

分布式版本控制系统，用于代码管理和协作：
```bash
sudo apt install git
```

CA证书，用于SSL/TLS连接验证：
```bash
sudo apt install ca-certificates
```

安装VS Code编辑器：
```bash
sudo apt install ./code_1.113.0-1774364744_amd64.deb
```

ARM嵌入式交叉编译工具链，用于编译PX4固件：
```bash
sudo apt install gcc-arm-none-eabi
```

移除调制解调器管理器，避免与串口设备冲突：
```bash
sudo apt-get remove modemmanager
```

GStreamer多媒体框架插件，用于视频流处理：
```bash
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl
```

FUSE（用户空间中的文件系统）库，支持挂载用户空间文件系统：
```bash
sudo apt install libfuse2
```

X11相关的图形库，用于桌面环境显示支持：
```bash
sudo apt install libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor-dev
```

ROS可视化调试工具集：
```bash
sudo apt install rqt-common-plugins
```

Python包管理器，用于安装Python库：
```bash
sudo apt install python3-pip
```

Python Tkinter库，用于GUI界面：
```bash
sudo apt install python3-tk
```

ROS安装生成器工具：
```bash
sudo apt install python3-rosinstall-generator
```

版本控制系统工具和OSRF Python通用库：
```bash
sudo apt install python3-vcstool python3-osrf-pycommon
```

ROS依赖管理工具：
```bash
sudo apt install python3-rosdep2
```

Mavlink协议转PCAP格式工具，用于网络包捕获分析：
```bash
sudo apt install mavlink2pcap
```

网络协议分析工具，用于实时网络流量监控：
```bash
sudo apt install wireshark
```

### 1.5 常用网站
- [ROS2 Humble 官方文档](https://ros2docs.robook.org/humble/) - `https://ros2docs.robook.org/humble/`
- [ROS2 Humble 源码仓库](https://github.com/ros2/ros2) - `https://github.com/ros2/ros2`
- [ArduPilot 官方文档](https://ardupilot.org/) - `https://ardupilot.org/`
- [ArduPilot 源码仓库](https://github.com/ArduPilot/ardupilot) - `https://github.com/ArduPilot/ardupilot`
- [QGroundControl 官方文档](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/index.html) - `https://docs.qgroundcontrol.com/master/en/qgc-user-guide/index.html`
- [MAVLink 官方文档](https://mavlink.io/en/) - `https://mavlink.io/en/`
- [mavros_msgs 官方文档](https://docs.ros.org/en/humble/p/mavros_msgs/) - `https://docs.ros.org/en/humble/p/mavros_msgs/`
- [MAVROS 官方文档](https://docs.ros.org/en/humble/p/mavros/) - `https://docs.ros.org/en/humble/p/mavros/`
- [PX4 官方文档](https://docs.px4.io/main/zh/) - `https://docs.px4.io/main/zh/`
- [PX4 Gazebo 模型仓库](https://github.com/PX4/PX4-gazebo-models) - `https://github.com/PX4/PX4-gazebo-models`
- [PX4 Gazebo 仿真 官方文档](https://docs.px4.io/main/zh/sim_gazebo_gz/) - `https://docs.px4.io/main/zh/sim_gazebo_gz/`
- [Gazebo Harmonic 官方文档](https://gazebosim.org/docs/harmonic/) - `https://gazebosim.org/docs/harmonic/`
- [Gazebo Sim 官方文档](https://gazebosim.org/api/sim/9/index.html) - `https://gazebosim.org/api/sim/9/index.html`
- [Gazebo Sim 创建插件 官方文档](https://gazebosim.org/api/sim/9/createsystemplugins.html) - `https://gazebosim.org/api/sim/9/createsystemplugins.html`
- [ros_gz_bridge 官方文档](https://docs.ros.org/en/ros2_packages/humble/api/ros_gz_bridge/) - `https://docs.ros.org/en/ros2_packages/humble/api/ros_gz_bridge/`

## 2. ROS2 Humble 安装与配置

### 2.1 添加ROS2仓库和密钥
```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### 2.2 安装ROS2 Humble Desktop
```bash
sudo apt update
sudo apt install ros-humble-desktop
```

### 2.3 配置ROS2环境变量
```bash
source /opt/ros/humble/setup.bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

## 3. MAVROS 安装与配置

### 3.1 安装MAVROS包
```bash
sudo apt update
sudo apt install -y ros-humble-mavros ros-humble-mavros-extras
```

### 3.2 配置MAVROS数据集
```bash
source /opt/ros/humble/setup.bash
ros2 run mavros install_geographiclib_datasets.sh
```


## 4. PX4 安装

### 4.1 克隆PX4仓库
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```

### 4.2 安装PX4依赖
```bash
cd PX4-Autopilot
./Tools/setup/ubuntu.sh
```

若需重新配置代理，请执行以下命令
```bash
cd PX4-Autopilot
http_proxy= https_proxy= bash ./Tools/setup/ubuntu.sh
```

## 5. Gazebo 仿真环境配置

### 5.1 安装Gazebo ROS2依赖
ROS2 Humble与Gazebo Harmonic集成包，提供ROS2与Gazebo仿真环境的桥梁功能：
```bash
sudo apt install ros-humble-ros-gzharmonic
```

## 6. PX4-Gazebo-ROS2 SITL (Simulation in the Loop)设置 （核心）

### 6.1 说明
本仿真环境用于实现固定翼无人机通过云台相机对地面车辆进行视觉伺服跟踪控制。

### 6.2 Gazebo 中添加模型
#### 6.2.1 添加带云台相机的固定翼无人机模型

官方仓库PX4-Autopilot/Tools/simulation/gz/models/文件夹下有支持PX4 SITL的gazebo模型,其中有gimbal文件夹是云台相机模型，rc_cessna文件夹是固定翼无人机模型。只需在models文件夹下添加rc_cessna_gimbal文件夹，并在rc_cessna_gimbal文件夹下添加model.config和model.sdf文件即可。文件内容如下：
- model.config文件:
    ```xml
    <?xml version="1.0"?>
    <model>
    <name>rc_cessna_gimbal</name>
    <version>1.0</version>
    <sdf version='1.9'>model.sdf</sdf>

    <author>
    <name>Benjamin Perseghetti</name>
    <email>bperseghetti@rudislabs.com</email>
    </author>

    <description>
        This is a model of an RC Cessna 182 with a camera attached on a gimbal.
    </description>
    </model>
    ```
- model.sdf文件:
    ```xml
    <?xml version="1.0"?>
    <sdf version='1.9'>
    <model name='rc_cessna_gimbal'>
        <include merge='true'>
        <uri>model://rc_cessna</uri>
        </include>
        <include merge='true'>
        <uri>model://gimbal</uri>
        <pose>0 0 0.26 0 0 3.14</pose>
        </include>
        <joint name="GimbalAttachJoint" type="fixed">
        <parent>base_link</parent>
        <child>cgo3_mount_link</child>
        </joint>
    </model>
    </sdf>
    ```
同时要修改gimbal文件夹下的model.sdf文件，减小gimbal模型质并去掉相机碰撞属性，使带云台相机的固定翼无人机模型rc_cessna_gimbal能正常起飞（与rc_cessna模型属性基本一致）。
具体修改内容如下：
- 四处mass由0.1修改为0.00001
    ```xml
    <mass>0.00001</mass>
    ```
- 去掉相机碰撞属性
    ```xml
    <!-- <collision name='cgo3_camera_collision'>
        <pose>-0.0412 0 -0.162 0 0 0</pose>
        <geometry>
          <sphere>
            <radius>0.035</radius>
          </sphere>
        </geometry>
        <surface>
          <friction>
            <ode>
              <mu>1</mu>
              <mu2>1</mu2>
            </ode>
          </friction>
          <contact>
            <ode>
              <kp>1e+8</kp>
              <kd>1</kd>
              <max_vel>0.01</max_vel>
              <min_depth>0.001</min_depth>
            </ode>
          </contact>
        </surface>
      </collision> -->
    ```
若要PX4 和Gazebo 分开运行（PX4_GZ_STANDALONE=1），需将PX4-Autopilot/Tools/simulation/gz/models/文件夹下的gimbal文件夹和rc_cessna_gimbal文件夹添加到.simulation-gazebo/models/文件夹下。（因为PX4_GZ_STANDALONE=1时，Gazebo 会从.simulation-gazebo/models/文件夹下加载模型。）

#### 6.2.2 添加Evata车辆模型
将Evata车辆模型添加到PX4-Autopilot/Tools/simulation/gz/models/文件夹下。

同理，若要PX4 和Gazebo 分开运行（PX4_GZ_STANDALONE=1），需将PX4-Autopilot/Tools/simulation/gz/models/文件夹下的Evata车辆文件夹添加到.simulation-gazebo/models/文件夹下。（因为PX4_GZ_STANDALONE=1时，Gazebo 会从.simulation-gazebo/models/文件夹下加载模型。）

### 6.3 PX4 中添加新模型
（可参考PX4官方文档`https://docs.px4.io/main/zh/sim_gazebo_gz/` 中的 [Adding New Worlds and Models](https://docs.px4.io/main/zh/sim_gazebo_gz/#adding-new-worlds-and-models)

在~/PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes/文件夹下创建4030_gz_rc_cessna_gimbal文件，文件内容如下：
```bash
#!/bin/sh
#
# @name Gazebo rc_cessna gimbal
# @type Fixedwing
#

PX4_SIM_MODEL=${PX4_SIM_MODEL:=rc_cessna_gimbal}

. ${R}etc/init.d-posix/airframes/4003_gz_rc_cessna

# Gimbal settings
param set-default MNT_MODE_IN 4
param set-default MNT_MODE_OUT 2
param set-default MNT_RC_IN_MODE 1

param set-default MNT_MAN_ROLL 1
param set-default MNT_MAN_PITCH 2
param set-default MNT_MAN_YAW 3

param set-default MNT_RANGE_ROLL 180
param set-default MNT_MAX_PITCH 45
param set-default MNT_MIN_PITCH -135
param set-default MNT_RANGE_YAW 720
```

并为机架添加 CMake 编译目标，即在~/PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes/CMakeLists.txt文件中添加一行：
```bash
4030_gz_rc_cessna_gimbal
```

在~/PX4-Autopilot/Tools/simulation/gz/worlds/文件夹下复制原default.sdf文件并重命名为evata.sdf文件，文件内容做相应修改。

修改sdf中world名字：
```xml
<world name="evata">
```
添加Evata模型到内容末尾world标签前：
```xml
  <model name="my_ground_vehicle">
      <include>
          <uri>model://Evata</uri>
      </include>

      <!-- 设置车辆在地面上的初始位置 (X, Y, Z, Roll, Pitch, Yaw) -->
      <pose>0 0 0 0 0 1.5708</pose>
  </model>
  </world>
```
至此，evata.sdf世界配置完成，即默认world中添加了一个名为my_ground_vehicle的模型，模型位置为（0， 0， 0， 0, 0， 1.5708）,车头指向x轴。


### 6.4 设置ros_gz_bridge进行gz和ros2话题的转接
（可参考ros_gz_bridge官方文档`https://docs.ros.org/en/ros2_packages/humble/api/ros_gz_bridge/#example-5-configuring-the-bridge-via-yaml` 中的 [Example 5: Configuring the Bridge via YAML](https://docs.ros.org/en/ros2_packages/humble/api/ros_gz_bridge/#example-5-configuring-the-bridge-via-yaml)）

新建camera_and_evata_bridge.yaml文件（此处在~/gz_ros路径下），文件内容如下：
```yaml
- ros_topic_name: "/camera/image_raw"
  gz_topic_name: "/world/evata/model/rc_cessna_gimbal_0/link/camera_link/sensor/camera/image"
  ros_type_name: "sensor_msgs/msg/Image"
  gz_type_name: "gz.msgs.Image"
  direction: "GZ_TO_ROS"

- ros_topic_name: "/camera/camera_info"
  gz_topic_name: "/world/evata/model/rc_cessna_gimbal_0/link/camera_link/sensor/camera/camera_info"
  ros_type_name: "sensor_msgs/msg/CameraInfo"
  gz_type_name: "gz.msgs.CameraInfo"
  direction: "GZ_TO_ROS"

- ros_topic_name: "/Evata/odom"
  gz_topic_name: "/Evata/odom"
  ros_type_name: "nav_msgs/msg/Odometry"
  gz_type_name: "gz.msgs.Odometry"
  direction: "GZ_TO_ROS"

- ros_topic_name: "/Evata/cmd_vel"
  gz_topic_name: "/Evata/cmd_vel"
  ros_type_name: "geometry_msgs/msg/Twist"
  gz_type_name: "gz.msgs.Twist"
  direction: "ROS_TO_GZ"
```

camera_and_evata_bridge.yaml文件同级目录（此处为cd ~/gz_ros）下打开终端，运行以下命令启动话题转接：
```bash
cd ~/gz_ros
ros2 run ros_gz_bridge parameter_bridge --ros-args -p config_file:=./camera_and_evata_bridge.yaml
```
运行成功后，终端将会输出以下信息：
```bash
[INFO] [1778056422.193349100] [ros_gz_bridge]: Creating GZ->ROS Bridge: [/world/evata/model/rc_cessna_gimbal_0/link/camera_link/sensor/camera/image (gz.msgs.Image) -> /camera/image_raw (sensor_msgs/msg/Image)] (Lazy 0)
[INFO] [1778056422.194673318] [ros_gz_bridge]: Creating GZ->ROS Bridge: [/world/evata/model/rc_cessna_gimbal_0/link/camera_link/sensor/camera/camera_info (gz.msgs.CameraInfo) -> /camera/camera_info (sensor_msgs/msg/CameraInfo)] (Lazy 0)
[INFO] [1778056422.195082390] [ros_gz_bridge]: Creating GZ->ROS Bridge: [/Evata/odom (gz.msgs.Odometry) -> /Evata/odom (nav_msgs/msg/Odometry)] (Lazy 0)
[INFO] [1778056422.195875323] [ros_gz_bridge]: Creating ROS->GZ Bridge: [/Evata/cmd_vel (geometry_msgs/msg/Twist) -> /Evata/cmd_vel (gz.msgs.Twist)] (Lazy 0)
```

在ros2工作空间（此处为~/gz_ros/src）中创建自定义ros2包`gz_pose_bridge`，实现从gazebo话题中获取车辆、无人机、相机位姿消息并发布到ROS2话题，包文件结构如下：
```
~/gz_ros/
└──src/
   └──gz_pose_bridge/
      ├── CMakeLists.txt
      ├── package.xml
      └── src/
          └── gz_pose_bridge.cpp
```
gz_pose_bridge.cpp文件内容如下：
```cpp
#include <rclcpp/rclcpp.hpp>
#include <gz/transport/Node.hh>
#include <gz/msgs/pose_v.pb.h>
#include <geometry_msgs/msg/pose_stamped.hpp>
#include <unordered_map>
#include <vector>

class MinimalPoseBridge : public rclcpp::Node {
public:
  MinimalPoseBridge() : Node("minimal_pose_bridge") {
    // 可配置的目标名称列表
    this->declare_parameter<std::vector<std::string>>(
      "target_names", 
      {"camera_link", "my_ground_vehicle", "rc_cessna_gimbal_0"}
    );
    
    auto names = this->get_parameter("target_names").as_string_array();
    
    for (const auto& name : names) {
      publishers_[name] = this->create_publisher<geometry_msgs::msg::PoseStamped>(
        "/" + name + "/pose", 10);
    }
    
    gz_node_ = std::make_unique<gz::transport::Node>();
    
    if (!gz_node_->Subscribe("/world/evata/dynamic_pose/info", 
                           &MinimalPoseBridge::onPose, this)) {
      RCLCPP_ERROR(this->get_logger(), "Subscribe failed!");
    }

    // 打印订阅话题名和发布话题名
    RCLCPP_INFO(this->get_logger(), "Subscribed to topic: /world/evata/dynamic_pose/info");
    for (const auto& name : names) {
      RCLCPP_INFO(this->get_logger(), "Publishing to topic: /%s/pose", name.c_str());
    }
  }

private:
  void onPose(const gz::msgs::Pose_V& msg) {
    for (int i = 0; i < msg.pose_size(); ++i) {
      const auto& p = msg.pose(i);
      if (publishers_.count(p.name())) {
        geometry_msgs::msg::PoseStamped ros_pose;
        ros_pose.header.stamp.sec = msg.header().stamp().sec();
        ros_pose.header.stamp.nanosec = msg.header().stamp().nsec();
        ros_pose.header.frame_id = "world";
        
        ros_pose.pose.position.x = p.position().x();
        ros_pose.pose.position.y = p.position().y();
        ros_pose.pose.position.z = p.position().z();
        
        ros_pose.pose.orientation.x = p.orientation().x();
        ros_pose.pose.orientation.y = p.orientation().y();
        ros_pose.pose.orientation.z = p.orientation().z();
        ros_pose.pose.orientation.w = p.orientation().w();
        
        publishers_[p.name()]->publish(ros_pose);
      }
    }
  }
  
  std::unique_ptr<gz::transport::Node> gz_node_;
  std::unordered_map<std::string, rclcpp::Publisher<geometry_msgs::msg::PoseStamped>::SharedPtr> publishers_;
};

int main(int argc, char** argv) {
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPoseBridge>());
  rclcpp::shutdown();
  return 0;
}
```

CMakeLists.txt文件内容如下：
```bash
cmake_minimum_required(VERSION 3.8)
project(gz_pose_bridge)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(geometry_msgs REQUIRED)

# 针对 Gazebo Harmonic (gz-transport13, gz-msgs10)
find_package(gz-transport13 REQUIRED)
find_package(gz-msgs10 REQUIRED)

add_executable(gz_pose_bridge_node src/gz_pose_bridge.cpp)
target_compile_features(gz_pose_bridge_node PUBLIC cxx_std_17)

ament_target_dependencies(gz_pose_bridge_node
  rclcpp
  geometry_msgs
)

target_link_libraries(gz_pose_bridge_node
  gz-transport13::core
  gz-msgs10::core
)

install(TARGETS gz_pose_bridge_node
  DESTINATION lib/${PROJECT_NAME}
)

ament_package()
```
package.xml文件内容如下：
```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>gz_pose_bridge</name>
  <version>0.0.1</version>
  <description>Minimal Gazebo Harmonic to ROS 2 pose bridge</description>
  <maintainer email="rainbow@todo.todo">rainbow</maintainer>
  <license>Apache-2.0</license>

  <depend>rclcpp</depend>
  <depend>geometry_msgs</depend>
  <depend>gz_transport</depend>
  <depend>gz_msgs</depend>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```

终端编译（假设ros2工作空间为~/gz_ros/src）：
```bash
cd ~/gz_ros
colcon build --packages-select gz_pose_bridge
```

### 6.5 启动仿真

#### Step 1: 启动 PX4 Gazebo 仿真环境
参考PX4官方文档[https://docs.px4.io/main/zh/sim_gazebo_gz/#usage-configuration-options](https://docs.px4.io/main/zh/sim_gazebo_gz/#usage-configuration-options)
```bash
cd ~/PX4-Autopilot
PX4_SYS_AUTOSTART=4030 PX4_GZ_MODEL_POSE="-20,-10" PX4_SIM_MODEL=gz_rc_cessna_gimbal PX4_GZ_WORLD=evata ./build/px4_sitl_default/bin/px4
```
其中：
- PX4_SYS_AUTOSTART=4030：启动PX4时，机型配置文件，4030为自定义rc_cessna_gimbal机型
- PX4_GZ_MODEL_POSE="-20,-10"：设置PX4在Gazebo中的初始位置为（-20, -10, 0, 0, 0, 0），即无人机位于x轴负方向20米，y轴负方向10米，z轴高度0米，旋转0度
- PX4_SIM_MODEL=gz_rc_cessna_gimbal：使用gazebo中自定义的带云台的固定翼无人机模型
- PX4_GZ_WORLD=evata：使用gazebo中自定义的evata世界

也可使用make命令启动仿真环境，但车辆和无人机位置都会在gazebo原点：
```bash
cd ~/PX4-Autopilot
make px4_sitl gz_rc_cessna_gimbal_evata
```
若报错未找到target,可先执行make clean，再执行make px4_sitl gz_rc_cessna_gimbal_evata

#### Step 2: 启动 GZ <-> ROS2 话题转接
在camera_and_evata_bridge.yaml文件同级目录（此处为cd ~/gz_ros）下打开终端中运行：
```bash
cd ~/gz_ros
ros2 run ros_gz_bridge parameter_bridge --ros-args -p config_file:=./camera_and_evata_bridge.yaml
```
启动自定义ros2节点`gz_pose_bridge_node`：
```bash
cd ~/gz_ros
source install/setup.bash
ros2 run gz_pose_bridge gz_pose_bridge_node
```

#### Step 3: 启动 Mavros 节点
```bash
ros2 launch mavros px4.launch fcu_url:=udp://127.0.0.1:14540@localhost:14557
```
注：udp://[local_bind_ip]:[local_port]@[remote_ip]:[remote_port]，其中local_bind_ip为本地IP地址，local_port为本地端口号，remote_ip为远程IP地址，remote_port为远程端口号。mavros中实际忽略了 remote_port，仍使用自动回发。（For UDP connection, the remote port in URL is only used if no packet has been received yet. Once a packet is received from a remote host, all communication is directed to that host.）

终端输入
```bash
ros2 topic echo /mavros/state
```
若connected为true，则说明Mavros节点连接PX4成功：
```bash
header:
  stamp:
    sec: 1778033063
    nanosec: 718739787
  frame_id: ''
connected: true
armed: false
guided: true
manual_input: false
mode: AUTO.LOITER
system_status: 0
---
```

#### 常用ROS2话题和服务汇总
`gz_pose_bridge`(自定义节点)
- /camera_link/pose [geometry_msgs::msg::PoseStamped]：获取云台相机位姿信息
- /rc_cessna_gimbal_0/pose [geometry_msgs::msg::PoseStamped]：获取固定翼无人机位姿信息
- /my_ground_vehicle/pose [geometry_msgs::msg::PoseStamped]：获取车辆位姿信息

`ros_gz_bridge`
- /Evata/cmd_vel [geometry_msgs::msg::Twist]：控制车辆的线速度和角速度
- /Evata/odom [nav_msgs::msg::Odometry]：获取车辆的位姿信息

- /camera/image_raw [sensor_msgs::msg::Image]：获取固定翼云台相机图像
- /camera/camera_info [sensor_msgs::msg::CameraInfo]：获取固定翼云台相机信息，包括相机参数、图像尺寸等

`mavros`
- /mavros/state [mavros_msgs::msg::State]：获取Mavros节点的状态
- /mavros/imu/data [sensor_msgs::msg::Imu]：获取IMU数据，包括加速度、角速度、磁力计等
- /mavros/mount_control/command [mavros_msgs::msg::MountControl]：控制固定翼云台的指向
- /mavros/cmd/arming [mavros_msgs::srv::CommandBool]：控制固定翼无人机解锁状态
- /mavros/set_mode [mavros_msgs::srv::SetMode]：设置固定翼无人机飞行模式
- /mavros/cmd/takeoff [mavros_msgs::srv::CommandTOL]：固定翼无人机起飞命令


### 6.6 删除仿真缓存文件
若仿真缓存文件过多，可删除缓存文件：
- PX4的飞行日志在：
~/PX4-Autopilot/build/px4_sitl_default/rootfs/log/
  ```bash
  rm -rfv ~/PX4-Autopilot/build/px4_sitl_default/rootfs/log/*
  ```
- ROS2的日志文件在：
~/.ros/log/
  ```bash
  rm -rfv ~/.ros/log/*
  ```

