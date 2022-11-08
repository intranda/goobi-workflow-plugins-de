---
description: >-
  Dies ist eine technische Dokumentation für das konfigurierbare Goobi Export Plugin. Es ermöglicht, den Export mithilfe der Projektkonfiguration anzupassen. Durch die Verwendung von Exportprojekten ist es auch möglich zu verschiedenen Speicherorten in einem Durchlauf zu exportieren.
---

# Konfigurierbarer VLM Export

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des VLM-Export-Plugins in Goobi.

Mithilfe dieses Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes an konfigurierte Orte gleichzeitig exportiert werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.10 und neuer |
| Dokumentationsdatum | 07.11.2022 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es manuell ausgeführt wird. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_plugin_export_vlm_de.png)

Das Plugin muss zunächst in folgendes Verzeichnis kopiert werden:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_vlm.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_vlm.xml
```
## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_export_vlm.xml` und über die Projekteinstellungen. Die Konfiguration kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
  <!-- The name of the system, e.g. AlmaIDDigital, AlephIDDigital, CatalogIDDigital.  -->
  <!-- MANDATORY -->
  <identifier>CatalogIDDigital</identifier>

  <!-- The name to be used to distinguish between different volumes of one book series. -->
  <!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
  <!-- Leave the default value unchanged if the book is a one-volume work. -->
  <!-- MANDATORY -->
  <volume>CurrentNoSorting</volume>

  <!-- The place you would like to use for the export. -->
  <!-- Absolute path expected. -->
  <!-- MANDATORY -->
  <path>/opt/digiverso/viewer/hotfolder/</path>

  <!-- The prefix you would like to use for subfolders for different volumes. -->
  <!-- Leave the default value unchanged if the book is a one-volume work. -->
  <!-- MANDATORY -->
  <subfolderPrefix>T_34_L_</subfolderPrefix>

</config_plugin>

```

| Parameter         | Erläuterung                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | Dieser Parameter legt fest, welches System genutzt wird. Verwendet wird hierbei der Name des Systems plus `IDDigital`. |
| `volume`          | Dieser Parameter legt fest, wie man verschiedene Bänden eines mehrbändigen Werks unterscheiden kann.                   |
| `path`            | Dieser Parameter legt fest, wo die Ordner für die Bücher erstellt werden sollen. Erwartet wird ein absoluter Pfad.                 |
| `subfolderPrefix` | Dieser Parameter legt fest, wie die übergabene Dateien weiter von VLM bearbeitet werden. Hierbei steht beispielsweise `T_34` für die Erkennung zur Erstellung eines Strukturknotens des Typs `Band` und das `L` gibt an, dass danach ein Text kommt. |
