--- 
layout: post 
title: IKEA TRÅDFRI Shortcut Button - Telegram Alarm 
date: 2021-05-05 23:41:39.000000000 +02:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - Home Automation 
    - IKEA TRADFRI 
    - Telegram Bot 
meta: 
    rank_math_seo_score: '85' 
    _edit_last: '1' 
    rank_math_internal_links_processed: '1' 
    rank_math_primary_category: '13' 
    rank_math_focus_keyword: IKEA TRÅDFRI 
    _ez-toc-disabled: '' 
    _ez-toc-insert: '' 
    _ez-toc-heading-levels: a:0:{} 
    _ez-toc-alttext: '' 
    _ez-toc-exclude: '' 
author: Smon
permalink: "/2021/05/05/ikea-tradfri-telegram/" 
---

**Home Assistant Einstellungen zum senden einer Telegram Nachricht durch drücken des Ikea Shortcut Knopfes**
------------------------------------------------------------------------------------------------------------

Mit der IKEA Tradfri Serie hat es das schwedische Unternehmen in den Smart Home-Markt geschafft. Damit ist es für jedermann auf einfachste Weise möglich Lampen, Steckdose, Rollos oder Lautsprecher via App zu steuern oder sogar zu automatisieren. Laut Ikea ist die aktuellen Serie auch noch nicht am Ende seiner Entwicklung und wird stetig weiterentwickelt. Das Home Smart System soll im kommenden Jahr noch durch weitere Produkte ergänzt werden.

### **Was benötige ich alles für den Shortcut Button Telegram Alarm**

*   [IKEA TRÅDFRI Gateway](https://www.ikea.com/de/de/p/tradfri-gateway-weiss-40337806/)
*   [IKEA TRÅDFRI Shortcut-Button](https://www.ikea.com/de/de/p/tradfri-shortcut-button-weiss-40356381/)
*   Raspberry Pi 4 mit Zubehör
*   [Home Assistant Operating System](https://www.home-assistant.io/installation/raspberrypi)

### **Raspberry Pi einrichten**

Wie man die SD Karte mit dem Home Assistant Operating System flashed findet man [hier](https://www.home-assistant.io/installation/raspberrypi).  
Nachdem die SD Karte geflashed, eingesteckt und der Raspberry Pi angeschaltet ist sollte die [Weboberfläche](http://homeassistant.local:8123) des Home Assistant nach wenigen Minuten erreichbar sein. Das Einrichten und eine kurze Einleitung ist [hier](https://www.home-assistant.io/getting-started/onboarding/) zu finden.

### **IKEA TRÅDFRI und Shortcut-Button einrichten**

Das einrichten des IKEA Gateways und Shortcut-Buttons findet man im Netz und auf der [Ikea Homepage](https://www.ikea.com/at/de/customer-service/product-support/smart-lighting/app_gateway-pub8ba3ca96) zu genüge dadurch werde ich nicht genauer darauf eingehen. Falls Nachfragen bestehen dann einfach in die Kommentare schreiben.

### **Einrichten von Telegram unter Home Assistant**

Um den Telegram Messenger in den Home Assistant einzubinden muss ein File auf dem Raspberry Pi angepasst werden. Das Anpassen des Files ist deutlich einfacher wenn man das [Add-On](https://www.home-assistant.io/getting-started/configuration/) "File Editor" installiert. Anschließend muss im "/config" Ordner die "configuration.yaml" Datei angepasst werden. Nach dem Anpassen einfach Speichern drücken. Wie man an den [API\_KEY](https://sendpulse.com/knowledge-base/chatbot/create-telegram-chatbot) und die eigene Chat ID kommt findet man hier.

    # Beispiel configuration.yaml für den Telegram Bot
    telegram_bot:
      - platform: polling 
        api_key: INSERTKEYHERE
        allowed_chat_ids: 
          - FIRSTCHATID 
          - SECONDCHATID 
    
    # Beispiel configuration.yaml für den zu Benachrichtigten
    notify:
       - name: SE_Message
         platform: telegram
         chat_id: FIRSTCHATID
    
    # Wenn benötigt für den zweiten zu Benachrichtigten
       - name: ME_Message
         platform: telegram
         chat_id: SECONDCHATID 

### **Home Assistant Automatisierung**

Unter Einstellungen -> Automatisierungen kann mittels "+ Automatisierung hinzufügen" eine neue Automation hinzugefügt werden. Einfach auf "Erstellen" und beim nächsten Fenster auf "Überspringen" drücken. Die einstellungen die ich Vorgenommen habe sind auf den nachfolgenden Bildern am besten zu sehen.

![IKEA TRÅDFRI Setup<br />
]({{ site.baseurl }}/assets/2021/05/Bildschirmfoto-2021-05-05-um-23.03.26-1024x675.png)

![IKEA TRÅDFRI Setup]({{ site.baseurl }}/assets/2021/05/Bildschirmfoto-2021-05-05-um-23.03.57-1024x912.png)

![IKEA TRÅDFRI Setup]({{ site.baseurl }}/assets/2021/05/Bildschirmfoto-2021-05-05-um-23.04.31-1024x919.png)

Jetzt nur noch speichern und nach dem drücken sollten die ersten Nachrichten aufm dem Handy eintreffen.

Nachträglich eine Anleitung zu schreiben ist immer schwer weil man meistens doch noch etwas vergisst zu notieren. Im falle von Fehlenden Informationen einfach eine Nachricht schreiben, ich versuche sie dann so schnell es geht zu beantworten. Vielleicht finde ich noch eine Möglichkeit einer Home Assistant [Widget](http://elesie.de/2021/02/11/scriptable-widgets/) Darstellung via Scriptable.