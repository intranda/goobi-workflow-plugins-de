---
description: >-
  Dies ist eine technische Dokumentation für das Goobi Export-Plugin zum Export
  in unterschiedliche Verzeichnisse für die Klassik Stiftung Weimar
---

# HAAB Export

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Export-Plugins in Goobi, wie es für die Klassik Stiftung Weimar innerhalb des Digitalisierungsprojektes benötigt wird.

Mit Hilfe dieses Export-Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes gleichzeitig an mehrere Orte exportiert werden. Dabei bleiben die Besonderheiten der Klassik Stiftung Weimar, wie die Nutzung der EPNs als Identifier sowie das Zusammenführen von Einbänden und Blattschnitten zu einem gemeinsamen Strukturelement, bestehen.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | HaabExport |
| Source code | [https://github.com/intranda/goobi-plugin-export-haab](https://github.com/intranda/goobi-plugin-export-haab) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2.1 und neuer |
| Dokumentationsdatum | 17.06.2015 |

## Voraussetzung und Installation

Voraussetzung für die Verwendung des Export-Plugins ist der Einsatz der Goobi 2.1 oder neuer und die anschließende korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte der Workflows.

## Konfiguration

Das Plugin muss zunächst in folgendes Verzeichnis installiert werden:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_Haab.jar
```

Im Goobi-Konfigurationsverzeichnis muss im Rahmen der Installation die zusätzliche Plugin-Konfigurationsdatei unter dem folgenden Pfad bereitgestellt werden:

```text
/opt/digiverso/goobi/config/plugin_HaabExportPlugin.xml
```

Der Inhalt dieser Konfigurationsdatei ist folgendermaßen aufgebaut:

{% code title="plugin\_HaabExportPlugin.xml" %}
```markup
<config_plugin>
        <exportFolder>/opt/digiverso/viewer/hotfolder/</exportFolder>
        <exportFolder>/opt/digiverso/archive/</exportFolder>
</config_plugin>
```
{% endcode %}

Mit Hilfe der Auflistung `<exportFolder>` können verschiedene Orte definiert werden, in die der Export erfolgen soll. Hierbei können beliebig viele Ordner definiert werden. Mindestens muss jedoch ein Ordner an dieser Stelle definiert sein.

Um das Export-Plugin nach der erfolgreichen Installation innerhalb des Workflows nutzen zu können, muss ein Arbeitsschritt definiert werden, bei dem die Funktion Export DMS aktiviert wurde. Darüber hinaus muss als Schritt-Plugin der Wert `HaabExport` eingetragen werden.

![](../.gitbook/assets/intranda_export_haab.png)

