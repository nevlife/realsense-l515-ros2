# realsense-l515-ros2

Intel RealSense **L515** (L500 series) on **ROS 2 Jazzy**.

> **Demo** unofficial bundle demonstrating how to run the L515 (dropped from newer librealsense) on ROS 2 Jazzy.

The L515 was dropped from librealsense in **2.55.1** (last supporting release: **2.54.2**), and the L515-aware ROS 2 wrapper is **realsense-ros 4.54.1**, which officially targets Humble/Iron.

## What's bundled

| Component | Version | Notes |
|-----------|---------|-------|
| `librealsense/` | 2.54.2 | last librealsense with L500/L515 support |
| `realsense-ros/` | 4.54.1 | last L515-aware wrapper; requires SDK ≥ 2.54.1 |

Upstream: <https://github.com/IntelRealSense/librealsense> and <https://github.com/IntelRealSense/realsense-ros>.

Slimmed for L515 only


**Dedicated workspace only**

- Use a dedicated workspace (e.g. `~/l515_ws`).
- Other RealSense cameras: build in a separate workspace, launch from its own sourced shell.
- Give each camera a distinct `camera_name` to avoid topic collisions.

## Build

```bash
# place this repo under a colcon workspace's src/, e.g. ~/l515_ws/src/realsense-l515-ros2
cd ~/l515_ws
source /opt/ros/jazzy/setup.bash
colcon build --merge-install \
  --cmake-args -DCMAKE_BUILD_TYPE=Release \
               -DBUILD_EXAMPLES=false -DBUILD_GRAPHICAL_EXAMPLES=false \
               -DIMPORT_DEPTH_CAM_FW=false -DCHECK_FOR_UPDATES=false
```

- `IMPORT_DEPTH_CAM_FW=false` skips a firmware download that otherwise hangs CMake configure.
- librealsense (`build_type: cmake`) and realsense-ros are both discovered by colcon's recursion.

## Run

```bash
source /opt/ros/jazzy/setup.bash
source ~/l515_ws/install/setup.bash

rs-enumerate-devices        # confirm the L515 is seen
ros2 launch realsense2_camera rs_launch.py serial_no:=<L515_SERIAL> camera_name:=l515
```

## Notes

- Recommended L515 firmware: **1.5.8.1**

## License & Attribution

This is a redistribution of Intel® software, both under the **Apache License 2.0**:
- **librealsense** © Intel Corporation — see `librealsense/LICENSE` and `librealsense/NOTICE`
- **realsense-ros** © Intel Corporation — see `realsense-ros/LICENSE` and `realsense-ros/NOTICE`.

"Intel" and "RealSense" are trademarks of Intel Corporation, used here descriptively only