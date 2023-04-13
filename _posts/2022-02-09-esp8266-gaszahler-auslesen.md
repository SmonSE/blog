--- 
layout: post 
title: ESP8266 - Gaszähler auslesen 
date: 2022-02-09 17:20:29.000000000 +01:00 
type: post
published: true
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - 3D Printer 
    - ESP8266 
    - G4 RF1 
    - Gaszähler 
    - KY-025 
    - Magnetschalter 
    - Reed Sensor 
    - Tinkercad 
    - WeMosD1 
    - WeMosD1 Mini Pro 
author: Smon
permalink: "/2022/02/09/esp8266-gaszahler-auslesen/" 
---

Mit der Hilfe eines ESP8266 und eines Magnetschalters ( Reed Sensor ) kann am Gaszähler ( G4 RF1) über den Internen Magneten der aktuelle Verbrauch abgelesen werden ( 1 Imp = 0.1m³ ). Eingebunden ist das ganze wie bereits beim [digitalen Stromzähler](http://elesie.de/2021/10/26/esp8266-stromzahler-auslesen/) in Home Assistant ( HASSIO ). Dort kann man sich die Verbrauchswerte für die unterschiedliche Zeiten (Tag, Woche, Monat, Jahr) anzeigen lassen.

Mein erstes Problem bestand darin das ich mittels eines normalen WeMos D1 keinen Empfang im Heizraum hatte. Daraufhin habe ich den WeMos D1 Mini Pro mittels externer Antenne ausprobiert, mit Erfolg. Die Signalstärke ist ausreichend um die Daten über Wlan zu senden.

**NEUE LÖSUNG MIT [AQARA TÜR FENSTERSENSOR](http://elesie.de/2022/09/29/gaszahler-auslesen/)**

**Benötigte Hardware**
----------------------

*   [ESP8266](https://www.amazon.de/AZDelivery-ESP8266-ESP-8266EX-Entwicklung-inklusive/dp/B08BTXCZC1/ref=sr_1_2_sspa?__mk_de_DE=ÅMÅŽÕÑ&crid=3P4D055YFXIAG&keywords=Memos%2Bd1%2Bmini&qid=1644400539&sprefix=memos%2Bd1%2Bmini%2Caps%2C82&sr=8-2-spons&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUE3TFNNMVJCU0tDMzQmZW5jcnlwdGVkSWQ9QTAwNTk4ODQxRzc2UkIzQ1NVR002JmVuY3J5cHRlZEFkSWQ9QTA2NzY4MjIxM0tSRFZaTEFVNVdUJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1) ( WeMos D1 Mini Pro )
*   [Reed Sensor](https://www.amazon.de/AZDelivery-KY-025-Magnetschalter-Sensor-gratis/dp/B089QJVBL7/ref=sr_1_1_sspa?__mk_de_DE=ÅMÅŽÕÑ&crid=8L95OBHAHWKM&keywords=reed%2Bsensor&qid=1644836930&sprefix=reed%2Bsensor%2Caps%2C76&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzUDhBV0xVQVoxVlhRJmVuY3J5cHRlZElkPUEwNTQ2MTc4MzhWT0UxWlJHTjdCTSZlbmNyeXB0ZWRBZElkPUEwNTIxMDg0MjRDNkdMWjRGNEIzNCZ3aWRnZXROYW1lPXNwX2F0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU&th=1) ( KY-025 )
*   Einzeladern
*   [3D Drucker](http://elesie.de/2022/03/21/3d-prints-tinkercad/) 
*   Gaszähler G4 RF1

**Benötigte Software**
----------------------

*   [TinkerCad](https://www.tinkercad.com)
*   [PrusaSlicer](https://www.prusa3d.de)
*   [Home Assistant](https://www.home-assistant.io/getting-started/)
*   [ESP Home](https://esphome.io)

**3D Gehäuse Tinkercad**
------------------------

*   [Gaszähler G4 RF1](https://www.tinkercad.com/things/gjmYXJnO9qC)

**Aufbau**
----------

<!-- ![]({{ site.baseurl }}/assets/2022/02/Bildschirmfoto-2022-02-09-um-11.07.32.png)
![]({{ site.baseurl }}/assets/2022/02/Bildschirmfoto-2022-02-09-um-11.23.15-730x1024.png)
![]({{ site.baseurl }}/assets/2022/02/IMG_9647-732x1024.jpg) -->

<p float="left">
    <img src="/assets/2022/02/Bildschirmfoto-2022-02-09-um-11.07.32.png" width="30%" height="30%">
    <img src="/assets/2022/02/Bildschirmfoto-2022-02-09-um-11.23.15-730x1024.png" width="30%" height="30%">
    <img src="/assets/2022/02/IMG_9647-732x1024.jpg" width="30%" height="30%">
</p>

In meinem Fall habe ich mir bei Thingiverse ein passendes Case für den WeMos D1 Mini Pro geholt und mittels Tinkercad auf mein entworfenes Case gesetzt. Somit kann jede beliebige ESP8266 Platine mit der Magnetsensor Grundplatte verbunden werden.

**KY-025 Magnetschalter Anschlussplan**
---------------------------------------

Beim Magnetschalter muss man sehr auf das Glasröhrchen achten. Ich habe das Glasröhrchen mit dem Reed Kontakt leicht nach unten gebogen damit es bündig mit dem Auslass am gedruckten Gehäuse abschließt.

*   WeMos 5V ----> + KY-025
*   WeMos GND --> - KY-025
*   WeMos D5 ----> A0 KY-025

**Gaszähler** Typ **G4 RF1**
----------------------------

<!-- ![G4 RF1 von Actaris ESP8266]({{ site.baseurl }}/assets/2022/02/IMG_9661-716x1024.jpg) -->
<img src="/assets/2022/02/IMG_9661-716x1024.jpg" width="70%" height="70%">

Bei meinem Gaszähler handelt es sich um das Model G4 RF1 von Actaris.

Das Model G4 RF1 von Pipersberg ist baugleich soweit ich weiß. Daher sollte das Case für den Sensor auch hier passen.

****ESP Home Sensor config.yaml****
-----------------------------------

Hier hatte ich zu Beginn Probleme beim Kompilieren der Software. Ich hatte unter board den d1_mini_pro stehen welcher normal über 16MB Speicher verfügt. Bei meiner Bestellung habe ich übersehen das mein WeMos D1 Mini Pro mit nur 4 MB bestückt ist. Daher musste ich die Software für einen normalen d1_mini kompilieren.

    esphome:
      name: gas
    
    # d1 mini pro but only with 4MB -> d1_mini
    esp8266:
      board: d1_mini 
    
    sensor:
      - platform: pulse_counter
        pin: 
          number: D5
          mode: INPUT_PULLUP
        name: "Gasverbrauch"
        state_class: total_increasing
        device_class: gas
        update_interval : 60s
        filters:
          - lambda: |-
              static float total_value = 0.0;
              total_value += x * 0.1;
              return total_value;
        unit_of_measurement: "m³"
        accuracy_decimals: 2
        icon: 'mdi:fire' 
        id: gasverbrauch
    
    time:
      - platform: sntp
        id: my_time    
    
    # Enable logging
    logger:
    
    # Enable Home Assistant API
    api:
    
    ota:
      password: "xxxxxxxxxxxxxxxxxxxxxxxxxx"
    
    wifi:
      ssid: !secret wifi_ssid
      password: !secret wifi_password
    
      # Enable fallback hotspot (captive portal) in case wifi connection fails
      ap:
        ssid: "Gas Fallback Hotspot"
        password: "xxxxxxxxxxxx"
    
    captive_portal:
    

**Hassio /config/configuration.yaml**
-------------------------------------

    ......
    
    utility_meter:
      gasverbrauch_daily:
        source: sensor.gasverbrauch
        cycle: daily
      gasverbrauch_weekly:  
        source: sensor.gasverbrauch
        cycle: weekly
      gasverbrauch_monthly:
        source: sensor.gasverbrauch
        cycle: monthly
      gasverbrauch_yearly:
        source: sensor.gasverbrauch
        cycle: yearly 
    
    ......

****Home Assistant grafische Darstellung****
--------------------------------------------

<!-- ![]({{ site.baseurl }}/assets/2022/02/IMG_9654-613x1024.jpg)
![]({{ site.baseurl }}/assets/2022/02/IMG_9656-609x1024.jpg) -->

<p float="left">
    <img src="/assets/2022/02/IMG_9654-613x1024.jpg" width="30%" height="30%">
    <img src="/assets/2022/02/IMG_9656-609x1024.jpg" width="30%" height="30%">
</p>
