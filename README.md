# tucan-configs

A centralised place to keep config files for the TUCAN tema of the IMAV 2024 indoor competition.

## MAVLink router

1. Copy the config file to the correct directory:


```sh
sudo cp mavlink-router.conf /etc/mavlink-router/main.conf
```


## PX4-config

*WIP*


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

## Configure ROS_DOMAIN_ID
1. Open the environemnt variable file in the editor:
```sh
sudo nano /etc/environment
```
2. Add the following line:
```sh
ROS_DOMAIN_ID=74
```

