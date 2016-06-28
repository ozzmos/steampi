# Steampi

Play your steam library on your TV

# Requirements

* A raspberry Pi with [Raspbian Jessie Lite](https://www.raspberrypi.org/downloads/raspbian/) connected to your local network
* A Windows PC compatible with [Nvidia GameStream](https://shield.nvidia.com/game-stream) and with [Geforce Experience](http://www.geforce.com/geforce-experience/system-requirements) installed
* a Bluetooth dongle to use PS3 controllers (optional)

## Installation

Update your Pi repository

    $ sudo apt-get update

Upgrade 

    $ sudo apt-get dist-upgrade
    
Add the Moonlight embedded repository to your */etc/apt/sources.list*

    deb http://archive.itimmer.nl/raspbian/moonlight jessie main

Update packages repository
 
    $ sudo apt-get update

Install moonlight

    $ sudo apt-get install moonlight-embedded


## Usage

For the first connection, you need to pair your raspberry pi with your pc. Make sure that your Windows PC is up and running then launch

    $ moonlight pair <PC_IP>

A pop-up appears on your PC waiting for your to enter the code tha your Pi has generated.

Ok, normally it's ready ! You can now test Steam in Big Picture mode

    $ moonlight -app Steam stream
    
For list of commands

    $ moonlight help

To lauch moonlight with PS3 controller mapping, 1080p resolution and local audio

    $ moonlight -app Steam -1080 -mapping dualshock3 -localaudio stream
    
## PS3 controllers


    $ sudo apt-get -y install libusb-dev joystick python-pygame
    $ cd ~
    $ wget http://www.pabr.org/sixlinux/sixpair.c
    $ gcc -o sixpair sixpair.c -lusb


Plug the controller into the Raspberry Pi with the USB cable and the Bluetooth dongle if you have not already.
We will also restart the Raspberry Pi to ensure the Bluetooth service is running

    sudo reboot
    sudo ~/sixpair

The sixpair code should re-configure the controller to talk with the dongle, if it worked you should see something like:

    Current Bluetooth master: 00:15:83:0c:bf:eb
    Setting master bd_addr to 00:15:83:0c:bf:eb

displayed on the terminal.
Now disconnect the controller from the USB cable.
Next we start the Bluetooth configuration tool and set the dongle so it can be seen by the controller:

    $ sudo bluetoothctl
    bluetooth# power on
    bluetooth# agent on
    bluetooth# scan on

Now you can press the PS button on the controller and it should attempt to talk to the Raspberry Pi.
You should see some log lines like this at a regular interval:

    [NEW] Device 38:C0:96:5C:C6:60 38-C0-96-5C-C6-60
    [CHG] Device 38:C0:96:5C:C6:60 Connected: no
    [DEL] Device 38:C0:96:5C:C6:60 38-C0-96-5C-C6-60

Try to connect to the MAC address displayed. Tis may take several attempt.

    connect 38:C0:96:5C:C6:60 38
    
    Attempting to connect to 38:C0:96:5C:C6:60
    [CHG] Device 38:C0:96:5C:C6:60 Modalias: usb:v054Cp0268d0100
    [CHG] Device 38:C0:96:5C:C6:60 UUIDs:
        00001124-0000-1000-8000-00805f9b34fb
        00001200-0000-1000-8000-00805f9b34fb

If the controller stops trying to connect press the PS button again before using the connect command again.
Once we have seen the UUID values we can use the trust command to allow the controller to connect on its own.
Use the trust command with the MAC address from earlier, in our example:

    trust 38:C0:96:5C:C6:60

If everything went well you should see something like:

    [CHG] Device 38:C0:96:5C:C6:60 Trusted: yes
    Changing 38:C0:96:5C:C6:60 trust succeeded

Finally exit the Bluetooth configuration tool and restart the Raspberry Pi.

    $ quit
    $ sudo reboot
    
Once you have logged back in press the PS button to test the connection.
The LEDs should briefly flash, then just one LED should remain lit.
You can then use the following command to list the connected joysticks:

    $ ls /dev/input/js*

At least one should be shown, probably /dev/input/js0.
Finally you can test the PS3 controller is working using the device name from the last command with jstest:

    $ jstest /dev/input/js0

The numbers shown should change as you move the joysticks around, if so everything is working properly.

Create a map file with moonlight

    $ moonlight map -input /dev/input/event0 custom.map

    
