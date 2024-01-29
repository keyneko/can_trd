```bash
$ ifconfig -a
can0: flags=193<UP,RUNNING,NOARP>  mtu 16
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 10  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

# 创建CAN接口
sudo ip link set can0 up type can bitrate 500000

# 安装can-utils工具库
sudo apt-get install can-utils

# 使用cansend发送CAN消息
cansend can0 123#1122334455667788

# 使用candump监听CAN消息
candump can0
```

# PCAN
```bash
# 查询驱动
$ modinfo peak_usb
filename:       /lib/modules/5.4.0-150-generic/kernel/drivers/net/can/usb/peak_usb/peak_usb.ko
license:        GPL v2
description:    CAN driver for PEAK-System USB adapters
author:         Stephane Grosjean <s.grosjean@peak-system.com>
srcversion:     5F352C3CBF669C33FB763D8
alias:          usb:v0C72p0014d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0C72p0013d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0C72p0011d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0C72p0012d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0C72p000Dd*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0C72p000Cd*dc*dsc*dp*ic*isc*ip*in*
depends:        can-dev
retpoline:      Y
intree:         Y
name:           peak_usb
vermagic:       5.4.0-150-generic SMP mod_unload modversions 
signat:         PKCS#7
signer:         
sig_key:        
sig_hashalgo:   md4
```

# SLCAN
```bash
# 启用CAN子系统和相应的驱动程序
sudo modprobe can
sudo modprobe can_raw
sudo modprobe can_dev
sudo modprobe slcan

# 创建CAN接口
sudo slcand -o -c -s6 /dev/ttyACM0
sudo ip link set up slcan0

# 使用cansend发送CAN消息
cansend slcan0 123#1122334455667788

# 使用candump监听CAN消息
candump slcan0
```
