---
description: >-
  Goobi Step Plugin für den automatischen Abruf von Bildern aus der Mets-Datei.
---

# Automatischer Abruf von Bildern aus Metadaten


## Einführung
Dieses Schritt-Plugin für den Goobi-Workflow holt automatisch Bilder aus einem konfigurierten Ordner oder von bestimmten URLs entsprechend den registrierten Namen bzw. registrierten URLs in der Mets-Datei. Die Bilder können weiter exportiert werden, wenn dies so konfiguriert ist.

| Details |  |
| :--- | :--- |
| Identifier | intranda_step_fetch_images_from_metadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata](https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2023.05 |
| Dokumentationsdatum | 04.06.2023 |


## Arbeitsweise des Plugins
Das Plugin prüft die vorhandenen Bilder im `media`-Ordner des Prozesses, um zu sehen, ob das gewünschte Bild bereits importiert wurde, und wenn nicht:
* wenn `useUrl` auf `true` gesetzt ist, wird das Plugin das Bild von der angegebenen URL herunterladen; 
* wenn `useUrl` auf `false` oder gar nicht gesetzt ist, wird das Plugin den konfigurierten Ordner durchsuchen, um zu sehen, ob das Bild dort zum Importieren bereit ist.

In beiden Fällen wird die Reihenfolge der importierten Bilder aktualisiert und in der mets-Datei gespeichert:
* wenn `useUrl` auf `true` gesetzt ist, entspricht die Reihenfolge der Dateien der in der mets-Datei registrierten Reihenfolge;
* wenn `useUrl` auf `false` oder gar nicht gesetzt ist, wird jede Datei, die nach dem Muster `{InventoryNumber} Nr_{SerialNumber}.{suffix}` benannt ist, an die erste Stelle befördert, während die anderen Bilder einfach nach ihren Namen sortiert werden.


## Installation
Das Plugin besteht aus folgenden Dateien:

```text
plugin_intranda_step_fetch_images_from_metadata.jar
plugin_intranda_step_fetch_images_from_metadata.xml
```

Die Datei `plugin_intranda_step_fetch_images_from_metadata.jar` muss in dem richtigen Verzeichnis installiert werden, so dass diese nach der Installation an folgendem Pfad vorliegt:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_fetch_images_from_metadata.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_fetch_images_from_metadata.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_step_fetch_images_from_metadata.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

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

        <!-- true if the images should be fetched from a url, false if the images should be fetched from the following configured folder. DEFAULT false -->
        <useUrl>false</useUrl>
        
        <!-- metadata containing the file name -->
        <filenameMetadata>SeparatedMaterial</filenameMetadata>
        <!-- mode="copy|move"   ignoreFileExtension="true|false"-->
        <fileHandling mode="copy" ignoreFileExtension="true" folder="/opt/digiverso/import/images/" />
        <!-- enabled= true|false exportImages=true|false -->
        <export enabled="true" exportImages="true" />

    </config>
</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `useUrl` | Dieser Parameter bestimmt den Quellort der abzurufenden Bilder. Wenn er auf `true` gesetzt ist, werden die Bilder von den registrierten URLs in der mets-Datei geholt, wenn er auf `false` oder gar nicht gesetzt ist, werden die Bilder aus dem folgenden konfigurierten Ordner geholt. |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `filenameMetadata` | Dieser Parameter definiert die Metadaten, die die Namen der gewünschten Bilder enthalten.  |
| `fileHandling` | Das Attribut `@mode` definiert, ob die Bilder durch Kopieren oder Verschieben importiert werden sollen. Das Attribut `@ignoreFileExtension` definiert, ob die Dateierweiterungen beim Durchsuchen der Bilder ignoriert werden sollen oder nicht. Das Attribut `@folder` enthält den absoluten Pfad zu dem Ordner, der die zu importierenden Bilder enthält.  |
| `export` | Das Attribut `@enabled` legt fest, ob der Prozess exportiert werden soll oder nicht, während das Attribut `@exportImages` definiert, ob die Bilder exportiert werden sollen oder nicht.  |


## Integration des Plugins in den Workflow
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_fetch_images_from_metadata_de.png)
