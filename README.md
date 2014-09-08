### CC3K Driver Demo

This is a sample binary firmware image for the [Spark Core](http://spark.io) with a development replacement [cc3k driver](https://github.com/wtfuzz/cc3k)

#### Installing

* DFU utilities must be installed, with a USB connection
* Enter DFU mode using the pushbuttons on the core
* Flash the demo firmware onto the core:
```
dfu-util -d 1d50:607f -a 0 -s 0x08005000:leave -D core-firmware.bin
```

#### Shell

A serial console with a small shell is available to setup the wireless network and show driver information,
it is running at 230400 baud

```
screen /dev/tty.usbmodem1451 230400
```

Press return after connecting to the serial console.

```
spark>
```

Connect to the wireless network:
```
spark> wifi up wpa2 MySSID MyPassword
```

The blue D7 LED with illuminate when the wifi connection has been established

Driver information can be shown with the 'info' command
```
spark> info
-----=====[ CC3K Driver Info ]====-----
Uptime (ms): 516509
INT Enabled: 1
IRQ Preempt: 0
Current Command: 4104
Select Pending: 1
CS Asserted: 0
INT Pin: 1
SPI Busy: 0
SPI Unhandled: 2
Buffers available: 6
WLAN Status: 3
State: 4
Last State: 4
Int State: 2
Unhandled State: 0
Commands TX: 40655
Events RX: 60886
DATA RX: 20204
DATA Bytes RX: 30420538
Unsolicited Events RX: 28
DMA Interrupts: 162399
HW Interrupts: 101541
Unhandled Interrupts: 0
```

IP Address information assigned to the CC3000 can be shown with the 'ifconfig' command:
```
spark> ifconfig
IP Address: 10.1.1.100
Netmask: 255.255.255.0
Default Gateway: 10.1.1.1
DHCP Server: 10.1.1.1
DNS Server: 10.1.1.1
```

#### UDP Server

There is a UDP server running on port 5000 which for now just discards received data.

You can send data to this socket and the driver will process the received packets and update the stats shown with the 'info' shell command.

Example flooding the socket with UDP data with netcat for stability testing:
```sh
cat /dev/zero |nc -u <CORE IP ADDRESS> 5000
```
