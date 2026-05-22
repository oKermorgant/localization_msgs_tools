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

An example is provided for all supported messages:

```
/**:
    ros__parameters:
        # subscribe to some Pose, publish as PoseWithCovarianceStamped
        pose0: pose_gt
        pose0.frame_id: base_link
        pose0.out: pose_with_cov
        pose0.cov:
            # linear covariance
            xyz: [.1, .1, .1]
            # angular covariance
            rpy: [.1, .1, .1]

        # IMU
        imu0: imu_raw
        imu0.out: imu_with_cov
        imu0.cov:
            # angular covariance
            rpy: [.1, .1, .1]
            # angular velocity covariance
            angvel: [.1, .1, .1]
            # acceleration covariance
            accel: [.1, .1, .1]

        # Odometry
        odom0: odom_raw
        odom0.out: odom_with_cov
        odom0.cov:
            # xyz: [] # do not change incoming covariance for pose
            # rpy: []
            linvel: [.1, .1, .1]
            angvel: [.1, .1, .1]

        # Some kind of Twist
        twist0: twist_raw
        twist0.out: twist_with_cov
        twist0.frame_id: base_link
        twist0.cov:
            linvel: [.1, .1, .1]
            angvel: [.1, .1, .1]

        # NavSat
        navsat0: navsat_raw
        navsat0.out: navsat_with_cov
        navsat0.cov.xyz: [5.,5.,1.]
```

## Changing covariances at runtime

All covariance parameters can be updated at runtime by setting the corresponding parameter:

```
ros2 param set /with_covariance pose0.cov.xyz [.1,.1,.1]
```


## Running the node

The node is available:

- as an executable:  `with_covariance`
- as a component: `with_covariance::WithCovariance`
