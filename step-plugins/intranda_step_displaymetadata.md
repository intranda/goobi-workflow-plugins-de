---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin für die Anzeige von
  beliebigen Metadaten in einem Workflow-Aufgabe
---

# Anzeige von Metadaten in einer Aufgabe

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zur Anzeige von Metadaten in einem Workflow-Schritt. Das Plugin kann beliebige Metadaten in einem Schritt anzeigen. Die Konfiguration von Prä- und Suffixen ist auch möglich.

| Details |  |
| :--- | :--- |
| Version | 1.1.0 |
| Identifier | intranda\_step\_displayMetadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-displaymetadata](https://github.com/intranda/goobi-plugin-step-displaymetadata) |
| Kompatibilität | Goobi Workflow 3.0.0 |
| Dokumentationsdatum | 28.11.2019 |

## Voraussetzung

Voraussetzung für die Verwendung des Plugins ist der Einsatz von Goobi workflow in Version 3.0.0 oder höher, die korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte des Workflows.

## Installation und Konfiguration

Zur Nutzung des Plugins müssen die beiden Artefakte an folgende Orte kopiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_displayMetadata.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_displayMetadata-GUI.jar
```

Die Konfiguration des Plugins wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_displayMetadata.xml
```

Es können mehrere Metadaten zur Anzeige konfiguriert werden, zusätzlich kann ein Präfix und ein Suffix angezeigt werden. Das `key`-Attribut dient für die Übersetzung des labels des Metadatums:

```markup
    <?xml version="1.0" encoding="UTF-8"?>
    <config_plugin>
        <config>
            <project>*</project>
            <step>*</step>
            <metadatalist>
                <metadata>Author</metadata>
                <metadata>TitleDocMain</metadata>
                <metadata>_urn</metadata>
                <metadata prefix="http://svdmzgoobiweb01.klassik-stiftung.de/viewer/image/" suffix="/1/" key="url">CatalogIDDigital</metadata>
            </metadatalist>
        </config>
    </config_plugin>
```

In Goobi muss dann noch der Schritt in den Workflow einkonfiguriert werden. Dafür muss in der Schritte-Konfiguration als Schritte-Plugin `intranda_step_displayMetadata` ausgewählt werden.

![Konfiguration des Schritts](../.gitbook/assets/displaymetadata_config.png)

Wenn dann nach erfolgreicher Konfiguration der Schritt geöffnet wird, werden alle Metadaten - sofern im Vorgang vorhanden - angezeigt:

![](../.gitbook/assets/displaymetadata_view.png)

