--- 
layout: post 
title: Super smart Home Assistant touch display station on Pi3 
date: 2021-11-10 21:11:55.000000000 +01:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - 3D Printer 
    - 5 inch 
    - Home Assistant 
    - Kuman LCD Display 
    - Raspberry Pi 3 Model B 
    - smart home 
meta: 
    rank_math_seo_score: '86' 
    _edit_last: '1'
    _ez-toc-disabled: '' 
    _ez-toc-insert: '' 
    _ez-toc-heading-levels: a:0:{} 
    _ez-toc-alttext: '' 
    _ez-toc-exclude: '' 
    rank_math_internal_links_processed: '1' 
    rank_math_focus_keyword: Home Assistant 
    rank_math_contentai_score: a:5:{s:9:"wordCount";s:3:"100";s:9:"linkCount";s:1:"0";s:12:"headingCount";s:3:"100";s:10:"mediaCount";s:5:"62.22";s:8:"keywords";s:5:"74.51";} 
    rank_math_primary_category: '89' 
    rank_math_og_content_image: a:2:{s:5:"check";s:32:"30665a4ab17dd1575f98f8f4f271b851";s:6:"images";a:1:{i:0;i:736;}} 
author: Smon
permalink: "/2021/11/10/home-assistant-station/" 
---

![Home Assistant station]({{ site.baseurl }}/assets/2021/11/IMG_8799-768x1024.jpg)

In order to make the daily electricity consumption for the family visible, I built a [small home assistant](http://elesie.de/2021/11/10/home-assistant-station/) monitor.

Since I still had an old Raspberry Pi 3 + Kuman Touch Display brought around, the idea was implemented quickly.

Most of the work on this project was the 3D printer case. I found various housings at thingiverse, but none of them were upright. I redesigned an existing project from Thingiverse for my purposes, but the picture currently only shows a prototype which I am very happy with.

**Hardware required for commissioning**
---------------------------------------

*   [Raspberry Pi 3 Model B](https://www.amazon.de/Raspberry-Pi-3BPLUS-R-Prozessor-Computer/dp/B07HX37B17/ref=sr_1_6?__mk_de_DE=ÅMÅŽÕÑ&crid=SQ7OAA1N9OAH&keywords=raspberry+pi+3+b%2B&qid=1636547572&qsid=262-3631401-7484166&sprefix=Raspberry+Pi+3+%2Caps%2C180&sr=8-6&sres=B07BFH96M3%2CB09HNPJCF7%2CB07N3QYTFJ%2CB07HX37B17%2CB08L6MLXM7%2CB07BDR5PDW%2CB01CD5VC92%2CB082QN6L1N%2CB00LPESRUK%2CB09HNQFPRK%2CB07ZV9C6QF%2CB09JJVTTFC%2CB082PSBBMM%2CB091LKF3ZT%2CB07ZCZ9Z6J%2CB09HNNP4YQ)
*   [Kuman For Raspberry Pi display 5 inch Resistive Touch](https://www.amazon.de/Kuman-Resistive-Protective-Touchscreen-Bildschirm/dp/B07M8QFKHK/ref=sr_1_1?adgrpid=69069141257&gclid=Cj0KCQiA-K2MBhC-ARIsAMtLKRtOy-l56cnGXyybLLshoDY-CNH60KyfYNzADLuFdmFIISiI_7-h3bIaAqtsEALw_wcB&hvadid=391628797118&hvdev=c&hvlocphy=9041871&hvnetw=g&hvqmt=e&hvrand=132073312680425130&hvtargid=kwd-415525993911&hydadcr=27927_1978089&keywords=kuman+5+inch&qid=1636547493&qsid=262-3631401-7484166&sr=8-1&sres=B07M8QFKHK%2CB07MBD6TTP%2CB07ZD4QGQJ%2CB081YQH7KQ%2CB07PG8XY3Y%2CB07JKZBG8H%2CB013JECYF2%2CB07YCBWRQP%2CB07QG87P9D%2CB07KJBCNS8%2CB071Y3NWCK%2CB087GFY4KL%2CB07H79XMLT%2CB07K6GHJFH%2CB07QJ8YQPX%2CB08B67KJ75&srpt=MONITOR) [Screen](https://www.amazon.de/Kuman-Resistive-Protective-Touchscreen-Bildschirm/dp/B07M8QFKHK/ref=sr_1_1?adgrpid=69069141257&gclid=Cj0KCQiA-K2MBhC-ARIsAMtLKRtOy-l56cnGXyybLLshoDY-CNH60KyfYNzADLuFdmFIISiI_7-h3bIaAqtsEALw_wcB&hvadid=391628797118&hvdev=c&hvlocphy=9041871&hvnetw=g&hvqmt=e&hvrand=132073312680425130&hvtargid=kwd-415525993911&hydadcr=27927_1978089&keywords=kuman+5+inch&qid=1636547493&qsid=262-3631401-7484166&sr=8-1&sres=B07M8QFKHK%2CB07MBD6TTP%2CB07ZD4QGQJ%2CB081YQH7KQ%2CB07PG8XY3Y%2CB07JKZBG8H%2CB013JECYF2%2CB07YCBWRQP%2CB07QG87P9D%2CB07KJBCNS8%2CB071Y3NWCK%2CB087GFY4KL%2CB07H79XMLT%2CB07K6GHJFH%2CB07QJ8YQPX%2CB08B67KJ75&srpt=MONITOR)
*   3D Printer for printing case
*   [USB C power supply](https://www.amazon.de/NorthPada-Raspberry-Netzteil-Schalter-Kontrollleuchte/dp/B07W5H9LQJ/ref=sr_1_1_sspa?__mk_de_DE=ÅMÅŽÕÑ&keywords=Netzteil+USB+micro&qid=1636547669&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzT1M5SE5aVjdQNkRKJmVuY3J5cHRlZElkPUEwODgyNDE0MURaMk8wMEM5VkJaNCZlbmNyeXB0ZWRBZElkPUEwMzIzMzkySTQ2Qjg0RzNIR0VWJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ==) ( min 2.5A )

**Required software**
---------------------

*   [Raspberry Pi OS with desktop](https://www.raspberrypi.com/software/operating-systems/)
*   PrusaSlicer or Cura for 3D Printer
*   SD Formatter
*   BelenaEtcher

**3D printer housing**
----------------------

![Home Assistant 3D Print]({{ site.baseurl }}/assets/2021/11/Bildschirmfoto-2021-11-10-um-13.43.44.png)

This is my first draft of a 3D printer case based on an existing project from [Thingiverse](https://www.thingiverse.com/thing:2871564).

I'm still trying to find time to familiarize myself with [OpenSCAD](http://openscad.org). At the moment I am creating new or working on existing projects with the help of [Tinkercad](https://www.tinkercad.com/). I am currently assuming that it will take longer to become familiar with OpenSCAD in order to design complex models.

**Software setup of Raspberry Pi OS**
-------------------------------------

The first steps on how to get a Raspberry Pi OS image on an SD card can be found quickly via google. I use the SD formatter to format the SD card and the BelenaEtcher program to write the image onto the SD card.

After the image has been flashed on the SD card, the standard settings in the Linux operating system are configured such as network, language, keyboard layout, etc. Then you will be asked whether the Linux system should be brought up to date.

The 5 inch display will probably already work with the Raspberry Pi 3, but without the touch screen function. The correct drivers have to be installed for this to work. Here are the steps that need to be taken in order for the touchscreen to function properly:

1.  Install [Raspberry Pi OS official mirror](https://www.raspberrypi.org/downloads/)
2.  Clone repo from GitHub to your pi
    *   _sudo rm -rf LCD-show_
    *   _git clone https://github.com/goodtft/LCD-show.git_
    *   _chmod -R 755 LCD-show_
    *   _cd LCD-show/_
3.  Step3, According to your LCD’s type, excute:
    *   In case of 5″ LCD
        *   _sudo ./LCD5-show_
    *   If you need to switch back to the traditional HDMI display
        *   _sudo ./LCD-hdmi_
4.  Wait a few minutes,the system will restart automatically, enjoy with your LCD
5.  Rotate LCD display
    *   _./rotate.sh 270_

Since the display is now running and rotated with the correct drivers, we can focus on the Chrome Kiosk mode. For this I found another [blog](https://www.stoffl.info/2020/03/25/how-to-raspberry-pi-chromium-kiosk-mode-autostart-und-vollbildmodus/) that describes these steps really well and in great detail. Therefore I will save myself a new description. Here is an example code for the auto start:

    # Mauszeiger Ausblenden
    @unclutter
    
    # Bildschirmschoner ausschalten
    #@xscreensaver -no-splash  
    @xset s off
    @xset -dpms
    @xset s noblank
    
    # Startet im Vollbild und im Inkognito Modus, ohne Adressleiste und Knöpfe und ohne die Möglichkeit den Vollbildmodus mit F11 zu deaktivieren 
    @chromium-browser --incognito --disable-notifications --kiosk http://homeassistant.local:8123/lovelace/0
    
    # Oder (dementsprechend aus kommentieren) 
    
    # Startet im Vollbildmodus, ohne Adressleiste und Knöpfe und im Inkognito Modus.
    #@chromium-browser --incognito --start-fullscreen http://homeassistant.local:8123/lovelace/0
    
    
     

**Software setup Home Assistant**
---------------------------------

After the repeated failure of Chrome to persistently write the login data for Home Assistant, I came across a page that says this is wanted. The only remedy is to specify the Raspberry Pi's IP address as a trusted network under /config/configuration.yaml. Here is a code snipped of my configuration.yaml:

    # Beginning of configuration.yaml
    homeassistant:
      name: homeAssistant
      auth_providers:
        - type: trusted_networks
          trusted_networks:
            - 192.168.178.XX
            - 127.0.0.1
            - ::1
          allow_bypass_login: true
        - type: homeassistant
    
    # Configure a default setup of Home Assistant
    ...
    ...

**Ready to start**
------------------

Now everything should start automatically after the power supply is plugged in. If this is not the case, I have probably forgotten something in my description. If so, drop me a line.