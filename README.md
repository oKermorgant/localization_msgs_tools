# Packages to bridge some messages for robot localization

## pose_to_tf

This package bridges any pose message to `/tf` to publish e.g. ground truth from a simulation.

## with_covariance

This package adds or modifies the covariance part of classical navigation messages to make them compatible with state estimation.
