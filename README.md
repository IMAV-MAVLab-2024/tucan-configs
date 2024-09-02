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


## Configure Environment Variables
1. Open the environemnt variable file in the editor:
```sh
sudo nano /etc/environment
```

2. Add the following lines:
```sh
RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
CYCLONEDDS_URI=file:///home/orangepi/cyclonedds-drone.xml 
ROS_DOMAIN_ID=74
```


## Optional: Enable ROS2 networking through tailscale

1. Install tailscale and add the drone to your network

2. Add your laptops IP to the cyclonedds.xml
```xml
<Peers>
<Peer address="100.96.167.37"/> # Anton
<Peer address="100.xx.xx.xx"/>  # Your IP
</Peers>
```

3. Restart Drone

4. Install Cyclone DDS on your laptop

```sh
sudo apt install ros-humble-rmw-cyclonedds-cpp
```

5. Clone this repository on your laptop


4. Set the following Environment variables on your laptop:

```sh
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI=file://<path_to>/cyclonedds-laptop.xml 
export ROS_DOMAIN_ID=74
```

5. And then in a terminal window on your laptop with the environment variables set run the following command. This should show the PX4 topics.
```sh
ros2 topic list
```

