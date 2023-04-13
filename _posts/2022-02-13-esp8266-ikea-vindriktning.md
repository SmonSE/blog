--- 
layout: post 
title: ESP8266 - IKEA VINDRIKTNING Luftqualitätssensor 
date: 2022-02-13 22:07:06.000000000 +01:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - ESP8266 
    - Ikea 
    - Ikea Vindriktning 
    - Luftqualitätssensor 
    - WeMosD1 
meta: 
  rank_math_seo_score: '90' 
  rank_math_primary_category: '13' 
  _edit_last: '1' 
  _ez-toc-disabled: '' 
  _ez-toc-insert: '' 
  _ez-toc-heading-levels: a:0:{} 
  _ez-toc-alttext: '' 
  _ez-toc-exclude: '' 
  rank_math_internal_links_processed: '1' 
  rank_math_focus_keyword: VINDRIKTNING 
author: Smon
permalink: "/2022/02/13/esp8266-ikea-vindriktning/" 
---

Zu dem Luftqualitätssensor von Ikea (Vindriktning) mittels ESP8266 findet man einige Anleitungen im Netz. Für die Vollständigkeit meiner Dokumentationen, werde ich trotzdem von meinem Umbau des Ikea Vindriktning Sensors berichten.

**Benötigte Hardware**
----------------------

*   [ESP8266](https://www.amazon.de/AZDelivery-ESP8266-ESP-8266EX-Entwicklung-inklusive/dp/B08BTXCZC1/ref=sr_1_2_sspa?__mk_de_DE=ÅMÅŽÕÑ&crid=3P4D055YFXIAG&keywords=Memos%2Bd1%2Bmini&qid=1644400539&sprefix=memos%2Bd1%2Bmini%2Caps%2C82&sr=8-2-spons&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUE3TFNNMVJCU0tDMzQmZW5jcnlwdGVkSWQ9QTAwNTk4ODQxRzc2UkIzQ1NVR002JmVuY3J5cHRlZEFkSWQ9QTA2NzY4MjIxM0tSRFZaTEFVNVdUJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1) ( WeMos D1 )
*   Einzeladern
*   [Ikea Vindriktning](https://www.ikea.com/de/de/p/vindriktning-luftqualitaetssensor-70498242/)
*   Lötkolben + Lötzinn
*   Heißklebepistole

**Benötigte Software**
----------------------

*   [Home Assistant](https://www.home-assistant.io/getting-started/)
*   [ESP Home](https://esphome.io)

**Aufbau**
----------

![WeMos D1]({{ site.baseurl }}/assets/2022/02/IMG_9565-768x1024.jpg)

![Ikea Vindriktning<br />
]({{ site.baseurl }}/assets/2022/02/IMG_9566-768x1024.jpg)

![Ikea VINDRIKTNING]({{ site.baseurl }}/assets/2022/02/IMG_9568-768x1024.jpg)

**Anschlussplan**
-----------------

Bei mir waren die Lötpads am Ikea Sensor bereits verzinnt was das anlöten einfacher macht. Zu beachten ist das der Lötkolben nicht zu heiß eingestellt ist. Ich habe meinen Lötkolben auf ca. 330°c eingestellt. Dadurch sollte ein zu heiß werden und ablösen der Lötpads verhindert werden.

*   WeMos 5V ----> + Vindriktning
*   WeMos GND -> – Vindriktning
*   WeMos D3 ----> REST Vindriktning

Nach dem anlöten der Einzelandern habe ich die WeMos D1 Platine mit einem Streifen Heißkleber im unteren Bereich fixiert. Im falls das man den WeMos erneut flashen muss hat so einen schnellen Zugriff an den USB Port. Rechts habe ich eine kleine Ecke von dem Kunststoff abgeknipst damit das Kabel besser nach oben durchgeführt werden kann.

**ESP Home Sensor config.yaml**
-------------------------------

    esphome:
      name: airquality
    
    esp8266:
      board: d1_mini  
    
    # Example configuration entry
    uart:
      rx_pin: D3
      baud_rate: 9600
    
    sensor:
      - platform: pm1006
        pm_2_5:
          name: "Airquality 2.5µm:"  
    
    # Enable logging
    logger:
    
    # Enable Home Assistant API
    api:
    
    ota:
      password: "xxxxxxxxxxxxxxxxxxxxxxx"
    
    wifi:
      ssid: !secret wifi_ssid
      password: !secret wifi_password
    
      # Enable fallback hotspot (captive portal) in case wifi connection fails
      ap:
        ssid: "Airquality Fallback Hotspot"
        password: "xxxxxxxxxxxxx"
    
    captive_portal:
    

**Home Assistant grafische Darstellung**
----------------------------------------

![]({{ site.baseurl }}/assets/2022/02/Bildschirmfoto-2022-02-13-um-21.44.38-1024x364.png)

    type: gauge
    min: 0
    max: 120
    entity: sensor.airquality_2_5um
    severity:
      green: 0
      yellow: 36
      red: 86
    needle: true

**Fazit zum Sensor**
--------------------

Wenn man sich die Platine genauer anschaut wird Ikea bald eine Variante mit einem Zigbee Modul herausbringen. Im oberen Bereich der Platine sind Anschlüsse für einen Stecker vorgesehen um ein Zigbee Modul anzuschließen.

Was mich leider ein wenig stört, ist der etwas brummende Lüfter des Moduls, der alle 20 Sekunden anfängt zu laufen (für die Messung). Ich bin aktuell am überlegen ob ich den Lüfter an die 3V des WeMos D1 anklemmen soll. Allerdings weiß ich dann nicht ob dadurch das Messergebnis verfälscht wird. Vielleicht fällt mir ja noch eine andere Lösung ein das leichte Brummen des Lüfters zu minimieren.

Mir gefällt die Gaube Darstellung für die Luftqualität in meinem [Smart Mirror](http://elesie.de/2022/01/15/smartmirror-home-assistant/).