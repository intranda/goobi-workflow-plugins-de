# EWIG Langzeitarchivierung

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zum Erstellen einer METS Datei für die Langzeitarchivierung EWIG. ​

| Details |  |
| :--- | :--- |
| Version des Plugins | 1.0.0 |
| Identifier | LzaExportEWIG |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi workflow 3.0 |
| Dokumentation vom | 28.10.2019 |

## ​Voraussetzung

​ Voraussetzung für die Verwendung des Plugins ist der Einsatz von Goobi workflow 3.0, die korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte der Workflows. ​ ​

## Installation und Konfiguration

​ Das Plugin besteht aus zwei Dateien: ​

```text
plugin_intranda_export_LZA_EWIG.jar
plugin_LzaExportEWIG.xml
```

​ Die Datei `plugin_intranda_export_LZA_EWIG.jar` enthält die Programmlogik und muss für den tomcat-Nutzer lesbar in folgendes Verzeichnis installiert werden:

```markup
/opt/digiverso/goobi/plugins/export/
```

Die Datei `plugin_LzaExportEWIG.xml` muss ebenfalls für den tomcat-Nutzer lesbar sein und in folgendes Verzeichnis installiert werden:

```markup
/opt/digiverso/goobi/config/
```

​ Folgende Datei dient zur Konfiguration des Plugins und muss wie folgt aufgebaut sein: ​

{% code title="plugin\_LzaExportEWIG.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <exportFolder>/opt/digiverso/</exportFolder>
</config_plugin>
```
{% endcode %}

​ Im Element `<exportFolder>` wird dabei festgelegt an welcher Stelle im Dateisystem die exportierten METS Dateien abgelegt werden.

## Einstellungen in Goobi workflow

​ Nachdem das Plugin installiert und konfiguriert wurde, kann es innerhalb eines Arbeitsschrittes genutzt werden. Dazu muss innerhalb der gewünschten Aufgabe das Plugin LzaExportEWIG ausgewählt werden. Des Weiteren müssen die Checkboxen Automatische Aufgabe und Export gesetzt sein. ​ ​ ​

![](../.gitbook/assets/plugin_export_ewig.png)

## Sonstiges

Der Arbeitsschritt innerhalb von Goobi workflow exportiert alle notwendigen Dateien für den EWIG Ingest. Der Upload selbst erfolgt über den intranda TaskManager. Dies ist sinnvoll, um zu vermeiden, das mehrere parallel laufende Uploadvorgänge Konflikte mit einander haben und das System verlangsamen. ​ Für den Upload siehe [Kapitel 4.17](https://docs.intranda.com/intranda-taskmanager-de/4/4.17-upload-von-dateien-in-das-ewig-langzeitarchiv) in der intranda TaskManager Dokumentation.

