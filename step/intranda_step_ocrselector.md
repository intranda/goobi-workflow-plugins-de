---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin zur Auswahl von
  Einzelseiten zur OCR-Durchführung oder nicht-Durchführung.
---

# OCR Seitenauswahl

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zur Seitenauswahl für eine nachgelagerte OCR Durchführung. Mit diesem Plugin kann auf Einzelseiten-Basis festgelegt werden, welche Bilder aus einem Vorgang mit welchem Schrifttyp zur OCR gesendet werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_ocrselector |
| Source code | [https://github.com/intranda/goobi-plugin-step-ocrselector](https://github.com/intranda/goobi-plugin-step-ocrselector) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 3.0.4 |
| Dokumentationsdatum | 04.03.2019 |

## Voraussetzung

Voraussetzung für die Verwendung des Plugins ist der Einsatz von Goobi workflow in Version 3.0.4 oder höher, die korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte des Workflows. Zusätzlich wird noch ein [Plugin für die eigentliche Durchführung der OCR und Zusammenführung der Ergebnisse](intranda_step_mixedocr.md) benötigt.

## Installation und Konfiguration

Zur Nutzung des Plugins müssen folgende Dateien installiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_ocrselector.jar
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/css/style.css
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/app.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/riot.min.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/tags.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/ugh.js
```

Die erste Datei ist der Java-Teil des Plugins, alle folgenden Dateien werden für die grafische Anzeige benötigt.

Das Plugin hat keine eigene Konfiguration, sondern liest den Standard-Wert für alle Seiten aus den Metadaten des Vorgangs aus.

## Einstellungen in Goobi

Nachdem das Plugin installiert wurde, kann es in der Nutzeroberfläche in einem Workflowschritt konfiguriert werden.

![Task-Details](../.gitbook/assets/intranda_step_ocrselector_config.png)

## Nutzung

Wurde der entsprechende Arbeitsschritt durch den jeweiligen Nutzer geöffnet, innerhalb dem das Plugin konfiguriert wurde, wird dem Nutzer das Plugin in dem Arbeitsschritt angezeigt. Nachdem das Plugin betreten wurde, öffnet sich eine neue Ansicht, in der alle zum Prozess gehörenden Bilder angezeigt werden.

![Plugin-Oberfl&#xE4;che](../.gitbook/assets/intranda_step_ocrselector_entry.png)

Die aktuelle Auswahl \(Antiqua, Fraktur, keine OCR\) wird unterhalb des jeweiligen Bildes angezeigt. Die Vorauswahl wird aus der Vorgangseigenschaft `Schrifttyp` ausgelesen.

Hier können nun einzelne Bilder per Linksklick ausgewählt werden. Eine Mehrfachauswahl ist per `Strg + Klick` für einzelne Seiten und `Shift + Klick` für einen Bereich von Seiten möglich.

![Mehrfachauswahl](../.gitbook/assets/intranda_step_ocrselector_selection.png)

Wenn eine oder mehrere Seiten ausgewählt sind, kann per Rechtsklick auf eine der ausgewählten Seiten ein Kontextmenü geöffnet werden.

![Kontextmen&#xFC;](../.gitbook/assets/intranda_step_ocrselector_context.png)

Hier stehen die drei Auswahlmöglichkeiten `antiqua`, `fraktur` und `keine OCR` zur Auswahl. Per Klick auf eine der drei Möglichkeiten wird diese auf alle ausgewählten Seiten angewandt.

![Aktualisiert - keine OCR f&#xFC;r Einband und leere Seiten](../.gitbook/assets/intranda_step_ocrselector_updated.png)

Das Plugin kann nach erfolgter Markierung per Klick auf `Plugin verlassen` verlassen werden. Dabei wird auch automatisch noch einmal gespeichert. Die Schaltfläche `Speichern` kann für zwischenzeitliches Speichern benutzt werden.
