# README.md

## Overview
This project aims to build a custom Android Wear application utilizing modern smartwatch sensors for continuous authentication and smart office analytics. The resulting prototype contributes to research in the MuLab and CSRL research labs. Students in Kingston may access modern wearables to test the application.

## Android Framework
Android supports three broad categories of sensors:

| Motion Sensors | Environment Sensors | Position Sensors |
|---------------|--------------------|-----------------|
| Acceleration forces and rotational forces along three axes (x, y, z). | Various environmental parameters: barometers, photometers, thermometers. | Physical position of the device. |

### Sensor Types
#### Hardware-based Sensors
Physical components built into the wearable device that measure specific environmental properties (e.g., acceleration, geomagnetic field).

#### Software-based Sensors
Mimic hardware-based sensors by deriving data from one or more hardware-based sensors (composite/virtual/synthetic sensors).

## Motion Sensor API
Many sensors are provided by the Android Platform which allow you to monitor the motion of the device. The four main sensor properties include:
Rotation Vector 
Gravity 
Accelerometer 
Gyroscope

Motion sensors allow the monitoring of device movements which are provided by the user, including tilt, shake, rotation or swing. In the first case, the motion is monitored relative to the device/application’s frame of reference. In the second case, the motion is monitored relative to the world’s frame of reference. Motion sensors are not specifically used with other sensors, such as the geomagnetic field sensor to determine the position of the device relative to the frame of reference of the world. Motion sensors return a multidimensional array of sensor values for each ‘SensorEvent’. 

## Accelerometer Sensor:
The accelerometer measures the acceleration of the device, including the force of gravity. For example, when the device is motionless on a table (no acceleration), the accelerometer reads the magnitude of ‘g = 9.81 m/s^2’. When the device is free-falling, the accelerometer reads a magnitude of ‘g = 0 m/s^2’. The high-pass filter can be applied to measure the real acceleration of the device without the inclusion of the force of gravity. The low-pass filter can be applied to get the force of gravity. 

## Linear Accelerometer Sensor:
A 3D vector representing acceleration along each device axis can be used to provide gesture detection. This vector is also used as an input for an inertial navigation system, which is known as dead reckoning. 

## Uncalibrated Accelerometer Sensor:
An uncalibrated accelerometer is similar to the accelerometer; however, there is no bias compensation applied to acceleration. 

## Gyroscope Sensor:
The gyroscope is used to measure the rate of rotation around the device's x-axis, y-axis and z-axis in **rad/s**. The rotation of the device is positive in the counterclockwise direction. The standard gyroscope displays raw rotational data without the filtration or correction for noise and drift (bias). The bias and noise are usually determined by monitoring other sensors such as gravity or accelerometer

## Uncalibrated Gyroscope Sensor:
An uncalibrated gyroscope is similar to a gyroscope; however, there is no gyro-drift applied to the rate of rotation. This makes it most useful for post-processing and melding orientation data. It also provides the estimated drift around each axis. 

## Rotation Vector Sensor:
The rotation vector is used to detect gestures and monitor angular change and relative orientation change. 

## Gravity Sensor:
The gravity sensor is used to detect gestures and monitor angular change and relative orientation change. 


![Motion Sensor Example](path/to/motion_sensor_image.png)

### Table 1 – Motion Sensor Methods

| Sensor | Event Data | Description | API |
|--------|-----------|-------------|-----|
| **TYPE_ACCELEROMETER (m/s²)** | `SensorEvent.values[0]`<br>`SensorEvent.values[1]`<br>`SensorEvent.values[2]` | Acceleration force along x-axis (including gravity).<br>Acceleration force along y-axis (including gravity).<br>Acceleration force along z-axis (including gravity). | 3 |
| **TYPE_ACCELEROMETER_UNCALIBRATED (m/s²)** | `SensorEvent.values[0]` to `SensorEvent.values[5]` | Measured acceleration along x/y/z-axes, excluding and including bias. | 26 |
| **TYPE_GRAVITY (m/s²)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Force of gravity along x, y, z axes. | 9 |
| **TYPE_GYROSCOPE (rad/s)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Rate of rotation around x, y, z axes. | 3 |
| **TYPE_GYROSCOPE_UNCALIBRATED (rad/s)** | `SensorEvent.values[0]` to `SensorEvent.values[5]` | Rate of rotation (without drift compensation) and estimated drift around x, y, z axes. | 18 |
| **TYPE_LINEAR_ACCELERATION (m/s²)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Acceleration force along x, y, z axes (excluding gravity). | 9 |
| **TYPE_ROTATION_VECTOR (Unitless)** | `SensorEvent.values[0]` to `SensorEvent.values[3]` | Rotation vector components along x, y, z axes and scalar component. | 9 |

## Position Sensor API
Useful to determine a device's physical position in the world's frame of reference. Android Framework provides (3) sensors to determine the position of the device:
Geomagnetic Field (hardware-based)
Accelerometer (hardware-based)
Proximity (hardware-based)

Due to the deprecation of the orientation sensor, determining device orientation – for more information, use readings from the device’s accelerometer and geomagnetic field sensor. Additionally, position sensors are not typically used to monitor device movement/motion (i.e. shake, tilt, thrust) – it is used in motion sensors.

## Game Rotation Vector Sensor:  
Identical to ‘TYPE_ROTATION_VECTOR’ (see motion sensor API), except it does not use a geomagnetic field. Therefore, relative rotations are more accurate due to not being impacted by magnetic field changes. Orientation may drift somewhat over time. E.g. the Y-axis doesn’t point north, but some other reference is allowed to drift the same magnitude as the gyroscope drift around the Z-axis. Useful for games and applications that do not care where magnetic poles are (i.e. where north is).

## Geomagnetic Rotation Vector Sensor: 
Similar to ‘TYPE_ROTATION_VECTOR’, except it uses the magnetometer instead of the gyroscope. Therefore, the accuracy recorded will be lower than the normal rotation vector sensor (more noisier – works better outside); however, power consumption is reduced. Use ONLY if you want to collect rotation information in the background without using too much battery. Useful when used in conjunction with batching.

## Geomagnetic Field Sensor: 
Allows monitoring changes in Earth’s magnetic field. This sensor provides raw field strength data for each of the three coordinate axes.

## Uncalibrated Magnetometer Sensor:
Similar to the geomagnetic field sensor, except no hard iron calibration is applied to the magnetic field (factory calibration and temperature compensation). An uncalibrated magnetometer provides estimated hard iron bias in each axis ‘i.e. calibrated_x ~= uncalibrated_x - bias_estimate_x’. Additionally, there are no periodic calibrations performed (no discontinuities in the data stream).

## Proximity Sensor: 
Determines how far an object is from a device. Usually used to determine how far away the user’s head is from the face of the device or used for wakeup activation. Most proximity sensors return absolute distance (cm); some only return binary values (near/far).


![Position Sensor Example](path/to/position_sensor_image.png)

### Table 2 – Position Sensor Methods

| Sensor | Event Data | Description | API |
|--------|-----------|-------------|-----|
| **TYPE_GAME_ROTATION_VECTOR (unitless)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Rotation vector components along x, y, z axes. | 18 |
| **TYPE_GEOMAGNETIC_ROTATION_VECTOR (unitless)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Rotation vector components along x, y, z axes. | 19 |
| **TYPE_MAGNETIC_FIELD (μT)** | `SensorEvent.values[0]` to `SensorEvent.values[2]` | Geomagnetic field strength along x, y, z axes. | 3 |
| **TYPE_MAGNETIC_FIELD_UNCALIBRATED (μT)** | `SensorEvent.values[0]` to `SensorEvent.values[5]` | Geomagnetic field strength and iron bias estimation along x, y, z axes. | 18 |
| **TYPE_PROXIMITY (cm/binary value)** | `SensorEvent.values[0]` | Distance from object. | 3 |

** TYPE_ORIENTATION deprecated in Android 4.4W (API Level 20) – sensor deprecated Android 2.2 (API Level 8)

## Environment Sensor API
Environment sensors monitor atmospheric conditions and return a single sensor value for each `SensorEvent`.

Android Framework provides (4) sensors to monitor environmental properties:
Relative Ambient Humidity (hardware-based)
Illuminance (hardware-based)
Ambient Pressure (hardware-based)
Ambient Temperature (hardware-based)

Unlike motion and position sensors, which return a multidimensional array of sensor values for each ‘SensorEvent’ – environment sensors return a single sensor value. 
Unlike motion and position sensors, which require high/low-pass filtering, environment sensors do not typically require any data filtering/processing

Light, Pressure, and Temperature Sensors
Raw data acquired from light, pressure, and temperature usually requires no calibration, filtering, or modification. 

Humidity Sensor
One can acquire raw sensor data by using same method as light, pressure, and temperature; however, if device has both a humidity sensor (‘TYPE_RELATIVE_HUMIDITY’) and temperature sensor (‘TYPE_AMBIENT_TEMPERATURE’), you can use these two data streams to calculate:

Dew Point – temperature which given volume of air must be cooled, at constant barometric pressure, for water vapor to condense into water:

![Environment Sensor Example](path/to/environment_sensor_image.png)

Absolute Humidity – mass of water vapor in given volume of dry air (regardless of air temperature) measured in [grams/meter3]:


### Table 3 – Environment Sensor Methods

| Sensor | Event Data | Description | API |
|--------|-----------|-------------|-----|
| **TYPE_AMBIENT_TEMPERATURE (°C)** | `SensorEvent.values[0]` | Ambient (room) temperature. | 14 |
| **TYPE_LIGHT (lx)** | `SensorEvent.values[0]` | Ambient light level. | 3 |
| **TYPE_RELATIVE_HUMIDITY (hPa/mbar)** | `SensorEvent.values[0]` | Atmospheric pressure. | 3 |
| **TYPE_MAGNETIC_FIELD_UNCALIBRATED (%)** | `SensorEvent.values[0]` | Relative ambient air humidity. | 14 |

## Requirements
Wear OS (formerly known as Android Wear) is a version of Google’s Android operating system designated for smartwatches and other wearables. One can pair with mobile phones running Android version 6.0 “Marshmallow” or later. Our scope focuses on the Google Pixel Watches, which is not supported by iOS version 10.0 or newer with Google’s pairing application.
## Google Pixel Watch 1
| Category | Specifications |
|----------|---------------|
| **Connectivity** | 4G LTE and UMTS, Bluetooth® 5.0, Wi-Fi 802.11 b/g/n 2.4 GHz, NFC |
| **Compatibility** | Most Android 8.0 or newer |
| **Power** | 294 mAh, Built-in rechargeable lithium-ion battery |
| **Chip** | Exynos 9110 SoC, Cortex M33 co-processor |
| **OS** | Wear OS 3.5 |
| **Storage and Memory** | 32 GB eMMC FLASH, 2 GB SDRAM |
| **Sensors** | Compass, Altimeter, Blood oxygen sensor, Multipurpose electrical sensor, Optical heart rate sensor, Accelerometer, Gyroscope, Ambient light sensor |

## Google Pixel Watch 2
| Category | Specifications |
|----------|---------------|
| **Connectivity** | 4G LTE and UMTS, Bluetooth® 5.0, Wi-Fi 802.11 b/g/n 2.4 GHz, NFC |
| **Compatibility** | Most Android 9.0 or newer<br>Requires Google Account and Google Pixel Watch app |
| **Power** | 306 mAh, Built-in rechargeable lithium-ion battery |
| **Chip** | Qualcomm SW5100, Cortex M33 co-processor |
| **OS** | Wear OS 4.0 |
| **Storage and Memory** | 32 GB eMMC FLASH, 2 GB SDRAM |
| **Sensors** | Compass, Altimeter, Red and infrared sensors for oxygen saturation (SpO2) monitoring, Multipurpose electrical sensors compatible with ECG app, Multipath optical heart rate sensor, 3-axis accelerometer, Gyroscope, Ambient light sensor, Electrical sensor to measure skin conductance (cEDA) for body response tracking, Skin temperature sensor, Barometer, Magnetometer |

## Sensor Availability
Sensor availability varies between devices and Android versions. Check the [Android Sensor Overview](https://developer.android.com/develop/sensors-and-location/sensors/sensors_overview) for updated compatibility.

## Citations
- [Android Sensor API](https://developer.android.com/reference/android/hardware/Sensor)
- [Wear OS Requirements](https://developer.android.com/google/play/requirements/target-sdk)
- [Google Pixel Watch Specs](https://support.google.com/googlepixelwatch/answer/12651869?hl=en)


