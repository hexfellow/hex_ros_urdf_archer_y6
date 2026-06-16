# hex_ros_urdf_archer_y6

A dual ROS1/ROS2 URDF package for the Archer Y6 6-DOF serial manipulator robot model, with optional GP100 gripper.

## Structure

```
hex_ros_urdf_archer_y6/
├── CMakeLists.txt            # Auto-detect ROS version, install assets
├── package.xml               # Dual ROS1/ROS2 dependencies
├── config/
│   ├── ros1/display.rviz     # RViz1 configuration
│   └── ros2/display.rviz     # RViz2 configuration
├── launch/
│   ├── ros1/
│   │   ├── display_empty.launch  # Visualize arm-only robot in RViz
│   │   └── display_gp100.launch  # Visualize arm+gripper robot in RViz
│   └── ros2/
│       ├── display_empty.launch.py  # Visualize arm-only robot in RViz2
│       └── display_gp100.launch.py  # Visualize arm+gripper robot in RViz2
├── meshes/                   # STL mesh assets
├── urdf/
│   ├── empty.urdf            # 6-DOF arm (no gripper)
│   ├── gp100_full.urdf       # 6-DOF arm + actuated GP100 gripper (revolute joints)
│   └── gp100_comp.urdf       # 6-DOF arm + simplified GP100 gripper (fixed joints)
└── README.md
```

## Usage

Three robot model variants are available:

### ROS1

```bash
# Arm only (6-DOF, no gripper)
roslaunch hex_ros_urdf_archer_y6 display_empty.launch

# Arm + GP100 gripper (full actuation, display_gp100.launch uses gp100_full.urdf by default)
roslaunch hex_ros_urdf_archer_y6 display_gp100.launch
```

### ROS2

```bash
# Arm only (6-DOF, no gripper)
ros2 launch hex_ros_urdf_archer_y6 display_empty.launch.py

# Arm + GP100 gripper (full actuation, display_gp100.launch.py uses gp100_full.urdf by default)
ros2 launch hex_ros_urdf_archer_y6 display_gp100.launch.py
```

## Robot Model

### empty.urdf — 6-DOF Arm

| Link | Type | Description |
|------|------|-------------|
| base_link | fixed | Base plate |
| link_1 ~ link_5 | actuated | Serial chain links driven by revolute joints |
| link_6 | actuated | End-effector mount flange (rotational) |

Joint axes alternate between Z and Y axes, providing 6 degrees of freedom suitable for reaching arbitrary poses within the workspace.

### gp100_full.urdf — Arm + Actuated GP100 Gripper

Extends the 6-DOF arm with a fully actuated GP100 2-DOF parallel gripper attached to link_5 (instead of link_6):

| Link | Type | Description |
|------|------|-------------|
| base_link | fixed | Base plate |
| link_1 ~ link_5 | actuated | Serial chain links driven by revolute joints |
| gp100_base_link | actuated | Gripper base (driven by joint_6, Z axis) |
| gp100_link_1 | actuated | Left gripper finger (driven by gp100_joint_1, X axis) |
| gp100_link_2 | actuated | Right gripper finger (mimics gp100_joint_1 with multiplier 1) |

Gripper joints: `gp100_joint_1` is a revolute joint (0 ~ 0.6 rad), `gp100_joint_2` is a mimic joint that mirrors `gp100_joint_1`.

### gp100_comp.urdf — Arm + Simplified GP100 Gripper

Same link structure as `gp100_full.urdf`, but the gripper finger joints (`gp100_joint_1`, `gp100_joint_2`) are of type `fixed`, making the gripper rigid. Useful for simplified simulations where gripper dynamics are not needed.
