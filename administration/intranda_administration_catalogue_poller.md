---
description: >-
  Goobi Administration Plugin für die periodische Aktualisierung bestehender
  METS-Dateien mit Inhalten aus einer Katalogabfrage
---

# Catalogue Poller

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Administration Plugins für die automatisiert wiederholte Katalogabfrage zur Aktualisierung von Datensätzen in Goobi workflow.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_administration\_catalogue\_poller |
| Source code | [https://github.com/intranda/goobi-plugin-administration-catalogue-poller](https://github.com/intranda/goobi-plugin-administration-catalogue-poller) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2021.02 |
| Dokumentationsdatum | 06.11.2021 |

## Arbeitsweise des Plugins

Das Catalogue Poller Plugin wird durch Goobi automatisch aktiviert. Seine Laufzeit beginnt zu der konfigurierten Startzeit und wiederholt sich entsprechend der konfigurierten Anzahl an Stunden.

Möchte ein Nutzer zusätzlich zu dieser Automatik ebenfalls Zugriff auf die Nutzeroberfläche des Plugins haben, so muss er einer Benutzergruppe angehören, die hierfür das folgende Plugin-spezifische Recht erhalten hat:

```text
Plugin_Goobi_CataloguePoller
```

Um dieses Recht zuzuweisen, muss der gewünschten Nutzergruppe zunächst die Berechtigung im rechten Bereich eingetragen werden.

![Eintragen der gew&#xFC;nschten Berechtigung](../.gitbook/assets/intranda_administration_catalogue_poller_01.png)

![Benutzergruppe mit zugewiesener Berechtigung](../.gitbook/assets/intranda_administration_catalogue_poller_02.png)

Sollte die Berechtigung für die Benutzergruppe neu eingetragen werden, so muss sich der Nutzer zunächst einmal neu in Goobi einloggen, um diese Berechtigungsstufe verwenden zu können. Anschließend kann er im Menü Administration auf das Plugin Catalogue Poller klicken und dort auch jederzeit eine Aktualisierung der Datensätze mittels Katalogabfrage manuell neu anstoßen.

![Erfolgreicher Durchlauf mit der Aktualisierung eines betroffenen Goobi Vorgangs](../.gitbook/assets/intranda_administration_catalogue_poller_03.png)

## Automatische Backups

Sollte das Plugin für einen Vorgang aktualisierte Metadaten finden und daher die METS-Datei aktualisieren, so wird zunächst automatisch ein Backup der aktuellen METS-Datei `meta.xml` und sofern relevant auch der `meta_anchor.xml` erzeugt. Das Backup wird neben der aktualisierten METS-Datei gespeichert.

![Mehrere Versionen der METS-Dateien werden als Backup aufgehoben](../.gitbook/assets/intranda_administration_catalogue_poller_04.png)

## Logging innerhalb des Vorgangslogs

Die Updates der Metadaten durch das Plugin finden üblicherweise vollautomatisch im Hintergrund statt. Um dennoch jederzeit für einen Datensatz nachvollziehen zu können, was mit diesem zwischenzeitlich passierte, werden die Ereignisse geloggt. Zu jedem Vorgang, für den es Änderung aus diesem Plugin gab, werden daher automatisch detaillierte Einträge innerhalb des `Vorgangslogs` eingefügt. Diese enthalten neben dem Zeitstempel unter anderem eine genaue Auflistung der geänderten Metadatenfelder samt der Inhalte. Somit ist es jederzeit möglich auch den vorherigen bzw. den neuen Wert nachvollziehen zu können.

![Innerhalb des Vorgangslogs sind die &#xC4;nderungen des Catalogue Pollers nachvollziehbar](../.gitbook/assets/intranda_administration_catalogue_poller_05.png)


## Installation

Das Plugin besteht insgesamt aus den folgenden zu installierenden Dateien

```text
plugin_intranda_administration_catalogue-poller.jar
plugin_intranda_administration_catalogue-poller-GUI.jar
```

Diese Dateien müssen in den richtigen Verzeichnissen installiert werden, so dass diese nach der Installation in folgenden Pfaden vorliegen:

```bash
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_catalogue-poller.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_catalogue-poller-GUI.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/config/plugin_intranda_administration_catalogue_poller.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_administration_catalogue_poller.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

{% code title="plugin\_intranda\_administration\_catalogue\_poller.xml" %}
```

```
{% endcode %}

```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>

   <!-- multiple different rules can be defined for individual use cases -->
   <rule title="SampleProject" startTime="22:00:00" delay="24">

        <!-- filter which items to run through (can be more then one, otherwise use *)
        please notice that blanks inside of the filter query need to be surrounded by quotation marks -->
        <filter>project:SampleProject</filter>
        <filter>"project:Manuscript items"</filter>

        <!-- which catalogue to use (GBV, Wiener, CBL Adlib ...) -->
        <catalogue>Wiener</catalogue>

        <!-- which catalogue field to use and which identifier to use for the catalogue request (use
        standard variable replacer compatible value here) -->
        <catalogueField fieldName="12" fieldValue="$(meta.CatalogIDDigital)" />
        
        <!-- define if existing structure subelements shall be kept (true),
        otherwise a complete new mets file is created and overwrites the
        existing one (false) -->
        <mergeRecords>true</mergeRecords>

        <!-- define if children shall be analysed as well. If a sub element contains an identifier, the metadata will get imported as well -->
        <analyseSubElements>true</analyseSubElements>

        <!-- execute an automatic export of updated records;
        this is only executed if mergeRecords is set to true -->
        <exportUpdatedRecords>false</exportUpdatedRecords>
       
        <!--fieldList: Must have a mode attribute which can contain either blacklist or whitelist as a value.
            blacklist: All fields are updated except the defined ones. This is a potentially dangerous setting!
            whitelist: Only the definied fields are updated. All others are skipped. 
            field: Use the internal metadata names from the ruleset as field definition
         -->
        <fieldList mode="blacklist">
            <field>viewerinstance</field>
            <field>singleDigCollection</field>
            <field>pathimagefiles</field>
            <field>_urn</field>
            <field>_representative</field>
        </fieldList>

    </rule>

   <!-- internal timestamp for the plugin to know when it was last executed -->
   <lastRun>1551731078691</lastRun>
</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `rule title` | An dieser Stelle wird ein interner Name angegeben, der hauptsächlich für die Nutzeroberfläche zur Unterscheidung der unterschiedlichen Regeln dient |
| `rule startTime` | Mit diesem Parameter wird die Startzeit festgelegt, wann das Plugin diese Regel ausführen soll. |
| `rule delay` | Hiermit kann festgelegt werden, wie häufig das Plugin ausgeführt werden soll. Die Angabe erfolgt hier in Form von Stunden. |
| `filter` | Mittels des Filters können ein oder mehrere Goobi-Projekte definiert werden, für die die hier definierten Regeln gelten sollen. Mittels `*` gilt die Regel für sämtliche Projekte. Enthaltene Leerzeichen innerhalb des Filters müssen genau wie innerhalb der Goobi Oberfläche mit Anführungszeichen umschlossen werden. |
| `catalogue` | Hier kann definiert werden, welcher Katalog für die Abfrage von neuen Daten verwendet werden soll. Hierbei handelt es sich um die Bezeichnung eines Kataloges, wie er innerhalb der globalen Goobi-Katalogkonfiguration innerhalb von `goobi_opac.xml` definiert wurde. |
| `fieldName` | Dieser Parameter steuert, innerhalb welchen Feldes der Katalog abgefragt werden. Häufig is dieser Wert beispielsweise `12`. |
| `fieldValue` | Definition desjenigen Metadatums aus der METS-Datei, das für die Abfrage des Katalogs verwendet werden soll. Üblicherweise handelt es sich hierbei um denjenigen Identifier, der auch bei der erstmaligen Katalogabfrage verwendet wurde und der zumeist innerhalb der Metadatums `${meta.CatalogIDDigital}` gespeichert vorliegt. |
| `exportUpdatedRecords` | Wenn dieser Wert auf `true` gesetzt wird, so erfolgt im Anschluß an die Katalogabfrage für all diejenigen Datensätze ein erneuter Datenexport, die im Verlauf der Katalogabfrage auch tatsächlich aktualisiert wurden. Als Datenexport wird in diesem Fall derjenige Arbeitsschritt ausgeführt, der als erster `Export`-Arbeitsschritt innerhalb des Workflows für den Vorgang definiert wurde. Damit ist üblicherweise der Export und damit die Veröffentlichung des Vorgangs innerhalb der Goobi viewers gemeint. Zu beachten ist hierbei, dass die Vorgänge nur dann exportiert werden, wenn der Mechanismus für `mergeRecords` ebenfalls auf `true`gesetzt ist. |
| `mergeRecords` | Wenn der Wert `true` gesetzt ist, wird die bestehende METS-Datei mit den aktuellen Daten aus dem Katalog aktualisiert. Eventuelle zusätzliche Metadaten können für die Aktualisierung ausgeschlossen werden. Auch bleibt der logische und physische Strukturbaum innerhalb der METS-Datei unverändert.Wenn der Wert auf `false` gesetzt wird, dann wird die bestehende METS-Datei vollständig durch eine neue METS-Datei ersetzt, die mittels der Katalogabfrage generiert wurde. |
| `analyseSubElements` | Mit diesem Parameter läßt sich definieren, ob auch Metadaten für bereits innerhalb der METS-Dateien vorhandene Strukturelemente vom Katalog abgefragt werden sollen. Hierfür muss pro Unterelement das festgelegte Metadatum für den abzufragenden Identifier vorhanden sein. |
| `fieldList` | Hier stehen die Modi `blacklist` und `whitelist` zur Verfügung. Falls der Modus `whitelist` gewählt wird, können hier die Metadatenfelder definiert werden, die durch ein Katalogabfrage aktualisiert werden sollen. Falls der Modus `blacklist` verwendet wird, können mehrere Metadatenfelder definiert werden, die keinesfalls durch eine Katalogabfrage geändert werden sollen. Dies ist insbesondere für diejenigen Felder sinnvoll, die nicht aus einer Katalogabfrage kommen und daher zuvor zusätzlich zu den Katalogdaten erfasst wurden. Typische Beispiele für solche Felder sind unter anderem `singleDigCollection`,`accesscondition` und `pathimagefiles`.Bitte beachten Sie, dass dieser Parameter nur dann Anwendung findet, wenn der Wert für `mergeRecords` auf `true` steht.  |