# gvid-Serverraum--berwachung-test

In einem Serverraum, der rund um die Uhr in Betrieb ist, müssen Temperatur und Luftzirkulation zuverlässig überwacht werden, um Überhitzung zu vermeiden und Energie effizient zu nutzen.

## Projektziel

Dieses Projekt zeigt eine einfache, praxisnahe **Serverraum-Überwachung mit automatischer Lüftersteuerung** auf Arduino-Basis.
Je nach Sketch werden Temperatur (TMP36 oder DHT11), Luftfeuchte (DHT11) und Abstand/Anwesenheit (HC-SR04) erfasst.

## Enthaltene Sketches

### 1) `serverroom_fan_control.ino`
Basisversion mit:
- **TMP36** (Temperatur über Analog-Pin)
- **HC-SR04** (Abstand/Personenerkennung)
- **I2C LCD (16x2)**
- **PWM-Lüftersteuerung**

### 2) `serverroom_fan_control_dht11.ino`
Erweiterung mit:
- **DHT11** (Temperatur + Luftfeuchte)
- **HC-SR04** (Abstand)
- **Failsafe-Logik** (bei Sensorfehler Lüfter auf 100 %)
- **Temperatur-Hysterese** für stabileres Schaltverhalten

### 3) `serverroom_fan_control_dht11withtempandhumid.ino`
Variante mit Fokus auf Darstellung von Temperatur/Feuchte inkl. DHT11-Logik sowie Lüfterregelung über PWM.

## Verwendete Hardware

- Arduino Mega 2560
- 5V PWM-Lüfter
- DHT11 oder TMP36 (je nach Sketch)
- HC-SR04 Ultraschallsensor
- I2C LCD (16x2, meist Adresse `0x27`, ggf. `0x3F` testen)

## Benötigte Libraries

Installiere über den Arduino Library Manager:
- `LiquidCrystal_I2C` (Frank de Brabander)
- `DHT sensor library` (Adafruit) *(für DHT11-Sketches)*
- `Adafruit Unified Sensor` *(Abhängigkeit für DHT library)*

## Funktionslogik (kurz)

- **Nicht-blockierende Loop** mit `millis()`-Intervallen für Sensoren und Display
- **Geglättete Temperaturwerte** per Moving Average
- **Hysterese** (`TEMP_FAN_ON` / `TEMP_FAN_OFF`) gegen ständiges Ein/Aus-Schalten
- **Personenerkennung**: Bei kurzer Distanz kann der Lüfter deaktiviert werden
- **Failsafe**: Bei Sensorfehlern wird der Lüfter zur Sicherheit hochgeregelt

## Projektstruktur

```text
.
├── README.md
├── LICENSE
├── serverroom_fan_control.ino
├── serverroom_fan_control_dht11.ino
├── serverroom_fan_control_dht11withtempandhumid.ino
├── serverraumüberwachung.fzz
├── IMG_2060.jpeg
├── IMG_2066.jpeg
└── IMG_2067.jpeg
```

## Bilder (Aufbau)

### Aufbau 1
![Aufbau 1](./IMG_2060.jpeg)

### Aufbau 2
![Aufbau 2](./IMG_2066.jpeg)

### Aufbau 3
![Aufbau 3](./IMG_2067.jpeg)
