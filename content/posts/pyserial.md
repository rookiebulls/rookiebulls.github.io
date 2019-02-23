---
title: 'Control serial port with pyserial'
date: 2016-08-21
categories:
  - 'Programming'
tags:
  - 'Python'
  - 'pyserial'
---

## Install pyserail

Use pip to install pyserail module.

`pip install pyserial`

## Open serail port

First of all, import pyserial module.

`import pyserial`

Secondly, open your serial port. If you don't know which port you are using, you can do it like this.

```python
def open_serialport():
    PORT_NUM = 1
    while True:
        SERIAL_PORT = 'COM%d' % PORT_NUM
        try:
            ser = serial.Serial(port=SERIAL_PORT,baudrate=1048576,parity='N',bytesize=8,stopbits=1,timeout=0)
            print "OPEN SERIAL PORT ON %s" % SERIAL_PORT
            return ser
        except Exception,e:
            PORT_NUM = PORT_NUM + 1
```

## Send data package

In my case, I need to send binary data to a device through serail port. I use the `struct` module to do this.

```python
import struct

senddata = "0015FEFF01FFFFFF0100010000020500"
str2 = ""

while True:
    if senddata:
         str1 = senddata[0:2]
         s = int(str1,16)
         str2 += struct.pack('B',s)
         senddata = senddata[2:]
    else:
        ser.write(str2)
        print repr(str2)
        time.sleep(2)
        ack = ser.read(100)
        print repr(ack)
```

## Unpack binary data

```python
for bdata in ack:
    ddata, = struct.unpack('B',bdata)
    print "%02x" % ddata
```
