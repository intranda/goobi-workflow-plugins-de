---
Beschreibung: >-
  Dieses Workflow-Plugin ermöglicht den Massenimport von einzelnen Seiten des Volksblatts aus Liechtenstein.
---

# Workflow-Plugin für den Import der Zeitungsseiten vom Liechtensteinen Volksblatt

## Einführung

Dieses Workflow-Plugin wurde implementiert, um Metadaten aus den Dateinamen sowie einer Konfigurationsdatei zu lesen und Prozesse und Metadaten korrekt zu erstellen oder zu aktualisieren. Dieses Plugin wurde ursprünglich für den Import von Zeitungsseiten aus dem Liechtensteiner Volksblatt entwickelt, kann aber auch für andere Zeitungsimportaufgaben verwendet werden, solange deren Seitennamen dem gleichen Muster wie `001_vbhp_4c_2019-01-11` folgen, wobei die ersten drei Ziffern die Ordnungsnummer dieser Seite innerhalb ihrer Ausgabe angeben und das abschließende Datum das Datum der Ausgabe ist. Der dazwischen liegende Substring der Beschreibung spielt keine Rolle, solange er NICHT mit dem regulären Ausdruck `\d{4}-\d{2}-\d{2}` übereinstimmt, der für die Speicherung des Ausgabedatums reserviert ist.

## Übersicht

| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_liechtenstein\_volksblatt\_importer |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer](https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 23.10 |
| Dokumentationsdatum | 16.11.2023 |

## Installation

* Für die Installation benötigt man `plugin_intranda_workflow_liechtenstein_volksblatt_importer.jar` und `plugin_intranda_workflow_liechtenstein_volksblatt_importer-GUI.jar`
```bash
{PATH_TO_GOOBI}/plugins/workflow/plugin_intranda_workflow_liechtenstein_volksblatt_importer.jar
{PATH_TO_GOOBI}/plugins/GUI/plugin_intranda_workflow_liechtenstein_volksblatt_importer-GUI.jar
```
* Die Konfigurationsdatei `plugin_intranda_workflow_liechtenstein_volksblatt_importer.xml` sollte kopiert zu:
```bash
{PATH_TO_GOOBI}/config/plugin_intranda_workflow_liechtenstein_volksblatt_importer.xml
```

## Konfiguration

Die Konfigurationen sollten in der Konfigurationsdatei vorgenommen werden, die wie das folgende Beispiel aussehen kann:

```xml
<config_plugin>
	<!-- which folder to use as import source -->
	<importFolder>/opt/digiverso/import/sample/</importFolder>

	<!-- which workflow to use -->
	<workflow>Newspaper_workflow</workflow>

	<!-- Whether or not to delete the images from the import folder once they are imported. OPTIONAL. DEFAULT false. -->
	<deleteFromSource>true</deleteFromSource>

	<!-- Configure here the metadata that shall be added to the anchor file or the volume part of the mets file. -->
	<!-- This tag accepts the following attributes:
		- @value: metadata value template, which may contain a variable defined by @var wrapped with _ from both sides
		- @type: metadata type
		- @var: variable that is ready to be used in @value. To use it, wrap it with _ from both sides and put it into the @value string. OPTIONAL.
					- Options are YEAR | MONTH | DAY | DATE | DATEFINE, where cases only matters for the references in @value string.
					- The only difference between DATE and DATEFINE are their representations of the date: DATE keeps the original format "yyyy-mm-dd" while DATEFINE takes a new one "dd. MMM. yyyy".
					- The value of an unknown variable will be its name.
					- For example,
						- IF one defines a @var by "year", then it should be referenced in @value using "_year_"
						- IF one defines a @var by "YEAR", then it should be referenced in @value using "_YEAR_", although YEAR and year are actually the same option
						- IF one defines a @var by "unknown", then any occurrences of "_unknown_" will be replaced by "unknown"
						- IF one wants to have a @value template containing "_Year_" as hard-coded, then "Year" should be avoided to be @var. If in such cases such a
						  variable is still needed, then one can define @var to be something like "yEaR".
		- @anchor: true if this metadata is an anchor metadata, false if not. OPTIONAL. DEFAULT false.
		- @volume: true if this metadata is a volume metadata, false if not. OPTIONAL. DEFAULT false.
		- @person: true if this metadata is a person metadata, false if not. OPTIONAL. DEFAULT false.
	 -->
	<metadata value="CHANGE_ME" type="TitleDocMain" anchor="true" volume="false" person="false" />
	<!-- The @volume attribute is by default false. -->
	<metadata value="CHANGE_ME" type="CatalogIDSource" anchor="true" person="false" />
	<!-- The @person attribute is by default false. -->
	<metadata value="CHANGE_ME" type="CatalogIDDigital" anchor="true" />

	<metadata value="CHANGE_ME" type="CurrentNoSorting" anchor="false" volume="true" />
	<!-- The @anchor attribute is by default false. -->
	<metadata value="zeitungen#livb" type="SubjectTopic" volume="true" />
	<metadata value="pt_zeitung" type="Publikationstyp" volume="true" />
	<metadata value="zeitungsherausgeber#livb" type="Classification" volume="true" />
	<metadata value="eli" type="ViewerInstance" volume="true" />

	<!-- Use @var to generate the final metadata values in the run. -->
	<metadata var="YEAR" value="Newspaper Volume _YEAR_" type="CatalogIDDigital" volume="true" />
	<metadata var="YEAR" value="Liechtensteiner Volksblatt (_YEAR_)" type="TitleDocMain" volume="true" />
	<metadata var="YEAR" value="_YEAR_" type="CurrentNo" volume="true" />
	<metadata var="YEAR" value="_YEAR_" type="PublicationYear" volume="true" />

</config_plugin>
```

| Wert | Beschreibung |
| :--- | :--- |
| `importFolder` | Pfad zu dem Ordner, der die separierten Zeitungsseiten für den Import enthält. |
| `workflow` | Name des Musterworkflows. |
| `deleteFromSource` | Bei `true` werden die Dateien aus dem Importordner gelöscht, sobald sie erfolgreich importiert wurden, andernfalls bleiben alle Dateien im Importordner unberührt. |
| `metadata` | Aus jedem Tag wird ein Metadatenobjekt erstellt. Er akzeptiert sechs Attribute, wobei `@value` und `@type` obligatorisch sind, während `@var`, `@anchor`, `@volume` und `@person` alle optional sind. Weitere Einzelheiten finden Sie in den Kommentaren in der Beispielkonfiguration. |

## Gebrauchsanweisung

* Diesem Plugin ist es egal, welche Dateiformate die zu importierenden Zeitungsseiten haben, da alle Metadaten, die abgespeichert werden müssen, direkt aus den Dateinamen sowie aus der Konfigurationsdatei gelesen werden. Die Seitendateien werden in die Master-Ordner der entsprechenden Goobi-Prozesse verteilt.
* Die Dateiformate in den Dateiverknüpfungen, die von diesem Plugin in die METS-Datei abgespeichert werden, werden in `tiff` und `jpg` geändert, da nur diese vom Metadaten-Editor richtig wiedergegeben werden können. Wenn man die Seiten nach dem Import richtig sehen will, muss man einen Konvertierungsschritt nach dem Import hinzufügen. ***ACHTUNG***: auch wenn dieser Konvertierungsschritt automatisch ablaufen soll, muss man ihn manuell auslösen, alternativ kann man einen leeren Schritt zwischen dem Importschritt und diesem Konvertierungsschritt einfügen und den Status dieses leeren Schrittes manuell ändern, wenn der Import abgeschlossen ist.
* Folgendes ist ein Beispiel zur Veranschaulichung der Erstellung und Verwendung eines solchen Konvertierungsschritts, im ursprünglichen Anwendungsfall für das Liechtensteiner Volksblatt, wobei die ursprünglichen Zeitungsseiten das Format PDF haben:

  1. Installieren Sie `pdftoppm`, falls noch nicht geschehen
  2. Legen Sie die folgende Skriptdatei in `{PATH_TO_GOOBI}/scripts/script_convertPdfToTiff.sh`
    ```bash
    #!/bin/bash

    # first variable: folder
    folder="$1"

    # change directory
    cd $folder

    # convert all pdf files to tiff
    for pdfFile in *.pdf; do
       pdftoppm $pdfFile -tiff -r 300 -cropbox ${pdfFile%.pdf};
    done

    # remove the stupid -1 added by pdftoppm
    for tifFile in *.tif; do
       mv -- "$tifFile" "${tifFile%-1.tif}.tif";
    done

    # delete all pdf files
    rm *.pdf
    ```
  3. Erstellen Sie ein Skriptschritt mit dem Skriptpfad `{PATH_TO_GOOBI}/scripts/script_convertPdfToTiff.sh "{origpath}"`
