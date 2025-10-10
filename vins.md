代码的文件目录
1、ar_demo：一个ar应用demo
2、benchmark_publisher：接收并发布数据集的基准值
3、camera_model
   calib：相机参数标定
   camera_models：各种相机模型类
   chessboard：检测棋盘格
   gpl
   sparse_graph
   intrinsic_calib.cc：相机标定模块main函数
4、config：系统配置文件存放处
5、feature_trackers：
   feature_tracker_node.cpp ROS 节点函数，回调函数
   feature_tracker.cpp 图像特征光流跟踪 6、pose_graph：
   keyframe.cpp 关键帧选取、描述子计算与匹配
   pose_graph.cpp 位姿图的建立与图优化
   pose_graph_node.cpp ROS 节点函数，回调函数，主线程
7、support_files：帮助文档、Bow字典、Brief模板文件
8、vins_estimator
   factor：实现IMU、camera等残差模型
   initial：系统初始化，外参标定，SFM
   utility：相机可视化，四元数等数据转换
   estimator.cpp：紧耦合的VIO状态估计器实现
   estimator_node.cpp：ROS 节点函数，回调函数， 主线程
   feature_manager.cpp：特征点管理，三角化，关键帧等
   parameters.cpp：读取参数
