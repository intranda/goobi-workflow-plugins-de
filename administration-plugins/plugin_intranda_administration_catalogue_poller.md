---
description: >-
  Goobi Administration Plugin für die periodische Aktualisierung bestehender
  METS-Dateien mit Inhalten aus einer Katalogabfrage
---

# Catalogue Poller

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Administration Plugins für die automatisiert wiederholte Katalogabfrage zur Aktualisierung von Datensätzen  in Goobi workflow.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | plugin\_intranda\_administration\_catalogue\_poller |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi workflow 3.0.4 und neuer |
| Dokumentationsdatum | 05.03.2019 |

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
```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
   
   <!-- multiple different rules can be defined for individual use cases -->
   <rule title="SampleProject">
        
        <!-- filter which items to run through (can be more then one, otherwise use *)
		please notice that blanks inside of the filter query need to be surrounded by quotation marks -->
		<filter>project:SampleProject</filter>
		<filter>"project:Manuscript items"</filter>
        
        <!-- which catalogue to use (GBV, Wiener, CBL Adlib ...) -->
        <catalogue>Wiener</catalogue>
        
        <!-- which identifier to use for the catalogue request (use 
        standard variable replacer compatible value here) -->
        <catalogueIdentifier>$(meta.CatalogIDDigital)</catalogueIdentifier>
        
        <!-- define if existing structure subelements shall be kept (true), 
        otherwise a complete new mets file is created and overwrites the 
        existing one (false) -->
        <mergeRecords>true</mergeRecords>
        
        <!-- execute an automatic export of updated records; 
        this is only executed if mergeRecords is set to true -->
		<exportUpdatedRecords>false</exportUpdatedRecords>
        
        <!-- if records shall be merged: which existing fields shall not 
        be replace with new values? (use the metadatatypes from ruleset)-->
        <skipField>viewerinstance</skipField>
        <skipField>singleDigCollection</skipField>    
        <skipField>pathimagefiles</skipField> 
        
    </rule>
   
   <!-- internal timestamp for the plugin to know when it was last executed -->
   <lastRun>1551731078691</lastRun>
</config_plugin>
```
{% endcode %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Erl&#xE4;uterung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>rule title</code>
      </td>
      <td style="text-align:left">An dieser Stelle wird ein interner Name angegeben, der haupts&#xE4;chlich
        f&#xFC;r die Nutzeroberfl&#xE4;che zur Unterscheidung der unterschiedlichen
        Regeln dient</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>filter</code>
      </td>
      <td style="text-align:left">Mittels des Filters k&#xF6;nnen ein oder mehrere Goobi-Projekte definiert
        werden, f&#xFC;r die die hier definierten Regeln gelten sollen. Mittels <code>*</code> gilt
        die Regel f&#xFC;r s&#xE4;mtliche Projekte. Enthaltene Leerzeichen innerhalb
        des Filters m&#xFC;ssen genau wie innerhalb der Goobi Oberfl&#xE4;che mit
        Anf&#xFC;hrungszeichen umschlossen werden.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>catalogue</code>
      </td>
      <td style="text-align:left">Hier kann definiert werden, welcher Katalog f&#xFC;r die Abfrage von neuen
        Daten verwendet werden soll. Hierbei handelt es sich um die Bezeichnung
        eines Kataloges, wie er innerhalb der globalen Goobi-Katalogkonfiguration
        innerhalb von <code>goobi_opac.xml</code> definiert wurde.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>catalogueIdentifier</code>
      </td>
      <td style="text-align:left">Definition desjenigen Metadatums aus der METS-Datei, das f&#xFC;r die
        Abfrage des Katalogs verwendet werden soll. &#xDC;blicherweise handelt
        es sich hierbei um denjenigen Identifier, der auch bei der erstmaligen
        Katalogabfrage verwendet wurde und der zumeist innerhalb der Metadatums <code>${meta.CatalogIDDigital}</code> gespeichert
        vorliegt..</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>exportUpdatedRecords</code>
      </td>
      <td style="text-align:left">Wenn dieser Wert auf <code>true</code> gesetzt wird, so erfolgt im Anschlu&#xDF;
        an die Katalogabfrage f&#xFC;r all diejenigen Datens&#xE4;tze ein erneuter
        Datenexport, die im Verlauf der Katalogabfrage auch tats&#xE4;chlich aktualisiert
        wurden. Als Datenexport wird in diesem Fall derjenige Arbeitsschritt ausgef&#xFC;hrt,
        der als erster <code>Export</code>-Arbeitsschritt innerhalb des Workflows
        f&#xFC;r den Vorgang definiert wurde. Damit ist &#xFC;blicherweise der
        Export und damit die Ver&#xF6;ffentlichung des Vorgangs innerhalb der Goobi
        viewers gemeint. Zu beachten ist hierbei, dass die Vorg&#xE4;nge nur dann
        exportiert werden, wenn der Mechanismus f&#xFC;r <code>mergeRecords</code> ebenfalls
        auf <code>true</code>gesetzt ist.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mergeRecords</code>
      </td>
      <td style="text-align:left">
        <p>Wenn der Wert<code>true</code> gesetzt ist, wird die bestehende METS-Datei
          mit den aktuellen Daten aus dem Katalog aktualisiert. Eventuelle zus&#xE4;tzliche
          Metadaten k&#xF6;nnen f&#xFC;r die Aktualisierung ausgeschlossen werden.
          Auch bleibt der logische und physische Strukturbaum innerhalb der METS-Datei
          unver&#xE4;ndert.</p>
        <p></p>
        <p>Wenn der Wert auf <code>false</code> gesetzt wird, dann wird die bestehende
          METS-Datei vollst&#xE4;ndig durch eine neue METS-Datei ersetzt, die mittels
          der Katalogabfrage generiert wurde.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>skipField</code>
      </td>
      <td style="text-align:left">
        <p>Hier k&#xF6;nnen mehrere Metadatenfelder definiert werden, die keinesfalls
          durch eine Katalogabfrage ge&#xE4;ndert werden sollen. Dies ist insbesondere
          f&#xFC;r diejenigen Felder sinnvoll, die nicht aus einer Katalogabfrage
          kommen und daher zuvor zus&#xE4;tzlich zu den Katalogdaten erfasst wurden.
          Typische Beispiele f&#xFC;r solche Felder sind unter anderem <code>singleDigCollection</code>, <code>accesscondition</code> und <code>pathimagefiles</code>.
          <br
          />
        </p>
        <p>Bitte beachten Sie, dass dieser Parameter nur dann Anwendung findet, wenn
          der Wert f&#xFC;r <code>mergeRecords</code> auf <code>true</code> steht.</p>
      </td>
    </tr>
  </tbody>
</table>## Nutzung in Goobi

Das Catalogue Poller Plugin wird durch Goobi automatisch aktiviert. Seine Laufzeit beginnt 22:00 Uhr und wiederholt sich alle 24 Stunden.

Möchte ein Nutzer zusätzlich zu dieser Automatik ebenfalls Zugriff auf die Nutzeroberfläche des Plugins haben, so muss er einer Benutzergruppe angehören, die hierfür das folgende Plugin-spezifische Recht erhalten hat:

```text
Plugin_Goobi_CataloguePoller 
```

Um dieses Recht zuzuweisen, muss der gewünschten Nutzergruppe zunächst die Berechtigung im rechten Bereich eingetragen werden.

![Eintragen der gew&#xFC;nschten Berechtigung](../.gitbook/assets/catalogue_poller_01.png)

![Benutzergruppe mit zugewiesener Berechtigung](../.gitbook/assets/catalogue_poller_02.png)

Sollte die Berechtigung für die Benutzergruppe neu eingetragen werden, so muss sich der Nutzer zunächst einmal neu in Goobi einloggen, um diese Berechtigungsstufe verwenden zu können. Anschließend kann er im Menü Administration auf das Plugin Catalogue Poller klicken und dort auch jederzeit eine Aktualisierung der Datensätze mittels Katalogabfrage manuell neu anstoßen.

![Erfolgreicher Durchlauf mit der Aktualisierung eines betroffenen Goobi Vorgangs](../.gitbook/assets/catalogue_poller_03.png)

## Automatische Backups

Sollte das Plugin für einen Vorgang aktualisierte Metadaten finden und daher die METS-Datei aktualisieren, so wird zunächst automatisch ein Backup der aktuellen METS-Datei `meta.xml` und sofern relevant auch der `meta_anchor.xml` erzeugt. Das Backup wird  neben der aktualisierten METS-Datei gespeichert.

![Mehrere Versionen der METS-Dateien werden als Backup aufgehoben](../.gitbook/assets/catalogue_poller_04.png)

## Logging innerhalb des Vorgangslogs

Die Updates der Metadaten durch das Plugin finden üblicherweise vollautomatisch im Hintergrund statt. Um dennoch jederzeit für einen Datensatz nachvollziehen zu können, was mit diesem zwischenzeitlich passierte, werden die Ereignisse geloggt. Zu jedem Vorgang, für den es Änderung aus diesem Plugin gab, werden daher automatisch detaillierte Einträge innerhalb des `Vorgangslogs` eingefügt. Diese enthalten neben dem Zeitstempel unter anderem eine genaue Auflistung der geänderten Metadatenfelder samt der Inhalte. Somit ist es jederzeit möglich auch den vorherigen bzw. den neuen Wert nachvollziehen zu können. 

![Innerhalb des Vorgangslogs sind die &#xC4;nderungen des Catalogue Pollers nachvollziehbar](../.gitbook/assets/catalogue_poller_05.png)



