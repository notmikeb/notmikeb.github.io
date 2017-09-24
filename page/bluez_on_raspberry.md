---
layout: page
title: "Bluetooth on Raspberry Pi with Buildroot"
description: ""
---
{% include JB/setup %}

https://delog.wordpress.com/2014/10/29/bluetooth-on-raspberry-pi-with-buildroot/

```
Bluetooth on Raspberry Pi with Buildroot
by DevendraOctober 29, 2014
      1 Vote

This post shows how to build Bluetooth support with Buildroot for a Raspberry Pi. You’ll need a Bluetooth adapter such as the Bluetooth 4.0 USB Module from Adafruit. That adapter is also Bluetooth Smart (LE) ready.

I recommend using Buildroot 2014.08 because bluez-tools such as bt-adapter and bt-agent work with its version of Bluez.

Kernel Configuration

Enable Bluetooth subsystem support under Networking support

Bluetooth subsystem support

Enable RFCOMM  protocol support under Bluetooth subsystem support, if you want Bluetooth SPP support. Enable BNEP protocol support, if you want support for networking over Bluetooth. Enable HIDP protocol support, if you want support for Bluetooth enabled keyboard and mice.

RFCOMM and BNEP support

Since we’re using a USB adapter, enable HCI USB driver under Bluetooth subsystem support, Bluetooth device drivers

HCI USB driver

Buildroot Packages

The Kernel Bluetooth stack is called BlueZ. BlueZ also provides a set of utilities that can be used from the command line. To add those, select bluez-utils under Target packages, Networking applications

bluez-utils

If using Buildroot 2014.08, select bluez-utils 5.x package instead

bluez5_utils

make the Linux system and copy to SD card.

Useful commands

Look for interfaces

hciconfig -a
Bring up HCI interface

hciconfig hci0 up
Make the device discoverable

hciconfig hci0 piscan
Scan available devices

hcitool scan
Ping a device

l2ping C8:3E:99:C6:1B:F8
Browse capabilities of a device

sdptool browse C8:3E:99:C6:1B:F8
Run bluetooth daemon

bluetoothd
If using Buildroot 2014.08

/usr/libexec/bluetooth/bluetoothd &
Browse local capabilities

sdptool browse local
Another means to discover available devices, using bluez-tools

bt-adapter -d
Connect and pair with a device listed by the command above

bt-device -c 5C:0E:8B:03:6E:4E
Create a network connection and obtain IP address

bt-network -c C8:3E:99:C6:1B:F8 nap
sudo dhclient bnep0
Create a serial port

sdptool add --channel=7 SP
rfcomm connect /dev/rfcomm0 C8:3E:99:C6:1B:F8 7
Use screen with the above serial port

screen /dev/rfcomm0 115200
Bluetooth LE commands

Scan for Bluetooth LE devices

hcitool lescan
Access services and characteristics provided by an LE peripheral

gatttool -b 7F:AE:48:2B:00:0C -t random -I
gatttool prompt supports a set of commands to interact with the peripheral

1
2
3
4
5
connect
primary
char-desc
char-read-hnd handle
char-write-req handle data
Enable advertising as an iBeacon

1
2
3
hciconfig hci0 leadv
hciconfig hci0 noscan
hcitool -i hci0 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 E2 0A 39 F4 73 F5 4B C4 A1 2F 17 D1 AD 07 A9 61 00
Bluetooth is big. Go ahead and do something with it.
```