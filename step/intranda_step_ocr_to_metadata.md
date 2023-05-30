---
description: >-
  Goobi Step Plugin zur Speicherung von OCR-Ergebnissen in einem konfigurierbaren Metadatenfeld.
---

# OCR zu Metadaten


## Einführung
Dieses Step-Plugin für den Goobi-Workflow liest die OCR-Ergebnisse automatisch ein, kombiniert sie und speichert sie in einem konfigurierbaren Metadatenfeld in der Mets-Datei.

| Details |  |
| :--- | :--- |
| Identifier | intranda_step_ocr_to_metadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-ocr-to-metadata](https://github.com/intranda/goobi-plugin-step-ocr-to-metadata) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2023.04 |
| Dokumentationsdatum | 30.05.2023 |


## Arbeitsweise des Plugins
Das Plugin prüft zunächst, ob der OCR-Text-Ordner oder der OCR-Alto-Ordner bereits existiert, wenn ja, wird der Inhalt seiner Dateien eingelesen und kombiniert und dann in einem Metadatenfeld mit dem konfigurierten Namen wie folgendermaßen gespeichert:
Wenn ein solches Metadatenfeld bereits existiert, wird sein Inhalt ersetzt. Andernfalls wird ein neues Metadatenfeld mit diesem Namen hinzugefügt. 

## Installation
Das Plugin besteht aus folgenden Dateien:

```text
plugin_intranda_step_ocr_to_metadata.jar
plugin_intranda_step_ocr_to_metadata.xml
```

Die Datei `plugin_intranda_step_ocr_to_metadata.jar` muss in dem richtigen Verzeichnis installiert werden, so dass diese nach der Installation an folgendem Pfad vorliegt:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_ocr_to_metadata.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_ocr_to_metadata.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_step_ocr_to_metadata.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
    -->
    
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- Name of the field where to store the OCR result -->
        <metadataField>AdditionalInformation</metadataField>
        
    </config>

</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `metadataField` | Dieser Parameter legt den Namen des Typs des Metadatenfeldes fest, das zur Speicherung des OCR-Ergebnisses verwendet werden soll.  |


## Integration des Plugins in den Workflow
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_ocr_to_metadata_de.png)
