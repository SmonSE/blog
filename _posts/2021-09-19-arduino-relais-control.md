--- 
layout: post 
title: Cheap Arduino Nano v3 - Relais control via GUI 
date: 2021-09-19 20:51:12.000000000 +02:00 
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
    - Arduino 
    - Arduino IDE 
    - Arduino Nano 
    - Arduino Relais 
    - Creality 
    - Ender 3 Pro 
    - Processing IDE 
    - Relais 
    - Schematics 
    - Tinkercad 
meta: 
    rank_math_seo_score: '82' 
    _edit_last: '1' 
    _ez-toc-disabled: '' 
    _ez-toc-insert: '' 
    _ez-toc-heading-levels: a:1:{i:3;i:3;} 
    _ez-toc-alttext: '' 
    _ez-toc-exclude: '' 
    rank_math_internal_links_processed: '1' 
    rank_math_primary_category: '13' 
    rank_math_focus_keyword: Arduino Nano 
    _wp_old_slug: cheap-arduino-nano-v3-relais-control-via-gui 
author: Smon
permalink: "/2021/09/19/arduino-relais-control/" 
---

**Control of a relay module using an Arduino Nano v3**
------------------------------------------------------

Since I have to reset my work hardware or switch the voltage several times a day from home office, I built a small circuit from an old Arduino Nano v3 and a relay module that enables me to do this. Since I found it awkward to start the relay every time via the Arduino IDE with the serial monitor, I also wrote a GUI that can be executed using JAVA JDK for easy handling.

### **Hardware required**

*   Arduino Nano v3
*   Relais Modul
*   RGB LED (not needed)
*   Single Wires
*   Solder stuff
*   [3D Printed case](https://www.tinkercad.com/things/5TZT1Vlvi8K)

### **Schematics**

![Tinkercad circuits Arduino Nano]({{ site.baseurl }}/assets/2021/09/Bildschirmfoto-2021-09-08-um-22.38.10-1024x773.png "Tinkercad circuits")

### **Code**

Code for Arduino IDE which worked with the attached GUI.

    #define LEDR 8     // red
    #define LEDG 9     // green
    #define LEDB 10    // blue
    #define RELAIS 11  // relais
    
    void setup() {
    
      // set LED's pin to output mode
      pinMode(LEDR, OUTPUT);
      pinMode(LEDG, OUTPUT);
      pinMode(LEDB, OUTPUT);
      pinMode(RELAIS, OUTPUT);
      
      Serial.begin(9600);          //start serial communication @9600 bps
    }
    
    void loop(){
      
      if(Serial.available()){      //id data is available to read
    
        char val = Serial.read();
    
        if(val == 'b'){             //if b received
          digitalWrite(11, HIGH);   //turn on relais
          digitalWrite(10, HIGH);   //turn on green led
        }
        if(val == 'f'){            //if f received
          digitalWrite(11, LOW);    //turn off all led
          digitalWrite(10, LOW);
        }      
      }
    }

### **Processing IDE GUI for MacOS**

With Processing IDE it is easy to create a JAVA GUI for Arduino which is running on MAC OS X and Windows. The only think what you have to install additional is the latest JAVA SDK.

![]({{ site.baseurl }}/assets/2021/09/Bildschirmfoto-2021-09-09-um-17.07.28.png)

### **3D Printer Settings**

![]({{ site.baseurl }}/assets/2021/09/IMG_8147.jpg)

**Print Settings**

*   Printer brand: [Creality](https://www.creality3dshop.de)
*   Printer: Ender 3 Pro
*   Infill:100
*   Filament brand: [Amazon Basics 3D-Drucker-Filament](https://www.amazon.de/s?keywords=Filament-3D-Druckmaterialien&i=industrial&rh=n%3A6589299031%2Cp_89%3AAmazon+Basics&dc&_encoding=UTF8&c=ts&qid=1631212845&rnid=669059031&ts_id=6589299031&ref=sr_nr_p_89_1)
*   Filament color: black
*   Filament material: PLA

### **Result**

![]({{ site.baseurl }}/assets/2021/09/IMG_8139-768x1024.jpg)