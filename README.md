# FHEM

Snap packaging for the [fhem](http://fhem.de/) home automation server

## Installation

Build the snap following the build instructions below, then install it using the command:

    sudo snap install --dangerous fhem_6.1_arm64.snap
    
Now allow the snapped application to rawly scan for attached USB devices:

    sudo snap connect fhem:raw-usb
    
If you use a USB->serial based device like an FHZ100PC to control your home, you nedd to enable
the USB hotplug feature for snaps:

    snap set system experimental.hotplug=true
    
Plug in our FHZ1000PC and check that it was detected by snapd:

```
$ snap interface serial-port
name:    serial-port
summary: allows accessing a specific serial port
slots:
  - pi:bt-serial
  - pi:serial0
  - pi:serial1
  - pi:serial2
  - pi:serial3
  - pi:serial4
  - pi:serial5
  - pi:serial6
  - pi:serial7
  - pi:serial8
  - pi:serial9
  - snapd:elvfhz1000pc (allows accessing a specific serial port)
$
```

Now you can connect the detected serial device to the fhem snap:

    snap connect fhem:serial-port snapd:elvfhz1000pc
    
The server should start now and auto-detect your actuators and heating controllers automatically,
go to `http://<ip of the fhem server>:8083/` and control your devices through the FHEM web interface.

## Configuration files

All configuration files, logs and other bits that need a writable filesystem live under:

    /var/snap/fhem/current/
    
## Building

Clone this repository, make sure you have the snapcraft snap installed on your system and simply
run `snapcraft` in the top-level directory of the checked out tree. This should create
a snap package for you. Note that you need to build the snap on an ARM device in case you want to
use it on i.e. a Raspberry Pi.
