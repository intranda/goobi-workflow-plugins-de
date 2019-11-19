---
description: >-
  Dies ist die technische Dokumentation für das Plugin zum Import von
  Excel-Dateien.
---

# Import von Datensätzen aus einer Excel-Datei

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Plugins für den Massenimport von Datensätzen aus Excel-Dateien.

| Details |  |
| :--- | :--- |
| Version des Plugins | 1.0.0 |
| Identifier | intranda\_import\_excel\_read\_headerdata |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi workflow 3.0 und neuer |
| Dokumentation vom | 02.10.2019 |

## Installation

Das Plugin muss in den folgenden Ordner installiert werden:

```text
/opt/digiverso/goobi/plugins/import/plugin_intranda_import_generic_excel.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```text
/opt/digiverso/goobi/config/plugin_intranda_import_excel_read_headerdata.xml
```

## Konfiguration 

Die Konfiguration erfolgt über die Datei `plugin_intranda_import_excel_read_headerdata.xml`. Diese Datei kann im laufenden Betrieb angepasst werden.

```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <config>
        <!-- which workflow template shall be used -->
        <template>*</template>
 
        <!-- which digital collection to use -->
        <collection>Example collection</collection>
 
        <!-- define if an opac request is made -->
        <useOpac>true</useOpac>
        <!-- name of the configured catalogue -->
        <opacName>K10Plus</opacName>
        <!-- field to search in -->
        <searchField>12</searchField>
 
        <!-- publication type to create -->
        <publicationType>Monograph</publicationType>
 
        <!-- define in which row the header is written, usually 1 -->
        <rowHeader>1</rowHeader>
        <!-- define in which row the data starts, usually 2 -->
        <rowDataStart>2</rowDataStart>
        <!-- define in which row the data ends, usually 20000 -->
        <rowDataEnd>20000</rowDataEnd>
 
        <!-- define which column is the one to use for catalogue requests and to identify the row during the import -->
        <identifierHeaderName>Identifier</identifierHeaderName>
 
        <!-- Rules to generate the process title, the same syntax as in goobi_projects.xml can be used. 
            Use the column names to get the right metadata values. 
            If the field is missing or empty, the value of the identifier column is used.
         -->
        <processTitleRule>'StaticPrefix_'+Identifier</processTitleRule>
 
        <!-- define which column contains the image folder name. Can be combined with <imageFolderPath> prefix or an absolute path. 
        If the field is missing, empty or does not contain an existing directory, no images will be imported -->
        <imageFolderHeaderName>image folder</imageFolderHeaderName>
        <!-- prefix path to the image folder. Can be empty or missing if the import doesn't contain images or if the excel field contains absolute path  -->
        <imageFolderPath>/mnt/images/</imageFolderPath>
 
        <!-- defines, if images are moved from the source folder to the destination (true) or copied (false) -->
        <moveImages>true</moveImages>
 
        <!-- Run the import as GoobiScript -->
        <runAsGoobiScript>true</runAsGoobiScript>
 
        <!-- define which columns shall be mapped to which ugh metadata 
            ugh: name of the metadata to use 
            headerName: title inside of the header column
            name: name of the process property. if it is empty or missing, no process property gets generated
            normdataHeaderName: title of the header column to use for a gnd authority identifier
            docType: define if the metadata should be added to the anchor or child element. Gets ignored, when the record is no multivolume. Default is 'child', valid values are 'child' and 'anchor'
             -->
 
        <metadata ugh="TitleDocMain"  headerName="Haupttitel" />
        <metadata name="Schrifttyp" headerName="Schrifttyp" />
        <metadata ugh="PlaceOfPublication" name="Ort" normdataHeaderName="Ort-GND" headerName="Ort" docType="anchor" />
 
        <!-- a configuration for a person might look like this -->
        <person ugh="Author" normdataHeaderName="Person-GND" docType="child">
            <!-- use this field if the column contains the complete name -->
            <nameFieldHeader>Person</nameFieldHeader>
            <!-- set this field to true, if the name must be splitted into first- and lastname. The complete name gets written into lastname -->
            <splitName>true</splitName>
            <!-- define at which character the name is separated. @firstNameIsFirstPart defines, if the firstname is the first or last part of the name -->
            <splitChar firstNameIsFirstPart="false">\, </splitChar>
        </person>
        <person ugh="Author" docType="child">
            <!-- use this fields, if the firstname and lastname are in different columns -->
            <firstname>Vorname</firstname>
            <lastname>Nachname</lastname>
        </person>
 
        <!-- a metadata group can contain sub configurations for the metadata and persons within this group -->
        <group ugh="Publishing" docType="child">
            <metadata ugh="Publisher"  headerName="Verlag" />
            <metadata ugh="PlaceOfPublication"  headerName="Ort" />
            <metadata ugh="PublicationYear"  headerName="Jahr" />
        </group>
    </config>
</config_plugin>
```

### Individuelle Konfigurierbarkeit

Es ist sowohl möglich, eine globale Konfiguration für alle Produktionsvorlagen, als auch individuelle Einstellungen für einzelne Produktionsvorlagen zu erstellen. Dazu kann das Element `config` in der XML Datei wiederholt werden. Wenn in Goobi der Massenimport ausgewählt wurde, wird jeweils derjenige Konfigurationsblock gesucht, bei dem im Element `template` der Name der ausgewählten Produktionsvorlage steht. Existiert solch ein Eintrag nicht, wird die `default` Konfiguration verwendet. Diese ist durch `*` gekennzeichnet.

```markup
<!-- which workflow template shall be used -->
<template>*</template>
```

### Sammlung

Mit dem optionalen Element `collection` ist es möglich, eine Sammlung zu definieren, die in alle Datensätze eingefügt werden soll. Daneben können aber auch Sammlungen aus der Oberfläche ausgewählt werden, oder die Sammlung kann als Teil der Excel-Datei oder aus dem Katalog mit importiert werden.

```markup
<!-- which digital collection to use -->
<collection>Example collection</collection>
```

### Katalogimport

Die nächsten drei Elemente `useOpac`, `opacName` und `searchField` steuern, ob während des Imports eine Katalogabfrage durchgeführt werden soll. Wenn `useOpac` den Wert `true` enthält, findet eine solche Abfrage statt. Hierzu werden der Katalog und das Suchfeld verwendet, die in den anderen beiden Feldern konfiguriert wurden. Der Name des Katalogs muss dabei einem Eintrag aus der Goobi-Konfigurationsdatei `goobi_projects.xml` entsprechen. Der Strukturtyp wird dabei automatisch anhand den OPAC-Daten erkannt.

```markup
<!-- define if an opac request is made -->
<useOpac>true</useOpac>
<!-- name of the configured catalogue -->
<opacName>K10Plus</opacName>
<!-- field to search in -->
<searchField>12</searchField>
```

Wird hingegen kein OPAC genutzt, muss der Strukturtyp der anzulegenden Vorgänge im Feld `publicationType` angegeben werden. Der hier verwendete Name muss entsprechend innerhalb des Regelsatzes existieren. Wenn der OPAC genutzt werden soll, wird dieses Feld nicht ausgewertet.

```markup
<!-- publication type to create -->
<publicationType>Monograph</publicationType>
```

### Zeilenbereich

Die folgenden Elemente beschreiben den Aufbau der zu importierenden Excel-Datei.

In `rowHeader` wird definiert, in welcher Zeile die Spaltenüberschriften eingetragen wurden, die später für das Mapping relevant sind. Üblicherweise ist dies die erste Zeile. Dies kann bei mehrzeiligen Angaben jedoch auch davon abweichen.

```markup
<!-- define in which row the header is written, usually 1 -->
<rowHeader>1</rowHeader>
```

`rowDataStart` und `rowDataEnd` beschreiben den Bereich, der die Daten enthält. Üblicherweise sind dies die Zeilen, die direkt dem `rowHeader` folgen, bei besonderen Formatierungen können jedoch auch Leerzeilen enthalten sein, die hierüber entfernt werden können.

```markup
<!-- define in which row the data starts, usually 2 -->
<rowDataStart>2</rowDataStart>
<!-- define in which row the data ends, usually 20000 -->
<rowDataEnd>20000</rowDataEnd>
```

### Identifier

Der Eintrag `identifierHeaderName` enthält die Überschrift derjenigen Spalte, in der ein Identifier enthalten ist. Dieses Feld wird intern zur Identifikation der Zeilen genutzt. Bei einer OPAC Abfrage wird dieser Wert verwendet. Darüber hinaus wird dieser Wert ebenso für die Generierung des Vorgangstitels genutzt, wenn keine andere Generierung für Vorgangstitel angegeben wurde.

```markup
<!-- define which column is the one to use for catalogue requests and to identify the row during the import -->
<identifierHeaderName>Identifier</identifierHeaderName>
     
```

### Vorgangstitel

Das Element `processTitleRule` dient zur Generierung des Vorgangstitel. Hier stehen dieselben Möglichkeiten zur Verfügung, die auch in der Goobi-Konfigurationsdatei `goobi_projects.xml` genutzt werden können.

```markup
<!-- Rules to generate the process title, the same syntax as in goobi_projects.xml can be used. 
     Use the column names to get the right metadata values. 
     If the field is missing or empty, the value of the identifier column is used.
-->
<processTitleRule>'StaticPrefix_'+Identifier</processTitleRule>
```

### Übernahme von Bildern

Mit Hilfe der Elemente `imageFolderHeaderName`, `imageFolderPath` und `moveImages` können zusätzlich zu den Metadaten auch Bilder importiert werden. In `imageFolderHeaderName` wird hierfür der Spaltenname eingetragen, in dem in der Excel-Datei die Ordnernamen zu finden sind, die die Bilder enthalten. Dort kann entweder ein absoluter Pfad oder auch ein relativer Pfad angegeben werden. Wenn hierbei ein relativer Pfad angegeben wird, muss das Element `imageFolderPath` den `root` Pfad zu den Bildern enthalten. 

Mittels des Elements`moveImages` kann gesteuert werden, ob die Bilder kopiert oder verschoben werden sollen.

```markup
<!-- define which column contains the image folder name. Can be combined with <imageFolderPath> prefix or an absolute path. 
      If the field is missing, empty or does not contain an existing directory, no images will be imported -->
<imageFolderHeaderName>image folder</imageFolderHeaderName>

<!-- prefix path to the image folder. Can be empty or missing if the import doesn't contain images or if the excel field contains absolute path  -->
<imageFolderPath>/mnt/images/</imageFolderPath>

<!-- defines, if images are moved from the source folder to the destination (true) or copied (false) -->
<moveImages>true</moveImages>
```

### Ausführung mittels GoobiScript

Das Element `runAsGoobiScript` steuert, ob ein Import asynchron im Hintergrund über die GoobiScript Warteschlange abgearbeitet werden soll oder ob der Import direkt innerhalb der Nutzersession verarbeitet werden soll. Hier muss abgewägt werden, welche Einstellung sinnvoll ist. Soll ein ein Import inklusive Bildern erfolgen oder enthält die Excel-Datei sehr viele Datensätze, so ist es vermutlich sinnvoller, diesen Import als GoobiScript durchzuführen. 

```markup
<!-- Run the import as GoobiScript -->
<runAsGoobiScript>true</runAsGoobiScript>
```

{% hint style="info" %}
**Achtung:** Wenn die Spalte `identifierHeaderName` keinen eindeutigen Identifier enthält oder nicht konfiguriert wurde, kann die Option `runAsGoobiScript` nicht genutzt werden.
{% endhint %}

### Konfiguration der einzelnen Excel-Spalten

Über die Felder `metadata`, `person` und `group` können einzelne Spalten als Metadatum oder als Vorgangseigenschaft importiert werden. Dazu enthält jedes Feld eine Reihe von Attributen und Unterelementen.

#### Import von Metadaten

Mit dem Element `metadata` werden deskriptive Metadaten erzeugt.

| Name | Typ | Beschreibung |
| :--- | :--- | :--- |
| `headerName` | Attribut | Spaltentitel in der Exceldatei |
| `ugh` | Attribut | Name des Metadatums |
| `name` | Attribut | Name der Eigenschaft |
| `docType` | Attribut | anchor oder child |
| `normdataHeaderName` | Attribut | Spaltentitel einer Spalte mit dazugehörigen Identifiern |

Das Attribut `headerName` enthält den Spaltentitel. Die Regel greift nur dann, wenn die Excel-Datei eine Spalte mit diesem Titel enthält und die Zelle nicht leer ist. Von den beiden Attributen `ugh` und `name` muss mindestens eines existieren. Das Feld `ugh` kann den Namen eines Metadatums enthalten. Wenn dies der Fall ist \(und das Metadatum für den konfigurierten Publikationstyp erlaubt ist\), wird ein neues Metadatum erzeugt. Mittels `name` wird eine Eigenschaft mit diesem Namen erstellt.

Das Attribut `docType` wird relevant, wenn aus dem Katalog ein mehrbändiges Werk oder eine Zeitschrift importiert wurde. Darüber kann gesteuert werden, ob das Feld zur Gesamtaufnahme oder zum Band gehören soll.

Falls zusätzlich zum Inhalt noch eine weitere Spalte mit Normdatenidentifiern oder URIs existiert, kann diese Spalte im Attribut `normdataHeaderName` hinzugefügt werden.

#### Import von Personen

Mittels des Elements `person` können automatisiert Personen angelegt werden.

| Name | Typ | Beschreibung |
| :--- | :--- | :--- |
| `ugh` | Attribut | Name der Personenrolle |
| `docType` | Attribut | anchor oder child |
| `normdataHeaderName` | Attribut | Spaltentitel einer Spalte mit dazugehörigen Identifiern |
| `firstnameFieldHeader` | Element | Spaltentitel des Feldes für Vorname |
| `lastnameFieldHeader` | Element | Spaltentitel für Nachnamen |
| `nameFieldHeader` | Element | Spaltentitel für den kompletten Namen |
| `splitName` | Element | Definiert, ob der Wert in nameFieldHeader gesplittet werden soll |
| `splitChar` | Element | Element, an dem gesplittet wird. Default ist das erste Leerzeichen |
| `firstNameIsFirstPart` | Attribut | Definiert, in welcher Reihenfolge die Angaben gemacht wurden |

Personen unterscheiden sich von normalen Metadaten dadurch, dass sie aus Vor- und Nachnamen bestehen. Diese Angabe kann in zwei verschiedenen Spalten stehen, dann werden die Elemente `firstnameFieldHeader` und `lastnameFieldHeader` genutzt. Stehen die Namen nur in einer Spalte, wird das Feld `nameFieldHeader` genutzt. In dem Fall wird geprüft, ob die Angaben nur den Nachnamen enthalten sollen, oder ob der Inhalt aufgesplittet werden muss. Dabei kann mit `splitChar` das Zeichen/die Sequenz gesetzt werden, an der die Aufsplittung erfolgen soll. Das Attribut `firstNameIsFirstPart` enthält die Information, ob der Name als `Vorname Nachname` oder `Nachname Vorname` zu importieren ist.

#### Import von Metadatengruppen 

Mittels des Elements `group` können Metadatengruppen erstellt werden.

| Name | Typ | Beschreibung |
| :--- | :--- | :--- |
| `ugh` | Attribut | Name der Metadatengruppe |
| `docType` | Attribut | anchor oder child |
| `metadata` | Element | Metadatum innerhalb der Gruppe |
| `person` | Element | Person innerhalb der Gruppe |

Eine Metadatengruppe besteht aus mehreren Metadaten und Personen. Die Konfiguration der einzelnen Unterelemente erfolgt identisch zu den einzelnen Metadaten und Personen.

## Nutzung in Goobi 

Um den Import zu nutzen, muss in den Produktionsvorlagen der Massenimportbereich geöffnet werden und im Reiter Dateiupload-Import das Plugin `intranda_import_excel_read_headerdata` ausgewählt werden. Anschließend kann eine Excel-Datei hochgeladen und importiert werden. 

Der Import erfolgt anschließend zeilenweise. Dabei wird für jede Zeile ein neuer Vorgang erzeugt und die konfigurierten Regeln angewendet. Wenn dabei ein valider Datensatz erzeugt wurde und der generierte Vorgangstitel noch nicht vergeben wurde, wird der Vorgang tatsächlich erzeugt und gespeichert.
