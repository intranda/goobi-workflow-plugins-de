---
description: >-
  OPAC Plugin für die Datenübernahme von JSON Datensätzen
---

# Generischer JSON Import

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Plugins. Mit Hilfe dieses Plugins können Daten aus einem externen System abgefragt und in Goobi übernommen werden. Der Katalog muss eine API haben, über die Datensätze als JSON ausgeliefert werden können.

Details             | &nbsp;
--------------------|-------------------------------------------
Version             | 1.0.0
Identifier          | intranda_opac_json
Source code         | [https://github.com/intranda/goobi-plugin-opac-json](https://github.com/intranda/goobi-plugin-opac-json)
Lizenz              | GPL 2.0 oder neuer
Kompatibilität      | 2020.05
Dokumentationsdatum | ​13.06.2020

## Installation

Das Plugin besteht aus zwei Dateien:

```bash
plugin_intranda_opac_json.jar
plugin_intranda_opac_json.xml
```

Die Datei `plugin_intranda_opac_json.jar` enthält die Programmlogik und muss für den Nutzer `tomcat` lesbar an folgendem Pfad installiert werden:

```bash
/opt/digiverso/goobi/plugins/opac/plugin_intranda_opac_json.jar
```

Die Datei `plugin_intranda_opac_json.xml` muss ebenfalls für den Nutzer `tomcat` lesbar sein und unter folgendem Pfad liegen:

```bash
/opt/digiverso/goobi/config/plugin_intranda_opac_json.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt in den folgenden Dateien, die sich im Verzeichnis `/opt/digiverso/goobi/config/` befinden.

```bash
goobi_opac.xml
plugin_intranda_opac_json.xml
```

In der Datei `goobi_opac.xml` muss die Schnittstelle zum gewünschten Katalogsystem bekannt gemacht werden. Dies geschieht durch einen Eintrag, der wie folgt aussieht:

```xml
<catalogue title="JSON">
    <config description="JSON OPAC" address="https://example.com/opac?id="
    port="443" database="x" iktlist="x" ucnf="x" opacType="intranda_opac_json" />
    <searchFields>
        <searchField label="Identifier" value="12"/>
    </searchFields>
</catalogue>
```

Das Attribut `title` enthält den Namen, unter dem der Katalog in der Nutzeroberfläche ausgewählt werden kann, `address` die URL zum API-Endpoint und `opacType` das zu nutzende Plugin. In diesem Fall muss der Eintrag `intranda_opac_json` lauten.

Das Mapping der Inhalte des JSON-Datensatzes hin zu Goobi Metadaten geschieht innerhalb der Datei `plugin_intranda_opac_json.xml`. Die Definition der Felder innerhalb des JSON-Datensatzes geschieht mittels `JSONPath`, dem XPath-Equivalent für JSON.

```xml
<config_plugin>
    <config>
        <recordType field="[?(@.recordType=='archival_object')]" docType="Monograph" />
        <recordType field="$.children[?(@.itemCount &gt; 1)]"  docType="Volume" anchorType="MultiVolumeWork" />
        <recordType field="$.children[?(@.itemCount == 1)]" docType="Monograph" />

        <defaultPublicationType>Monograph</defaultPublicationType>

        <metadata metadata="PublicationYear" field="$.date" />
        <metadata metadata="DocLanguage" field="$.language" />
        <metadata metadata="CatalogIDDigital" field="$.identifier" docType="volume" />
        <metadata metadata="CatalogIDDigital" field="$.children[?(@.itemCount > 1)].children[0].itemId" docType="volume"  />
        <metadata metadata="CatalogIDDigital" field="$.uri" regularExpression="s/\/some-prefix\/(.+)/$1/g" docType="anchor"  />
        <metadata metadata="shelfmarksource" field="$.identifierShelfMark" docType="volume" />
        <metadata metadata="TitleDocMain" field="$.title" docType="volume" />  
        <metadata metadata="OtherTitle"  field="$.alternativeTitle" docType="volume"  />
        <metadata metadata="CurrentNo" field="$..children[0].children[0].sequenceNumber" docType="volume"  />
        <metadata metadata="CurrentNoSorting" field="$..children[0].children[0].sequenceNumber" docType="volume"  />

        <person metadata="Author" field="creator" firstname="s/^(.+?)\, (.+?)$/$2/g" lastname="s/^(.+?)\, (.+?)$/$1/g" validationExpression="/^.+?\, .+?\, .+$/" regularExpression="s/^(.+?)\, (.+?)\, .+/$1\, $2/g"/>

    </config>
</config_plugin>
```

Die Konfigurationsdatei enthält vier Arten von Feldern:

Feldtyp                  | Beschreibung
-------------------------|----------------------------------------------------------------------
`recordType`             | Dieser Typ dient zum Erkennen des Dokumententyps des JSON Datensatzes
`defaultPublicationType` | Dieser Typ wird genutzt, wenn zuvor kein Dokumententyp erkannt wurde
`metadata`               | Dieser Typ dient zum Mapping von JSON Feldern zu Metadaten
`person`                 | Dieser Typ dient zum Mapping von JSON Feldern zu Personen

### Feldtyp: recordType

Das Element `recordType` enthält die Attribute `field`, `docType` und `anchorType`. In `field` wird ein JSONPath-Ausdruck angegeben, der auf den Datensatz angewendet wird. Falls es sich bei dem Typ um ein mehrbändiges Werk oder eine Zeitung/Zeitschrift handelt, muss der zu nutzende `anchor` Typ im Feld `anchorType` angegeben werden. Existiert ein Feld mit so einem Ausdruck, wird der in `docType` definierte Dokumententyp erstellt. Wenn nicht, wird der nächste konfigurierte `recordType` überprüft.

Dabei gibt es eine Reihe von Zeichen, die in dieser Datei maskiert sind. Das betrifft zum einen Zeichen wie `< > & "`, die in XML eine besondere Bedeutung haben und daher als `&lt; &gt; &amp; &quot;` angegeben werden müssen. Daneben ist noch das `Komma` betroffen, das mittels Backslash ebenfalls als `\,` maskiert werden muss.

### Feldtyp: defaultPublicationType
Wenn keine der Definitionen zutreffen, kann ein Dokument mit dem Typ aus `defaultPublicationType` erzeugt werden. Wenn dieses Feld fehlt oder leer ist, wird stattdessen kein Datensatz angelegt.

### Feldtypen: metadata & person
Die beiden Felder `metadata` und `person` dienen zum Import einzelner Inhalte aus dem JSON-Datensatz in in die jeweiligen Metadaten. Dazu stehen eine Reihe von Attributen zur Verfügung:

Attribut               | Bedeutung
-----------------------|--------------------------------------------------------------------------------------------------------------------------------------
`metadata`             | Enthält den Namen des Metadatums oder der Person
`field`                | Pfad zum Inhalt innerhalb des JSON-Objekts
`docType`              | Darf den Wert `anchor` oder `volume` haben. Der default-Wert ist `volume`. Mit `anchor` gekennzeichnete Felder werden nur bei Mehrbändigen Werken überprüft und importiert.
`validationExpression` | Regulärer Ausdruck, der prüft ob der gefundene Wert dem definierten Ausdruck entspricht. Ist dies nicht der Fall, wird der Wert ignoriert.
`regularExpression`    | Ein regulärer Ausdruck zur Manipulation des Wertes. Dieser wird nach der Prüfung der `validationExpression` angewendet.
`firstname`            | Ein regulärer Ausdruck, mit dem bei Personen der Vorname aus dem Feldinhalt ermittelt wird.
`lastname`             | Ein regulärer Ausdruck, mit dem bei Personen der Nachname aus dem Feldinhalt ermittelt wird.

## Nutzung

Wenn in Goobi nach einem Identifier gesucht wird, wird im Hintergrund eine Anfrage an die konfigurierte URL gestellt.

![Oberfläche von Goobi workflow zur Abfrage des Katalogs](../.gitbook/assets/plugin_opac_json_de.png)

Passend zur oben beschriebenen Konfiguration entspricht dies etwa der folgenden URL:

```
https://example.com/opac?id=[IDENTIFIER]
```

Sofern unter dieser URL ein gültiger Datensatz gefunden wird, wird dieser nach den innerhalb von `recordType` definierten Feldern durchsucht, in dem der Dokumententyp stehen soll. Wenn keine Felder definiert wurden oder sie nicht gefunden wurden, wird stattdessen der Typ aus dem konfigurierten Element `defaultPublicationType` genutzt. Mit dem ermittelten Typ wird dann das gewünschte Strukturelement erzeugt.

Im Anschluß daran werden die konfigurierten Ausdrücke der `metadata` und `person` der Reihe nach ausgewertet. Sofern mit einem Ausdruck Daten gefunden werden, wird das entsprechend angegebene Metadatum erzeugt.

## Nützliche Links
Für die Installation bzw. insbesondere für die Konfiguration des Plugins könnten die folgenden URLs eine weiterführende Hilfe sein:

JSONPath Online Evaluator: https://jsonpath.com/

JSONPath Description: https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html
