---
description: >-
	Dieses Step Plugin erlaubt das automatische Generieren von Laufzetteln in verschiedenen Dateiformaten.
---

# Generierung von Laufzetteln

## Einführung

Dieses Plugin kann in beliebigen Aufgaben dafür verwendet werden, bei Abarbeitung der Aufgabe automatisch einen Laufzettel zu generieren und zu speichern. Das Plugin unterstützt PDF und TIFF-Dateien.

## Übersicht

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_save_docket_file |
| Source code | [https://github.com/intranda/goobi-plugin-step-save-docket-file](https://github.com/intranda/goobi-plugin-step-save-docket-file) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.04 |
| Dokumentationsdatum | 28.04.2022 |

## Installation

Zur Installation des Plugins muss folgende Datei vorhanden sein:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_save_docket_file.jar
```

Um zu konfigurieren, wie sich das Plugin verhalten soll, können verschiedene Werte in der Konfigurationsdatei angepasst werden. Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_save_docket_file.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

```markup
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

        <template file="/opt/digiverso/goobi/xslt/docket.xsl" />


        <!-- mimeType: image/tiff or application/pdf -->
        <!-- filename: name of the output file-->
        <!-- folder: name of the output folder, e.g. 'master' or 'media' -->
        <output mimeType="image/tiff" filename="00000000.tif" folder="master" />

        <!-- Set the number of dots per inch for the output file here. Common values are 300 or 600 -->
        <dotsPerInch>150</dotsPerInch>

    </config>
</config_plugin>
```

| Wert | Beschreibung |
| :--- | :--- |
| `project` |  Dieser Parameter legt fest, für welches Project der aktuelle `<config>` Block gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. Wird der Wert auf `*` gesetzt, so werden alle Projekte berücksichtigt. |
| `step` | Dieser Parameter legt fest, für welche Arbeitsschritte der `<config>` Block gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. Wird der Wert auf `*` gesetzt, so werden alle Arbeitsschrite berücksichtigt. |
| `template` | Dieses Element gibt an, welches Laufzettel-Vorlage für die Generierung verwendet werden soll. Mit dem `file` Parameter wird ein absoluter Dateipfad zu einer existierenden `.xsl` Datei angegeben. |
| `output` | Dieses Element gibt an, wo und wie die Zieldatei gespeichert werden soll. Mit dem `folder` Parameter wird der entsprechende Unterordner innerhalb des `images` Ordners eines Projektes angegeben. Hier kann zum Beispiel `master` oder `media` ausgewählt werden. Mit dem Parameter `filename` wird der Dateiname angegeben. Die Dateiendung muss dabei zum angegebenen MimeType passen. Dieser wird mit dem Parameter `mimeType` konfiguriert und kann entweder auf `image/tiff` oder `application/pdf` gesetzt werden. |
| `dotsPerInch` | Dieser Wert gibt die Auflösung des exportierten Dokumentes an. Die Einheit ist Pixel pro Zoll. Der Standardwert beträgt 150 DPI. |

## Anwendung des Plugins

Im Fall einer automatischen Aufgabe wird das Plugin automatisch ausgeführt, sobald die entsprechende Aufgabe ausgeführt wird. Es gibt zusätzlich einen Button bei der enstprechenden Aufgabe in den Vorgangs-Details, mit dem der Laufzettel jederzeit manuell generiert werden kann.
