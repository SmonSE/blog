--- 
layout: post 
title: Mycroft AI in a safety Docker Container on a Raspberry Pi 3 
date: 2021-11-03 17:29:10.000000000 +01:00 
type: post 
parent_id: '0' 
published: true 
password: '' 
status: publish 
categories: 
    - Blog 
    - HomeAssistant 
tags: 
    - Docker 
    - HeyMycroft 
    - MyCroft 
    - MycroftAI 
    - NoAlexa 
    - NoHeyGoogle 
    - PiCroft 
    - VoiceAssistant 
meta: 
    rank_math_seo_score: '81' 
    _edit_last: '1' 
    _ez-toc-disabled: '' 
    _ez-toc-insert: '' 
    _ez-toc-heading-levels: a:0:{} 
    _ez-toc-alttext: '' 
    _ez-toc-exclude: '' 
    rank_math_internal_links_processed: '1' 
    rank_math_primary_category: '13' 
    rank_math_focus_keyword: Mycroft 
    rank_math_og_content_image: a:2:{s:5:"check";s:32:"d50cf88c33377e8762ca55d41bc0e079";s:6:"images";a:1:{i:0;i:716;}} 
author: Smon
permalink: "/2021/11/03/mycroft-docker-raspberry/" 
---

I currently have [my home automation](https://elesie.de/category/homeassistant/) running on a Raspberry Pi 4 with Home Assistant (hassio). My idea was to control the whole thing with a voice assistant. Since you can run the Mycroft in a Docker environment, I want to do the whole setup with only one Raspberry Pi. I don't think that the Pi’s resources are sufficient, but it's worth a try. The first attempts with a Raspberry Pi 3 show that it has to struggle with Mycroft via docker.

**Required Hardware:**
----------------------

*   Raspberry Pi 3/4 + standard cables
*   Micro SD card min 8 GB
*   Jack to jack cable
*   Speaker with audio jack
*   USB Microphone
*   HMI cable and Monitor for first setup

**Required Software**
---------------------

*   [Raspberry Pi OS arm64](https://downloads.raspberrypi.org/raspios_arm64/images/)
*   [Docker for Raspberry Pi](https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo)
*   [Mycroft Container](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/get-mycroft/docker)
*   [Github Mycroft Sources](https://github.com/MycroftAI/docker-mycroft)
*   Terminal for ssh connection
*   [Belena Etcher](https://www.balena.io/etcher/)
*   SD Formatter

**Raspberry Pi setup**
----------------------

Most will know how to create a boot image on an SD card. With belena.io, the whole thing works very quickly and is actually self-explanatory. After inserting the SD card and starting Debian, you have to make the standard settings (WiFi setup, language, …).

The next step should be to enable the ssh interface under Debian (Settings -> Interface -> Activate SSH). Now you no longer need the monitor and you can continue to set up everything via the SSH terminal. An SSH terminal connection can be established with the Macbook with the following command:

    sudo ssh pi@192.168.178.xx

Now we are on the Raspberry Pi and can do the first installations. The first thing you should do is bring Debian up to date with the following command:

    sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade

**Docker on Raspberry Pi**
--------------------------

I stuck to this [website](https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo) when installing Docker. Many thanks for the clear instructions.

**Get Mycroft Docker Development Environment**
----------------------------------------------

I tried to get the whole thing to work with the instructions on Github but failed again and again in some places. Ultimately, I found my own way that is similar to the Github guide.

Pull mycroft respository from GitHub

    git clone https://github.com/MycroftAI/docker-mycroft.git

Build the docker image in the directory were you pulled the repository.

    docker build -t mycroft .

Run the following command to start up mycroft:

    docker run -d \
    --name mycroft \
    -v /home/pi/mycroft:/root/.mycroft \
    -v /home/pi/mycroft_skills:/opt/mycroft/skills \
    --device /dev/snd \
    -e PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native \
    -v ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native \
    -v ~/.config/pulse/cookie:/root/.config/pulse/cookie \
    -p 8181:8181 \
    mycroft mycroftai/docker-mycroft:latest

Check if the docker container is up and running by typing:

pi@raspberrypi:~ $ **_docker ps -a_**

If you want to remove the docker container you have to type in:

pi@raspberrypi:~ $ **_docker rm mycroft_**

Your Docker container is now set up and is just waiting to be started. With the command " **sudo** **systemctl** **enable** **docker** ", which is described under " **Docker on Raspberry Pi** ", Docker is started automatically when booting.

The best thing to do after restarting is to check whether the Docker container is already there ( **docker ps -a** ). The command to start mycroft Container is:

    docker start [container_ID]

Enter container:

    docker exec -it mycroft /bin/bash

After Mycroft is running, the device must still be connected to the MycroftAI. To do this, you first have to register at [mycroft.ai](https://mycroft.ai). For the device connection you have to excecute the following command:

    ./startup.sh 

The speaker will let you know what to do ( register on [mycroftAI](https://mycroft.ai) ). A voice tell you to add the device ID. After that it will take a few moment until the terminal gui will appear.

By typing only **./start-mycroft.sh** you will get an info which commands you can use. Now you can execute all the commands you want which are available, for example (simple audio test):

    ./start-mycroft.sh audiotest

With the command **./startup.sh cli** the terminal cli will be opened like in the Picroft Image.

![Mycroft CLI]({{ site.baseurl }}/assets/2021/11/Bildschirmfoto-2021-11-03-um-17.11.21-1024x505.png)

Install nano to edit container file
-----------------------------------

To change a file directly in the terminal you need an Editor. In my case I will use nano. To install vi you have to paste the following commands:

    apt-get update
    apt-get install nano