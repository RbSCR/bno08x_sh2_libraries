# bno08x_sh2_libraries

![Project Status](https://img.shields.io/badge/Status-Work%20In%20Progress-orange)
![ROS 2](https://img.shields.io/badge/ROS%202-Jazzy%20(Ubuntu%2024.04)%20%20Kilted%20(Ubuntu%2024.04)-blue?style=flat&logo=ros&logoSize=auto)
![C++](https://img.shields.io/badge/C++-17-blue?style=flat&logo=cplusplus&logoColor=white)
![License](https://img.shields.io/github/license/RbSCR/bno08x_sh2_libraries?label=License)

## Overview

A library package for the CEVA BNO08x 9-DOF IMU's over I2C.

The BNO08x family (BNO085/BNO086) is a compact System in Package (SiP) with integrated accelerometer, gyroscope, magnetometer, and a 32-bit ARM® Cortex™-M0+ running CEVA's SH-2 firmware. It delivers real-time 3D orientation, heading, calibrated acceleration, and angular velocity, with on-board sensor fusion algorithms and calibration. It supports I2C, SPI, and UART interfaces for sensor data output.

>[!NOTE]
>This package only supports I2C.

This package contains 2 libraries: `sh2` and `bno08x`.
These libraries can be used in other ROS2 packages that need to use a CEVA BNO08x 9-DOF IMU.

**⚠️ Status:**

- Tested and validated, using another ROS package, on a Raspberry Pi 5 running ROS 2 Kilted/2026-08-21 (Ubuntu 24.04, aarch64) with real BNO085 hardware.
- Not yet tested on ROS2 Jazzy
- Work in progress

## Installation

Clone the repository:

```bash
cd ~/ros_ws/src
git clone https://github.com/RbSCR/bno08x_sh2_libraries.git
```

Build the package:

```bash
colcon build --packages-select bno08x_sh2_libraries
```

## Usage

To use the libraries in another ROS package add a dependency to `bno08x_sh2_libraries` in the `package.xml` and `CMakeLists.txt`.

TODO provide short example

This package is not available on a distro, so you need to install this package in your workspace (see above).

## Datasheet and documents

| Name | Document number | Document revision | Date | By |
| ---- | --------------- | ----------------- | ---- | -- |
| [BNO080_085-Datasheet.pdf](https://www.ceva-ip.com/wp-content/uploads/BNO080_085-Datasheet.pdf) | 1000-3927 | 1.17 | July 24 2023 | CEVA hillcrestlabs |
| [SH-2-Reference-Manual.pdf](https://www.ceva-ip.com/wp-content/uploads/SH-2-Reference-Manual.pdf) | 1000-3625 | 1.9 | June 2021 | hillcrestlabs |
| [Sensor-Hub-Transport-Protocol-v1.7.pdf](https://cdn.sparkfun.com/assets/7/6/9/3/c/Sensor-Hub-Transport-Protocol-v1.7.pdf) | 1000-3535 | 1.7 | 02/16/2017 | hillcrestlabs |
| [Sensor-Calibration-Procedure-v1.1.pdf](https://cdn.sparkfun.com/assets/9/e/1/d/9/Sensor-Calibration-Procedure-v1.1.pdf) | 1000-4044 | 1.1 | February 2017 | hillcrestlabs |
| [BNO080-BNO085-Tare-Function-Usage-Guide.pdf](https://www.ceva-ip.com/wp-content/uploads/BNO080-BNO085-Tare-Function-Usage-Guide.pdf) | 1000-4045 | 1.3 | February 2023 | CEVA hillcrestlabs |

## Acknowledgements

This package uses the SH-2 protocol library provided by Hillcrest Labs.
It can be found in the `include/sh2` and `src/sh2` directories.
Visit the official repository here: [SH-2 Protocol Library](https://github.com/ceva-dsp/sh2.git)

This package also uses the code from the "BNO08X ROS Driver" package (see link below).
The code has been updated with new functionality that is used in another ROS2 Control plugin.
It can be found in the `include/bno08x` and `src/bno08x` directories.

### Inspiration

Inspiration for this package came from:

- the "BNO08X ROS Driver" package by bnbhat (<https://github.com/bnbhat/bno08x_ros2_driver>)

## License

This package is licensed under the Apache License 2.0. You can find the full license text in the [LICENSE](./LICENSE) file of the repository.
