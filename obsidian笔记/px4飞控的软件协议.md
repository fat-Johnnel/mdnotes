# mavlink 和mavros
## 二、MAVLink与MAVROS概述

### 1. MAVLink通信协议
- **基本特性**：
  - 2009年首次发布的无人机通信协议
  - 支持固定翼/旋翼飞行器、地面载具等多种设备
  - 定义消息库形式的参数传输规则
  - 应用于飞控与机载处理器、地面站间的通信

### 2. MAVROS功能
- **核心作用**：
  - 实现ROS与MAVLink协议的桥梁
  - 将MAVLink消息转换为ROS Topic
  - 支持双向通信（读取飞控数据/发送控制指令）

### 3. 系统连接架构
```
飞控(Pixhawk) ↔ MAVLink ↔ MAVROS(ROS melodic) ↔ 机载处理器
```

## 二、MAVROS控制指令

### 1. 位置控制
- **主题**：`/mavros/setpoint_position/local`
- **坐标系**：NED坐标系
- **消息类型**：`geometry_msgs/PoseStamped`
  - **Position参数**：
    ```yaml
    x: 前后位置（前+ 后-）
    y: 左右位置（左+ 右-） 
    z: 高度位置（上+ 下-）
    ```
  - **Orientation参数**（四元数）：
    ```yaml
    x: 俯仰角
    y: 滚转角
    z+w: 偏航角组合
    ```

### 2. 速度控制
- **主题**：`/mavros/setpoint_velocity/cmd_vel_unstamped`
- **消息类型**：`geometry_msgs/Twist`
  - **Linear参数**：
    ```yaml
    x: 前后速度（前+ 后-）
    y: 左右速度（左+ 右-）
    z: 升降速度（上+ 下-）
    ```
  - **Angular参数**：
    ```yaml
    z: 旋转速度（左+ 右-）
    ```

### 3. 加速度控制
- **主题**：`/mavros/setpoint_accel/accel`
- **消息类型**：`geometry_msgs/Vector3Stamped`
  - **参数**：
    ```yaml
    x: 前后加速度（前+ 后-）
    y: 左右加速度（左+ 右-）
    z: 升降加速度（上+ 下-）
    ```

## 三、自定义通信协议

### 1. 协议结构体
```cpp
std_msgs/Header header
uint32 comid    // 指令序列ID
uint8 command  // 指令类型枚举
uint8 sub_mode // 控制模式标志位
float32[3] pos_sp   // 位置指令[x,y,z]
float32[3] vel_sp   // 速度指令[x,y,z] 
float32 yaw_sp      // 航向角指令
float32 yaw_rate_sp // 转向速率指令
```

### 2. Command枚举类型
```cpp
Move_ENU        // ENU坐标系运动
Move_Body       // 机体坐标系运动 
Hold            // 悬停
Land            // 降落
Disarm          // 电机上锁
Failsafe_land   // 紧急降落
Custom          // 自定义模式
```

### 3. sub_mode标志位
- **2-bit编码**：
  ```markdown
  0b00: xy位置 + z位置
  0b01: xy位置 + z速度
  0b10: xy速度 + z位置  
  0b11: xy速度 + z速度
  ```

## 四、控制架构

### 1. 系统组成
```
[逻辑任务程序] → [Pose控制器] 
       ↓
    (TX2平台)
       ↓
    [MAVROS] ↔ [PX4飞控] 
                   ↓
                [Mixer] → [电机电调]
```

### 2. 功能划分
- **橙色区域**（上层控制）：
  - 逻辑任务处理
  - 姿态位置计算
  - MAVROS通信接口

- **蓝色区域**（底层控制）：
  - PX4飞控核心
  - 混控器与电机驱动


# 自主定位
## 二、自主定位框架

### 1. 定位方式对比
- **激光SLAM**（Cartographer框架）：
  - 适用场景：视觉信息差/环境昏暗
  - 传感器融合：
    - 激光雷达扫描数据 + IMU积分 → 获取X/Y位置和偏航角
    - 对地红外信号 + IMU → 获取高度数据

- **视觉SLAM**（Intel T265）：
  - 适用场景：纹理信息丰富的环境
  - 传感器融合：
    - T265位置信息 + 对地红外信号 + IMU → 获取高度数据

### 2. 传感器数据融合
```
[激光雷达/视觉传感器] → 位置信息
[IMU] → 角速度/加速度
[红外传感器] → 高度数据
↓
组合导航数据替代GPS
```

### 3. 激光雷达参数
- **主题**：`/scan`
- **消息类型**：`sensor_msgs/LaserScan`
- **关键参数**：
  ```yaml
  angle_min: 0°       # 起始扫描角度
  angle_max: 360°     # 终止扫描角度
  angle_increment: 1° # 角度分辨率
  range_min: 0.15m    # 最小测量距离
  range_max: 12m      # 最大测量距离
  ranges: [360]       # 距离测量数组
  intensities: [360]  # 反射强度数组
  ```

## 二、传感器数据说明

### 1. IMU数据
- **主题**：`/mavros/imu/data_raw`
- **消息类型**：`sensor_msgs/Imu`
- **数据结构**：
  ```yaml
  orientation: 四元数姿态
  angular_velocity: 陀螺仪角速度(x,y,z)
  linear_acceleration: 加速度计数据(x,y,z)
  ```

### 2. 定位结果输出
- **主题**：`/mavros/vision_pose/pose`
- **消息类型**：`geometry_msgs/PoseStamped`
- **坐标系定义**：
  ```yaml
  position:
    x: 前后位置（前+ 后-）
    y: 左右位置（左+ 右-）
    z: 高度位置（上+ 下-）
  orientation: 四元数姿态
    x: 俯仰角
    y: 滚转角
    z+w: 偏航角组合
  ```

## 三、飞控数据交互

### 1. 组合导航数据流
```
[RPLIDAR/T265] → [Cartographer] → [Pose Estimator]
                      ↑
                    [IMU]
                      ↓
[MAVROS] ↔ [PX4飞控] → [电机控制]
```

### 2. 定位数据订阅
- **主题**：`/mavros/local_position/pose`
- **消息类型**：`geometry_msgs/PoseStamped`
- **编程应用**：
  - 实时获取无人机位姿
  - 自主导航的基准参考系

## 四、代码实现示例

### 1. 位置订阅器初始化
```cpp
ros::Subscriber position_sub = nh.subscribe<geometry_msgs::PoseStamped>(
    "/mavros/local_position/pose", 100, pos_cb);
```

### 2. 回调函数处理
```cpp
void pos_cb(const geometry_msgs::PoseStamped::ConstPtr& msg) {
    pos_drone = *msg;  // 存储当前位姿信息
}
```

### 3. 源码实现路径
- 核心定位算法：`pose_estimator.cpp`
- 数据融合逻辑：IMU/传感器数据融合实现
