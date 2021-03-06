# homebridge-zwave

[![Version](https://img.shields.io/npm/v/homebridge-zwave.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-zwave)
[![Build Status](https://img.shields.io/travis/mike182uk/homebridge-zwave.svg?style=flat-square)](http://travis-ci.org/mike182uk/homebridge-zwave)
[![npm](https://img.shields.io/npm/dm/homebridge-zwave.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-zwave)
[![License](https://img.shields.io/github/license/mike182uk/homebridge-zwave.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-zwave)

A [Homebridge](https://github.com/nfarina/homebridge) plugin for [ZWave](https://www.z-wave.com/) devices.

## Index

- [homebridge-zwave](#homebridge-zwave)
  - [Index](#index)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Notes](#notes)
    - [Devices tested with this plugin](#devices-tested-with-this-plugin)
  - [FAQ's](#faqs)
    - [How do I install OpenZWave on macOS](#how-do-i-install-openzwave-on-macos)

## Prerequisites

1. Node.js >= 12.13.0
2. [OpenZWave](http://www.openzwave.com/) 1.6
3. Homebridge >= 1.0.0
4. A good understanding of how Homebridge, HomeKit and ZWave works

## Installation

```sh
npm i -g homebridge-zwave
```

## Usage

Register the plugin with Homebridge by adding a new entry to the `platforms` section of your Homebridge config:

```json
{
  "platform": "ZWavePlatform",
  "name": "ZWavePlatform",
  "zwave": {
    "devicePath": "/dev/cu.usbmodem14201",
    "networkKey": "0xE0, 0x40, 0x58, 0x10, 0x79, 0x32, 0x9B, 0x52, 0x96, 0x4B, 0x8F, 0xC5, 0x62, 0x4F, 0x79, 0x3B"
  },
  "accessories": [
    {
      "zwaveNodeId": 2,
      "displayName": "Desk Power Socket",
      "homekitCategory": "Outlet",
      "homekitServices": [
        "Outlet"
      ]
    },
    {
      "zwaveNodeId": 3,
      "displayName": "Light Switch",
      "homekitCategory": "Switch",
      "homekitServices": [
        "Switch"
      ]
    },
    {
      "zwaveNodeId": 4,
      "displayName": "Office Multisensor",
      "homekitCategory": "Sensor",
      "homekitServices": [
        "Battery",
        "MotionSensor",
        "HumiditySensor",
        "TemperatureSensor"
      ]
    }
  ],
  "noCache": false
}
```

If you do not have a `platforms` section in your Homebridge config you will need to define one.

`zwave.devicePath` is the path to your ZWave controller. This can vary based on the controller manufacturer and the operating system you are using.

`zwave.networkKey` is the network key to use for encrypting secure messages over the network. This property can be omitted if you do not require this functionality.

`accessories` is where you will map a ZWave node to a HomeKit accessory. To define an accessory the information needed is:

- `zwaveNodeId` - the ID of the node in the ZWave network that this accessory is for
- `zwaveNodeInstance` - the instance on the node (for a node with multiple instances)
- `displayName` - the name that will be used for this accessory in Homekit
- `homekitCategory` - the type of accessory HomeKit will see this accessory as. Supported categories are:
  - `Outlet`
  - `Sensor`
  - `Switch`
- `homekitServices` - the HomeKit services provided by this accessory. Supported services are:
  - If `homekitCategory` is `Outlet`:
    - `Outlet`
  - If `homekitCategory` is `Sensor`:
    - `Battery` 
    - `HumiditySensor`
    - `LightSensor`
    - `MotionSensor`
    - `TemperatureSensor`
  - if `homekitCategory` is `Switch`
    - `Switch`

_You do not have to specify the service for an accessory if the accessory does not support it (i.e a multisensor that only measures temperature and humidity only needs the `HumiditySensor` and `TemperatureSensor` services)._

`noCache` sets whether accessories registered by this plugin should be cached or not. By default this value is `false`. Set to `true` to disable the cache.

## Notes

### Devices tested with this plugin

- [Aeotec Z-Stick Gen5](https://aeotec.com/z-wave-usb-stick)
- [Aeotec Smart Switch 6](https://aeotec.com/z-wave-plug-in-switch) (UK version)
- [Aeotec MultiSensor 6](https://aeotec.com/z-wave-sensor)

## FAQ's

### How do I install OpenZWave on macOS

You can install `v1.6` of `OpenZWave` using [Homebrew](https://brew.sh/):

```sh
brew tap mike182uk/tap

brew install mike182uk/tap/open-zwave
```

### How do I install OpenZWave on Raspbian

You can easily compile OpenZWave from source by downloading the most recent v1.6 source from [OpenZWave.com](http://old.openzwave.com/downloads/).

```sh
cd ~
mkdir openzwave
cd openzwave
wget http://old.openzwave.com/downloads/openzwave-1.6.1392.tar.gz
```

Extract, compile, install and update libary caches

```sh
tar -xvf openzwave-1.6.1392.tar.gz

cd openzwave-1.6.1392
make && sudo make install && sudo ldconfig
```

⚠️ **WARNING**

If you run:

```sh
brew install open-zwave
```

This will install an older version of `OpenZWave` that **will not work** with this plugin.
