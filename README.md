# URDF Telemetry Dashboard

A single-file, no-install viewer for URDF robots with live ROS 2 telemetry,
joint control, and teleop — just open `urdf_dashboard.html` in Chrome.

<img width="700" height="500" alt="Screenshot from 2026-11-05 15-06-23" src="https://github.com/user-attachments/assets/4fe2608c-5a4c-426b-a162-34ce3c5ed1d1" />

## Requirements

- Chrome (or any modern browser)
- Internet on first open (loads three.js from a CDN)
- No installation, no server, no build step

## Quick start

1. Open `urdf_dashboard.html`.
2. Click **Open URDF** and select your `.urdf` file
   (convert xacro first: `xacro robot.urdf.xacro > robot.urdf`).
3. Click **Add mesh folder** and select the folder containing your meshes
   (e.g. `..._description/meshes`). Status should read "N of N loaded."
4. Use **Joints → Animate** to sanity-check the model with no robot connected.

No URDF handy? Click **Load demo robot**.

## Connecting to a real robot

1. On a machine that can see the robot's ROS 2 topics, run:
   ```bash
   ros2 launch rosbridge_server rosbridge_websocket_launch.xml
   ```
2. In the dashboard, enter `ws://<that-machine-ip>:9090` and click **Connect**.
3. Click **Load from robot_description** to pull the live URDF, or use your
   own local copy + mesh folder.
4. Set **Joints → Live**, pointing at your `/joint_states` topic (or
   whatever equivalent your robot actually publishes — some platforms need
   a small bridge node to convert their native state topic into standard
   `sensor_msgs/JointState`).
5. Optional: **Follow odometry** if `/odom` is published.
6. **Drive (cmd_vel)** panel sends teleop commands while a button/key is held.

## Topics / Nodes / Services panel

Browse everything live on the ROS graph, click any topic to inspect its
messages, and click any number inside a message to plot it over time.

## Sharing this file

It's fully self-contained — send the `.html` file directly (email, Slack,
drive). The recipient needs their own URDF + mesh folder; this dashboard
doesn't bundle or transmit robot files anywhere.

## Known limitations

- `package://` mesh paths are resolved by matching filenames against
  whatever folder you add — not by actual ROS package resolution.
- Very large point clouds / images are throttled, not disabled.
- Gripper/finger animation requires separate jaw meshes in the URDF; a
  single solid gripper mesh can't be split into moving parts automatically.
