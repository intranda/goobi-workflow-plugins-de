---
description: >-
  Goobi Step Plugin für das Umbenennen eines Vorgangs
---

# Umbenennen von Vorgängen


## Einführung
Dieses Step Plugin für Goobi workflow benennt den Vorgang mit dem in der Konfiguration angegebenen Namen um.


| Details |  |
| :--- | :--- |
| Identifier | intranda_step_rename_process |
| Source code | [https://github.com/intranda/goobi-plugin-step-rename-process](https://github.com/intranda/goobi-plugin-step-rename-process) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.10 und neuer |
| Dokumentationsdatum | 16.11.2022 |


## Installation
Das Plugin besteht aus der folgenden Datei:

```text
plugin_intranda_step_rename_process.jar
```

Diese Datei muss in dem richtigen Verzeichnis installiert werden, so dass diese nach der Installation an folgendem Pfad vorliegt:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_rename_process.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_rename_process.xml
```


## Konfiguration
Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_step_rename_process.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

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
        <project>*</project>
        <step>*</step>

        <!-- The process title is generated based on the following configuration. You can use
             Goobi variables here as well as the characters underscore _ and minus -
             Empty properties are skipped. Spaces are trimmed.

             Example:
               {processproperty.Institution} = Public Library of Example City
               {processproperty.Font} =
               {meta.CatalogIDDigitalLocalCatalogue} = ID123456789

               config = {processproperty.Institution}_{processproperty.Font}_{meta.CatalogIDDigitalLocalCatalogue}

               result: PublicLibraryofExampleCity__ID123456789
         -->

        <newProcessTitle>{process.Institution}_{meta.CatalogIDDigitalLocalCatalogue}</newProcessTitle>
    </config>

    <config>
    	<project>Archive_Project</project>
    	<step>*</step>
    	<newProcessTitle>{process.CreatorsAllOrigin}_{meta.PublicationYear}</newProcessTitle>
    </config>

    <config>
    	<project>*</project>
    	<step>STEP_NAME</step>
    	<newProcessTitle>{product.DocType}-{process.Creator of digital edition}-{process.Template}</newProcessTitle>
    </config>

    <config>
    	<project>PROJECT_NAME</project>
    	<step>STEP_NAME</step>
    	<newProcessTitle>Some Title</newProcessTitle>
    </config>

</config_plugin>
```


## Integration des Plugins in den Workflow
Zur Inbetriebnahme des Plugins muss dieses für einen oder mehrere gewünschte automatische Aufgaben im Workflow aktiviert werden. Dies erfolgt wie im folgenden Screenshot aufgezeigt durch Auswahl des Plugins `intranda_step_rename_process` aus der Liste der installierten Plugins.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_rename_process_de.png)
