---
description: >-
  Dies ist eine technische Dokumentation für das Plugin zum automatischen
  Anreichern des Prozesses mit Bildern basierend auf Metadaten mit den Dateinamen im Vorgang.
---

# Kopieren von Dateien aus Metadatenfeldern

## Einführung
Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Plugins. Mit Hilfe dieses Plugins können Bilder anhand des im Vorgangs hinterlegten Dateinamens in den gewünschte Ordner des Vorgangs kopiert oder bewegt werden. 

| Details |  |
| :--- | :--- |
| Identifier | intranda-step-fetch-images-from-metadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata](https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata) |
| Lizenz | GPL 2.0 oder neuer |
| Dokumentationsdatum | 25.11.2022 |


## Arbeitsweise des Plugins
Das Plugin wird üblicherweise vollautomatisch innerhalb des Workflows ausgeführt. Es ermittelt zunächst, ob das in der Konfiguration spezifizierte Metadatum vorhanden ist und wertet dieses anschließend aus. Die in dem Metadatum angegebene Datei wird anschließend anhand ihres Namens und der Dateiendung in den media-Ordner des Vorgangs kopiert oder bewegt.


## Bedienung des Plugins
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes muss der Workflow entsprechend konfiguriert und dort das Plugin ausgewählt werden. Eine manuelle Verwendung dieses Plugins durch einen Anwender ist nicht notwendig.


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

Die Konfigurationsdatei `plugin_intranda_step_fetch_images_from_metadata.xml` muss ebenfalls für den `tomcat`-Nutzer lesbar sein und in folgendes Verzeichnis installiert werden:

```bash
/opt/digiverso/goobi/config/
```

Diese Konfigurationsdatei ist in etwa wie folgt aufgebaut:

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
         <!-- fileHandling:
              mode: Defines if files are copied or moved. Possible values are "copy" and "move". Defaults to "copy".
              ignoreFileExtension: If the filenameMetadata contains the value "image1.tif" for example you can configure here if the plugin
              shall search for the exact filename or if the file extension shall be ignored which
              allowes to find "image1.jpg", too. Possible values are "true" and "false". Default is "false".
              folder: Absolut path to the directory where to search for the files to be imported.
        -->
        <fileHandling mode="copy|move" ignoreFileExtension="true|false" folder="/opt/digiverso/import/images/" />
    </config>

</config_plugin>
```

Die einzelnen Parameter haben die folgende Funktion:

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>`-Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>`-Block vorkommen. |
| `filenameMetadata` | Hier ist der Name des Metadatenfeldes (üblicherweise aus der METS-Datei) angegeben, der den Dateinamen der zu importierenden Datei enthält. |

Die Attribute des für das Element `fileHandling` werden wie folgt konfiguriert:

| Attribut | Erläuterung |
| :--- | :--- |
| `mode` | Dieses Attribut legt fest, ob die zu übernehmenden Dateien in den Ordner des Vorgangs kopiert werden sollen oder ob sie dorthin bewegt werden. |
| `ignoreFileExtension` | Dieses Attribut steuert, ob die Dateiendung für den Kopiervorgang ignoriert werden soll oder exakt stimmen muss. |
| `folder` | Dieses Attribut gibt den Ordner an, in dem sich die zu importierenden Dateien befinden. |