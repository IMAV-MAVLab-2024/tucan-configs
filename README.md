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

