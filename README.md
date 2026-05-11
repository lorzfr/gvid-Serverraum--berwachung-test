# Serverraumüberwachung mit automatischer Lüftersteuerung

Dieses Repository enthält ein Arduino-Projekt zur Überwachung eines Serverraums mit temperaturabhängiger Lüftersteuerung, Abstandserkennung, DHT11-/TMP36-Sensorik und LCD-Anzeige.

## Projektziel

Ziel ist eine robuste, leicht nachvollziehbare Steuerung, die:
- Temperatur kontinuierlich überwacht,
- optional Luftfeuchtigkeit über den DHT11 erfasst,
- den Lüfter per PWM regelt,
- bei Personenerkennung in Lüfternähe den Lüfter stoppt,
- bei Sensorfehlern automatisch in einen Failsafe-Modus geht.

## Aktueller Projektstand

### Arduino-Sketches

1. `serverroom_fan_control-TMP36.ino`
   - Sensorik: **TMP36** (Temperatur, analog) + **HC-SR04** (Abstand)
   - Anzeige: **I2C LCD 16x2**
   - Logik: Temperatur-Hysterese, PWM-Mapping, Sonar-Failsafe

2. `serverroom_fan_control_dht11.ino`
   - Sensorik: **DHT11** (Temperatur + Luftfeuchte) + **HC-SR04** (Abstand)
   - Anzeige: **I2C LCD 16x2**
   - Logik: Temperatur-Hysterese, PWM-Mapping, DHT/Sonar-Failsafe

> Hinweis: In älteren Dokumentationen werden teilweise andere Dateinamen genannt. Maßgeblich sind die oben genannten tatsächlich vorhandenen Sketch-Dateien.

### Fritzing- und Bilddateien

- `serverraumüberwachung.fzz` ist die aktuelle Fritzing-Projektdatei.
- `serverraumüberwachung_bb.png` ist der aktuelle exportierte Fritzing-Aufbau mit Arduino Mega 2560, DHT11, HC-SR04, I2C-LCD und Lüfter.
- Die alte Backup-Datei `serverraumüberwachung.fzz.old` wurde entfernt, damit nur die aktuelle Fritzing-Version im Repository liegt.
- `IMG_2060.jpeg`, `IMG_2066.jpeg` und `IMG_2067.jpeg` dokumentieren den realen Aufbau.

## Verwendete Hardware

- Arduino Mega 2560
- 5V PWM-Lüfter
- TMP36 **oder** DHT11
- HC-SR04 Ultraschallsensor
- I2C LCD (16x2, häufig Adresse `0x27`, alternativ `0x3F`)
- Jumper-Kabel / Breadboard

## Pinbelegung

### DHT11-Version

| Komponente | Anschluss | Arduino Mega 2560 |
|---|---:|---:|
| DHT11 | DAT | D2 |
| DHT11 | VCC | 5V |
| DHT11 | GND | GND |
| HC-SR04 | TRIG | D3 |
| HC-SR04 | ECHO | D4 |
| HC-SR04 | VCC | 5V |
| HC-SR04 | GND | GND |
| Lüfter | PWM/Signal | D9 |
| Lüfter | VCC | 5V |
| Lüfter | GND | GND |
| I2C-LCD | SDA | D20/SDA |
| I2C-LCD | SCL | D21/SCL |
| I2C-LCD | VCC | 5V |
| I2C-LCD | GND | GND |

### TMP36-Version

| Komponente | Anschluss | Arduino Mega 2560 |
|---|---:|---:|
| TMP36 | Signal | A0 |
| TMP36 | VCC | 5V |
| TMP36 | GND | GND |
| HC-SR04 | TRIG | D3 |
| HC-SR04 | ECHO | D4 |
| Lüfter | PWM/Signal | D9 |
| I2C-LCD | SDA | D20/SDA |
| I2C-LCD | SCL | D21/SCL |

## Benötigte Libraries

Über den Arduino Library Manager installieren:
- `LiquidCrystal_I2C` (Frank de Brabander)
- `DHT sensor library` (Adafruit) *(nur für DHT11-Sketch)*
- `Adafruit Unified Sensor` *(Abhängigkeit der DHT-Library)*

## Kernlogik

- Nicht-blockierende Zeitsteuerung mit `millis()`
- Temperaturglättung per Moving Average
- Hysterese mit `TEMP_FAN_ON` und `TEMP_FAN_OFF`
- Personenerkennung über Distanzschwelle (`PERSON_DIST_CM`)
- Failsafe:
  - TMP36-Sketch: bei Sonarfehler Lüfter auf 100 %
  - DHT11-Sketch: bei DHT- **oder** Sonarfehler Lüfter auf 100 %

## Schnellstart

1. Hardware gemäß Fritzing-Aufbau und Pinbelegung anschließen.
2. Passenden Sketch öffnen:
   - `serverroom_fan_control-TMP36.ino` **oder**
   - `serverroom_fan_control_dht11.ino`
3. Bibliotheken installieren.
4. Sketch auf den Arduino Mega 2560 hochladen.
5. LCD prüfen (bei leerem Display I2C-Adresse `0x27`/`0x3F` testen).
6. Funktion testen:
   - Sensor erwärmen: Lüfter startet ab ca. `30°C`.
   - Hand vor HC-SR04 halten: Lüfter stoppt bei weniger als `50 cm` Abstand.
   - Sensorleitung testweise trennen: Failsafe schaltet den Lüfter auf 100 %.

## Projektstruktur

```text
.
├── README.md
├── LICENSE
├── doc/
│   ├── dokumenation.md
│   └── Dokumentation-als-PDF.txt
├── serverroom_fan_control-TMP36.ino
├── serverroom_fan_control_dht11.ino
├── serverraumüberwachung.fzz
├── serverraumüberwachung_bb.png
├── IMG_2060.jpeg
├── IMG_2066.jpeg
└── IMG_2067.jpeg
```

## Dokumentation & Medien

- Ausführliche Projektdokumentation: `doc/dokumenation.md`
- Hinweis zur PDF-Abgabe: `doc/Dokumentation-als-PDF.txt`
- Fritzing-Datei: `serverraumüberwachung.fzz`
- Fritzing-Export des Breadboard-/Schaltaufbaus: `serverraumüberwachung_bb.png`
- Aufbaufotos:
  - `IMG_2060.jpeg`
  - `IMG_2066.jpeg`
  - `IMG_2067.jpeg`

## Bilder

### Fritzing-Aufbau

![Fritzing-Export des Breadboard-Aufbaus](./serverraumüberwachung_bb.png)

### Realer Aufbau

![Aufbau 1](./IMG_2060.jpeg)
![Aufbau 2](./IMG_2066.jpeg)
![Aufbau 3](./IMG_2067.jpeg)
