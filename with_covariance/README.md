# Add covariance to ROS messages

This package is meant to bridge ROS messages by adding or replacing missing covariance info. The classical use is messages
coming out of Gazebo that usually have no covariance. Some frameworks or sensors also publish covariance-free messages, making them unsuitable for use with classical ROS tools.


# Main node

The `with_covariance` node takes in a number of messages, and republish them on another topic after adding covariance information. Supported messages are:

-  `geometry_msgs/PoseWithCovarianceStamped`
    - can also take in `geometry_msgs/Pose` or `geometry_msgs/PoseStamped` 
- `geometry_msgs/TwistWithCovarianceStamped`  
    - can also take in `geometry_msgs/Twist` or  `geometry_msgs/TwistStamped`
- `sensor_msgs/Imu`
- `sensor_msgs/NavSatFix`
- `nav_msgs/Odometry`

## Parameters

Similarly to the `robot_localization` package, the `with_covariance` node takes in possibly various messages, each of them having to be associated:

- an input type, defined by the name of the parameter (`pose0`, `imu0`, etc.)
- input and output topics
- covariance information to be added as length-3 vectors
    - `xyz` and `rpy` for pose
    - `linvel` and `angvel` for twist
    - `accel` for acceleration
    - covariances that are not set from the parameters will be copied from the incoming message, if any
- `frame_id` if they are not part of the incoming messages e.g. (`Pose` and `Twist`)

An example is provided for all supported messages.

## Changing covariances at runtime

All covariance parameters can be updated at runtime by setting the corresponding parameter:

```
ros2 param set /with_covariance pose0.cov.xyz [.1,.1,.1]
```


## Running the node

The node is available:

- as an executable:  `with_covariance`
- as a component: `with_covariance::WithCovariance`
