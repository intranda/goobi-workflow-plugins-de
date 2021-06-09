---
description: >-
  Dies ist ein Goobi workflow Plugin für den Upload von mehreren Dateien, um
  eine automatische Vorgangserstellung auf Basis der hochgeladenen Dateien zu
  ermöglichen. Aus Dateien mit ähnlichen Namen werd
---

# Vorgangserstellung durch Dateiupload

## Einführung

Dieses Workflow-Plugin erlaubt einen Massenupload von Dateien, auf deren Basis automatisch Goobi-Vorgänge erzeugt werden. Die zu verwendende Produktionsvorlage kann hierbei ebenso wie der zu erzeugende Publikationstyp über eine Konfiguration festgelegt werden. Zusammengehörige Dateien werden nach dem Erzeugen des Vorgangs automatisch zugewiesen.

## Übersicht

| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_fileUploadProcessCreation |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-fileupload-processcreation](https://github.com/intranda/goobi-plugin-workflow-fileupload-processcreation) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 20.12 |
| Dokumentationsdatum | 24.04.2021 |

## Installation

Zur Installation des Plugins müssen folgende beiden Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_fileUploadProcessCreation.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_fileUploadProcessCreation-GUI.jar
```

Um zu konfigurieren, wie sich das Plugin verhalten soll, können verschiedene Werte in der Konfigurationsdatei angepasst werden. Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_fileUploadProcessCreation.xml
```

Der Inhalt dieser Konfigurationsdatei sieht wie folgt aus:

```markup
<config_plugin>

    <!-- which file types shall be allowed for uploading these -->
    <allowed-file-extensions>/(\.|\/)(gif|jpe?g|png|tiff?|jp2|pdf)$/</allowed-file-extensions>

    <!-- which process template shall be used to create the new processes -->
    <processTemplateName>ImportWorkflow</processTemplateName>

    <!-- which publication type shall be used inside of the created METS files -->
    <metadataDocumentType>Monograph</metadataDocumentType>

    <!-- define a naming schema for the file names to be matched for the process creation -->
    <namingSchema>/.*(BA_\\d+[_-](\\d+)).*\\.jpg/</namingSchema>

</config_plugin>
```

Für eine Nutzung dieses Plugins muss der Nutzer über die korrekte Rollenberechtigung verfügen.

![Ohne korrekte Berechtigung ist das Plugin nicht nutzbar](../.gitbook/assets/intranda_workflow_fileupload_processcreation1_de.png)

Bitte weisen Sie daher der Gruppe die Rolle `Plugin_Goobi_Massupload` zu.

![Korrekt zugewiesene Rolle f&#xFC;r die Nutzer](../.gitbook/assets/intranda_workflow_fileupload_processcreation2_de.png)

## Erläuterung der Konfigurationsoptionen

Die Konfiguration des Plugins gestaltet sich wie folgt:

| Wert | Beschreibung |
| :--- | :--- |
| `allowed-file-extensions` | Mit diesem Parameter wird festgelegt, welche Datein hochgeladen werden dürfen. Hierbei handelt es sich um einen regulären Ausdruck. |
| `processTemplateName` | Mit diesem Parameter kann definiert werden, welche Produktionsvorlage für die zu erzeugenden Vorgänge verwendet werden sollen. |
| `metadataDocumentType` | Legen Sie hiermit fest, welcher Publikationstyp innerhalb der METS-Datei verwendet werden soll für die neu zu erzeugenden Vorgänge. |
| `namingSchema` | Definieren Sie hier einen regulären Ausdruck, der auf das Benennungsschema der hochgeladenen Dateien zutreffen soll. Dieser ist maßgeblich für die Erzeugung der Vorgänge sowie die Zusammengehörigkeit der Bilder zu den Vorgängen. |

## Bedienung des Plugins

Wenn das Plugin korrekt installiert und konfiguriert wurde, ist es innerhalb des Menüpunkts `Workflow` zu finden.

![Ge&#xF6;ffnetes Plugin f&#xFC;r den Upload](../.gitbook/assets/intranda_workflow_fileupload_processcreation3_de.png)

An dieser Stellen können nun Dateien hochgeladen werden. Nach der Analyse der Dateinamen erzeugt Goobi die entsprechenden Vorgänge automatisch.

