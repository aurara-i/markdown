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

**编译**
由于我的eigein库是3.4的，所以重新安装了地版本的矩阵库，然后编译ceres库1.1.14库，然后才能够成功运行。
<img width="696" height="264" alt="图片" src="https://github.com/user-attachments/assets/6ecc08c2-cbdf-4c27-9c55-5076b9216933" />
按照readme进行安装即可
需要：
1、播放bag包  rosbag play YOUR_PATH_TO_DATASET/MH_01_easy.bag 
2、播放   roslaunch vins_estimator euroc.launch （yan@yan-Inspiron-15-5510:~/Vins$ roslaunch vins_estimator euroc.launch）
这一步会启动 VINS-Mono 的主要算法，包括：
图像特征跟踪、IMU 融合、位姿估计、回环检测（loop closure）
3、播放   roslaunch vins_estimator vins_rviz.launch （运行 rviz -d /home/yan/Vins/src/VINS-Mono-master/config/vins_rviz_config.rviz）这一步只是打开可视化界面，用于查看 VINS 的运行结果，比如轨迹、特征点、回环、坐标系等。
<img width="1392" height="876" alt="图片" src="https://github.com/user-attachments/assets/b2601aa3-d881-46d5-a577-e751172ae2dc" />
<img width="1375" height="774" alt="图片" src="https://github.com/user-attachments/assets/5c62fac7-5fc8-47b3-8ce4-f6af1ddb80b1" />
⚠️ 注意：RViz 只是用来“看结果”，VINS 算法本身是在 euroc.launch 里运行的。
左上角和中间上方是 tracked_image、loop_match_image，说明特征点跟踪和回环检测在正常工作；
右边的 3D 视图里有绿色轨迹线和红色轨迹线，分别对应：
🟩 绿色：VINS 估计的轨迹
🟥 红色：回环检测到的闭环位置（pose graph）
轨迹有回环闭合的形状，代表系统正在进行位姿图优化（pose graph optimization）。
（组会确实能让人由动力干活）
<br>**源码阅读**
    <br>在正常运行过程中，初始化只进行一次。前端负责不断地提取特征点发给后端；后端负责IMU数据采集，预积分和优化/滑窗等操作。前端和后端在运行过程中不断地循环。
    <br>这个系统包括以下几个过程：特征提取与发布；IMU提取与预积分；初始化；滑窗与优化；回环检测。
    <br><img width="718" height="370" alt="图片" src="https://github.com/user-attachments/assets/d9b92fbb-4395-44ca-b1b7-1d60cc9c228b" /><br>
    <br>**问题1：VO与SFM区分**<br>
    <br> VO是相邻特征匹配，属于连续视频帧，获得相邻帧的位姿估计，BA优化进行局部BA。SFM是一组照片（离线），任意两帧，进行全局优化。VO只关心局部帧间相对运动；做的是局部BA或滑动窗口优化；目的是实时定位；SFM处理全局所有帧；使用全局BA 优化所有相机位姿和3D点；目的是高精度重建；<br>
    <img width="694" height="338" alt="图片" src="https://github.com/user-attachments/assets/75b5ff39-434c-41f8-97d7-c4b01cbb4a1f" />
    <img width="884" height="444" alt="图片" src="https://github.com/user-attachments/assets/df616e78-1a4d-4288-a462-0c59c538904b" />
    <br>代码主要包括3个node：feature_tracker，vins_estimator，pose_graph，其中feature_tracker仅负责特征点提取和发布，pose_graph关键帧的选择/位姿图建立/回环检测，而vins_estimator的内容最多了，包含了初始化，滑窗，优化等后端内容和IMU预积分等前端内容，并且在这个结点里分出了2个线程。
    <br>VINS系统，他的前端仅包括光流计算；初始化等工作都放在vins_estimator里面了。
我的理解它之所以这么做，是因为IMU的数据获取如果放在前端，预积分完了之后还需要advertise到ROS系统里，让后端接收，与其这样，还不如让后端直接接收IMU数据就在后端处理了，减少通信过程。
    <br>**1、前端 feature_tracker**
    <br>这个package下面主要包括4个功能：
- feature_tracker——包含特征提取/光流追踪的所有算法函数；
- feature_tracker_node——特征提取的主入口，负责一个特征处理节点的功能；
- parameters——负责读取来自配置文件的参数；
- tic_toc——计时器；
   <br> 🕐feature_tracker.h










