# FAST-CALIB-MULTI-ROS2

> Multi-board extension of [ichangjian/FAST-Calib-ROS2](https://github.com/ichangjian/FAST-Calib-ROS2), which itself is a ROS2 port of [hku-mars/FAST-Calib](https://github.com/hku-mars/FAST-Calib).
> This fork adds **multi-scene / multi-board** calibration on top of the single-scene flow, tested on **Ubuntu 22.04 + ROS2 Humble** with **Livox MID-360** + **See3CAM_24CUG (AR0234)**.

## What's new in this fork
- `src/multi_scene.cpp` + `launch/multi_calib.launch.py`: aggregate multiple board poses into one extrinsic.
- `config/qr_params.yaml`: AR0234 1920x1080 intrinsics and `multi_fast_calib` parameter group.
- `.gitignore`: exclude colcon `build/`, `install/`, `log/` and rosbag `*.db3*`.

---

# FAST-Calib ROS2 版本 (upstream notes)

在 [engine1wu](https://github.com/hku-mars/FAST-Calib/issues/35) 的基础上将 ROS1 的 FAST-Calib 项目转换成了 ROS2。仅在 ubuntu 22.04 humble 上进行了测试。
## 运行说明
### 参数配置
在`calib_data/mid360_11`中提供了测试数据，是将[sample data](https://connecthkuhk-my.sharepoint.com/:f:/g/personal/zhengcr_connect_hku_hk/Eq_k_4Mf_11Eggg4a5lbRzgBHwd0EivtCJd2ExtcNlu1FA?e=vjm4gH)中的`mid360/11`点云裁剪后转成了ros2格式。只需要修改 `config/qr_params.yaml` 文件中的路径相关的参数就可以跑这组测试数据了，其中`bag_path`是*ros2 bag PointCloud2*的文件夹。
### 启动节点
```bash
ros2 launch fast_calib calib.launch.py
```


# FAST-Calib
FAST-Calib is an automatic target-based extrinsic calibration tool for LiDAR-camera systems (eg., [FAST-LIVO2](https://github.com/hku-mars/FAST-LIVO2)). 

**Key highlights include:** 

1. Support solid-state and mechanical LiDAR.
2. No need for any initial extrinsic parameters.
3. Achieve highly accurate calibration results **in just 2 seconds**.

**In short, it makes extrinsic calibration as simple as intrinsic calibration.**

📬 For further assistance or inquiries, please feel free to contact Chunran Zheng at zhengcr@connect.hku.hk.

<p align="center">
  <img src="./pics/calib.jpg" width="100%">
  <font color=#a0a0a0 size=2>Left: Example of circle extraction from Mid360 point cloud | Right: Point cloud colored with calibrated extrinsic.</font>
</p>

## 1. Prerequisites
PCL>=1.8, OpenCV>=4.0.

## 2. Run our examples
1. Prepare the static acquisition data in `calib_data` folder (see [sample data](https://connecthkuhk-my.sharepoint.com/:f:/g/personal/zhengcr_connect_hku_hk/Eq_k_4Mf_11Eggg4a5lbRzgBHwd0EivtCJd2ExtcNlu1FA?e=vjm4gH) from Mid360, Avia and Ouster):
- rosbag containing point cloud messages
- corresponding image

2. Run the calibration process:
```bash
roslaunch fast_calib calib.launch
```

## 3. Run on your own sensor suite
1. Customize the calibration target in the image below.
2. Record data to rosbag.
3. Provide the instrinsic matrix in `qr_params.yaml`.
4. Set distance filter in `qr_params.yaml` for board point cloud (extra points are acceptable).
5. Calibrate now!

<p align="center">
  <img src="./pics/calibration_target.jpg" width="100%">
  <font color=#a0a0a0 size=2>Left: Actual calibration target | Right: Technical drawing with annotated dimensions.</font>
</p>

## 4. Appendix
Related article is coming soon...

The calibration target design is based on the [velo2cam_calibration](https://github.com/beltransen/velo2cam_calibration).

For further details on the algorithm workflow, see [this document](https://github.com/xuankuzcr/FAST-Calib/blob/main/workflow.md).
## 5. Acknowledgments

Special thanks to [Jiaming Xu](https://github.com/Xujiaming1) for his support, [Haotian Li](https://github.com/luo-xue) for the equipment, and the [velo2cam_calibration](https://github.com/beltransen/velo2cam_calibration) algorithm.