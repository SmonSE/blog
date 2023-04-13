--- 
layout: post 
title: ESP32 - Digitaler Stromzähler auslesen WebServer 
date: 2022-04-08 12:42:59.000000000 +02:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - Projekte 
tags: 
    - 3D Printer 
    - Digitaler Stromzähler 
    - ESP32 
    - Strom sparen 
    - Stromzähler auslesen 
    - TCRT5000 
    - Tinkercad 
    - WeMosD1 
meta: 
    rank_math_seo_score: '88' 
    rank_math_primary_category: '13' 
    _edit_last: '1' 
    _ez-toc-disabled: '' 
    _ez-toc-insert: '' 
    _ez-toc-heading-levels: a:0:{} 
    _ez-toc-alttext: '' 
    _ez-toc-exclude: '' 
    rank_math_internal_links_processed: '1' 
    rank_math_focus_keyword: ESP32 
    rank_math_contentai_score: a:5:{s:9:"wordCount";s:3:"100";s:9:"linkCount";s:1:"0";  s:12:"headingCount";s:3:"100";s:10:"mediaCount";s:5:"62.22";s:8:"keywords";s:5:"74.51";}   
    rank_math_og_content_image: a:2:{s:5:"check";s:32:"5b1a2014c08effd9ff07277a5091b395";s:6:"images";a:1:{i:0;i:1047;}} 
author: Smon
permalink: "2022-04-08-esp32-stromzahler-auslesen-webserver/"
---

Mit der Hilfe eines ESP32 und eines einfachen IR Moduls kann am Stromzähler der aktuelle Stromverbrauch ausgelesen werden **ohne** das man den Stromzähler PIN benötigt. Mittels einem WebServer der vom ESP32 aufgebaut wird kann der aktuelle Verbrauch ausgelesen und grafisch dargestellt werden.

Da nicht jeder das Know-how bzw. die Lust hat sich einen kompletten HomeAssistant einzurichten um den Energieverbrauch auszulesen habe ich mir eine Standalone Lösung überlegt. Dadurch werden auch die Kosten der Anschaffung stark minimiert. Die Kosten für das benötigte Material belaufen sich auf unter 15€ (je nach Bauteile / Hersteller).

[Alternative Lösung](http://elesie.de/2022/08/01/esp32-stromzaehler-arduino-cloud/) via Arduino IoT Remote App (nur ein File zu flashen).

Wer eine Home Assistant am laufen hat kann sich gerne meine Hassio Lösung anschauen, [hier](http://elesie.de/2021/10/26/esp8266-stromzahler-auslesen/).

**Digitaler Stromzähler Standalone WebServer**
----------------------------------------------

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-08-um-12.38.45-1024x810.png)

**Benötigte Hardware**
----------------------

*   ESP32-WROOM-32 D1 mini (AZ-Delivery)
*   [IR Modul](https://www.amazon.de/AZDelivery-TRCT5000-Infrarot-Hindernis-Vermeidung/dp/B07DRCKV3X/ref=sr_1_2_sspa?__mk_de_DE=ÅMÅŽÕÑ&dchild=1&keywords=TCRT5000&qid=1635199629&sr=8-2-spons&psc=1&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUFQTFpMMFZCM0wyV1AmZW5jcnlwdGVkSWQ9QTAxMDUwNjMxOU1NMlZLVllIUU9FJmVuY3J5cHRlZEFkSWQ9QTA5Njk5MjFKNERJWDlPNUxENk8md2lkZ2V0TmFtZT1zcF9hdGYmYWN0aW9uPWNsaWNrUmVkaXJlY3QmZG9Ob3RMb2dDbGljaz10cnVl) (TCRT5000) 
*   3D Drucker + Filament
*   Lötstation mit Equipment
*   Einzeladern

**Benötigte Software**
----------------------

*   [TinkerCad](https://www.tinkercad.com)
*   [PrusaSlicer](https://www.prusa3d.de)
*   Arduino IDE

**Aufbau Gehäuse**
------------------

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-07-um-08.52.41.png)

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-07-um-08.56.10.png)

Das neue Zählergehäuse findet man [hier](https://www.tinkercad.com/things/gfMDpMCaC8Z).

**IR Modul TCRT5000 modifikation**
----------------------------------

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-01-07-um-21.30.02-610x1024.png)

Auf dem TCRT5000 befindet sich eine Lesediode und eine Schreibdiode. Die Schreibdiode wird auch als Näherungssensor verwendet wodurch es zu Problemen kommen kann beim Ablesen des Stromzählers.

In meinem Fall habe ich die hellere (weiße Diode) einfach ausgelötet. In anderen Beiträgen habe ich gesehen, dass auch nur der Vorwiderstand entfernt wird.

Den TCRT5000 habe ich am ESP32 an die Pins G14, 5V und GND angeschlossen.

**eHz Elektronischer Haushaltszähler**
--------------------------------------

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-08-um-12.13.14-659x1024.png)

Das Gehäuse aus dem 3D Drucker passt beim eHz Elektronischen Haushaltszähler. Mit zwei Magneten wird das Gehäuse am Zähler befestigt da der Stromzähler Eigentum des Netzbetreibers ist!

Die rechten beiden IR Dioden werden beim IR Lese-Schreibekopf verwendet ([Volkszähler](https://www.volkszaehler.org)). Dabei wird permanent zwischen den Unterschiedlichen zur verfügung stehenden Zählerständen geschalten und diese ausgelesen (PIN muss beim jeweiligen Energiekonzern angefragt werden).

**Zähler Aufsteckmodul ESP32 + TCRT5000**
-----------------------------------------

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-08-um-07.56.57-722x1024.png)

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-08-um-07.57.09-713x1024.png)

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-08-um-07.57.25-674x1024.png)

**Einrichten des WebServers**
-----------------------------

Der Wemos D1 mini ESP32 generiert zu Beginn sein eigenes WLAN (SSID = StromZaehler). Wenn man dieses auswählt kann man über die statische IP: 192.168.4.1 den Wi-Fi Manager öffnen um dort von seinem eigene Heimnetz die SSID und das Passwort zu hinterlegen. Das Gateway (Router IP) muss angegeben werden da sonst Zeit und Datum nicht mit dem NTP Server abgeglichen werden können. Bei einem nicht funktionierenden NTP Server werden die Werte um Mitternacht nicht zurückgesetzt. Die Daten sind direkt auf dem ESP32 im Filessystem hinterlegt und können nicht nach außen gelangen.

![]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-06-um-14.06.08-1024x737.png)

SSID: Wlan Name

Password: Wlan Passwort

IP Adresse: WebServer IP

Gateway Adresse: Router IP

**WebServer grafische Darstellung**
-----------------------------------

Den WebServer mit der Sensor Auswertung erreicht man dann über die im Wi-Fi Manager konfigurierten

![WebServer on ESP32<br />
]({{ site.baseurl }}/assets/2022/04/Bildschirmfoto-2022-04-06-um-13.25.18-1024x512.png)

**Stromversorgung ESP32**
-------------------------

Man kann den ESP32 auch mit einer Powerbank betreiben. Dabei muss man aber bedenken das durch den WebServer (permanentes WiFi) der Stromverbrauch sehr hoch ist. Da meine Lösung ja nur zum aktuellen Energieverbrauch messen gedacht ist bzw. um zu schauen was man so über einen kompletten Tag verbraucht, reicht eine Powerbank aus.

Wer die Energiewerte dauerhaft auslesen möchte sollte sich dann doch lieber eine Schuko Steckdose in den Verteilerkasten montieren lassen.

**Software**
------------

Der ESP32 enthält ein File System (SPIFFS). SPIFFS ist ein kleines Dateisystem, das für Mikrocontroller mit einem Flash-Chip entwickelt wurde die über einen SPI-Bus verfügen. Unter [dieser](https://randomnerdtutorials.com/install-esp32-filesystem-uploader-arduino-ide/) [Seite](https://randomnerdtutorials.com/esp32-plot-readings-charts-multiple/) kann man die genaue Anleitung finden und wie man dieses File System einrichtet. Anstelle des Temperatursensoren muss man nur den TCRT5000 anlöten sowie minimale Anpassungen an der Software vornehmen.

**Code**
--------

    /* Sensor TCRT5000 PIN14*/
    const int digital_pin = 14;
    int state = 0;
    int laststate = 1;
    unsigned int counter = 0;
    
    ......
    
    /* Loop sensor reading*/
    void loop() {
    
      state=digitalRead(digital_pin);
      if ( laststate == 1 && state == 0 )
      {
        counter++;
      }
      
      laststate = state;
      watt = counter * 6;
    }

**Fazit**
---------

Mit der Energieverbrauch Messung kann man sehr schnell sehen wo sich die Stromfresser im Haushalt befinden. Wir haben bei der letzten Messung herausgefunden, dass der SAECO Kaffeevollautomat im eingeschalteten Zustand während dem Frühstück 400W zieht. Das liegt an dem permanent aktiven Heizelement. Der Standby Timer lag bei 30 Minuten was zu einem hohen Stromverbrauch in der Zeit führt, dafür das kein Kaffee gemacht wird. Wir haben den Timer auf das minimalsten Wert gesetzt bzw. machen den Vollautomaten direkt nach der Benutzung aus.

Es wird ja immer von Standby Geräten bzw. Netzteilstecker gesprochen die sich permanent am Strom befinden. Jedoch kann ich hier keinen erhöhten Energieverbrauch feststellen.

Was hier am meisten Sinn macht und zu einer erstaunliche Einsparung führt ist das Balkonkraftwerk. Daher sieht man in den Grafiken des öfteren das Der Stromzähler Watt Wert Null beträgt.

**Das Projekt befindet sich noch in der Entwicklung, bei weiteren Fragen einfach schreiben.**