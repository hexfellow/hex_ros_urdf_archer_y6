# hex_ros_urdf_template

A dual ROS1/ROS2 URDF template package with a 7-DOF serial manipulator robot model.

## Structure

```
hex_ros_urdf_template/
├── CMakeLists.txt            # Auto-detect ROS version, install assets
├── package.xml               # Dual ROS1/ROS2 dependencies
├── config/
│   ├── ros1/display.rviz     # RViz1 configuration
│   └── ros2/display.rviz     # RViz2 configuration
├── launch/
│   ├── ros1/
│   │   └── display.launch    # Visualize robot in RViz
│   └── ros2/
│       └── display.launch.py # Visualize robot in RViz2
├── meshes/                   # Placeholder for mesh assets
├── urdf/
│   └── model.urdf            # 7-DOF arm URDF description
└── README.md
```

## Usage

### ROS1

```bash
# Display in RViz
roslaunch hex_ros_urdf_template display.launch
```

### ROS2

```bash
# Display in RViz2
ros2 launch hex_ros_urdf_template display.launch.py
```

## Robot Model

The package models a 7-DOF serial manipulator arm with 8 links:

| Link | Type | Description |
|------|------|-------------|
| base_link | fixed | Base plate |
| link_1 ~ link_7 | actuated | Serial chain links driven by revolute joints |

Joint axes alternate between Z and Y axes, providing 7 degrees of freedom suitable for reaching arbitrary poses within the workspace.
