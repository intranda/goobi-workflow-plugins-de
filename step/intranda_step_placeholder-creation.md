---
description: >-
  Dieses Step Plugin für Goobi workflow dient zur Erzeugung einer definierten
  Anzahl von Platzhalterbildern innerhalb des Master-Ordners.
---

# Generierung von Platzhalterbildern

## Einführung

Dieses Plugin dient zur Generierung von Platzhalterbildern innerhalb des Master-Ordners eines Vorgangs von Goobi workflow. Die Anzahl der zu generierenden Bilder ist hierbei innerhalb der Nutzeroberfläche definierbar.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_placeholder-creation |
| Source code | [https://github.com/intranda/goobi-plugin-step-placeholder](https://github.com/intranda/goobi-plugin-step-placeholder) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.06 |
| Dokumentationsdatum | 13.06.2020 |

## Installation

Zur Nutzung des Plugins müssen diese beiden Dateien an folgende Orte kopiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_placeholder.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_placeholder-GUI.jar
```

Dieses Plugin verfügt über keine Konfigurationsdatei und ist entsprechend nicht weitergehend konfigurierbar.

## Bedienung des Plugins

Dieses Plugin wird in den Workflow so integriert, dass es für eine ausgewählte Aufgabe zur Verfügung steht. Nach dem Annehmen der Aufgabe, kann der Nutzer festlegen, welche Anzahl an Bildern durch das Plugin generiert werden soll und bestätigt dies. Unmittelbar darauf erzeugt das Plugin die definierte Anzahl an Bildern und speichert diese innerhalb des Master-Ordners zu dem Vorgang ab.

![Integration des Plugins in eine Aufgabe](../.gitbook/assets/intranda_step_placeholder-creation-1_de.png)

Ab diesem Moment existieren zu dem Vorgang die generierten auffälligen Platzhalterbilder und können im Laufe des weiteren Workflows angezeigt und auch ersetzt werden.

![Anzeige der Platzhalterbilder z.B. innerhalb des METS-Editors](../.gitbook/assets/intranda_step_placeholder-creation-2_de.png)

