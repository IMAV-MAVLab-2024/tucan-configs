# tucan-configs

A centralised place to keep config files for the TUCAN tema of the IMAV 2024 indoor competition.

## MAVLink router

1. Copy the config file to the correct directory:


```sh
sudo cp mavlink-router.conf /etc/mavlink-router/main.conf
```


## PX4-config

The config for the PX4 is saved in the px4_config.params file, you can simply load this file by connecting the Pixracer/Pixhawk FCU to a PC with USB, starting QGroundControl and going to:
Vehicle setup -> Parameters -> Tools -> Load from file..., and select the .params file. The main changes involve setting the baud rates on TEL2 and GPS1 correctly to interface with the companion computer.


## Enabling serial ports

1. Open the u-boot config file.
```sh
sudo nano /etc/default/u-boot
```

2. Set the u boot overlay:
```sh
U_BOOT_FDT_OVERLAYS="device-tree/rockchip/overlay/rk3588-uart0-m2.dtbo device-tree/rockchip/overlay/rk3588-uart1-m1.dtbo"
```

3. Update u-boot config for next boot:
```sh
sudo u-boot-update
```

On the Orange Pi version of Ubuntu 22.04, 
```sh
sudo orangepi-config
```
Go to hardware and enable the uart0-m2 and uart1-m1 and save.

## Configure ROS2 DDS

We use Cyclone DDS since Tailscale does not support multicast traffic, which means automatic node discovery does not work by default.

1. Install Cyclone DDS
```sh
sudo apt install ros-humble-rmw-cyclonedds-cpp
```

## Install Zenoh bridge on the drone

```sh
echo "deb [trusted=yes] https://download.eclipse.org/zenoh/debian-repo/ /" | sudo tee -a /etc/apt/sources.list > /dev/null
sudo apt update
sudo apt install zenoh-bridge-dds
```

## Configure Environment Variables
1. Open the environemnt variable file in the editor:
```sh
sudo nano /etc/environment
```

2. Add the following lines:
```sh
ROS_DOMAIN_ID=74
RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
CYCLONEDDS_URI=file:///home/orangepi/tucan-configs/cyclonedds-drone.xml
```


## Optional: Receive
Currently, PX4 topics are not visible on the external device (f.e. laptop). I dont know why but it is not that important.

1. Install tailscale and add the drone to your network

2. Install zenoh bridge on laptop

```sh
echo "deb [trusted=yes] https://download.eclipse.org/zenoh/debian-repo/ /" | sudo tee -a /etc/apt/sources.list > /dev/null
sudo apt update
sudo apt install zenoh-bridge-dds
```

3. Start zenoh brdige on the drone by running
```sh
zenoh-bridge-ros2dds -e tcp/100.122.190.14:7447
```

4. Install Cyclone DDS on your laptop

```sh
sudo apt install ros-humble-rmw-cyclonedds-cpp
```

5. Clone this repository on your laptop


4. Set the following Environment variables on your laptop:

```sh
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export ROS_DOMAIN_ID=74
export CYCLONEDDS_URI=file://<path_to>/cyclonedds-laptop.xml 
```

5. And then in a terminal window on your laptop with the environment variables set run the following command.
```sh
ros2 topic list
```

