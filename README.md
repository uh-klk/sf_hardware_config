Hardware config to create a symlink for ttyUSB devices using udev rules

The "90-persistent-usb.rules" in sf1-2 folder should be copied to /etc/udev/rules.d/

Then Run the command below to trigger the new udev rules

    #udevadm control --reload-rules
    #udevadm trigger

Now plug in the pioneer and dynamixel usb devices and you should see /dev/p3dxBase and /dev/dynamixel ports created automatically.

If not Check that your devices ids matched the configurations in 90-persistent-usb.rules. See instraction below and update the rules accordingly if needed..


1. Unplug your robot/dynamixel usb connector from pc then run the command below, then plug in the usb device for identification. 

    #udevadm monitor -u
    UDEV  [2224.268312] add      /devices/pci0000:00/0000:00:04.0/usb3/3-4 (usb)
    UDEV  [2224.273835] add      /devices/pci0000:00/0000:00:04.0/usb3/3-4/3-4:1.0 (usb)
    UDEV  [2224.276554] add      /devices/pci0000:00/0000:00:04.0/usb3/3-4/3-4:1.0/ttyUSB0 (usb-serial)
    UDEV  [2224.283136] add      /devices/pci0000:00/0000:00:04.0/usb3/3-4/3-4:1.0/ttyUSB0/tty/ttyUSB0 (tty)

2. Then run the command below using the identified device to get detail info, e.g. for above dynamixed device, to make sure the matched the configurations, else modified the configuration file to matched the info listed.

    #udevadm info -a --path=/sys/devices/pci0000:00/0000:00:04.0/usb3/3-4

    looking at device '/devices/pci0000:00/0000:00:04.0/usb3/3-4':
        KERNEL=="3-4"
        SUBSYSTEM=="usb"
        DRIVER=="usb"
        ATTR{bDeviceSubClass}=="00"
        ATTR{bDeviceProtocol}=="00"
        ATTR{devpath}=="4"
        ATTR{idVendor}=="0403"
        ATTR{speed}=="12"
        ATTR{bNumInterfaces}==" 1"
        ATTR{bConfigurationValue}=="1"
        ATTR{bMaxPacketSize0}=="8"
        ATTR{busnum}=="3"
        ATTR{devnum}=="11"
        ATTR{configuration}==""
        ATTR{bMaxPower}=="90mA"
        ATTR{authorized}=="1"
        ATTR{bmAttributes}=="a0"
        ...

3. You can use udevadm to get the info and use grep to print the you need, i.e. the serial attribute
    
    #udevadm info -a -n /dev/ttyUSB0 | grep '{serial}' 
    ATTRS{serial}=="A700etWE"

4. Run the command below to trigger the new udev rules

    #udevadm control --reload-rules
    #udevadm trigger

