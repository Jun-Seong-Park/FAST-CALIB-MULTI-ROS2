# FAST-Calib (ROS 2)

Target-based LiDAR–Camera extrinsic calibration, ported to ROS 2.
Tested on Ubuntu 22.04 + ROS 2 Humble.

## Build

```bash
cd ~/your_ros2_ws
colcon build --packages-select fast_calib
source install/setup.bash
```

## Configure

Edit `config/qr_params.yaml` and set at least:

- `image_path`   — path to the calibration image (PNG/JPG)
- `bag_path`     — folder of a `ros2 bag` recording containing `PointCloud2`
- `lidar_topic`  — point-cloud topic name inside the bag (e.g. `/livox/lidar`)
- `output_path`  — folder where results are written
- camera intrinsics (`fx, fy, cx, cy, k1, k2, p1, p2`)
- target geometry (`marker_size`, `delta_width_*`, `delta_height_*`, `circle_radius`)
- ROI box (`x_min … z_max`) that contains the calibration board

Sample data is provided under `calib_data/mid360_11`.

## Run

### Single-scene calibration

```bash
ros2 launch fast_calib calib.launch.py
```

Writes `calib_result.txt`, `colored_cloud.pcd/.ply`, and `qr_detect.png` into `output_path`.
Each run also appends the four detected circle-center pairs to `circle_center_record.txt`.

### Multi-scene calibration

The multi-scene node jointly solves the extrinsic from several single-scene results.

1. Run `calib.launch.py` at least `min_scenes` times (default `3`), each time with a different
   scene's `image_path` / `bag_path`. Every run appends one block of four LiDAR / QR center pairs
   to `<output_path>/circle_center_record.txt`.
2. Then solve jointly over the last `min_scenes` blocks:

```bash
ros2 launch fast_calib multi_calib.launch.py
```

Writes `<output_path>/multi_calib_result.txt` (R, t, RMSE, scenes used).
Both nodes read the same `config/qr_params.yaml`; `min_scenes` lives there too.
