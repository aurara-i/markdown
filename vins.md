代码的文件目录
<br>1、ar_demo：一个ar应用demo
<br>2、benchmark_publisher：接收并发布数据集的基准值
<br>3、camera_model
  <br> calib：相机参数标定
  <br> camera_models：各种相机模型类
 <br>  chessboard：检测棋盘格
  <br> gpl
  <br> sparse_graph
   <br>intrinsic_calib.cc：相机标定模块main函数
<br>4、config：系统配置文件存放处
<br>5、feature_trackers：
   <br>feature_tracker_node.cpp ROS 节点函数，回调函数
   <br>feature_tracker.cpp 图像特征光流跟踪 6、pose_graph：
   <br>keyframe.cpp 关键帧选取、描述子计算与匹配
   <br>pose_graph.cpp 位姿图的建立与图优化
   <br>pose_graph_node.cpp ROS 节点函数，回调函数，主线程
<br>7、support_files：帮助文档、Bow字典、Brief模板文件
<br>8、vins_estimator
   <br>factor：实现IMU、camera等残差模型
   <br>initial：系统初始化，外参标定，SFM
   <br>utility：相机可视化，四元数等数据转换
   <br>estimator.cpp：紧耦合的VIO状态估计器实现
   <br>estimator_node.cpp：ROS 节点函数，回调函数， 主线程
   <br>feature_manager.cpp：特征点管理，三角化，关键帧等
   <br>parameters.cpp：读取参数

VINS-Mono 是一个实时的单目视觉惯性 SLAM 框架，采用基于优化的滑动窗口方法，能实现高精度视觉惯性里程计。它具备以下特性：
高效 IMU 预积分 + 零偏校正、自动初始化、在线外参标定、失败检测与恢复、回环检测与全局位姿图优化、地图合并与复用、在线时间标定、支持滚动快门相机
主要用于无人机的状态估计与反馈控制，也可用于增强现实（AR）定位。代码运行于 Linux，并与 ROS 完全集成。



