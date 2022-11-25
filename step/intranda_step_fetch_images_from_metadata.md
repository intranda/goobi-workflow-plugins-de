---
description: >-
  Dies ist eine technische Dokumentation für das Plugin zum automatischen
  Anreichern des Prozesses mit Bildern basierend auf Metadaten mit den Dateinamen im Vorgang.
---

# Automatische Paginierung auf Basis der Dateinamen

## Einführung
Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Plugins. Mit Hilfe dieses Plugins können Bilder anhand des im Vorgangs hinterlegten Dateinamens in den gewünschte Ordner des Prozesses kopiert werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_imagename\_analyse |
| Source code | [https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata](https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 3.0.10 |
| Dokumentationsdatum | 25.11.2022 |


## Arbeitsweise des Plugins
Das Plugin wird üblicherweise vollautomatisch innerhalb des Workflows ausgeführt. Es ermittelt zunächst, ob das in der Konfiguration spezifizierte Metadatum vorhanden ist und kopiert anschließend das Bild mit dem im Metadatum spezifizierten Namen und der in der Konfiguration spezifizierten Dateiendung in den media-Ordner des Vorgangs.


## Bedienung des Plugins
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_fetch-images-from-metadata.png)


## Installation und Konfiguration
Das Plugin besteht aus zwei Dateien:

```bash
plugin_intranda_step_fetch_images_from_metadata.jar
plugin_intranda_step_fetch_images_from_metadata.xml
```

Die Datei `plugin_intranda_step_fetch_images_from_metadata.jar` enthält die Programmlogik und muss für den `tomcat`-Nutzer lesbar in folgendes Verzeichnis installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/
```

Die Datei `plugin_intranda_step_fetch_images_from_metadata.xml` muss ebenfalls für den `tomcat`-Nutzer lesbar sein und in folgendes Verzeichnis installiert werden:

```bash
/opt/digiverso/goobi/config/
```

Diese Datei dient zur Konfiguration des Plugins und muss wie folgt aufgebaut sein:

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
        
        <!-- metadata containing the file name -->
        <filenameMetadata>SeparatedMaterial</filenameMetadata>
        
        <!-- path to images -->     
        <imagesFolder>/opt/digiverso/import/images/</imagesFolder>
        
          <!-- image file type -->     
        <imageType>jpg</imageType>
        <!-- remove existing filetype -->
        <removeOldFileType>false</removeOldFileType>    

    </config>

</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `filenameMetadata` |Der Typ des Metadatums in dem der Dateiname der Bildateien hinterlegt wird. |
| `imagesFolder` | Der Ordner in dem sich die zu importierenden Bilder befinden. |
| `imageType` | Der Bildtyp. In der Regel wird im Metadatum nur der Dateiname ohne Dateityp hinterlegt. Hier kann bestimmt werden, welchen Dateityp die Bilder haben. |
| `removeOldFileType` | Falls im Metadatum doch der komplette Dateiname mit Dateityp hinterlegt wurde, kann hier angegeben werden, dass er bei der Verarbeitung ignoriert wird. |



