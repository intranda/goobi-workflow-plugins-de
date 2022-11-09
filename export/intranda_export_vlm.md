---
description: >-
  Dies ist eine technische Dokumentation für das VLM Export Plugin. Es ermöglicht, den Export in eine VLM Instanz.
---

# VLM Export

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des VLM-Export-Plugins in Goobi.

Mithilfe dieses Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes an den konfigurierten Ort für VLM exportiert werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.10 und neuer |
| Dokumentationsdatum | 07.11.2022 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

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

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_export_vlm.xml`. Die Konfiguration kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
  <!-- The object identifier, typically CatalogIDDigital  -->
  <!-- MANDATORY -->
  <identifier>CatalogIDDigital</identifier>

  <!-- The name to be used to distinguish between different volumes of multi volume works. -->
  <!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
  <!-- MANDATORY -->
  <volume>CurrentNoSorting</volume>

  <!-- The place you would like to use for the export. -->
  <!-- Absolute path expected. -->
  <!-- MANDATORY -->
  <path>/opt/digiverso/viewer/hotfolder/</path>

  <!-- The prefix you would like to use for subfolders for different volumes. -->
  <!-- MANDATORY -->
  <subfolderPrefix>T_34_L_</subfolderPrefix>

</config_plugin>
```

| Parameter         | Erläuterung                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | Dieser Parameter legt fest, welches Metadatum als Ordnername verwendet werden soll. |
| `volume`          | Dieser Parameter steuert, mit dem Inhalt welchen Metadatums die Unterverzeichnisse für Bände benannt werden sollen. |
| `path`            | Dieser Parameter legt den Export-Pfad fest, wohin die Daten exportiert werden sollen. Erwartet wird ein absoluter Pfad. |
| `subfolderPrefix` | Dieser Parameter beschreibt den Präfix, der für jeden Band eines mehrbändigen Werkes in der Ornderbezeichnung vorangestellt werden soll. (Beispiel `T_34_L_`: Hier steht `T_34` für die Erkennung zur Erstellung eines Strukturknotens des Typs `Band` und das `L` gibt an, dass danach ein Text kommt.) |
