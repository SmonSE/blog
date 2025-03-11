---
layout: post
title: GpsPiRacer - Gps-Trainiertes Raspberry Pi Racecar
date: 2025-03-10 16:10:54.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
  - AI
  - DonkeyCar
  - Raspberry Pi 4
  - RC Car
tags:
  - AI
  - GPS
  - KI
  - Neuronal Network
  - Carduino
  - Raspberry Pi 4
  - 3D Printer
  - DonkeyCar
author: Smon
permalink: "/2025/03/10/GpsPiRacer/"
---

<p align="center">
  <img src="/assets/2025/03/gpspiracer_01.png" height="200" />
</p>


## DonkeyCar GPS Line Following: Training für eine perfekte Runde

Das autonome Fahren mit einem DonkeyCar ist bereits eine spannende Herausforderung – doch was passiert, wenn keine festen Fahrbahnmarkierungen oder Linien existieren? Genau hier kommt das GPS Line Following ins Spiel!

In diesem Beitrag zeige ich, wie ich meinen DonkeyCar dazu gebracht habe, eine Strecke anhand von GPS-Koordinaten nachzufahren. Der Trainingsprozess basiert darauf, mehrere Runden zu fahren, die GPS-Daten aufzuzeichnen und anschließend aus diesen Daten eine optimierte Route zu berechnen. Dabei werden die GPS-Koordinaten und Throttle-Werte (Geschwindigkeit) über mehrere Durchläufe gemittelt, um eine möglichst exakte und stabile Fahrspur zu erzeugen.

---

## Grafische Darstellung der GPS Daten
 
Kopiere das File gpsData.csv vom DonkeyCar auf deinen PC. Lege das Skript create_graph.py an und führe das Skript in dem gleichen Ordern auf deinem PC aus.

### File erzeugen create_graph.py

```sh
touch create_graph.py  
```

### Kopiere folgendes Skript nach create_graph.py

```sh
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec

# Load CSV file
df = pd.read_csv("gpsData.csv", sep=",", header=None)
df.columns = ["latitude", "longitude", "throttle"]

# Create figure and define grid layout
fig = plt.figure(figsize=(10, 6))
gs = gridspec.GridSpec(1, 2, width_ratios=[0.6, 0.4])  # Left plot = 0.6, Right plot = 0.4

# Left Plot (Main GPS Path)
ax1 = plt.subplot(gs[0])
ax1.scatter(df["longitude"], df["latitude"], c=df["throttle"], cmap="coolwarm", edgecolors="k", s=50)
ax1.plot(df["longitude"], df["latitude"], linestyle="-", color="gray", alpha=0.7)
ax1.set_xlabel("Longitude")
ax1.set_ylabel("Latitude")
ax1.set_title("GPS Path")
ax1.grid()

# Right Plot (Throttle over Time)
ax2 = plt.subplot(gs[1])
ax2.plot(df.index, df["throttle"], color="red", linestyle="-", marker="o")
ax2.set_xlabel("Time (Index)")
ax2.set_ylabel("Throttle")
ax2.set_title("Throttle over Time")
ax2.grid()

# Adjust subplot layout
plt.subplots_adjust(left=0.3, right=0.95, top=0.9, bottom=0.1, wspace=0.3)  # Left margin at 0.3

# Show the plots
plt.show()
```

### File ausführbar machen
```sh
chmod +x create_graph.py
```

### Skript ausführen
```sh
python3 create_graph.py
```

<p align="center">
  <img src="/assets/2025/03/gpspiracer_02.png" height="200" />
</p>

---