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

This package is not available in a distro, so you need to install this package in your workspace.

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

To use the libraries in another ROS package you need to:

- install this package (see [above](#installation))
- update your `package.xml`, `CMakeLists.txt` and cpp- or hpp-file(s).

**package.xml:**

Add a `depend`:

```xml
...
<depend>bno08x_sh2_libraries</depend>
...
```

**CMakeLists.txt:**

Add a `find_package` and a `target_link_libraries`:

```cmake
...
find_package(bno08x_sh2_libraries REQUIRED)
...
target_link_libraries(<YOUR-EXECUTABLE/LIBRARY>
  ${bno08x_sh2_libraries_TARGETS}
  ...
)
...
```

**cpp/hpp-files**:

Add includes:

```cpp
...
#include "bno08x/bno08x.hpp"
#include "bno08x/i2c_interface.hpp"
#include "bno08x/watchdog.hpp"
...
```

### Github-actions build workflow

When you use a github-actions workflow to build your package, you need to add the `bno08x_sh2_dependendy.repos` file (provided in `misc/example-repo-file/`) to your package and update your workflow to use the file.

If you use `ros-tooling/action-ros-ci` use this example:

```yml
...
- name: Build and Test
        uses: ros-tooling/action-ros-ci@0.4.8
        id: action_ros_ci_step
        with:
          target-ros2-distro: <YOUR-DISTRO>
          package-name: <YOUR-PACKAGE>
          vcs-repo-file-url: "${{ github.workspace }}/<PATH-TO-FILE-IN-YOUR-PACKAGE>/bno08x_sh2_dependency.repos"
          ...
```

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

This package also uses the code from the [BNO08X ROS Driver](https://github.com/bnbhat/bno08x_ros2_driver) package by bnbhat.
The code has been updated with new functionality that is used in another ROS2 Control plugin.
The (updated) code can be found in the `include/bno08x` and `src/bno08x` directories.

### Inspiration

Inspiration for this package came from:

- the [BNO08X ROS Driver](https://github.com/bnbhat/bno08x_ros2_driver) package by bnbhat.

## License

This package is licensed under the Apache License 2.0. You can find the full license text in the [LICENSE](./LICENSE) file of the repository.
