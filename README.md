# Steampi

The ultimate gaming console. 
Play steam and emulators games on your TV

## Table of Contents
* [Installation](#installation)
  * [Emulators configuration]()
    * [Dolphin](doc/dolphin/dolphin.md)
  * [Controllers](doc/controllers.md)
    * [dualshock3](doc/controllers.md#dualshock-3)
* [Usage](#usage)


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

    $ moonlight -app Steam -mapping dualshock3 -localaudio stream
