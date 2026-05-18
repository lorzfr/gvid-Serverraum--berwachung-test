# 5. Zusammenfassung und Reflexion

Dieses Kapitel fasst den aktuellen Stand der Serverraumüberwachung zusammen, bewertet die erreichten Ergebnisse und beschreibt sinnvolle Erweiterungen für eine mögliche Version 2.0. Im Mittelpunkt stehen die temperaturabhängige Lüftersteuerung, die Sicherheitsabschaltung über Abstandserkennung, die Anzeige der Betriebswerte und der Umgang mit Sensorfehlern.

## 5.1 Ergebnisse: Was funktioniert gut?

Das Projekt erfüllt die wichtigsten Anforderungen an eine einfache Serverraumüberwachung. Die Temperatur wird regelmäßig erfasst und als Grundlage für die automatische Lüftersteuerung genutzt. Dadurch muss der Lüfter nicht dauerhaft mit voller Leistung laufen, sondern wird abhängig von der gemessenen Temperatur geregelt. Das spart Energie, reduziert Geräusche und sorgt trotzdem dafür, dass bei steigender Temperatur aktiv gekühlt wird.

Besonders gut funktioniert die Hysterese zwischen Einschalt- und Ausschaltpunkt. Der Lüfter startet erst ab einer definierten Temperatur und schaltet erst wieder ab, wenn die Temperatur ausreichend gefallen ist. Dadurch wird verhindert, dass der Lüfter bei kleinen Messschwankungen ständig an- und ausgeht. Diese Logik macht das System stabiler und praxistauglicher.

Auch die PWM-Regelung ist ein wichtiger Vorteil. Der Lüfter wird nicht nur ein- oder ausgeschaltet, sondern kann in seiner Leistung angepasst werden. Mit einem Mindest-PWM-Wert wird sichergestellt, dass der Lüfter zuverlässig anlaufen kann, sobald Kühlung benötigt wird. Bei höheren Temperaturen wird die Lüfterleistung weiter erhöht.

Die Glättung der Temperaturwerte verbessert die Anzeige und die Regelung. Einzelne Ausreißer oder kleine Messungenauigkeiten wirken sich dadurch weniger stark auf das Verhalten des Lüfters aus. Das ist vor allem bei analogen Sensoren wie dem TMP36 hilfreich, aber auch bei digitalen Messwerten sinnvoll.

Die Personenerkennung über den HC-SR04 ist ebenfalls ein sinnvoller Sicherheitsbestandteil. Wenn eine Person oder ein Gegenstand zu nah am Lüfter erkannt wird, stoppt der Lüfter automatisch. Damit wird das Projekt nicht nur als Temperaturregelung, sondern auch als sicherheitsbewusstes Überwachungssystem umgesetzt.

Ein weiterer positiver Punkt ist die Fehlerbehandlung. Bei ungültigen Sensorwerten oder Problemen mit der Abstandsmessung wird ein Failsafe aktiviert. Der Lüfter läuft dann mit voller Leistung, damit im Zweifel eher gekühlt wird, anstatt eine mögliche Überhitzung zu riskieren. Besonders in einem Serverraum ist dieses Verhalten sinnvoll, weil ein Ausfall der Kühlung größere Schäden verursachen könnte.

Die Ausgabe auf dem LCD und über den seriellen Monitor erleichtert die Kontrolle des Systems. Temperatur, Lüfterleistung, Abstand und Statusmeldungen können direkt beobachtet werden. Dadurch lassen sich Tests einfacher durchführen und Fehler schneller eingrenzen.

Zusammengefasst funktionieren vor allem diese Punkte gut:

- automatische Temperaturüberwachung,
- temperaturabhängige Lüftersteuerung mit PWM,
- stabile Hysterese gegen häufiges Ein- und Ausschalten,
- geglättete Messwerte für ruhigere Regelung,
- Personenerkennung als Sicherheitsfunktion,
- Failsafe bei Sensorfehlern,
- verständliche Anzeige von Messwerten und Statusinformationen.

## 5.2 Probleme: Wo gibt es noch offene Baustellen?

Trotz der funktionierenden Grundfunktionen gibt es noch einige offene Punkte. Die Genauigkeit der Temperaturmessung hängt stark vom verwendeten Sensor und von der Positionierung im Aufbau ab. Ein TMP36 kann durch analoge Störungen beeinflusst werden, während ein DHT11 vergleichsweise langsam und ungenau ist. Für eine echte Serverraumüberwachung wäre eine genauere und professionellere Sensorik sinnvoll.

Auch die Luftfeuchtigkeit wird nur in der DHT11-Version erfasst. In einem Serverraum ist Luftfeuchtigkeit jedoch ebenfalls relevant, weil zu hohe oder zu niedrige Werte langfristig problematisch für elektronische Geräte sein können. In der aktuellen Umsetzung wird die Luftfeuchtigkeit zwar angezeigt, aber noch nicht aktiv für Warnungen oder Regelentscheidungen genutzt.

Die Abstandsmessung mit dem HC-SR04 kann in bestimmten Situationen unzuverlässig sein. Schräg stehende Flächen, weiche Materialien oder ungünstige Reflexionen können Messfehler verursachen. Außerdem ist die Personenerkennung sehr einfach umgesetzt: Alles unterhalb des Grenzwerts wird als Person oder Hindernis interpretiert. Eine genauere Unterscheidung findet noch nicht statt.

Eine weitere Baustelle ist die Hardware-Auslegung des Lüfters. Der Arduino-Pin darf den Lüfter nicht direkt mit Strom versorgen. Für einen dauerhaft zuverlässigen Betrieb ist eine passende Treiberstufe mit Transistor oder MOSFET sowie eine sichere Spannungsversorgung notwendig. Dieser Punkt ist wichtig, wenn aus dem Versuchsaufbau ein dauerhaft nutzbares System werden soll.

Die Anzeige ist nützlich, aber durch das 16x2-LCD begrenzt. Es können nur wenige Informationen gleichzeitig dargestellt werden. Für ausführlichere Diagnosen, Verlaufsdaten oder mehrere Sensorwerte wäre eine umfangreichere Benutzeroberfläche besser geeignet.

Außerdem speichert das System aktuell keine Messwerte. Nach einem Neustart oder nach einem Fehler ist nicht nachvollziehbar, wie sich Temperatur, Abstand oder Fehlerzustände vorher entwickelt haben. Für eine echte Überwachung wäre Logging sehr hilfreich.

Offene Baustellen sind daher vor allem:

- begrenzte Genauigkeit der verwendeten Sensoren,
- keine Kalibrierung der Temperaturmessung,
- Luftfeuchtigkeit wird noch nicht für Alarme genutzt,
- mögliche Fehlmessungen beim Ultraschallsensor,
- einfache Hindernis-/Personenerkennung ohne Plausibilitätsprüfung,
- notwendige robuste Lüfter-Treiberstufe für den Dauerbetrieb,
- begrenzte Informationsdarstellung auf dem LCD,
- keine Speicherung oder Auswertung von Messdaten.

## 5.3 Ausblick: Welche Funktionen könnten in einer Version 2.0 ergänzt werden?

In einer Version 2.0 könnte das Projekt deutlich erweitert und näher an eine professionelle Serverraumüberwachung gebracht werden. Eine wichtige Verbesserung wäre der Einsatz genauerer Sensoren, zum Beispiel eines DHT22, BME280 oder eines anderen präziseren Temperatur- und Feuchtigkeitssensors. Damit könnten Temperatur und Luftfeuchtigkeit zuverlässiger gemessen und besser ausgewertet werden.

Zusätzlich wäre ein Datenlogging sinnvoll. Messwerte könnten auf einer SD-Karte gespeichert oder über eine Netzwerkverbindung an einen Server gesendet werden. Dadurch ließen sich Temperaturverläufe analysieren, Fehlerereignisse dokumentieren und Langzeittests besser auswerten.

Eine Alarmfunktion wäre ebenfalls ein großer Mehrwert. Bei zu hoher Temperatur, kritischer Luftfeuchtigkeit oder Sensorfehlern könnte das System über eine LED, einen Buzzer, eine E-Mail, eine Push-Nachricht oder eine Web-Oberfläche warnen. Dadurch müsste nicht dauerhaft jemand das LCD beobachten.

Für eine moderne Umsetzung könnte ein ESP32 oder ein ähnlicher Mikrocontroller genutzt werden. Damit wäre eine WLAN-Anbindung möglich. Das System könnte dann ein Web-Dashboard bereitstellen, Messwerte live anzeigen und Einstellungen wie Grenzwerte oder Betriebsmodi direkt über den Browser anpassbar machen.

Auch mehrere Sensoren wären sinnvoll. In einem Serverraum gibt es oft Temperaturunterschiede zwischen verschiedenen Bereichen. Mit mehreren Messpunkten könnte das System Hotspots erkennen und die Lüftersteuerung genauer anpassen.

Für den praktischen Betrieb wären außerdem verschiedene Betriebsmodi hilfreich. Neben dem Automatikmodus könnte es einen manuellen Modus, einen Wartungsmodus und einen Notfallmodus geben. So könnte der Lüfter bei Tests gezielt gesteuert oder bei Wartungsarbeiten sicher deaktiviert werden.

Mögliche Erweiterungen für Version 2.0 sind:

- präzisere Sensoren wie DHT22 oder BME280,
- Kalibrierfunktion für Temperatur- und Feuchtigkeitswerte,
- Speicherung von Messwerten auf SD-Karte,
- Netzwerkübertragung der Daten,
- Web-Dashboard für Live-Anzeige und Konfiguration,
- Alarmierung per LED, Buzzer, E-Mail oder Push-Nachricht,
- mehrere Temperaturmesspunkte im Raum,
- Grenzwerte für Luftfeuchtigkeit,
- bessere Plausibilitätsprüfung der Abstandsmessung,
- einstellbare Betriebsmodi wie Auto, Manuell, Wartung und Notfall,
- stabilere Hardware mit geeigneter Lüfter-Treiberstufe und Gehäuse.

Insgesamt bietet die aktuelle Version eine funktionierende Grundlage. Die wichtigsten Kernfunktionen sind vorhanden, aber Version 2.0 könnte das System genauer, komfortabler, besser vernetzbar und zuverlässiger für den Dauerbetrieb machen.
