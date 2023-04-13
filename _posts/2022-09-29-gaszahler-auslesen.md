---
layout: post
title: Gaszähler G4 RF1 auslesen mit Aqara Sensor
date: 2022-09-29 22:10:54.000000000 +02:00
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
  - Aqara
  - Aqara Sensor
  - Gaszähler
  - IoT
  - Tinkercad
meta:
  rank_math_internal_links_processed: '1'
  rank_math_seo_score: '86'
  rank_math_primary_category: '89'
  rank_math_focus_keyword: Gaszähler
  _edit_last: '1'
  _ez-toc-disabled: ''
  _ez-toc-insert: ''
  _ez-toc-heading-levels: a:0:{}
  _ez-toc-alttext: ''
  _ez-toc-exclude: ''
author: Smon
permalink: "/2022/09/29/gaszahler-auslesen/"
---

Mit der Hilfe eines Aqara Tür und Fenster Sensors ( Reed Kontakt Sensor ) kann am Gaszähler ( G4 RF1) über den Internen Magneten der aktuelle Verbrauch abgelesen werden ( 1 Imp = 0.1m³ ). Eingebunden ist das ganze wie bereits beim&nbsp;<a href="https://affengriff.net/2021/10/26/esp8266-stromzahler-auslesen/" target="_blank" rel="noreferrer noopener">digitalen Stromzähler</a>&nbsp;in Home Assistant ( HASSIO ). Dort kann man sich die Verbrauchswerte für die unterschiedliche Zeiten (Tag, Woche, Monat, Jahr) anzeigen lassen.

## Benötigte Hardware

* <a href="https://www.amazon.de/Aqara-MCCGQ11LM-Window-Sensor-Fensterssensor/dp/B07D37VDM3" target="_blank" rel="noreferrer noopener">Aqara Tür- und Fenstersensor</a>
* <a href="https://affengriff.net/category/3d-prints/" target="_blank" rel="noreferrer noopener">3D Drucker&nbsp;</a>
* Gaszähler G4 RF1
* Zigbee USB Stick (ConBee2 oder SkyConnect)


## Benötigte Software

* <a href="https://www.tinkercad.com" target="_blank" rel="noreferrer noopener">TinkerCad</a>
* <a href="https://www.prusa3d.de" target="_blank" rel="noreferrer noopener">PrusaSlicer</a>
* <a href="https://www.home-assistant.io/getting-started/" target="_blank" rel="noreferrer noopener">Home Assistant</a>

## 3D Gehäuse Tinkercad

* <a href="https://www.tinkercad.com/things/9ECOQKcssap" target="_blank" rel="noreferrer noopener">Gaszähler Aqara G4 RF1</a>
* <a href="https://www.tinkercad.com/things/9uSG6HJZTm5" target="_blank" rel="noreferrer noopener">Gaszähler Aqara Cap</a>

## Aufbau

![Aqara Tür Fenster Sensor]({{ site.baseurl }}/assets/2022/09/IMG_1410-768x1024.jpg)

![3D Printer Case]({{ site.baseurl }}/assets/2022/09/IMG_1411-768x1024.jpg)

![]({{ site.baseurl }}/assets/2022/09/IMG_1412-scaled.jpg)

## Gaszähler Typ G4 RF1

![Gaszähler G4 RF1]({{ site.baseurl }}/assets/2022/09/IMG_1413-768x1024.jpg)

Bei meinem Gaszähler handelt es sich um das Model G4 RF1 von Actaris.

Das Model G4 RF1 von Pipersberg ist baugleich soweit ich weiß. Daher sollte das Case für den Sensor auch hier passen.

Der Magnet befindet sich hinter der ersten roten Zahl nach dem Komma ( 1imp = 0.1m³ ).

## Hassio configuration.yaml

{% highlight yaml %}
{% raw %}
# GASTZÄHLER
counter:
    gaszaehler:
      initial: 0
      step: 1
      icon: mdi:meter-gas-outline
# CAN ALSO BE ADDED TO Automation.yaml
automation:
# COUNTER INCREMENT
- id: '1664298491581'
  alias: increaseCounter
  description: ''
  trigger:
  - type: turned_off
    platform: device
    device_id: c0d6f8ef97c5bffd76e0be955598bcfc
    entity_id: binary_sensor.openclose_8
    domain: binary_sensor
    for:
      hours: 0
      minutes: 0
      seconds: 0
  condition: []
  action:
  - service: counter.increment
    data: {}
    target:
      entity_id: counter.gaszaehler
  mode: single
# COUNTER RESET AT MIDNIGHT
- id: '1664298606458'
  alias: counterReset
  description: ''
  trigger:
  - platform: time
    at: 00:00:00
  condition: []
  action:
  - service: counter.reset
    data: {}
    target:
      entity_id: counter.gaszaehler
  mode: single
# VIRTUAL SENSOR
template:
  - sensor:
      - name: "Gaszaehler_m3"
        unit_of_measurement: "m³"
        device_class: energy
        state_class: total_increasing
        state: >
          {% set gas = states('counter.gaszaehler') | int %}
          {{ ((gas) * 0.1) | round(1, default=0) }}
  - sensor:
      - name: "Gaszaehler_kWh"
        unit_of_measurement: "kWh"
        device_class: energy
        state_class: total_increasing
        state: >
          {% set kwh = states('sensor.gaszaehler_m3') | float %}
          {{ ((kwh) * 10.5) | round(1, default=0) }}
# 10.5 NEEDS TO BE CHECKED AT YOUR GAS INVOICE ( Z-ZAHL )
# 11.404 Laut meiner Rechnung der Stadtwerke!
{% endraw %}
{% endhighlight %}

## Home Assistant grafische Darstellung

![]({{ site.baseurl }}/assets/2022/09/Bildschirmfoto-2022-12-16-um-08.12.06.png)

## Zigbee USB-Gateway Deconz - ConBeeII

Der Aqara Tür und Fenster Sensors ist über Zigbee mit HomeAssistant connected. Dazu muss der Aqara Sensor mit dem USB-Gateway connected werden.

![]({{ site.baseurl }}/assets/2022/09/Bildschirmfoto-2022-12-16-um-08.23.08-1024x455.png)

* Gehen Sie im Abschnitt Geräte auf Sensoren.
* Drücke Sie Neuen Sensor Verbinden.
* Wählen Sie „Andere“ als Anbieter aus
* Sensor-Suche ist jetzt für 3 Minuten aktiv.
* Drücken Sie kurz die kleine Taste am Sensor.
* Bei Problemen, nähern Sie sich dem ConBee II und drücken Sie die Taste erneut.
* Sensor umbenennen.

## Deconz Geräteübersicht

Hier sind alle devices aufgelistet. In meinem Fall hatte ich Probleme eine gute Verbindung im gesamten Haus zu bekommen. Für eine bessere Abdeckung habe ich zwei IKEA TRÅDFRI  Signalverstärker in den Stockwerken verbaut.

![]({{ site.baseurl }}/assets/2022/09/Bildschirmfoto-2022-12-16-um-08.33.35.png)

## Der neue SkyConnect von HomeAssistant ist endlich da!

Da ich einen Raspberry Pi 4 verwende kann ich auf den SkyConnect Matter Stick umsteigen der von HomeAssistant vertrieben wird. Ich habe mir aktuell noch keinen HomeAssistant Yellow bestellt, mal schauen ob ich das noch machen werde. Aber aktuell sind die Lieferzeiten utopisch. Vor allem bekommt man kein Raspberry Pi Compute Modul zu normalen Preisen.

Jetzt muss ich leider alle Zigbee Devices mit dem SkyConnect Stick neu connecten.

![]({{ site.baseurl }}/assets/2022/09/Bildschirmfoto-2022-12-16-um-08.39.48-625x1024.png)
