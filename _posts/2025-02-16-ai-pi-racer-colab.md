---
layout: post
title: AiPiRacer - Ai-Pi-Racer mit Google Colab trainieren
date: 2025-02-16 15:10:54.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
  - AI
  - Raspberry Pi
  - RC Car
  - google colab
tags:
  - AI
  - KI
  - Neuronal Network
  - Carduino
  - Raspberry Pi 4
  - 3D Printer
  - DonkeyCar
  - google colab
author: Smon
permalink: "/2025/02/16/AiPiRacerColab/"
---

## Ai-Pi-Racer mit Google Colab trainieren

# Einleitung

Der **Ai-Pi-Racer** nutzt ein **neuronales Netz**, um selbstständig auf einer Rennstrecke zu fahren. Damit das Fahrzeug lernen kann, sich effizient zu bewegen, muss es zunächst mit Trainingsdaten versorgt und anschließend ein Modell trainiert werden.

Hier kommt **Google Colab** ins Spiel: Eine cloudbasierte Plattform, die es ermöglicht, Python-Code direkt im Browser auszuführen. Da das Training eines neuronalen Netzes viel Rechenleistung erfordert, bietet Colab eine ideale Umgebung, um diesen Prozess zu beschleunigen – insbesondere durch die Nutzung von kostenlosen **GPUs** und **TPUs**.

Mit **Google Colab** kannst du:

✅ Große Datensätze verarbeiten und dein Modell trainieren, ohne lokale Ressourcen zu belasten.  
✅ Direkt mit TensorFlow, PyTorch und anderen Machine-Learning-Frameworks arbeiten.  
✅ Dein neuronales Netz iterativ verbessern und verschiedene Architekturen testen.  

---

## Google Colab Notebook für den Ai-Pi-Racer

Ich habe ein Google Colab Notebook erstellt, mit dem du direkt dein Modell für den **Ai-Pi-Racer** trainieren kannst.  
Klicke auf den folgenden Link, um das Notebook zu öffnen und es in deinem eigenen Google Drive zu speichern:

> **🔗 Mein Google Colab Notebook:** [Hier klicken](https://colab.research.google.com/drive/1w5oP6nInVgHqcRKXCCZjzD_msqsTfMtU?usp=sharing)  

**Hinweis:** Du kannst das Notebook in deinem Google Drive speichern und deine eigenen Trainingsdaten verwenden.


1. **GPU-Laufzeit aktivieren:**  
   - Gehe zu **"Laufzeit" → "Laufzeittyp ändern"**  
   - Wähle **"Hardware-Beschleuniger: GPU"**  
   - Falls möglich, stelle sicher, dass eine **T4 GPU** genutzt wird.

2. **Ausführen der Skripte:**  
   - Alle Code-Blöcke müssen **nacheinander** ausgeführt werden, um die nötigen **Treiber** und **Software** zu installieren.
   - Erst danach kann das Training des neuronalen Netzes starten.

3. **Session aufzeichnen:**  
   - Du kannst aus deiner Session ein **Video schneiden** und anschließend downloaden.
   - Dies ist hilfreich für die spätere Analyse deines Trainingsprozesses.

🎉 **Viel Spaß beim Ausprobieren und Trainieren deines Ai-Pi-Racers!** 🚗💨

---

# Auswertung meines letzten Trainingsverlaufs 

<img src="/assets/2025/02/model_loss_01.png" width="50%" height="50%">

Dieses Diagramm zeigt die **Model Loss**-Kurve deines neuronalen Netzes während des Trainings deines Ai-Pi-Racers.  

## Beobachtungen  

1. **Anfangs hoher Verlust**  
   - Zu Beginn (Epoch 0) ist der Verlust (Loss) sowohl für das Trainingsset (**blau**) als auch für das Validierungsset (**orange**) hoch.  
   - Das ist normal, da das Modell zu Beginn noch nicht optimiert ist.  

2. **Schneller Abfall des Loss-Werts**  
   - In den ersten wenigen Epochen sinkt der Loss-Wert rapide, was zeigt, dass das Modell schnell lernt und sich verbessert.  

3. **Konvergenz gegen Ende**  
   - Nach etwa **30–40 Epochen** nähert sich der Loss-Wert einem stabilen, niedrigen Wert.  
   - Dies deutet darauf hin, dass das Modell weitgehend austrainiert ist und sich nicht mehr stark verbessert.  

4. **Validierungsloss ist stabil und niedrig**  
   - Der **Validierungsloss (orange)** folgt dem **Trainingsloss (blau)** eng, ohne stark zu steigen.  
   - Das ist ein gutes Zeichen, da es bedeutet, dass das Modell nicht übertrainiert (**Overfitting**) ist und auch auf neuen Daten gut performt.  

## Fazit  

Dein Modell scheint stabil zu trainieren und zeigt keine Anzeichen von Overfitting. Falls du noch weitere Verbesserungen testen willst, könntest du:  

- Die Anzahl der **Epochen** weiter erhöhen, falls der Loss noch leicht sinkt.  
- Mit der **Lernrate** spielen (ggf. kleiner machen, falls der Loss noch schwankt).  
- **Datenaugmentation** ausprobieren, um die Robustheit des Modells zu verbessern.  

🚀 **Sieht insgesamt nach einem erfolgreichen Training aus!** 🎉
