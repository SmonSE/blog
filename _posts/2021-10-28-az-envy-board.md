--- 
layout: post 
title: AZ-Envy Dev Board with ESP8266 
date: 2021-10-28 14:40:59.000000000 +02:00 
type: post
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - az-delivery 
    - az-envy 
    - ESP8266 
    - humidity 
    - Rauchmelder 
    - Temperatur 
author: Smon
permalink: "/2021/10/28/az-envy-board/" 
---

While looking for new sensors, I came across the AZ-Envy from AZ-Delivery and was initially enthusiastic about the description of the sensor board. Of course, I ordered three of them. Anyone who has already dealt with the price issue for sensors knows that one sensor alone is quite expensive, but three or five in a pack usually only cost a few euros more. This is probably due to the fact that the free shipping costs and the processing fee which is also part of the price.

Now a few sentences about the board. I don't understand why no data lines were added to the USB port. As a result, installing the firmware (for beginners as described so nicely in the ad) is extremely complicated. The standard ESP32 or ESP8266 can be flashed much more easily. Of course, once the first firmware is flashed, all other changes can be made via an Over The Air Update (OTA).

But what bothers me most about the board design is that the temperature sensor is right next to the heater of the gas sensor. Even if the sensor has been running for a while, the temperature sensor spits out a wide variety of values. Unfortunately, no offset helps to adjust the temperature. I haven't compared the humidity with any other product so far, but from what I've read on the Internet, the Dev-Board doesn't seem to work reliably either.

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-16.33.03.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-16.33.03.png" width="70%" height="70%">

**Hardware required for commissioning**
---------------------------------------

*   [AZ-Envy Dev Board](https://www.amazon.de/AZDelivery-ESP8266-12F-Entwicklungsboard-Feuchtigkeits-Luftqualitätssensor/dp/B08LKVSH2B/ref=sr_1_8?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=gas%2Bsensor%2Barduino&qid=1635408960&qsid=262-3631401-7484166&sr=8-8&sres=B07V9WCQL4%2CB07XLKQ5SD%2CB086HCSM6N%2CB08LKVSH2B%2CB075Y21H24%2CB09BZH8YZ6%2CB08DGLY3J2%2CB07RX5SXQ6%2CB07PV85YRV%2CB07DQVKJCY%2CB08M615S73%2CB01MS2NJU9%2CB01NB8OKNK%2CB071RR7XZY%2CB01M30ZWQR%2CB078S7FCZ9%2CB0834KMKNH%2CB016KIXSMM%2CB07NK79C7R%2CB071GV6L41&srpt=GAS_SMOKE_FIRE_ALARM&th=1)
*   [FT232RL Flasher Board](https://www.amazon.de/AZDelivery-Adapter-FT232RL-Serial-gratis/dp/B01N9RZK6I/ref=sr_1_3?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=ftdi+flasher+arduino&qid=1635410376&qsid=262-3631401-7484166&sr=8-3&sres=B01N9RZK6I%2CB07R17BMTL%2CB07LH54QHM%2CB07TXVRQ7V%2CB01C2P9GD2%2CB07N2YLH26%2CB07BBPX8B8%2CB08T24NML9%2CB07G87GW4K%2CB01M755991%2CB07TFSZ3ZP%2CB079TZZMR4%2CB0178HVEH0%2CB07K76Q2DX%2CB07WX2DSVB%2CB088TS73YC)
*   [Jumper Wire](https://www.amazon.de/Female-Female-Male-Female-Male-Male-Steckbrücken-Drahtbrücken/dp/B08P9928LB/ref=sr_1_1?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=arduino+einzeladern&qid=1635410730&qsid=262-3631401-7484166&sr=8-1&sres=B08P9928LB%2CB07TRM2G8W%2CB01EV70C78%2CB07KFQ6483%2CB07KFQ1KP1%2CB084LGC1FY%2CB077K6HCFT%2CB077K4M2SX%2CB00591I7PU%2CB07WCB23W8%2CB077K1SM3D%2CB07T3621DH%2CB07C1Z2CDY%2CB077K2GRPY%2CB077JZNWV7%2CB077JZZN2L%2CB073X7GZ1P%2CB077K1DMRL%2CB07CMZXSHB%2CB00JXDP08C) (connection cable)
*   USB cable from flasher board to PC (Mini USB **data cable**)

**Required software**
---------------------

*   [ESPHome.io](https://esphome.io)
*   [HomeAssistant](https://www.home-assistant.io) (only for a nice view)

**3D printer housing**
----------------------

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-19.44.27.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-19.44.27.png" width="70%" height="70%">

Download [3D-print Box](https://cdn.shopify.com/s/files/1/1509/1638/files/3D_print.zip?v=1606923703)

**Creation of the firmware flash file**
---------------------------------------

There are two ways to create the flash file. Here I am describing one of them.

*   Flash file created with ESPHome.io
*   Flash file created with HomeAssistant

**Generate firmware with HomeAssistant**
----------------------------------------

<!-- ![ESPHome]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-13.32.37-1-1024x640.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-13.32.37-1-1024x640.png" width="70%" height="70%">

Install ESPHome under HomeAssitant.

Configure and open ESPHome

Press the + button at the bottom of the page

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-19.55.32-765x1024.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-19.55.32-765x1024.png" width="70%" height="70%">

Enter Projektnamens

Enter SSID (WLAN Name)

Enter WLAN Passworts

Press button "next"

Choose ESP8266

Press button"next".

A congratulation logo should appear. In case of an error, simply press "next" again

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-13.34.49-1024x558.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-13.34.49-1024x558.png" width="70%" height="70%">


The new project has been created and is not yet connected (therefore red).

Press Install and then Manual download

The firmware is compiled and then downloaded.

This file can then be installed via the terminal.

**EZ-Envy-1.yaml**
------------------

    esphome:
      name: humidity
      platform: ESP8266
      board: nodemcuv2
      
    i2c:
      - sda: GPIO4
        scl: GPIO5
        id: bus_a
    
    sensor:
      #Temperatur und Luftfeuchtigkeit
      - platform: sht3xd
        i2c_id: bus_a
        address: 0x44
        
        temperature:
          name: "Temperatur"
          id: "temperature"
          filters:
          - offset: -10.0
        humidity:
          name: "Luftfeuchtigkeit"
          id: "humidity"
          filters:
          - offset: 8.0
        update_interval: 10s
        
      # MQ2 Smoke Gas Sensor
      - platform: adc
        pin: A0
        name: "Gassensor"
        update_interval: 10s
        #filters:
        #  - multiply: 100
            #unit_of_measurement: "%"
        filters:
         lambda: |- 
           return (x * 1024/5);
        unit_of_measurement: "ppm"  
        icon: "mdi:percent" 
        
        
    # Enable logging
    logger:
    
    ....
    ....

**AZ-Envy Firmware flashing via Terminal**
------------------------------------------

<!-- ![AZ-Envy]({{ site.baseurl }}/assets/2021/10/IMG_8677-768x1024.jpg) -->
<img src="/assets/2021/10/IMG_8677-768x1024.jpg" width="70%" height="70%">

First you have to connect the FT232RL Flasher Board to the AZ-Envy. However, you only need three individual wires (jumper wire). Only the pins GND, RX, TX need to be connected.

Then the AZ-Envy must be supplied with voltage and the FT232RL USB cable is then connected to the PC. You can also connect both USB cables to a USB hub (with external power supply) and then go from there to the PC.

The following steps have to be taken:

*   Create Firmware flashfile with HomeAssistant or ESPHome.
*   Connect EZ-Envy to FT232RL
*   Connect the AZ-Envy and FT232RL to a USB hub with an external power supply
*   Connect the USB hub to the PC
*   Open Terminal and go to the ESPTool folder
*   Check the **usbPort** name with command (mac os): **ls -l /dev/tty.\***
*   Command: sudo python esptool.py --port/**usbPort**/write\_flash 0x00000 yourFile.bin

    sudo python esptool.py --port /dev/tty.usbserial-A50285BI write_flash 0x00000 ez-envy-1.bin
    

**Known issues during flash process**
-------------------------------------

If the device is not recognized correctly and the following output appears.

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-12.15.17-1024x270.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-12.15.17-1024x270.png" width="70%" height="70%">

1.  Hold down the flash button on the EZ-Envy as long as step 2 is finished.
2.  Unplug the EZ-Envy's USB cable and plug it in again.
3.  Now the firmware should be flashed (see picture below)

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-28-um-12.30.08-1024x373.png) -->
<img src="/assets/2021/10/Bildschirmfoto-2021-10-28-um-12.30.08-1024x373.png" width="70%" height="70%">


Check your Python Version (in my case Python 3.10.0):

    python3 --version

Install Python3 with the following command:

    pip3 install -U https://github.com/platformio/platformio-core/archive/develop.zip

**Improvements of AZ Envy**
---------------------------

To get correct results from AZ Envy temperature sensor I have desoldered the PM-2 gas sensor. Then I soldered the gas sensor using single wires so that I got a greater distance to the temperature sensor. After that the temp results were much better but not great. I found out that the ESP8266 also emits heat on the metal housing, which slightly fake the measurement results. For this case, I glued a heatsink to the ESP8266's metal case. Now I only have an offset of -1.0 degrees to my room thermostat.

<!-- ![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2022-01-25-um-13.34.22-1024x976.png)
![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2022-01-25-um-13.34.42-1024x701.png) -->

<p float="left">
    <img src="/assets/2021/10/Bildschirmfoto-2022-01-25-um-13.34.22-1024x976.png" width="40%" height="40%">
    <img src="/assets/2021/10/Bildschirmfoto-2022-01-25-um-13.34.42-1024x701.png" width="40%" height="40%">
</p>

**My conclusion on the AZ Envy**
--------------------------------

As a development board, I think the board is really successful and it's fun to work with. If you print out the corresponding case, the price is still reasonably reasonable. The housing also shields the temperature sensor a little from the heating of the gas sensor. Unfortunately, this does not make the temperature values more accurate either.

I will now use my sensor as a gas sensor in the boiler room.
