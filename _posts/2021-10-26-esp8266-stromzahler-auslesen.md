--- 
layout: post 
title: ESP8266 - Digitaler Stromzähler auslesen 
date: 2021-10-26 00:13:08.000000000 +02:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - Digitaler Stromzähler 
    - ESP8266 
    - Hassio 
    - Home Assitant 
    - IR Diode 
    - nodemcuv2 
    - Stromzähler IR Diode 
    - TCRT5000 
author: Smon
permalink: "/2021/10/26/esp8266-stromzahler-auslesen/" 
---

Mit der Hilfe eines ESP8266 und eines einfachen IR Modules kann am Stromzähler der aktuelle Stromverbrauch ausgelesen werden **ohne** das man den Stromzähler PIN benötigt. Die ausgelesenen Werte können dann mittels Home Assistant HASSIO grafisch dargestellt werden.

Das ganze ist entstanden aufgrund meines [Balkonkraftwerkes](https://www.hello-yuma.de/produkt/yuma-balcony-eco-l1/). Ich wollte einfach wissen was ich aktuell einspeise und was ich verbrauche. Bei einem Balkonkraftwerk sollte man vor dem Kauf wissen welchen Grundverbrauch man hat damit man ermitteln kann ob es sich überhaupt lohnt ein doppeltes Solarmodul mit 600W zu kaufen oder doch lieber ein einzelnes mit 300W. Aktuell habe ich ein 300W Solarmodul am EG Stromzähler und ein doppeltes Modul mit 600W am OG Stromzähler hängen. Dadurch ist der Grundbedarf gedeckt und die Solarmodule sollten sich nach ca. 4-5 Jahren amortisiert haben.

[Hier](https://elesie.de/2022/04/08/esp32-stromzahler-auslesen-webserver/) geht es zu meiner Standalone Lösung **ohne Home Assistant**.

**Digitaler Stromzähler**
=========================

**Benötigte Hardware**
----------------------

*   [ESP8266](https://www.amazon.de/AZDelivery-NodeMCU-ESP8266-ESP-12E-Development/dp/B074Q2WM1Y/ref=sr_1_4?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=ESP8266&qid=1635199669&qsid=262-3631401-7484166&sr=8-4&sres=B0754N794H%2CB074Q2WM1Y%2CB0754HWZSQ%2CB07Z68HYW1%2CB074RL7YR3%2CB08BTH77F3%2CB07V4M3SGT%2CB07Y8JZNCQ%2CB099YMHSF6%2CB07Q17XJ36%2CB08JQKCRL7%2CB088LV6ZBG%2CB082TNGSRN%2CB07WR96QDG%2CB099WR5X5P%2CB083QHJW21) (nodemcuv2)
*   [IR Modul](https://www.amazon.de/AZDelivery-TRCT5000-Infrarot-Hindernis-Vermeidung/dp/B07DRCKV3X/ref=sr_1_2_sspa?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=TCRT5000&qid=1635199629&sr=8-2-spons&psc=1&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUFQTFpMMFZCM0wyV1AmZW5jcnlwdGVkSWQ9QTAxMDUwNjMxOU1NMlZLVllIUU9FJmVuY3J5cHRlZEFkSWQ9QTA5Njk5MjFKNERJWDlPNUxENk8md2lkZ2V0TmFtZT1zcF9hdGYmYWN0aW9uPWNsaWNrUmVkaXJlY3QmZG9Ob3RMb2dDbGljaz10cnVl) (TCRT5000)
*   Einzeladern
*   [3D Drucker](http://elesie.de/2022/03/21/3d-prints-tinkercad/)
*   Stromzähler eHz

**Benötigte Software**
----------------------

*   [TinkerCad](https://www.tinkercad.com)
*   [PrusaSlicer](https://www.prusa3d.de)
*   [Home Assistant](https://www.home-assistant.io/getting-started/)
*   [ESP Home](https://esphome.io)

**3D Gehäuse Tinkercad**
------------------------

*   [Stromzähler EHZ](https://www.tinkercad.com/things/bA5jzcPJsMT) (einzelne IR Diode nur lesen)
*   [Neodym Magnete](https://www.amazon.de/dp/B07DPV2KVF/ref=cm_sw_r_cp_api_glt_i_VJFA6XER7STF33S4G6HA?_encoding=UTF8&psc=1) (20x10x1 mm)

**Aufbau**
----------

![]({{ site.baseurl }}/assets/2021/10/IMG_8663.jpg)

![]({{ site.baseurl }}/assets/2021/10/IMG_8664-1.jpg)

![]({{ site.baseurl }}/assets/2021/10/IMG_8665-1.jpg)

**IR Modul TCRT5000 modifikation**
----------------------------------

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2022-01-07-um-21.30.02-610x1024.png)

Auf dem TCRT5000 befindet sich eine Lesediode und eine Schreibdiode. Die Schreibdiode wird auch als Näherungssensor verwendet wodurch es zu Problemen kommen kann beim Ablesen des Stromzählers.

In meinem Fall habe ich die hellere (weiße Diode) einfach ausgelötet. In anderen Beiträgen habe ich gesehen das auch nur der Vorwiderstand entfernt wird.

Den TCRT5000 habe ich am Nodemcuv2 an den Pins D5, 3V3 und GND angeschlossen die direkt nebeneinander liegen.

**eHz Elektronischer Haushaltszähler**
--------------------------------------

![eHz Digitaler Stromzähler (ESP8266)]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-25-um-23.31.46.png)

Das Gehäuse aus dem 3D Drucker passt beim eHz Elektronischen Haushaltszähler. Mit zwei Magneten wird das Gehäuse am Zähler befestigt da der Stromzähler Eigentum des Netzbetreibers ist!

Die rechten beiden IR Dioden werden beim IR Lese-Schreibekopf verwendet ([Volkszähler](https://www.volkszaehler.org)). Dabei wird permanent zwischen den Unterschiedlichen zur verfügung stehenden Zählerständen geschalten und diese ausgelesen (PIN muss beim jeweiligen Energiekonzern angefragt werden).

In unserem Fall benutzen wir die mittlere IR-Diode zum auslesen des aktuellen Stromverbrauchs am Zähler. Zum testen einfach den IR Sensor mit der Hand an die zu testende IR-Diode halten und in den ESP-Home live loggs schauen ob sich der Wert verändert. Ich hatte noch einen Baustrahler im Keller angeschlossen um zu schauen ob die Werte plausibel sind.

**Ein Sensor am ESP8266**
=========================

**ESP Home ein Sensor config.yaml**
-----------------------------------

Update Intervall ist hier in der yaml auf 60s eingestellt. Alle 60s sollte sich der aktuelle Stromverbrauch ändern.

    esphome:
      name: strom
      platform: ESP8266
      board: nodemcuv2
      
    # Sensor configuration
    sensor:
      - platform: pulse_counter
        pin: D5
        name: "Energieverbrauch"
        unit_of_measurement: 'W'
        update_interval: 60s
        filters:
          - multiply: 6
        accuracy_decimals: 0
        icon: 'mdi:flash'
        id: energieverbrauch_w
        
      - platform: total_daily_energy
         name: "Tagesverbrauch"
         power_id: energieverbrauch_w
    
    time:
      - platform: sntp
        id: my_time
    
    # Enable logging
    logger:
    
    # Enable Home Assistant API
    api:
    
    ota:
      password: „xxxxxxxxxx“
    
    wifi:
      ssid: !secret wifi_ssid
      password: !secret wifi_password
    
    captive_portal:

**Home Assistant grafische Darstellung**
----------------------------------------

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-25-um-23.48.17.png)

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-25-um-23.48.35.png)

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2021-10-25-um-23.49.18-1.png)

**Spannungsversorgung Verteilerkasten**
---------------------------------------

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2022-01-18-um-11.17.13-1-658x1024.png)

Aktuell habe ich im Haus an jedem meiner Zähler einen Sensor mit Magnet befestigt. Die Sensoren sind an einem ESP8266 nodemcuv2 angeschlossen (Spannung, Masse, D5 und D6).

Die [Zählerschranksteckdose](https://www.amazon.de/Wago-SCHALTSCHRANKSTECKDOSE-709-581/dp/B07DTHKJYP/ref=sr_1_12?__mk_de_DE=ÅMÅŽÕÑ&crid=1WOZVDGGSAS3W&keywords=Zählerschrank+steckdose&qid=1642083581&sprefix=zählerschrank+steckdose%2Caps%2C76&sr=8-12) die den Nodemcuv2 betreibt habe ich mir extra dafür eingebaut

**ESP Home Logs**
-----------------

    INFO Reading configuration /config/esphome/strom.yaml...
    INFO Detected timezone 'CET' with UTC offset 1 and daylight saving time from 27 March 02:00:00 to 30 October 03:00:00
    INFO Starting log output from strom.local using esphome API
    INFO Successfully connected to strom.local
    [19:32:31][I][app:102]: ESPHome version 2021.9.3 compiled on Oct 10 2021, 21:35:03
    [19:32:31][C][wifi:501]: WiFi:
    [19:32:31][C][wifi:361]:   SSID: 'xxxxxxxxxxxxxxx'[redacted]
    [19:32:31][C][wifi:362]:   IP Address: xxx.xxx.xxx.xx
    [19:32:31][C][wifi:364]:   BSSID: xx:xx:xx:xx:xx:xx[redacted]
    [19:32:31][C][wifi:365]:   Hostname: 'strom'
    [19:32:31][C][wifi:369]:   Signal strength: -51 dB ▂▄▆█
    [19:32:31][C][wifi:373]:   Channel: 6
    [19:32:31][C][wifi:374]:   Subnet: 255.255.255.0
    [19:32:31][C][wifi:375]:   Gateway: xxx.xxx.xxx.x
    [19:32:31][C][wifi:376]:   DNS1: xxx.xxx.xxx.x
    [19:32:31][C][wifi:377]:   DNS2: (IP unset)
    [19:32:31][C][logger:193]: Logger:
    [19:32:31][C][logger:194]:   Level: DEBUG
    [19:32:31][C][logger:195]:   Log Baud Rate: 115200
    [19:32:31][C][logger:196]:   Hardware UART: UART0
    [19:32:31][C][pulse_counter:147]: Pulse Counter 'Energieverbrauch'
    [19:32:31][C][pulse_counter:147]:   State Class: 'measurement'
    [19:32:31][C][pulse_counter:147]:   Unit of Measurement: 'W'
    [19:32:31][C][pulse_counter:147]:   Accuracy Decimals: 0
    [19:32:31][C][pulse_counter:147]:   Icon: 'mdi:flash'
    [19:32:31][C][pulse_counter:148]:   Pin: GPIO14 (Mode: INPUT)
    [19:32:31][C][pulse_counter:149]:   Rising Edge: INCREMENT
    [19:32:31][C][pulse_counter:150]:   Falling Edge: DISABLE
    [19:32:31][C][pulse_counter:151]:   Filtering pulses shorter than 13 µs
    [19:32:31][C][pulse_counter:152]:   Update Interval: 60.0s
    [19:32:31][C][total_daily_energy:024]: Total Daily Energy 'Tagesverbrauch'
    [19:32:31][C][total_daily_energy:024]:   Device Class: 'energy'
    [19:32:31][C][total_daily_energy:024]:   State Class: 'total_increasing'
    [19:32:31][C][total_daily_energy:024]:   Unit of Measurement: 'Wh'
    [19:32:31][C][total_daily_energy:024]:   Accuracy Decimals: 2
    [19:32:31][C][total_daily_energy:024]:   Icon: 'mdi:flash'
    [19:32:31][C][captive_portal:148]: Captive Portal:
    [19:32:31][C][ota:029]: Over-The-Air Updates:
    [19:32:31][C][ota:030]:   Address: strom.local:8266
    [19:32:31][C][ota:032]:   Using Password.
    [19:32:31][C][api:135]: API Server:
    [19:32:31][C][api:136]:   Address: strom.local:6053
    [19:32:31][C][api:140]:   Using noise encryption: NO
    [19:32:31][C][sntp:044]: SNTP Time:
    [19:32:31][C][sntp:045]:   Server 1: '0.pool.ntp.org'
    [19:32:31][C][sntp:046]:   Server 2: '1.pool.ntp.org'
    [19:32:31][C][sntp:047]:   Server 3: '2.pool.ntp.org'
    [19:32:31][C][sntp:048]:   Timezone: 'CET-1CEST-2,M3.4.0/2,M10.5.0/3'
    [19:32:32][D][pulse_counter:159]: 'Energieverbrauch': Retrieved counter: 68.00 pulses/min
    [19:32:32][D][sensor:121]: 'Energieverbrauch': Sending state 408.00000 W with 0 decimals of accuracy
    [19:32:32][D][sensor:121]: 'Tagesverbrauch': Sending state 6002.70361 Wh with 2 decimals of accuracy
    [19:32:32][D][pulse_counter:159]: 'Energieverbrauch': Retrieved counter: 68.00 pulses/min
    [19:32:32][D][sensor:121]: 'Energieverbrauch': Sending state 408.00000 W with 0 decimals of accuracy
    [19:32:32][D][sensor:121]: 'Tagesverbrauch': Sending state 6002.70361 Wh with 2 decimals of accuracy
    [19:33:32][D][pulse_counter:159]: 'Energieverbrauch': Retrieved counter: 67.00 pulses/min
    [19:33:32][D][sensor:121]: 'Energieverbrauch': Sending state 402.00000 W with 0 decimals of accuracy
    [19:33:32][D][sensor:121]: 'Tagesverbrauch': Sending state 6009.40381 Wh with 2 decimals of accuracy
    

**Zwei Sensoren an einem ESP8266**
==================================

![]({{ site.baseurl }}/assets/2021/10/Bildschirmfoto-2022-01-18-um-11.16.44-925x1024.png)

Natürlich kann man auch zwei Stromzähler mit einem ESP8266 betreiben.

An den grün markierten Stellen haben ich die Jumper wire eingesteckt.

**ESP Home zwei Sensoren config.yaml**
--------------------------------------

    esphome:
      name: strom
      platform: ESP8266
      board: nodemcuv2
      
    # Stromzähler OG
    sensor:
      - platform: pulse_counter
        pin: D5
        name: "Energieverbrauch_OG"
        unit_of_measurement: 'W'
        update_interval: 60s
        filters:
        # RL = 10000 Imp./kWh 
          - multiply: 6    
        accuracy_decimals: 0
        icon: 'mdi:flash'
        id: energieverbrauch_og
        
      # Dieser Sensor errechnet die Wh aus den aktuellen Verbrauchswerten in W  
      - platform: total_daily_energy
        name: "Tagesverbrauch_OG"
        power_id: energieverbrauch_og
    
    # Stromzähler EG    
      - platform: pulse_counter
        pin: D6
        name: "Energieverbrauch_EG"
        unit_of_measurement: 'W'
        update_interval: 60s
        filters:
        # RL = 10000 Imp./kWh 
          - multiply: 6
        accuracy_decimals: 0
        icon: 'mdi:flash'
        id: energieverbrauch_eg
        
      # Dieser Sensor errechnet die Wh aus den aktuellen Verbrauchswerten in W  
      - platform: total_daily_energy
        name: "Tagesverbrauch_EG"
        power_id: energieverbrauch_eg    
        
    time:
      - platform: sntp
        id: my_time
    
    # Enable logging
    logger:
    
    # Enable Home Assistant API
    api:
    
    ota:
      password: "xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    
    wifi:
      ssid: !secret wifi_ssid
      password: !secret wifi_password
    
    captive_portal:

**ESP Home Logs** zwei Sensoren
-------------------------------

    [11:22:17][D][pulse_counter:159]: 'Energieverbrauch_EG': Retrieved counter: 2.00 pulses/min
    [11:22:17][D][sensor:121]: 'Energieverbrauch_EG': Sending state 12.00000 W with 0 decimals of accuracy
    [11:22:17][D][sensor:121]: 'Tagesverbrauch_EG': Sending state 146.35190 Wh with 2 decimals of accuracy
    
    [11:22:17][D][pulse_counter:159]: 'Energieverbrauch_OG': Retrieved counter: 96.00 pulses/min
    [11:22:17][D][sensor:121]: 'Energieverbrauch_OG': Sending state 576.00000 W with 0 decimals of accuracy
    [11:22:17][D][sensor:121]: 'Tagesverbrauch_OG': Sending state 516.67249 Wh with 2 decimals of accuracy
    
    [11:23:17][D][pulse_counter:159]: 'Energieverbrauch_EG': Retrieved counter: 0.00 pulses/min
    [11:23:17][D][sensor:121]: 'Energieverbrauch_EG': Sending state 0.00000 W with 0 decimals of accuracy
    [11:23:17][D][sensor:121]: 'Tagesverbrauch_EG': Sending state 146.35190 Wh with 2 decimals of accuracy
    
    [11:23:17][D][pulse_counter:159]: 'Energieverbrauch_OG': Retrieved counter: 98.00 pulses/min
    [11:23:17][D][sensor:121]: 'Energieverbrauch_OG': Sending state 588.00000 W with 0 decimals of accuracy
    [11:23:17][D][sensor:121]: 'Tagesverbrauch_OG': Sending state 526.47168 Wh with 2 decimals of accuracy
    

Wie man dem Log entnehmen kann ist im unteren Abschnitt bei EG 0.00 Pulses zu sehen, bitte nicht verwundern das liegt an meinem Balkonkraftwerk was gerade mehr Leistung generiert als verbraucht wird.

**Wemos D1 mini**
-----------------

Das ganze kann auch mit einem Wemos D1 mini realisiert werden. Der einzige Unterschied besteht darin das die Software unter ESPHome richtig kompiliert werden muss. Dazu muss die Board Version geändert werden. Wenn ihr den Sensor am Board angeschlossen habt könnt ihr euren Aufbau mittels einer einfachen Taschenlampe testen. Einfach mit der Taschenlampe über die Diode am TCRT fahren. Dabei sollte die LED am TCRT blinken. In den Logs solltet dann auch der Counter ansteigen.

    ......
    
    esp8266:
      board: d1_mini
    
    ......

**Bei weiteren Fragen einfach schreiben.**