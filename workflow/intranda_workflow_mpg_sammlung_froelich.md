---
description: >-
  Dieses Workflow Plugin wurde für den Massenimport der Sammlung Frölich aus einer Excel-Datei geschrieben.
---

# Massenimport aus Excel-Dateien mit EAD-Anreicherung

## Einführung
Dieses Workflow Plugin für Goobi workflow erlaubt einen Massenimport von Metadaten aus Excel-Dateien, wobeiVorgänge in Goobi erzeugt werden, zu denen jeweils eine METS-Datei gehört. 

Dieses Plugin wurde für den Import von Daten des Max-Planck-Instituts für Rechtsgeschichte und Rechtstheorie entwickelt. Im Rahmen des Imports ist eine Exceldatei vorgesehen, in der die zu erstellenden Vorgänge und Strukturelemente beschrieben werden. Das Plugin kann dabei auch Vorgänge aus mehreren Zeilen zusammensetzen. Die beschriebenen Strukturelemente werden dabei als Unterelemente dem Oberlement hinzugefügt. 


## Übersicht
| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow_mpg_sammlung_froelich |
| Source code | [https://github.com/intranda/goobi-plugin-mpg-sammlung-froelich](https://github.com/intranda/goobi-plugin-workflow-mpg-sammlung-froelich) |
| Lizenz | GPL 2.0 oder neuer |
| Dokumentationsdatum | 02.03.2023 |


## Installation
Das Plugin besteht aus folgenden Dateien:

```bash
plugin_intranda_workflow_mpg_sammlung_froelich.jar
plugin_intranda_workflow_mpg_sammlung_froelich-GUI.jar
```

Die Datei `plugin_intranda_workflow_mpg_sammlung_froelich.jar` muss in folgendes Verzeichnis kopiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/
```
Die Datei `plugin_intranda_workflow_hu_importer-GUI.jar` muss in folgendes Verzeichnis kopiert werden:
```bash
/opt/digiverso/goobi/plugins/GUI
```

Daneben gibt es die Konfigurationsdatei `plugin_intranda_workflow_mpg_sammlung_froelich-GUI.jar`, die in folgendem Ordner abgelegt werden muss:

```bash
/opt/digiverso/goobi/config/
```


## Konfiguration
Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_workflow_mpg_sammlung_froelich.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<config_plugin>
    <!--    
        import set for excel files (Attributes in [] are optional):
        - name: name of the export set, which will be displayed in the dropdonwnmenu
        - metadataFolder: where are the documents located
        - [mediaFolder]: where are the media files located
        - workflow: the workflow that shall be used
        - [project]: the goobi project that shall be used
        - mappingSet: the mapping that shall be used by the import set
        - [publicationType]: the publicationType that shall be used i.e. Monograph,... is only used as a fallback
        - [rowStart]: first row that will be read
        - [rowEnd]: last row that will be read
      -->
    
    <importSet name="Sammlung Froelich" 
        metadataFolder="/opt/digiverso/import/froelich/"
        mediaFolder ="/opt/digiverso/import/froelich/bilder/" 
        workflow="Sample_Workflow_mpi"
        project="Archive_Project" 
        mappingSet="froelich"
    />
    <!-- 
        mapping set -> set of fields that describe a mapping:
        - name = name of the MappingSet
         
        field - mapping for a row of an excel file (Attributes in [] are optional):
        - column: column of the xls-file that will be mapped
        - [label]: optional attribute to make configuration more comprehensible
        - [mets] mets of metadata element
        - type: person, metadata, media, processEntryAfter 
        - [separator]: default value is ;
        - [blankBeforeSeparator]: default value is false
        - [blankAfterSeparator]: default value is false
        - [gndColumn]: specify the column where the gnd normdata-id is located
        - regex: must contain a capture group, only the first capture group in the expression will be evaluated
    -->
    
    <!-- Metadata for structure elements (Image, Envelope, BundleOfPictures, Document) -->
    <mappingSet name="froelich">
        <field column="F" label="Titel (titel_lang)" type="metadata" mets="TitleDocMain"/>
        <field column="AE" label="Materialbeschreibung (medientyp)" type="metadata" mets="MaterialDescription"/>
        <field column="AJ" label="Sammlung (Einträge fehlen noch)" type="metadata" mets="singleDigCollection"/>
        
        <field column="U" label="Schlagwort 1 (motiv_kategorie)" type="metadata" mets="Subject" />
        <field column="M" label="Schlagwort 2 (Ort)" type="metadata" mets="City" gndColumn="N" />
        <field column="K" label="Schlagwort 3 (Landkreis)" type="metadata" mets="County" gndColumn="L"/>
        <field column="I" label="Schlagwort 4 (Bundesland)" type="metadata" mets="State" gndColumn="J"/>
        <field column="G" label="Schlagwort 5 (Staat)" type="metadata" mets="Country" gndColumn="H"/>

        <field column="X" label="Anmerkung 1 (anmerkungen)" type="metadata" mets="Note" />
        <field column="W" label="Anmerkung 2 (publiziert_in_aufgeloest)" type="metadata" mets="Note" />
        <field column="AD" label="Anmerkung 3 (provenienz)" type="metadata" mets="Provenience" />
	    <field column="Z" label="Dateiname" type="media" ignoreInSubRow="false" />
        
        <field column="B" label="Identifikator" type="metadata" mets="CatalogIDDigital" />
        <!-- plugin related fields -->
        <field column="AC" label="dateiname 3" type="subRow" regex="\d+([a-z])" />  
        <field separator="mpilhlt_sf_" column="Y" type="processEntryAfter" />
        <field column="B" type="processName" />       
        <field column="AF" type="structureType" />

        <!-- use of regex -->
        <field column="E" label="dia_nr (kasten)" type="metadata" mets="Note" regex="SF=M0*(\d+)_\d+" />
        <field column="E" label="dia_nr" type="metadata" mets="Note" regex="SF=[FM]\d+_0*(\d+)" />

        <!-- old URLs -->
        <field column="C" label="alte URL (fixed)" type="metadata" mets="Note" ignoreInSubRow="false"/>
        <field column="D" label="alte URL (broken)" type="metadata" mets="Note" ignoreInSubRow="false"/>
    </mappingSet>

    <structureTypeMapping>
	    	<entry key="Bild" mets="Image" />
	    	<entry key="Umschlag" mets="Envelope" />
	    	<entry key="Bildkonvolut" mets="BundleOfPictures" />
	    	<entry key="Textdokument" mets="Document" />
    </structureTypeMapping>
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
|`rowStart`| Festlegung für die erste Zeile der Excel-Datei im metadataFolder, die ausgewertet werden soll. Das Attribut ist optional.|
|`rowEnd`| Festlegung für die letzte Zeile der Excel-Datei im metadataFolder, die ausgewertet werden soll. Das Attribut ist optional. Wird als Wert `0` angegeben, wird die vollständige Datei ausgewertet. |

### Die Elemente mappingSet und field
Ein Element des Typs `mappingSet` verfügt nur über das Attribut `name`. Damit kann es dann für das Mapping direkt angesprochen werden. Als Unterelemente sind beliebig viele Elemente `field` erlaubt.


| Attribut | Beschreibung |
| :--- | :--- |
|`column`| Spalte(n) die gemappt werden soll(en). Der eingelesene Werte wird dann dem Metadatum zugewiesen, das im Attribut `mets` definiert wurde. Alternativ kann der Inhalt der Zelle auch einer Vorgangseigenschaft wie beispielsweise dem Vorgangstitel zugeordnet werden, wenn als type `ProcessName` spezifiziert wird. Wenn mehrere Spalten in einem Wert abgebildet werden sollen, können diese einfach mit Komma getrennt aufgeführt werden (z.B. `A,B,AA`). Das Attribut ist obligatorisch. |
|`label`| Hier kann optional der Spaltentitel angegeben werden. Er wird vom Plugin nicht ausgewertet und dient nur der Dokumentation. |
|`mets`| Dieses Attribut legt fest, welchem Metadatentyp der eingelesene Wert zugeordnet werden soll. Zulässig sind hier alle Werte, die laut Regelsatz ein gültiges Metadatum für das entsprechende Strukturelements sind. |
|`type`| Dieser Parameter ist obligatorisch und kann die Werte: `person`, `metadata`, `media` und `processEntryAfter`annehmen. Die Werte werden unten näher erläutert. |
|`gndColumn`| Für Mappings deren Attribut `type` den Wert `metadata` oder `person` annimmt, kann hier die Spalte angegeben werden, in der sich eine Normdatenbank-URL befindet, die den Datensatz näher beschreibt (z.B. https://d-nb.info/gnd/118551310 oder nur 118551310)  .Der Normdatensatz wird dann mit dem Metadatum verknüpft. |
|`separator`| Dieser Separator wird verwendet, wenn mehrere Elemente in einem Wert abgebildet werden sollen. Der Standardwert ist `;`.  Das Attribut ist optional.|
|`blankBeforeSeparator`| Falls der Inhalt mehrerer Spalten in einen Wert gemappt werden soll, kann hier bestimmt werden, ob ein Leerzeichen vor dem Separator gesetzt werden soll. Der Standardwert ist `false`.|
|`blankAfterSeparator`| Falls der Inhalt mehrerer Spalten in einen Wert abgebildet werden soll, kann hier bestimmt werden, ob ein Leerzeichen nach dem Separator gesetzt werden soll. Der Standardwert ist `false`. |
|`subRow`|Hier kann man spezifizieren in welcher Spalte sich das Element befindet anhand dessen subrows identifiziert werden sollen.|
|`ignoreInSubRow`|Der Standardwert ist true. Wenn mehrere Zeilen auf dasselbe Strukturelement abgebildet werden, können Sie hier entscheiden, ob Sie die Metadaten der Unterelemente auf ihre übergeordneten Elemente abbilden wollen|

#### Werte des Type-Attributes
| Wert | Beschreibung |
| :--- | :--- |
|`metadata`| Ein Element vom Typ `metadata` wird dem im Attribut `mets` spezifizierten Metadatum in der METS-Datei zugeordnet |
| `person`| Hierbei handelt es sich um einen METS-Datentyp. Wenn der Typ `person` verwendet wird, sollte immer auch das Attribut `mets` gesetzt werden. |
|`media`| In der angegebenen Spalte müssen sich ein oder mehrere Dateinamen befinden. Der Seperator ist hierbei normalerweise `,`. Er kann aber bei Bedarf durch Verwendung des Attributes `separator` anders festgelegt werden. Es wird davon ausgegangen, dass sich die Datei im Verzeichnis `mediaFolder` befindet. Siehe hierzu bei der Konfiguration von `Importset`. |
|`structureType`| Mit dem Typ `structureType` kann der MODS-Typ des Strukturelemetes in der Exceltabelle bestimmt werden. Bei Bedarf kann der String in Zelle mit der Hilfe des `structureTypeMapping` auf einen anderen Wert gemappt werden.|

### Die Elemente structureTypeMapping und entry

Ein `structureTypeMapping` kann mehrere entry Elemente haben. Ein Ein `entry` Element hat die Attribute `key` und `mods`.

| Wert | Beschreibung |
| :--- | :--- |
| `key`| Wenn dieser Wert in der Zelle steht, wird er durch den im Attribut `mods` hinterlegten Wert ersetzt|
| `mods`| Strukturtyp der verwendet wird, wenn der im Key hinterlegte Wert in der Zelle steht|

## Benutzung des Plugins
Nach der Installation und Inbetriebnahme des Plugins steht dieses innerhalb des Menüs `Workflow` zur Verfügung. Nach dem Aufruf kann ein ImportSet ausgewählt werden und der Datenimport gestartet werden. 

Die im `mediaFolder` liegenden Dateien werden während des Imports in die Verzeichnisse der erzeugten Vorgänge kopiert.

![Nutzeroberfläche des Plugins](../.gitbook/assets/intranda_workflow_hu_importer_de.png)

