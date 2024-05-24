# 如何创建gazebo自定义模型

## 1.在solidworks中建立模型物体

## 2.导出stl文件

## 3.使用solidworks计算惯性矩，质心和重量

**通过质量属性计算质心**：

+ 打开您想要计算质心的零件或装配体。

+ 在顶部菜单栏中，选择“评估”选项卡（在某些较早版本中可能位于“工具”菜单下）。

+  在“评估”选项卡中点击“质量属性”按钮。

+ 在弹出的“质量属性”对话框中，SolidWorks会自动计算并显示零部件的总质量和质心坐标（X、Y、Z方向的坐标值）。

## 4.参照如下格式构建gazebo自建模型包

+ gazebo添加一个模型的文件目录格式为：

  ```
  model_name	改为你模型的名字
  	model.sdf	              .sdf文件
  	model.config	          .config文件
  	meshes                    文件夹：存放模型文件
  	materials	              文件夹
  	| --scripts	              文件夹
  	       land_mark.material 纹理信息
  	| --textures	          文件夹
  	       h.png	          图像
  ```

+ model.config:

  ```xml
  <?xml version="1.0"?>
  <model>
    <name>t_pipe</name>
    <version>1.0</version>
    <sdf version="1.4">t_pipe.sdf</sdf>
    
    <author>
      <name>xxxxx</name>
      <email>xxxxxx@qq.com</email>
    </author>
   
    <description>
      A model of a pipe.
    </description>
  </model>
  ```

  参数说明：

  ```xml
  <name>名字
  <version>版本,默认1.0即可
  <sdf>对应SDF文件的名字，版本默认1.4即可
  <author>作者信息
  	<name>姓名
  	<email>邮件
  <description>模型介绍
  ```

+ model.sdf

  ```xml
  <?xml version="1.0" ?>
  <sdf version="1.4">
    <model name="t_pipe">
      <static>false</static>
      <link name="link">
      <inertial>
        <pose>0.0465 0.04 0.020 0 0 0</pose>
        <mass>0.049263</mass>
  	  <inertia>
  	    <ixx>0.000032494</ixx>
  	    <ixy>0.0</ixy>
  	    <ixz>0.0</ixz>
  	    <iyy>0.000044475</iyy>
  	    <iyz>0.0</iyz>
  	    <izz>0.00005166</izz>
  	  </inertia>
      </inertial>
        <collision name="collision">
           <geometry>
            <mesh>
              <scale>0.001 0.001 0.001</scale>
              <uri>/catkin_ws/src/models/t_pipe/meshes/pipe.STL</uri>  
            </mesh>
          </geometry>  
        </collision>
        <visual name="visual">    
          <geometry>
            <mesh>
              <scale>0.001 0.001 0.001</scale>
              <uri>/catkin_ws/src/models/t_pipe/meshes/pipe.STL</uri>  
            </mesh>
          </geometry>        
        </visual>
      </link>
    </model>
  </sdf>
  ```

  参数说明：

  ```xml
  <?xml version ?>  xml的版本
  <sdf version>  sdf的版本，和config里<sdf>的版本要一样呀
  <model name> 模型的名字
  <static> 选择模型是否固定
  <link> 包含模型的一个主体的物理属性，尽量减少模型中链接数量以提高性能和稳定
  	<inertial>: 惯性元素，描述了link的动态特性，例如质量和转动惯量矩阵
  		<pose> 质心
  		<mass> 质量
  		<inertia> 转动惯量
  			<ixx> <ixy> <ixz> <iyy> <iyz> <izz>
  	<collision>: 用于碰撞检查，一个link可以有多个碰撞元素
  		<geometry> 物体
  			<box> | <sphere> | <cylinder>形状名字
  				<size> x y z长度 | <radius>半径 | <radius> & <length>
  			<mesh> 使用模型来表示碰撞实体
  		<surface> 平面
  			<friction>设置地面摩擦力
  				<ode> <mu> <slip>
  	<visual>: 可视化
  		<geometry> 几何形状
  			<box> | <sphere> | <cylinder>形状名字
  				<size> x y z长度 | <radius>半径 | <radius> & <length>
  			<mesh> 使用模型来表示可视化
  		<material>	纹理信息
  			<script>
  				<uri>model://model_name/materials/scripts</uri>注意把model_name改成对应的文件夹名称
              	<uri>model://model_name/materials/textures</uri>注意把model_name改成对应的文件夹名称
              	<name>Mark/Diffuse</name>与之后的.material文件对应
  
  	<sensor>: 从world收集数据用于plugin
  	<light>: 光源
  <joint>关节 关节连接两个link，用于旋转轴和关节限制等
  <plugin>插件  用于控制模型
  
  ```

  
