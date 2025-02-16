---
layout: post
title: AiPiRacer - AI-Trainiertes Raspberry Pi Racecar Bug
date: 2025-02-12 15:10:54.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
  - AI
  - Raspberry Pi
  - RC Car
tags:
  - AI
  - KI
  - Neuronal Network
  - Carduino
  - Raspberry Pi 4
  - 3D Printer
  - DonkeyCar
author: Smon
permalink: "/2025/02/12/AiPiRacer/"
---

<img src="/assets/2025/02/aipiracer_02.png" width="40%" height="40%">

## Einleitung
Der **Ai-Pi-Racer-Bug** ist ein KI-gesteuertes Rennauto, das mit einem Raspberry Pi betrieben wird. In diesem Guide erfährst du, wie du dein Raspberry Pi OS 64 Lite vorbereitest und **DonkeyCar** installierst, um dein AI-Racecar zum Laufen zu bringen.

Weitere Informationen findest du in der offiziellen **DonkeyCar-Dokumentation**: [docs.donkeycar.com](https://docs.donkeycar.com)

---

<img src="/assets/2025/02/aipiracer_01.png" width="40%" height="40%">

## Raspberry Pi OS 64 Lite Flashen & Vorbereiten

1. **Lade Raspberry Pi OS 64 Lite herunter:**  
   [Download Raspberry Pi OS Lite (64-bit)](https://www.raspberrypi.com/software/operating-systems/)

2. **Flashe das Image mit Raspberry Pi Imager:**  
   - Wähle „Raspberry Pi OS Lite (64-bit)“
   - Setze den Hostnamen & SSH-Zugang für die Remote-Steuerung
   - Flashe das Image auf die microSD-Karte

3. **Starte den Raspberry Pi und verbinde dich per SSH:**  
   ```sh
   ssh pi@<DEINE_PI_IP>
   ```

4. **System aktualisieren:**  
   ```sh
   sudo apt update && sudo apt full-upgrade -y
   ```

---

## Installation von DonkeyCar auf Raspberry Pi OS 64 Lite

### Notwendige Pakete installieren
```sh
sudo apt -y install pip git
sudo apt install -y python3-picamera2
sudo apt install -y python3-pyqt5 python3-opengl
sudo apt install -y python3-opencv
sudo apt install -y opencv-data
sudo apt-get install python3-picamera2
sudo apt-get install libcamera-dev libcamera-apps python3-picamera2
```

### Virtuelle Umgebung (VENV) erstellen
```sh
python3 -m virtualenv -p python3 env --system-site-packages
```
Automatische Aktivierung der Umgebung:
```sh
echo "source ~/env/bin/activate" >> ~/.bashrc
source ~/.bashrc
```

### Treiber installieren
```sh
sudo apt install libcap-dev libhdf5-dev libhdf5-serial-dev
```

### DonkeyCar installieren
```sh
pip install donkeycar[pi]
```

### I2C Unterstützung einrichten
```sh
sudo apt-get update
sudo apt-get install i2c-tools
sudo apt-get install python3-smbus
sudo i2cdetect -y 1
```

---

## DonkeyCar-Projekt erstellen
```sh
donkey createcar --path ~/myCar
```

---

## GPIO & Steuerung einrichten

### GPIO-Daemon starten & beim Boot aktivieren
```sh
sudo pigpiod
ps aux | grep pigpiod
sudo apt-get install pigpio python3-pigpio
```

### Tastatureingabe aktivieren
```sh
pip install pynput
```

---

## Stop recording Knopf in `manage.py`
```python
import os
import signal
import subprocess
import pigpio
import donkeycar as dk

# Initialize DonkeyCar
car = dk.vehicle.Vehicle()

# Define the name of the process (e.g., DonkeyCar)
process_name = "manage.py"  # Replace with the actual process name (e.g., "manage.py" for DonkeyCar)

BUTTON_GPIO = 22

# Initialize pigpio
pi = pigpio.pi()
if not pi.connected:
    exit()

# Callback function to stop recording or perform actions
def button_pressed_callback(gpio, level, tick):
    print("Button pressed! Stopping recording...")
    try:
        pid = int(subprocess.check_output(["pgrep", "-f", process_name]).strip())
        print(f"Found process {process_name} with PID {pid}")
        
        # SIGINT = Simulate Ctrl+C 
        os.kill(pid, signal.SIGINT)
        print(f"Sent SIGINT to process {process_name} with PID {pid}")

    except subprocess.CalledProcessError:
        print(f"No running process found with name {process_name}")

# Set up the GPIO pin mode
pi.set_mode(BUTTON_GPIO, pigpio.INPUT)
pi.set_pull_up_down(BUTTON_GPIO, pigpio.PUD_UP)

# Set up the button press detection
pi.callback(BUTTON_GPIO, pigpio.FALLING_EDGE, button_pressed_callback)
```

---

## DonkeyCar starten
```sh
cd ~/myCar
python manage.py drive
```

---

## Fazit
Mit dieser Anleitung ist dein **Ai-Pi-Racer** bereit für die ersten Tests. 🚀 Falls du weitere Anpassungen oder Sensoren hinzufügen möchtest, kannst du die Konfiguration nach Belieben erweitern. Viel Erfolg beim Fahren! 🎉

