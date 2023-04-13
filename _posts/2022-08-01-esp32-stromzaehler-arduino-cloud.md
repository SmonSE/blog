--- 
layout: post 
title: ESP32 - Digitaler Stromzähler auslesen Arduino IoT Cloud Remote 
date: 2022-08-01 15:05:42.000000000 +02:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
  - Blog 
  - Projekte 
tags: 
  - 3D Drucker 
  - Arduino IoT Cloud 
  - Digitaler Stromzaehler 
  - ESP32 
  - IoT 
  - Remote App 
author: Smon
permalink: "/2022/08/01/esp32-stromzaehler-arduino-cloud/" 
---

Mit der Hilfe eines ESP32 und eines einfachen IR Moduls kann am Stromzähler der aktuelle Stromverbrauch ausgelesen werden **ohne** das man den Stromzähler PIN benötigt. Auf der Suche nach einer Alternative zu der doch komplizierten [WebServer Lösung](http://elesie.de/2022-04-08-esp32-stromzahler-auslesen-webserver/) bin ich auf die Arduino IoT Cloud [Seite](https://docs.arduino.cc/arduino-cloud/) gestoßen. Nach kürzester Zeit hatte ich die Daten meines ESP32 + IR Moduls auf dem Handy Bildschirm.

**Arduino IoT Cloud Remote App**
--------------------------------

<!-- ![Arduino IoT Remote App ]({{ site.baseurl }}/assets/2022/08/IMG_0842.jpg) -->
<img src="/assets/2022/08/IMG_0842.jpg" width="70%" height="70%">

Die Werte sind so niedrig da mein Solarkraftwerk zu dem Zeitpunkt Strom generiert hat.

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
*   Arduino IoT Cloud

**Aufbau Gehäuse**
------------------
<!-- ![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-07-um-08.52.16.png)
![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-07-um-08.52.41.png) -->

<p float="left">
    <img src="/assets/2022/08/Bildschirmfoto-2022-04-07-um-08.52.16.png" width="50%" height="50%">
    <img src="/assets/2022/08/Bildschirmfoto-2022-04-07-um-08.52.41.png" width="50%" height="50%">
</p>

Das neue Zählergehäuse findet man [hier](https://www.tinkercad.com/things/gfMDpMCaC8Z).

**IR Modul TCRT5000 modifikation**
----------------------------------

<!-- ![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-01-07-um-21.30.02-610x1024.png) --> 
<img src="/assets/2022/08/Bildschirmfoto-2022-01-07-um-21.30.02-610x1024.png" width="60%" height="60%">

Auf dem TCRT5000 befindet sich eine Lesediode und eine Schreibdiode. Die Schreibdiode wird auch als Näherungssensor verwendet wodurch es zu Problemen kommen kann beim Ablesen des Stromzählers. 

In meinem Fall habe ich die hellere (weiße Diode) einfach ausgelötet. In anderen Beiträgen habe ich gesehen, dass auch nur der Vorwiderstand entfernt wurde. 

Den TCRT5000 habe ich am ESP32 an die Pins G14, 5V und GND angeschlossen.

**eHz Elektronischer Haushaltszähler**
--------------------------------------

<!-- ![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-08-um-12.13.14-659x1024.png) --> 
<img src="/assets/2022/08/Bildschirmfoto-2022-04-08-um-12.13.14-659x1024.png" width="50%" height="50%">

Das Gehäuse aus dem 3D Drucker passt beim eHz Elektronischen Haushaltszähler. Mit zwei Magneten wird das Gehäuse am Zähler befestigt da der Stromzähler Eigentum des Netzbetreibers ist! 

Die rechten beiden IR Dioden werden beim IR Lese-Schreibekopf verwendet ([Volkszähler](https://www.volkszaehler.org)). Dabei wird permanent zwischen den Unterschiedlichen zur verfügung stehenden Zählerständen geschalten und diese ausgelesen (PIN muss beim jeweiligen Netzbetreiber angefragt werden).

**Zähler Aufsteckmodul ESP32 + TCRT5000**
-----------------------------------------

<!-- ![Case open]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.56.57.png)
![ESP32 + TCRT5000]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.57.09.png)
![Case closed]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.57.25.png) -->

<p float="left">
    <img src="/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.56.57.png" width="30%" height="30%">
    <img src="/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.57.09.png" width="30%" height="30%">
    <img src="/assets/2022/08/Bildschirmfoto-2022-04-08-um-07.57.25.png" width="30%" height="30%">
</p>

**Einrichten von Arduino IoT Cloud Remote**
-------------------------------------------

Nachdem man sich bei Arduino Cloud IoT registriert hat, kann man sich ein neues Projekt mit dem Button "Create" anlegen. Anschließend sind die Felder Devices, Network und Variables auszufüllen. Das Setup von Devices und Network ist eigentlich selbsterklärend, daher werde ich nicht genauer auf diese beiden Punkte eingehen.

### **Variables**

Durch drücken des Add Button kann man eine Variable hinzufügen. In unserem Fall lautet die Variable "Strom". Als Datentyp verwenden wir einen integer number mit der permission Read only. Bei send value geben wir on change (threshold: 0) an. Das ganze sollte dann so aussehen.

<!-- ![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-08-01-um-14.41.03-1024x406.png) --> 
<img src="/assets/2022/08/Bildschirmfoto-2022-08-01-um-14.41.03-1024x406.png" width="70%" height="70%">


Die Variable Strom wird im Sketch angegeben und automatisch generiert. Das heißt sie muss im Code nicht extra deklariert werden (kann direkt verwendet werden).

### **Dashboard**

Damit wir später an der Handy App das Diagramm zu sehen bekommen müssen wir unter Dashboards ein Chart hinzufügen mit dem Button "Add". Hier muss man nur noch die Verlinkung zwischen Chart und der Variable Strom herstellen (rechts außen).

<!-- ![]({{ site.baseurl }}/assets/2022/08/Bildschirmfoto-2022-08-01-um-14.44.48.png) -->
<img src="/assets/2022/08/Bildschirmfoto-2022-08-01-um-14.44.48.png" width="70%" height="70%">

**Code (Sketch)**
-----------------

    /* 
      Sketch generated by the Arduino IoT Cloud Thing "Untitled"
      https://create.arduino.cc/cloud/things/xxxx-xxxx-xxxx-xxxx 
    
      Arduino IoT Cloud Variables description
    
      The following variables are automatically generated and updated when changes are made to the Thing
    
      int strom;
    
      Variables which are marked as READ/WRITE in the Cloud Thing will also have functions
      which are called when their values are changed from the Dashboard.
      These functions are generated with the Thing and added at the end of this sketch.
    */
    
    #include "thingProperties.h"
    #include <time.h>
    
    /* Configuration of NTP */
    #define MY_NTP_SERVER "2.europe.pool.ntp.org"
    #define MY_TZ "CET-1CEST,M3.5.0,M10.5.0/3"
    
    /* Globals NTP */
    time_t now;
    tm tm;
    
    /* Sensor TCRT5000 PIN14 */
    const int digital_pin = 14;
    int state = 0;
    int laststate = 1;
    double watt = 0;
    unsigned int counter = 0;
    
    /* Update Time for Chart */
    int period = 60*1000; // 1min
    unsigned long time_now = 0;
    
    /* Reset function after page update */
    void resetFunc(){
      counter = 0;
      watt = 0;
    }
    
    void setup() {
      // Initialize serial and wait for port to open:
      Serial.begin(9600);
      // This delay gives the chance to wait for a Serial Monitor without blocking if none is found
      delay(1500); 
    
      // Defined in thingProperties.h
      initProperties();
    
      // Connect to Arduino IoT Cloud
      ArduinoCloud.begin(ArduinoIoTPreferredConnection);
      
     /*
         The following function allows you to obtain more information
         related to the state of network and IoT Cloud connection and errors
         the higher number the more granular information you’ll get.
         The default is 0 (only errors).
         Maximum is 4
     */
      setDebugMessageLevel(2);
      ArduinoCloud.printDebugInfo();
      
      /* Time NTP */
      configTime(0, 0, MY_NTP_SERVER); 
      setenv("TZ", MY_TZ, 1); // Set your time zone
      tzset();
      
      delay(1000);
    }
    
    void loop() {
      ArduinoCloud.update(); 
    
      // Impulse counter to watt
      state=digitalRead(digital_pin);
      if ( laststate == 1 && state == 0 )
      {
        counter++;
      }
      laststate = state;
    
      watt = counter * 6;
      
      if(millis() >= time_now + period){
        time_now += period;
        strom = watt;
        resetFunc();
        Serial.println(strom);
      }
    }
    
    /*
      Since Strom is READ_WRITE variable, onStromChange() is
      executed every time a new value is received from IoT Cloud.
    */
    void onStromChange()  {
      // Add your code here to act upon Strom change
    }

**Handy IoT Remote App**
------------------------

![]({{ site.baseurl }}/assets/2022/08/IMG_0842.jpg)
<img src="/assets/2022/08/IMG_0842.jpg" width="70%" height="70%">
