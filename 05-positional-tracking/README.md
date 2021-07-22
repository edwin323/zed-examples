# Positional Tracking

<video width="100%" height="auto" playsinline="" autoplay="" loop="" muted="" preload="auto" style="max-width:700px;margin-top:20px">
    <source src="https://cdn.stereolabs.com/docs/positional-tracking/images/zed-positional-tracking.mp4" type="video/mp4">
    <source src="https://cdn.stereolabs.com/docs/positional-tracking/images/zed-positional-tracking.webm" type="video/webm">
</video>

## Overview

Positional tracking is the ability of a device to estimate its position relative to the world around it. Also called motion tracking or odometry, this is used to track the movement of a camera or user in 3D space with six degrees of freedom (6DoF).

The ZED uses visual tracking of its surroundings to understand the movement of its user or system. As the camera moves in the real-world, it reports its new position and orientation. This information is called the camera 6DoF pose. Pose information is output at the frame rate of the camera, up to 60fps in HD720 and 100fps in WVGA mode.




### Getting Position and Orientation

The ZED SDK returns pose information for each frame. The pose is given for the left eye of the camera. It is relative to a reference [coordinate frame](https://www.stereolabs.com/docs/positional-tracking/coordinate-frames/), usually the World Frame which is fixed in space.

To learn how to get Position and Orientation, see [using the API](https://www.stereolabs.com/docs/positional-tracking/using-tracking/) section.


## Overview

* [Positional Tracking Configuration](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#positional-tracking-configuration)
* [Enabling Positional Tracking](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#enabling-positional-tracking)
* [Getting Pose](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#getting-pose)
* [Code Example](https://github.com/qt-truong/zed-examples/tree/master/05-PositionalTracking#code-example)

## Code Example
For code examples, check out the [Tutorial](https://github.com/qt-truong/zed-examples/tree/master/09-Tutorials/tutorial%204%20-%20positional%20tracking) and [Sample](https://github.com/qt-truong/zed-examples/tree/master/10-Samples/positional%20tracking) on GitHub.

### Integrations

#### API

* [Javascript](https://github.com/snowplow/snowplow-javascript-tracker)
* [AMP](https://docs.snowplowanalytics.com/docs/collecting-data/collecting-from-own-applications/google-amp-tracker/)

#### API

* [Javascript](https://github.com/snowplow/snowplow-javascript-tracker)
* [AMP](https://docs.snowplowanalytics.com/docs/collecting-data/collecting-from-own-applications/google-amp-tracker/)




## Positional Tracking Configuration
To configure positional tracking, use `InitParameters` at initialization and `RuntimeParameters` to change specific parameters during use.

**C++**
```cpp
// Set configuration parameters
InitParameters init_params;
init_params.camera_resolution = RESOLUTION::HD720; // Use HD720 video mode (default fps: 60)
init_params.coordinate_system = COORDINATE_SYSTEM::RIGHT_HANDED_Y_UP; // Use a right-handed Y-up coordinate system
init_params.coordinate_units = UNIT::METER; // Set units in meters
```

**Python**
```python
# Set configuration parameters
init_params = sl.InitParameters()
init_params.camera_resolution = sl.RESOLUTION.HD720 # Use HD720 video mode (default fps: 60)
init_params.coordinate_system = sl.COORDINATE_SYSTEM.RIGHT_HANDED_Y_UP # Use a right-handed Y-up coordinate system
init_params.coordinate_units = sl.UNIT.METER # Set units in meters
```

**C#**
```csharp
// Set configuration parameters
InitParameters init_params = new InitParameters();
init_params.resolution = RESOLUTION.HD720; // Use HD720 video mode (default fps: 60)
init_params.coordinateSystem = COORDINATE_SYSTEM.RIGHT_HANDED_Y_UP; // Use a right-handed Y-up coordinate system
init_params.coordinateUnits = UNIT.METER; // Set units in meters
```

Positional tracking uses image and depth information to estimate the position of the camera in 3D space. To improve tracking results, use high FPS video modes such as HD720 and WVGA.


## Enabling Positional Tracking
After opening the camera, enable positional tracking using `enablePositionalTracking()` with default `PositionalTrackingParameters`.

**C++**
```cpp
// Enable positional tracking with default parameters
sl::PositionalTrackingParameters tracking_parameters;
err = zed.enablePositionalTracking(tracking_parameters);
```

**Python**
```python
# Enable positional tracking with default parameters
tracking_parameters = sl.PositionalTrackingParameters()
err = zed.enable_positional_tracking(tracking_parameters)
```

**C#**
```csharp
// Enable positional tracking with default parameters
PositionalTrackingParameters trackingParams = new PositionalTrackingParameters();
zed.EnablePositionalTracking(ref trackingParams);
```

You can disable tracking anytime using `disablePositionalTracking()`. For more information on positional tracking settings, see [PositionalTrackingParameters](https://www.stereolabs.com/docs/api/structsl_1_1PositionalTrackingParameters.html).


## Getting Pose
Position gets updated with every new frame. To retrieve pose data, use `getPosition()` after grabbing a  frame. In the following example, we extract the pose relative to the [World Frame](/positional-tracking/coordinate-frames/#world-frame) and retrieve the translation and orientation quaternion values using `getTranslation()` and `getOrientation()`.

**C++**
```cpp
sl::Pose zed_pose;
if (zed.grab() == SUCCESS) {
        // Get the pose of the camera relative to the world frame
        POSITIONAL_TRACKING_STATE state = zed.getPosition(zed_pose, REFERENCE_FRAME::WORLD);
        // Display translation and timestamp
        printf("Translation: tx: %.3f, ty:  %.3f, tz:  %.3f, timestamp: %llu\r",
        zed_pose.getTranslation().tx, zed_pose.getTranslation().ty, zed_pose.getTranslation().tz, zed_pose.timestamp);
        // Display orientation quaternion
        printf("Orientation: ox: %.3f, oy:  %.3f, oz:  %.3f, ow: %.3f\r",
        zed_pose.getOrientation().ox, zed_pose.getOrientation().oy, zed_pose.getOrientation().oz, zed_pose.getOrientation().ow);
    }
}
```

**Python**
```python
zed_pose = sl.Pose()
if zed.grab(runtime_parameters) == SUCCESS :
        # Get the pose of the camera relative to the world frame
        state = zed.get_position(zed_pose, sl.REFERENCE_FRAME.FRAME_WORLD)
        # Display translation and timestamp
        py_translation = sl.Translation()
        tx = round(zed_pose.get_translation(py_translation).get()[0], 3)
        ty = round(zed_pose.get_translation(py_translation).get()[1], 3)
        tz = round(zed_pose.get_translation(py_translation).get()[2], 3)
        print("Translation: tx: {0}, ty:  {1}, tz:  {2}, timestamp: {3}\n".format(tx, ty, tz, zed_pose.timestamp))
        #Display orientation quaternion
        py_orientation = sl.Orientation()
        ox = round(zed_pose.get_orientation(py_orientation).get()[0], 3)
        oy = round(zed_pose.get_orientation(py_orientation).get()[1], 3)
        oz = round(zed_pose.get_orientation(py_orientation).get()[2], 3)
        ow = round(zed_pose.get_orientation(py_orientation).get()[3], 3)
        print("Orientation: ox: {0}, oy:  {1}, oz: {2}, ow: {3}\n".format(ox, oy, oz, ow))
```

**C#**
```csharp
Pose zed_pose = new Pose();
RuntimeParameters runtimeParameters = new RuntimesParameters();
if (zed.Grab(ref runtimeParameters) == ERROR_CODE.SUCCESS) {
        // Get the pose of the camera relative to the world frame
        POSITIONAL_TRACKING_STATE state = zed.GetPosition(ref zed_pose, REFERENCE_FRAME.WORLD);
        // Display translation and timestamp
        Console.WriteLine("Translation: tx: " + zed_pose.translation.X + "ty: " + zed_pose.translation.Y + "tz: " + zed_pose.translation.Z + "Timestamp: " + zed_pose.timestamp);
        Console.WriteLine("Rotation: ox: " + zed_pose.rotation.X + "oy: " + zed_pose.rotation.Y + "oz: " + zed_pose.rotation.Z + "ow: " + zed_pose.rotation.w);
}
```

The class `Pose` is used to store camera position and additional information such as timestamp and confidence. Since position is always relative to a reference, it is important to set the [coordinate frame](https://www.stereolabs.com/docs/positional-tracking/coordinate-frames/) that will be used as base. To get camera position in real world space, use `REFERENCE_FRAME::WORLD`, otherwise use `REFERENCE_FRAME::CAMERA` to get the change in pose relative to the last position (odometry).

By default, the pose of the left eye of the camera is returned. To get the pose at the center of the camera, see [Frame Transforms](https://www.stereolabs.com/docs/positional-tracking/coordinate-frames/#frame-transforms). You can also retrieve a translation, orientation or rotation matrix using `getTranslation()`, `getOrientation()` and `getRotation()`.

#### Tracking States
`getPosition()` returns a `TRACKING_STATE`. When a new position is available, `TRACKING_STATE::OK` is returned. During initialization or when an Area file is loaded, `TRACKING_STATE::SEARCHING` is returned till the camera recognizes the area. If positional tracking framerate is too low, the state will turn to `TRACKING_STATE::FPS_TOO_LOW` and stop returning new positions.
