---
description: >-
 Dies ist ein Goobi Step-Plugin, um die Registrierung von digitalen Objekten beim DataCite DOI-Dienst zu ermöglichen.
---

# Plugin zur Registrierung von DOIs via DataCite API

## Einführung

Diese Dokumentation beschreibt die Installation, Konfiguration und Verwendung des Plugins.

| Details | |
| :--- | :--- |
| Identifier | plugin_intranda_step_datacite_doi |
| Source code | [https://github.com/intranda/intranda_step_datacite_doi](https://github.com/intranda/intranda_step_datacite_doi) |
| Lizenz | GPL 2.0 or newer |
| Kompatibilität | Goobi workflow 2021.03 |
| Dokumentationsdatum | 15.03.2021 |

## Installation

Das Plugin besteht aus den folgenden Dateien:

```bash
plugin_intranda_step_datacite_doi.jar
plugin_intranda_step_datacite_doi.xml
plugin_intranda_step_datacite_mapping.xml
```

Die Datei `plugin_intranda_step_datacite_doi.jar` enthält die Programmlogik. Sie muss unter folgendem Pfad installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_datacite_doi.jar
```

Bei der Datei `plugin_intranda_step_datacite_mapping.xml` handelt es sich um die Mapping-Datei, die definiert, wie lokale Metadaten in die für die DOI-Registrierung erforderliche Form übersetzt werden sollen. Sie muss unter diesem Pfad installiert werden:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_datacite_mapping.xml
```

Die Datei `plugin_intranda_step_datacite_doi.xml` ist die Hauptkonfigurationsdatei für das Plugin. Sie muss unter diesem Pfad installiert werden:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_datacite_doi.xml
```

## Konfiguration des Plugins

### Hauptkonfiguration

Die Konfiguration erfolgt über die Konfigurationsdatei `plugin_intranda_step_datacite_doi.xml` und kann im laufenden Betrieb angepasst werden. Sie ist wie folgt aufgebaut:

```xml
<config_plugin>
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>

        <!-- authentication and main information -->
        <!-- For testing: for deployment, remove "test" -->
        <SERVICE_ADDRESS>https://mds.test.datacite.org/</SERVICE_ADDRESS>

        <base>10.80831</base>
        <url>https://viewer.goobi.io/idresolver?doi=</url>
        <USERNAME>YZVP.GOOBI</USERNAME>
        <PASSWORD>password</PASSWORD>

        <!-- configuration for DOIs -->
        <prefix>go</prefix>
        <name>goobi</name>
        <separator>-</separator>
        <handleMetadata>DOI</handleMetadata>

        <!-- configuration for DOIs -->
        <doiMapping>/opt/digiverso/goobi/config/plugin_intranda_step_datacite_mapping.xml</doiMapping>

        <!-- Type of DocStruct which should be given DOIs -->
        <typeForDOI>Article</typeForDOI>

        <!-- display button to finish the task directly from within the entered plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

Der Block `<config>` kann für verschiedene Projekte oder Workflow-Schritte mehrfach vorkommen, um unterschiedliche Aktionen innerhalb verschiedener Workflows durchführen zu können. Die weiteren Parameter innerhalb dieser Konfigurationsdatei haben die folgende Bedeutung:

| Wert | Beschreibung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `SERVICE_ADDRESS` | Dieser Parameter definiert die URL für den Datacite-Dienst. Im obigen Beispiel ist es der Testserver. |
| `base` | Dieser Parameter definiert die DOI-Basis für die Einrichtung, die bei Datacite registriert wurde. |
| `url` | Der Parameter url definiert den Präfix, den jeder DOI-Link erhält. Ein DOI "10.80831/goobi-1" erhält hier z. B. den Hyperlink "https://viewer.goobi.io/idresolver?doi=10.80831/goobi-1" |
| `USERNAME` | Dies ist der Benutzername, der für die DataCite-Registrierung verwendet wird.|
| `PASSWORD` | Dies ist das Passwort, das für die DataCite-Registrierung verwendet wird.|
| `prefix` | Dies ist das Präfix, das dem DOI vor dem Namen und der ID des Dokuments gegeben werden soll.|
| `name` | Dies ist der Name, der dem DOI vor der ID des Dokuments gegeben werden soll. |
| `separator` | Definieren Sie hier ein Trennzeichen, das zwischen den verschiedenen Teilen des DOI verwendet werden soll. |
| `handleMetadata` | Dieser Parameter gibt an, unter welchem Metadatennamen der DOI in der METS-MODS-Datei gespeichert werden soll. Standard ist `_urn`.|
| `doiMapping` | In diesem Parameter wird der Pfad zur Mapping-Datei für die DOI-Registrierung festgelegt. |
| `typeForDOI` | Mit diesem Parameter kann der DocStruct-Typ definiert werden, dem DOIs zugewiesen werden sollen. Wenn dieser leer ist oder fehlt, erhält nur das oberste DocStruct-Element einen DOI. Enthält der Parameter den Namen einer Sub-DocStruct, so werden diese mit DOIs versehen.|

## Konfiguration innerhalb der Mapping-Datei

Die Mapping-Konfigurationsdatei sieht in etwa so aus:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Mapping>
  <!-- Mandatory fields: -->
  <map>
    <field>title</field>
    <metadata>TitleDocMain</metadata>
    <altMetadata>TitleDocMainShort</altMetadata>
    <altMetadata>Title</altMetadata>
    <default>Fragment</default>
  </map>

  <map>
    <field>author</field>
    <metadata>Author</metadata>
    <altMetadata>Composer</altMetadata>
    <altMetadata>IllustratorArtist</altMetadata>
    <altMetadata>WriterCorporate</altMetadata>
    <default>unkn</default>
  </map>

  <map>
    <field>publisher</field>
    <metadata>Publisher</metadata>
    <altMetadata>PublisherName</altMetadata>
    <altMetadata>PublisherPerson</altMetadata>
    <default>unkn</default>
  </map>

  <map>
    <field>publicationYear</field>
    <metadata>_dateDigitization</metadata>
    <default>#CurrentYear</default>
  </map>

  <map>
    <field>inst</field>
    <default>intranda</default>
  </map>

  <map>
    <field>resourceType</field>
    <default>document</default>
  </map>

  <!-- Optional fields: -->
  <map>
    <field>editor</field>
    <metadata>Editor</metadata>
  </map>

</Mapping>
```

Für jede `<map>` gibt das `<field>` den Namen des DOI-Elements an, und die Einträge `<metadata>` und `<altMetadata>` geben an, aus welchen Metadaten der Strukturelemente der Reihe nach der Wert genommen werden soll. Wenn es keinen solchen Eintrag in den Strukturelementen gibt, dann wird der `<default>`-Wert genommen. Der Wert `"unkn"` für "unbekannt" wird von Datacite für fehlende Daten empfohlen.

Für die Pflichtfelder muss ein `<default>` angegeben werden; für optionale Felder ist dies nicht notwendig, kann aber auf Wunsch erfolgen.

Der Standardeintrag `#CurrentYear` ist ein Sonderfall: er wird im DOI durch das aktuelle Jahr ersetzt.

## Einbindung des Plugins in den Workflow

Um das Plugin in Betrieb zu nehmen, muss es für eine oder mehrere gewünschte Aufgaben im Workflow aktiviert werden. Dies geschieht wie im folgenden Screenshot gezeigt durch Auswahl des Plugins `intranda_step_datacite_doi` aus der Liste der installierten Plugins.

![Zuweisen des Plugins zu einer bestimmten Aufgabe](../.gitbook/assets/intranda_step_datacite_doi_de.png)

Da dieses Plugin in der Regel automatisch ausgeführt werden soll, sollte der Workflow-Schritt im Workflow als automatisch konfiguriert werden. Da das Plugin den DOI in die Metadatendatei des Vorgangs schreibt, sollte das Kontrollkästchen für `Update metadata index when finishing` ebenfalls aktiviert sein.

## Funktionsweise des Plugins

Das Programm untersucht die Metadatenfelder der METS/MODS-Datei aus dem Goobi-Vorgang. Wenn ein `<typeForDOI>` angegeben ist, dann geht es jedes Strukturlement dieses Typs in der Datei durch. Wenn nicht, dann nimmt es das oberste Strukturlement. Daraus erstellt es die Daten für ein DOI, wobei es die Mapping-Datei zum Übersetzen verwendet. Dann wird der DOI über die MDS-API von DataCite registriert, wobei der DOI durch die `<base>` zusammen mit einem beliebigen `<prefix>` und `<name>` und der ID des Dokuments (seine `CatalogIDDigital`) plus einem inkrementierten Zähler angegeben wird, falls mehr als ein DOI für das gegebene Dokument erzeugt wurde. Der Datensatz erhält eine registrierte URL, die durch `<url>` definiert ist, gefolgt von der DOI. Der generierte DOI wird in die METS/MODS-Datei unter den in `<handleMetadata>` angegebenem Metadatum gespeichert. Wenn der Wert für `<typeForDOI>` zum Beispiel `Article` lautet, dann erhält jeder Artikel in der METS/MODS-Datei einen DOI, der in den Metadaten unter `<handleMetadata>` für jeden Artikel gespeichert wird.

