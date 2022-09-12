---
description: >-
  Dieses Workflow Plugin für Goobi workflow erlaubt den Massenimport von Daten ausgehend von Metadaten innerhalb von Excel-Dateien.
---

# Massenimport aus Excel-Datien mit EAD-Anreicherung

## Einführung
Dieses Workflow Plugin für Goobi workflow erlaubt einen Massenimport von Metadaten aus Excel-Dateien, wobei nicht nur Vorgänge in Goobi erzeugt werden, zu denen jeweils eine METS-Datei gehört. Zusätzlich wird ausserdem eine vorliegende EAD-Datei mit weiteren Knoten angereichert. 

Dieses Plugin wurde für den Import von Daten der HU Berlin zur Anreicherung des Sammlungsportals entwickelt. In der Regel kommen bei einem solchen Import mindestens zwei verschiedene Exceldateien unterschiedlichen Typs zum Einsatz. Typ 1 spezifiziert dabei die Metadaten für das Objekt selbst (TopStruct mit Metadaten wie Haupttitel, Schlagworte usw.) und Daten für die Eigenschaften des Vorgangs (z.B. zukünftiger Name des Vorgangs). Der Typ 2 hingegen spezifiziert in der Regel diejenigen Strukturelemente, die als Unterelemente innerhalb der METS-Datei erzeugt werden sollen und welche Metadaten diese erhalten sollen (im Fall der HU Berlin z.B. Kapitel/Lochkarte, Name einer Mediendatei usw.).


## Übersicht
| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_hu\_importer |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-hu-importer](https://github.com/intranda/goobi-plugin-workflow-hu-importer) |
| Lizenz | GPL 2.0 oder neuer |
| Dokumentationsdatum | 01.06.2022 |


## Installation
Das Plugin besteht aus folgenden Dateien:

```bash
plugin_intranda_workflow_hu_importer.jar
plugin_intranda_workflow_hu_importer-GUI.jar
```

Die Datei `plugin_intranda_workflow_hu_importer.jar` muss in folgendes Verzeichnis kopiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/
```
Die Datei `plugin_intranda_workflow_hu_importer-GUI.jar` muss in folgendes Verzeichnis kopiert werden:
```bash
/opt/digiverso/goobi/plugins/GUI
```

Daneben gibt es die Konfigurationsdatei `plugin_intranda_workflow_hu_importer.xml`, die in folgendem Ordner abgelegt werden muss:

```bash
/opt/digiverso/goobi/config/
```


## Konfiguration
Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_workflow_hu_importer.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<config_plugin>

	<!--import set for excel files
	Attributes in [] are optional
		name: name of the import set, which will be displayed in the dropdonwnmenu
		metadataFolder: where are the documents located
		[mediaFolder]: where are the media files located
		workflow: the workflow that shall be used
		[project]: the goobi project that shall be used
		mappingSet: the mapping that shall be used by the import set
		publicationType: the publicationType that shall be used i.e. Monograph,...
		structureType: structureType the metadata will be mapped to
		[importSetDescription]: Path to xls-file with description of importset-files
		[descriptionMappingSet]: Mapping for the importsetdescription file
		[eadType]: Type of the EAD-entry
		[eadFile]: Name of the EAD-file/database
		[eadNode]:"ID of the parent node that this Imporset will use"
		[rowStart]: first row that will be read
		[rowEnd]: last row that will be read
	  -->
  <importSet name="Lochkarten EAD"
		metadataFolder="/opt/digiverso/import/mdvos/"
		mediaFolder ="/opt/digiverso/import/mdvos/Lochkartei_Testdaten"
		workflow="Sample_Workflow"
		project="Archive_Project"
		mappingSet="Lochkarten"
		publicationType="Trenner"
		structureType="Lochkarte"
		importSetDescription="/opt/digiverso/import/description/description.xlsx"
		descriptionMappingSet="Description"
		eadType="file"
		eadFile="EAD-Store - Sudanarchäologie.xml"
		eadNode="f91585c7-9cd4-47f1-b9e4-5f26a6744fe3"/>

	<!-- mapping set -> set of fields that describe a mapping
		name = name of the MappingSet

		field - mapping for a row of an excel file
		Attributes in [] are optional
		column: column of the xls-file that will be mapped
        [label]; column header
        [mets] mets of metadata element
        type: person, metadata, media   //maybe add eadonly
        [separator]: default value is ;
        [blankBeforeSeparator]: default value is false
        [blankAfterSeparator]: default value is false
        [ead] name of the ead metadata type
	-->
	<mappingSet name="Lochkarten">
		<field column="D" label="Inventar-Nr./Signatur" type="metadata" mets="shelfmarksource"/>
		<field column="E" label="Institutionsnachweis/ Sammlung" type="metadata" mets="PhysicalLocation"/>
		<field column="F" label="Titel" type="metadata" mets="TitleDocMain"/>
		<field column="G" label="Schlagwort" type="metadata" mets="Subject"/> <!--mets="Subject"-->
		<field column="H" label="Schlagwort" type="metadata" mets="Subject"/>
		<field column="I" label="Material" type="metadata" mets="FormatSourcePrint"/>
		<field column="J" label="Maße" type="metadata" mets="SizeSourcePrint"/>
		<field column="K" label="Trenner" type="metadata" mets="OtherTitle"/>
		<field column="L" label="Geokoordinaten" type="metadata" mets=""/>
		<field column="M,N" label="Datei recto" type="metadata" mets="IdentifierRelatedWork"/>
		<field column="M,N" label="Datei recto" type="media" mets="IdentifierRelatedWork"/>
	</mappingSet>

	<mappingSet name="Description">
		<field column="A" label="Dateiname" type="FileName" />
		<field column="B" label="Titel" type="metadata" mets="TitleDocMain"  ead="unittitle"/>
		<field column="D" label="Prozessname" type="ProcessName"/>
		<field column="E" label="Date" type="metadata" ead="unitdate" />
	</mappingSet>

</config_plugin>
```

### Elementtypen
Das Plugin kennt zunächst die folgenden Elementtypen.

| Element | Beschreibung |
| :--- | :--- |
| `config_plugin`  | `config_plugin` ist das Hauptelement der Konfigurationsdatei und enthält alle anderen Elemente  |
| `importSet`  | Ein ImportSet beschreibt gleichartige Importvorgänge.   |
| `mappingSet`  |  Ein MappingSet besteht aus einer Menge von `field`-Elementen.|
| `field`  | `field`-Elemente sind Kindelemente von `mappingSet`. Jedes Element ordnet einer Spalte des Exceldokumentes eine Eigenschaft des zu erzeugenden Vorgangs oder dessen Metadaten zu.  |

### Element: importSet

| Attribut | Beschreibung |
| :--- | :--- |
| `name` | Name des ImportSets. Dieser wird später innerhalb der Nutzeroberfläche angezeigt. |
|`metadataFolder`| Hier muss angegeben werden, wo sich der Ordner mit den Dateien befindet, die mit dem im Attribut `mappingSet` spzefizierten MappingSet gemappt werden.|
|`mediaFolder`| Dieses optionale Attribut gibt an, wo sich die Mediendateien befinden. |
|`workflow`| Hier muss angegeben werden, welche Produktionsvorlage zum Anlegen der Vorgänge verwendet werden soll. |
|`project`| Hier kann angegeben werden, welchem Projekt der Vorgang zugeordnet werden soll. Das Attribut ist optional. Wird kein Projekt angegeben, wird das in der Produktionsvorlage spezifizierte Projekt verwendet. |
|`mappingSet`| Hier muss angegeben werden, mit welchen Mapping die Datei im `metadataFolder` verarbeitet werden sollen.|
|`publicationType`| Der Publikationstyp des Vorgangs (z.B. Monographie) |
|`structureType`| Der Strukturtyp der Elemente, die im Vorgang als Unterelement angelegt werden sollen (z.B. Kapitel oder Lochkarten). Jede Zeile eines Exceldokumentes des Typs 2 beschreibt dabei ein Element dieses Strukturtyps. |
|`importSetDescription`| Dieses optionale Attribut gibt an, wo sich die Datei mit den Metadaten zu den Vorgängen befindet. Ist der Parameter gesetzt, muss zu jeder Datei im `metadataFolder` eine Zeile in diesem Dokument spezifiziert sein. |
|`descriptionMappingSet`| Hier kann das MappingSet für die Datei mit der ImportSet-Beschreibung spezifiziert werden. Das Attribut ist optional.|
|`eadType`| Typ des EAD-Knotens (z.B. file). Das Attribut ist optional.|
|`eadFile`| Name der EAD-Datei/Datenbank. Das Attribut ist optional.|
|`eadNode`| ID des Parent-Knotens, unterhalb dessen der neue EAD-Knoten eingefügt werden soll. Das Attribut ist optional.|
|`rowStart`| Festlegung für die erste Zeile der Excel-Datei im metadataFolder, die ausgewertet werden soll. Das Attribut ist optional.|
|`rowEnd`| Festlegung für die letzte Zeile der Excel-Datei im metadataFolder, die ausgewertet werden soll. Das Attribut ist optional. Wird als Wert `0` angegeben, wird die vollständige Datei ausgewertet. |
|`useFileNameAsProcessTitle`| Wenn dieser optionale Parameter auf `true` gesetzt wird, wird der Dateiname als Prozesstitel verwendet. |


### Die Elemente mappingSet und field
Ein Element des Typs `mappingSet` verfügt nur über das Attribut `name`. Damit kann es dann für das Mapping direkt angesprochen werden. Als Unterelemente sind beliebig viele Elemente `field` erlaubt.


| Attribut | Beschreibung |
| :--- | :--- |
|`column`| Spalte(n) die gemappt werden soll(en). Der eingelesene Werte wird dann dem Metadatum zugewiesen, das im Attribut `mets` definiert wurde. Alternativ kann der Inhalt der Zelle auch einer Vorgangseigenschaft wie beispielsweise dem Vorgangstitel zugeordnet werden, wenn als type `ProcessName` spezifiziert wird. Wenn mehrere Spalten in einem Wert abgebildet werden sollen, können diese einfach mit Komma getrennt aufgeführt werden (z.B. `A,B,AA`). Das Attribut ist obligatorisch. |
|`label`| Hier kann optional der Spaltentitel angegeben werden. Er wird vom Plugin nicht ausgewertet und dient nur der Dokumentation. |
|`mets`| Dieses Attribut legt fest, welchem Metadatentyp der eingelesene Wert zugeordnet werden soll. Zulässig sind hier alle Werte, die laut Regelsatz ein gültiges Metadatum für das entsprechende Strukturelements sind. |
|`type`| Dieser Parameter ist obligatorisch und kann die Werte: `person`, `personWithGnd`, `metadata`, `media`, `FileName` und `ProcessName`  annehmen. Die Werte werden unten näher erläutert. |
|`separator`| Dieser Separator wird verwendet, wenn mehrere Elemente in einem Wert abgebildet werden sollen. Der Standardwert ist `;`.  Das Attribut ist optional.|
|`blankBeforeSeparator`| Falls der Inhalt mehrerer Spalten in einen Wert gemappt werden soll, kann hier bestimmt werden, ob ein Leerzeichen vor dem Separator gesetzt werden soll. Der Standardwert ist `false`.|
|`blankAfterSeparator`| Falls der Inhalt mehrerer Spalten in einen Wert abgebildet werden soll, kann hier bestimmt werden, ob ein Leerzeichen nach dem Separator gesetzt werden soll. Der Standardwert ist `false`. |
|`ead`| Wenn dieser Parameter gesetzt ist, wird der Inhalt der Zelle diesem EAD-Metadatentyp zugeordnet. |

#### Werte des Type-Attributes
| Wert | Beschreibung |
| :--- | :--- |
|`metadata`| Ein Element vom Typ `metadata` wird dem im Attribut `mets` spezifizierten Metadatum in der METS-Datei zugeordnet |
| `person`| Hierbei handelt es sich um einen METS-Datentyp. Wenn der Typ `person` verwendet wird, sollte immer auch das Attribut `mets` gesetzt werden. |
|`personWithGnd` | Hierbei handelt es sich um eine Spezialisierung von `person`. Das Plugin geht davon aus, dass sich in der letzten Spalte die `GND-ID` befindet. Es können entweder 2 oder 3 Spalten angegeben werden. Werden nur 2 Spalten angegeben geht, das Plugin davon aus, dass sich in der ersten Spalte `Vorname` und `Nachname` befinden.  |
|`media`| In der angegebenen Spalte muss sich ein Dateiname befinden. Es wird davon ausgegangen, dass sich die Datei im `mediaFolder` befindet -> siehe `Importset`.|
|`FileName`| Dieser Typ muss verwendet werden, um die Spalte mit dem Dateinamen der Prozessbeschreibung anzugeben. Dieser Feldtyp ist also nur in einem `descriptionMappingSet` sinnvoll.  |
|`ProcessName` | Dieser Typ muss verwendet werden, um die Spalte mit dem zukünftigen Prozessnamen zu spezifizieren.  |


## Benutzung des Plugins
Nach der Installation und Inbetriebnahme des Plugins steht dieses innerhalb des Menüs `Workflow` zur Verfügung. Nach dem Aufruf kann ein ImportSet ausgewählt werden und der Datenimport gestartet werden. Das Plugin wird versuchen im `metadataFolder` die Ordner `processed`und `failure` anzulegen. Es sollte also darauf geachtet werden, dass Goobi in diesem Ordner Schreibrechte hat. Wird eine Datei ohne Fehler eingelesen, wird Sie in den Ordner `processed` verschoben. Falls ein Fehler auftritt, landet sie im Ordner `failure`. 

Die im `mediaFolder` liegenden Dateien werden während des Imports in die Verzeichnisse der erzeugten Vorgänge kopiert.

![Nutzeroberfläche des Plugins](../.gitbook/assets/intranda_workflow_hu_importer_de.png)
