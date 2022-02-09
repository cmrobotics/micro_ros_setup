# Quick start

Download here (TO DO: create docker image) the micro-ROS docker image that contains a pre-installed client and agent as well as some compiled examples.

# Building

Create a ROS 2 workspace and build this package for a given ROS 2 distro (see table above):

```bash
source /opt/ros/$ROS_DISTRO/setup.bash

mkdir uros_ws && cd uros_ws

git clone -b cmr_devel git@github.com:cmrobotics/micro_ros_setup.git

rosdep update && rosdep install --from-path src --ignore-src -y

colcon build

source install/local_setup.bash
```

Once the package is built, the firmware scripts are ready to run.

## Creating micro-ROS firmware

```bash
# Creating a FreeRTOS + micro-ROS firmware workspace
ros2 run micro_ros_setup create_firmware_ws.sh freertos olimex-stm32-e407
```

## Configuring micro-ROS firmware

By running `configure_firmware.sh` command the installed firmware is configured and modified in a pre-build step. This command will show its usage. Replace `$AGENT_IP` by the set static IP or the IP given by the router.

```
ros2 run micro_ros_setup configure_firmware.sh bno055 -t udp -i $AGENT_IP -p 8888
```

By running this command without any argument the available demo applications and configurations will be shown.

Common options available at this configuration step are:
  - `--transport` or `-t`: `udp`, `serial` or any hardware specific transport label
  - `--dev` or `-d`: agent string descriptor in a serial-like transport (optional)
  - `--ip` or `-i`: agent IP in a network-like transport (optional)
  - `--port` or `-p`: agent port in a network-like transport (optional)


Please note that each RTOS has its configuration approach that you might use for further customization of these base configurations. Visit the [micro-ROS webpage](https://micro-ros.github.io/docs/tutorials/core/first_application_rtos/) for detailed information about RTOS configuration.

In summary, the supported configurations for transports are:

|                               |       NuttX        |     FreeRTOS      |       Zephyr       |
| ----------------------------- | :----------------: | :---------------: | :----------------: |
| Olimex STM32-E407             | USB, UART, Network |   UART, Network   |     USB, UART      |
| ST B-L475E-IOT01A             |         -          |         -         | USB, UART, Network |
| Crazyflie 2.1                 |         -          | Custom Radio Link |         -          |
| Espressif ESP32               |         -          |  UART, WiFI UDP   |         -          |
| ST Nucleo F446RE <sup>1</sup> |         -          |       UART        |         -          |
| ST Nucleo F446ZE <sup>1</sup> |         -          |       UART        |         -          |
| ST Nucleo H743ZI <sup>1</sup> |         -          |         -         |        UART        |
| ST Nucleo F746ZG <sup>1</sup> |         -          |       UART        |        UART        |
| ST Nucleo F767ZI <sup>1</sup> |         -          |       UART        |         -          |

*<sup>1</sup> Community supported, may have lack of official support*

## Building micro-ROS firmware

By running `build_firmware.sh` the firmware is built:

```
ros2 run micro_ros_setup build_firmware.sh
```

## Flashing micro-ROS firmware

In order to flash the target platform run `flash_firmware.sh` command.
This step may need some platform-specific procedure to boot the platform in flashing mode:

```
ros2 run micro_ros_setup flash_firmware.sh
```

# Building micro-ROS-Agent

Using this package is possible to install a ready to use **micro-ROS-Agent**:

```
ros2 run micro_ros_setup create_agent_ws.sh
ros2 run micro_ros_setup build_agent.sh
source install/local_setup.sh
ros2 run micro_ros_agent micro_ros_agent udp4 -p 8888
```

# License

This repository is open-sourced under the Apache-2.0 license. See the [LICENSE](LICENSE) file for details.

For a list of other open-source components included in ROS 2 system_modes,
see the file [3rd-party-licenses.txt](3rd-party-licenses.txt).

# Known Issues / Limitations

There are no known limitations.

If you find issues, [please report them](https://github.com/micro-ROS/micro_ros_setup/issues).
