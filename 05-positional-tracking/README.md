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

#### API

* [C++](https://github.com/snowplow/snowplow-javascript-tracker)
* [Python](https://github.com/snowplow/snowplow-javascript-tracker)
* [C#](https://docs.snowplowanalytics.com/docs/collecting-data/collecting-from-own-applications/google-amp-tracker/)
* [C](https://docs.snowplowanalytics.com/docs/collecting-data/collecting-from-own-applications/google-amp-tracker/)

#### Interfaces

* [Unity](https://github.com/snowplow/snowplow-javascript-tracker)
* [Unreal](https://docs.snowplowanalytics.com/docs/collecting-data/collecting-from-own-applications/google-amp-tracker/)



### How It Works

The ZED uses visual tracking of its surroundings to understand the movement of its user or system. As the camera moves in the real-world, it reports its new position and orientation. This information is called the camera 6DoF pose. Pose information is output at the frame rate of the camera, up to 60fps in HD720 and 100fps in WVGA mode.

To learn how to get Position and Orientation, see [using the API](https://www.stereolabs.com/docs/positional-tracking/using-tracking/) docs.


Positional tracking uses image and depth information to estimate the position of the camera in 3D space. To improve tracking results, use high FPS video modes such as HD720 and WVGA.


### Enabling Positional Tracking
After opening the camera, enable positional tracking using `enablePositionalTracking()` with default `PositionalTrackingParameters`.

You can disable tracking anytime using `disablePositionalTracking()`. For more information on positional tracking settings, see [PositionalTrackingParameters](https://www.stereolabs.com/docs/api/structsl_1_1PositionalTrackingParameters.html).


### Getting Pose
Position gets updated with every new frame. To retrieve pose data, use `getPosition()` after grabbing a  frame. In the following example, we extract the pose relative to the [World Frame](/positional-tracking/coordinate-frames/#world-frame) and retrieve the translation and orientation quaternion values using `getTranslation()` and `getOrientation()`.

The class `Pose` is used to store camera position and additional information such as timestamp and confidence. Since position is always relative to a reference, it is important to set the [coordinate frame](https://www.stereolabs.com/docs/positional-tracking/coordinate-frames/) that will be used as base. To get camera position in real world space, use `REFERENCE_FRAME::WORLD`, otherwise use `REFERENCE_FRAME::CAMERA` to get the change in pose relative to the last position (odometry).

By default, the pose of the left eye of the camera is returned. To get the pose at the center of the camera, see [Frame Transforms](https://www.stereolabs.com/docs/positional-tracking/coordinate-frames/#frame-transforms). You can also retrieve a translation, orientation or rotation matrix using `getTranslation()`, `getOrientation()` and `getRotation()`.

### Tracking States
`getPosition()` returns a `TRACKING_STATE`. When a new position is available, `TRACKING_STATE::OK` is returned. During initialization or when an Area file is loaded, `TRACKING_STATE::SEARCHING` is returned till the camera recognizes the area. If positional tracking framerate is too low, the state will turn to `TRACKING_STATE::FPS_TOO_LOW` and stop returning new positions.

### Positional Tracking Configuration
To configure positional tracking, use `InitParameters` at initialization and `RuntimeParameters` to change specific parameters during use.

### Code Examples
For code examples, check out the [Tutorial](https://github.com/qt-truong/zed-examples/tree/master/09-Tutorials/tutorial%204%20-%20positional%20tracking) and [Sample](https://github.com/qt-truong/zed-examples/tree/master/10-Samples/positional%20tracking) on GitHub.
