--- 
layout: post 
title: SmartMirror - Home Assistant 
date: 2022-01-15 22:28:22.000000000 +01:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - Chromium 
    - DIY 
    - HomeAssistant 
    - Kiosk Mode 
    - MirrorOS 
    - RaspberryPi3 
    - SmartMirror 
meta: 
rank_math_seo_score: '86' 
_edit_last: '1' 
_ez-toc-disabled: '' 
_ez-toc-insert: '' 
_ez-toc-heading-levels: a:0:{} 
_ez-toc-alttext: '' 
_ez-toc-exclude: '' 
rank_math_internal_links_processed: '1' 
rank_math_focus_keyword: SmartMirror 
rank_math_primary_category: '13' 
rank_math_og_content_image: a:2:{s:5:"check";s:32:"daea2ef284f777f0f65a3525d1d68d10";s:6:"images";a:1:{i:0;i:826;}} 
author: Smon
permalink: "/2022/01/15/smartmirror-home-assistant/" 
---

vor längerer Zeit habe ich mir einen SmartMirror für das Betriebssystem [MirrorOs](https://glancr.de/mirr-os/) von der Firma Glancr gebaut. Das Einrichten des Betriebssystem ist für jeden Einsteiger in das Thema SmartMirror der auch nicht viel Software Kenntnisse hat genau das richtige. Vor allem die das [Video](https://glancr.de/smart-mirror-selbst-bauen/) und die Tipps für den Bau des Spiegels waren sehr hilfreich. Mit der Zeit ist mir das ganze aber zu langweilig geworden nur Termine, Tankpreise, Wetter, Uhrzeit, etc. anzeigen zu lassen. Da ich im letzten Jahr viel mit HomeAssistant gearbeitet und einige Sensoren im Haus am laufen habe, fand ich die Idee meinen HomeAssistant Lovelace auf dem SmartMirror anzeigen zu lassen ziemlich cool.

**Bau des SmartMirro**r
-----------------------

![SmartMirror Innenleben<br />
]({{ site.baseurl }}/assets/2022/01/IMG_9344.jpg)

*   Raspberry Pi 3
*   Netzteil (sehr schmal)
*   Flachbildschirm oder alter Monitor
*   [USB-A auf Micro-USB-B-Stecker](https://www.amazon.de/AmazonBasics-7T9MV4-Verbindungskabel-Stecker-Micro-USB-B-Stecker/dp/B0711PVX6Z/ref=sr_1_2?__mk_de_DE=ÅMÅŽÕÑ&crid=1I7P3E4HGIDSJ&keywords=usb%2Busb%2Bmicro&qid=1642277670&sprefix=usb%2Busb%2Bmicro%2Caps%2C75&sr=8-2&th=1)
*   [HDMI Kabel](https://www.amazon.de/AmazonBasics-PBH-48914-Hochgeschwindigkeits-HDMI-Kabel-Ethernet-4K-Videowiedergabe-schwarz/dp/B01D5H93FW/ref=sr_1_1_sspa?keywords=hdmi+kabel&qid=1642277637&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExTzhJQkJRMzVVS1JKJmVuY3J5cHRlZElkPUEwOTczNTUyM0hLUVgwMjBUSkRXRyZlbmNyeXB0ZWRBZElkPUEwMTY5Mjg0M0ZSUjZOTjA5RTNMMSZ3aWRnZXROYW1lPXNwX2F0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU=)
*   [Leinwand Keilrahmen (nach Maß)](https://framago.de/leinwandrahmen)
*   [Fixmaß Dünn-MDF Platte schwarz](https://www.hornbach.de/shop/Fixmass-Duenn-MDF-Platte-einseitig-schwarz-1200x600x3-mm/6617871/artikel.html)
*   [Magnetleisten für Spiegelglas](https://www.amazon.de/dp/B08F54YGC9/ref=redir_mobile_desktop?_encoding=UTF8&aaxitk=c1e42be31b2b30dbfcfb223bf5c115ce&hsa_cr_id=2697979200102&pd_rd_plhdr=t&pd_rd_r=b4e9ae5a-4af5-4c15-aa32-3e195d64c9cc&pd_rd_w=mRG3x&pd_rd_wg=gOclq&ref_=sbx_be_s_sparkle_mcd_asin_2_img)
*   [Stahlblättchen für an Spiegelglas](https://www.amazon.de/Halterungsset-Klebe-Magnete-Stahl-Plättchen-Klebestücke-Fernbedienungen/dp/B08P53RWZD/ref=sr_1_8?__mk_de_DE=ÅMÅŽÕÑ&crid=2446RYQHNM9QZ&keywords=stahlplättchen&qid=1642277530&s=diy&sprefix=stahlblättchen%2Cdiy%2C71&sr=1-8)
*   Holzleisten
*   Schrauben
*   [Spionspiegel auf Maß bestellt](https://www.glas-star.de/collections/mirropane-chrome-spy)

![SmartMirror fertig]({{ site.baseurl }}/assets/2022/01/IMG_9349.jpg)

So sieht mein fertiger SmarMirror aus. Ich habe mir damals eine Steckdose für hinter den Spiegel installieren lassen die über den unteren rechten Schalter geschalten wird.

Der Spionspiegel ist mit Magneten im Rahmen befestigt wodurch ich ihn jederzeit herausnehmen kann. Das Spionspiegelglas sollte auf der Rückseite mit schwarzer Folie abgelebt werden bis auf den Teil wo der Monitor sitzt. Ansonsten kann es sein das man ein wenig durchsehen kann.

**Bildergalerie**
-----------------

![]({{ site.baseurl }}/assets/2022/01/IMG_9343.jpg)

![]({{ site.baseurl }}/assets/2022/01/IMG_9345.jpg)

![]({{ site.baseurl }}/assets/2022/01/IMG_9347.jpg)

**Benötigte Software**
----------------------

*   [Raspberry Pi OS](https://www.raspberrypi.com/software/) für HomeAssitant
*   oder [Mirror OS](http://glancr.de/mirr-os/) von Glancr

**Raspberry Pi OS Anpassung**
-----------------------------

Damit der Raspberry Pi mit der gewünschten HTML aufstartet benötigt man den Chromium Kiosk Modus. Da wir die Einstellungen unter Raspberry Pi OS vornehmen ist Chromium bereits vorinstalliert. Dadurch benötigen wir nur noch die Software Unclutter um den Mauszeiger ausblenden zu können.

    sudo apt-get update 
    sudo apt-get install unclutter

Als nächstes richten wir den Autostart von Chromium ein. Hierfür öffnen wir folgende Datei:

    /home/pi/.config/lxsession/LXDE-pi/autostart
    
    oder
    
    sudo nano /etc/xdg/lxsession/LXDE-pi/autostart

Hier muss jetzt folgendes stehen:

    # Mauszeiger Ausblenden
    @unclutter
    
    # Bildschirmschoner deaktivieren
    #@xscreensaver -no-splash  
    @xset s off
    @xset -dpms
    @xset s noblank
    
    # Startet im Vollbild und im Inkognito Modus, ohne Adressleiste und Knöpfe und ohne die Möglichkeit den Vollbildmodus mit F11 zu deaktivieren 
    @chromium-browser --incognito --disable-notifications --kiosk http://homeassistant.local:8123/lovelace/0
    

**Hassio Anpassung an SmartMirror**
-----------------------------------

Nach dem wiederholten Scheitern von Chrome, die [Anmeldedaten für Home Assistant](http://elesie.de/2021/11/10/home-assistant-station/) dauerhaft zu schreiben, bin ich auf eine Seite gestoßen, auf der steht, dass dies gewollt ist. Abhilfe schafft nur, die IP-Adresse des Raspberry Pi als vertrauenswürdiges Netzwerk unter /config/configuration.yaml anzugeben. Hier ist ein Codeausschnitt meiner configuration.yaml:

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
    

**Home Assistant Lovelace**
---------------------------

![]({{ site.baseurl }}/assets/2022/01/Bildschirmfoto-2022-01-15-um-22.21.51-1-668x1024.png)

Die Darstellung der Benutzeroberfläche kann mittels individuellem [Hassio Lovelace](https://www.home-assistant.io/lovelace/) angepasst werden.

Hier können alle möglichen und zur verfügung stehenden Entitäten angezeigt werden.