--- 
layout: post 
title: Pelletsensor - Füllstandsanzeige mit ESP8266 & Ultraschallsensor 
date: 2022-01-13 12:41:14.000000000 +01:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - 3D Drucker 
    - ESP8266 
    - Füllstandsanzeige 
    - Hassio 
    - Home Assistant 
    - Home Automation 
    - Pelletsensor 
    - Pelletstank 
    - Tinkercad 
    - Ultraschallsensor 
    - Wandgehäuse 
    - WeMosD1 
author: Smon
permalink: "/2022/01/13/pelletsensor/" 
---

Mit der Hilfe eines WEMOS D1 und eines einfachen Radar Sensors kann im Pellet Tank der aktuelle Füllstand gemessen und grafisch dargestellt werden. In meinem Fall verarbeite ich die Werte mittels HomeAssistant (hassio). Im Falle von einer geringen Füllstandsmenge kann als Beispiel eine Benachrichtigung per Telegram eingerichtet werden.

**Benötigte Hardware**
----------------------

*   [WeMos D1](https://www.amazon.de/AZDelivery-ESP8266-ESP-8266EX-Entwicklung-inklusive/dp/B08BTXCZC1/ref=sr_1_2_sspa?__mk_de_DE=ÅMÅŽÕÑ&crid=U8KOMRZ4GI5M&keywords=wemos%2Bd1&qid=1642017050&sprefix=wemos%2Bd1%2Caps%2C62&sr=8-2-spons&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzVlVIOFVOVk43M1lRJmVuY3J5cHRlZElkPUEwMDIxNjgwMVg0SVZUNzc2MVNYNiZlbmNyeXB0ZWRBZElkPUEwNzAwMTYyVlRVQ0xJMEdPQk1CJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1)
*   [Ultraschallsensor Wasserdicht](https://www.amazon.de/Ultraschallwellen-Entfernungs-Messbrett-Ranging-Wasserdichtem/dp/B076SWF5FV/ref=asc_df_B076SWF5FV/?tag=googshopde-21&linkCode=df0&hvadid=207958123109&hvpos=&hvnetw=g&hvrand=2429683880360571440&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9041871&hvtargid=pla-466570717578&psc=1&th=1&psc=1)
*   [3D Drucker](https://affengriff.net/category/3d-prints/)
*   [Abzweigdose für WeMos & Sensorplatine](https://www.bauhaus.info/schalterdosen-deckel/abzweigdose/p/12165125?ef_id=Cj0KCQiAuP-OBhDqARIsAD4XHpel7Nj7fkhUPnZj6r4L9gY6V7y49IDJaOieZ2ryWZfE8c3YVOD92CkaArGrEALw_wcB:G:s&s_kwcid=AL!5677!3!427263744379!!!g!445337366317!&cid=PSEGoo9709541002_99153654693&pla_campid=9709541002&pla_adgrid=99153654693&pla_prpaid=445337366317&pla_prid=12165125&pla_adt=pla&pla_prch=online&pla_stco=&gclid=Cj0KCQiAuP-OBhDqARIsAD4XHpel7Nj7fkhUPnZj6r4L9gY6V7y49IDJaOieZ2ryWZfE8c3YVOD92CkaArGrEALw_wcB)
*   [Einzeladern](https://www.amazon.de/dp/B01EV70C78/ref=redir_mobile_desktop?_encoding=UTF8&aaxitk=462d7dfcfffc725cf01b45f7665d90a4&hsa_cr_id=4181511570302&pd_rd_plhdr=t&pd_rd_r=ff46d288-430f-4a4c-8a4e-fef582ba1cb3&pd_rd_w=bNbO0&pd_rd_wg=zVtOt&ref_=sbx_be_s_sparkle_mcd_asin_0_img)

![]({{ site.baseurl }}/assets/2022/01/IMG_9295.jpg)

**Benötigte Software**
----------------------

*   [TinkerCad](https://www.tinkercad.com)
*   [PrusaSlicer](https://www.prusa3d.de)
*   [Home Assistant](https://www.home-assistant.io/getting-started/)
*   [ESP Home](https://esphome.io)
*   [Ultraschallsensor ESPHome](https://esphome.io/components/sensor/ultrasonic.html)

**3D Gehäuse Tinkercad**
------------------------

*   [Wandgehäuse 35 grad](https://www.tinkercad.com/things/hMeTBKCHtJG)
*   [Wandgehäuse 30 grad](https://www.tinkercad.com/things/kUD2l47NRp0)

**Aufbau** **Pelletsensor**
---------------------------

![]({{ site.baseurl }}/assets/2022/01/IMG_9293.jpg)

![]({{ site.baseurl }}/assets/2022/01/Bildschirmfoto-2022-01-13-um-12.45.56-851x1024.png)

![]({{ site.baseurl }}/assets/2022/01/IMG_9294-768x1024.jpg)

**ESP Home config.yaml**
------------------------

Update Intervall ist hier in der yaml auf 1h eingestellt. Somit wird jede Stunde der Füllstand abgefragt.

    esphome:
      name: ultraschall
      platform: ESP8266
      board: d1_mini
    
    # Enable logging
    logger:
    
    # Enable Home Assistant API
    api:
    
    ota:
      password: „XXX“
    
    wifi:
      networks:
      - ssid: XXX
        password: XXX
      # Enable fallback hotspot (captive portal) in case wifi connection fails
      ap:
        ssid: Ultraschall
        password: XXX
    
    captive_portal:
    
    sensor:
      - platform: ultrasonic
        trigger_pin: D1
        echo_pin: D2
        name: "Pelletsfüllstand"
        update_interval: 1h

**Home Assistant grafische Darstellung**
----------------------------------------

![]({{ site.baseurl }}/assets/2022/01/IMG_9255.jpg)