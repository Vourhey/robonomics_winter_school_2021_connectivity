# Robonomics Winter School 2021: Connectivity

> Find the Russian version of this document [here](README_ru.md) 

## IoT as a Multiple Pie

* Device Software
    * FreeRTOS
    * ESP/Arduino
    * Single-board computers (RPi, LattePanda etc)
* Connectivity
    * IoT Hub
    * IoT Manager
* Analytics Services
    * AWS
    * Google Cloud IoT Core
    * ThingsBoard

As a rule, most are not interested in sensors and servers, but data analytics.
To get it, you need to decide which device to use, how to work with it and where to connect

## Device Software

Consider the example of a home weather station. It is necessary to collect data on air pollution (SDS011), temperature and humidity (BME). The ESP8266 microcontroller can handle this task.

Requirements:

* Correctly read data from sensors
* Have a unique identifier
* Transfer data to a known server
* Provide digital signature of data (optional)

You can find the current firmware [here](https://github.com/LoSk-p/sensors-software/tree/366b19bf447a5fc19220ef89eab0f2440f8db1c2)

## What is Connectivity? 

In the IoT world, connectivity refers to the connection of various IoT devices to the Internet to send data and / or control the device.

Well-known architectural solutions can be roughly divided into 3 groups:

* Fully decentralized. For example, devices are connected by a mesh network. Not suitable for wide area networks due to high hardware requirements
* Centralized. For example, AWS. Provides a single entry point and ease of connection, but there is a high risk of failure in case of server problems
* Hybrid. For example, [Robonomics Connectivity](https://github.com/airalab/sensors-connectivity). Provides an address for devices on a "local" network and publishes data to a distributed IPFS message channel

## Comparison of AWS and Robonomics Connectivity

| Management services 	| AWS                               	|               Robonomics              	|
|---------------------	|-----------------------------------	|:-------------------------------------:	|
| Transaction type    	| Technical                         	| Technical and economic                	|
| Security            	| IT-company cloud control          	| Polkadot and Ethereum                 	|
| Protocol            	| HTTPS, MQTT                       	| IPFS, Robonomics                      	|
| Ecosystem           	| Private                           	| Shared                                	|
| Access to DeFi      	| No                                	| Yes                                   	|
| Costs               	| Pushing data - $1-2 a sensor      	| Pushing data - $0                     	|
|                     	| Shadow         - from $10 a month 	| Digital Twin    - $0,01 a transaction 	|

## Practice

### Trajectory 1. Flash a sensor ESP + SDS011

Requirements:

* ESP8266
* At least one of sensors SDS011, BME280, HTU21D

Use the [instruction](https://wiki.robonomics.network/docs/connect-sensor-to-robonomics/) to connect a sensor to Robonomics Connectivity. 

Check that your sensor appears on our [map](https://sensors.robonomics.network/#/).

### Trajectory 2. Launch Connectivity

Requirements:

* ROS
* Python
* Nix (optional)

Build and launch [sensors-connectivity](https://github.com/airalab/sensors-connectivity#get-a-package-and-build)

> How it build, install [here](https://wiki.robonomics.network/docs/iot-sensors-connectivity/) and configure [here](https://wiki.robonomics.network/docs/configuration-options-description/)

General scheme of the package:

```
    station1 \                        / feeder1
    station2 -  sensors-connectivity  - feeder2
    station3 /                        \ feeder3
```

The choice is proposed to implement either a new station, for example, a random number generator, or a new feeder, for example, displaying a string on the screen.

Interface `IStation` [here](https://github.com/airalab/sensors-connectivity/blob/master/src/stations/istation.py#L73).

Interface `IFeeder` [here](https://github.com/airalab/sensors-connectivity/blob/master/src/feeders/ifeeder.py#L5)

