# G933-battery-information-script-for-linux
Python script to get battery information of a Logitech G933 headset on linux without Logitech Gaming Software

#NO WARRANTY#
Work in progress. Battery levels are not accurate yet > mostly guesses based on logged values
Currently disconnects audio during the operation for under second. Might be possible to avoid in the future
Writen using Python3, uses pyusb python package
Requires su for the usb communication

Usage: "sudo python3 g933battery.py"

Returns: "Battery:~86% (estimated from:2/2,151/255) Status: Idle"

PROTOCOL for the communication (Bus 7, Device 6, interfaces 0 and 3 in this case):

host>7.6.0
Endpoint: 0x00, Direction: OUT
Request: 0x21 0x09 0x0211 0x0003
Data: 11ff080a00000000000000000000000000000000

7.6.0>host (some kind of default packet, probably ack)
Endpoint: 0x00, Direction: OUT

7.6.3>host
Endpoint: 0x83, Direction: IN
Data: 11ff080c0ed00100000000000000000000000000

host>7.6.3 (some kind of default packet, probably ack)
Endpoint: 0x83, Direction: IN

From the returning data bytes 3-6 are battery information:
0xa 0xf 0xe9 0x1
|   |   |    |
|   |   |    Headset status
|   |   Battery level pt2
|   Battery level pt1
Unknown level. Often 0xa or 0xc

