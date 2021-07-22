# Positional Tracking

https://cdn.stereolabs.com/docs/positional-tracking/images/zed-positional-tracking.mp4

Positional tracking is the ability of a device to estimate its position relative to the world around it. Also called motion tracking or odometry, this is used to track the movement of a camera or user in 3D space with six degrees of freedom (6DoF).

### Overview

* [Enabling Positional Tracking](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#enabling-positional-tracking)
* [Getting Pose](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#getting-pose)
* [Tracking States](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#getting-pose)
* [Area Memory](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#getting-pose)
* [Positional Tracking Configuration](#getting-pose)
* [Code Examples](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#code-example)

### Integrations

Check [Integrations](https://github.com/qt-truong/zed-examples/tree/master/11-Integrations) to use positional tracking with your favorite suite of tools and libraries.



### How It Works

The ZED uses visual tracking of its surroundings to understand the movement of its user or system. As the camera moves in the real-world, the ZED SDK reports its position and orientation for each new frame. This information is called the camera 6DoF pose. Pose information is output at the frame rate of the camera, up to 60fps in HD720 and 100fps in WVGA mode.

The pose is relative to a reference [coordinate frame](/positional-tracking/coordinate-frames/), usually the World Frame which is fixed in space.

To learn how to get Position and Orientation, see [using the API](https://www.stereolabs.com/docs/positional-tracking/using-tracking/) docs.


*Note:* Positional tracking uses image and depth information to estimate the position of the camera in 3D space. To improve tracking results, use high FPS video modes such as HD720 and WVGA.


### Enabling Positional Tracking
After opening the camera, enable positional tracking using `enablePositionalTracking()` with default `PositionalTrackingParameters`.

For more information on positional tracking settings, see [PositionalTrackingParameters](https://www.stereolabs.com/docs/api/structsl_1_1PositionalTrackingParameters.html).


### Getting Pose
Position gets updated with every new frame. To retrieve pose data, use `getPosition()` after grabbing a frame.

The following pose data is returned:

-	**Position**: the location of the camera in space is available as a vector [X, Y, Z]. Its norm represents the total distance traveled between the current camera position and the reference coordinate frame.

-	**Orientation**: the orientation of the camera in space is available as a quaternion [X, Y, Z, W]. A quaternion can be directly manipulated to reflect yaw, pitch and roll in different coordinate frames.

The `POSE` class also contains timestamp, confidence value and a rotation matrix that describes the rotation of the camera with respect to the World Frame.

### Tracking States
`getPosition()` returns a `TRACKING_STATE`. When a new position is available, `TRACKING_STATE::OK` is returned. During initialization or when an Area file is loaded, `TRACKING_STATE::SEARCHING` is returned till the camera recognizes the area. If positional tracking framerate is too low, the state will turn to `TRACKING_STATE::FPS_TOO_LOW` and stop returning new positions.

### Positional Tracking Configuration
To configure positional tracking, use `InitParameters` at initialization and `RuntimeParameters` to change specific parameters during use.

### Code Examples
For code examples, check out the [Tutorial](https://github.com/qt-truong/zed-examples/tree/master/09-Tutorials/tutorial%204%20-%20positional%20tracking) and [Sample](https://github.com/qt-truong/zed-examples/tree/master/10-Samples/positional%20tracking) on GitHub.

### Read More
For more information, read the [Documentation](https://www.stereolabs.com/docs/positional-tracking).
