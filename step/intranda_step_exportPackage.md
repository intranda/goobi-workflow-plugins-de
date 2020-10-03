---
description: >-
  Dieses Step Plugin ermöglicht einen flexiblen Export von Metadaten und Inhalten eines Goobi Vorgangs an einen konfigurierbaren Pfad
---

# Export Package

## Einführung

Dieses Plugin erlaubt einen flexiblen Export von Daten eines Vorgangs in ein definiertes Zielverzeichnis. Dabei kann dieses Plugin sehr granular konfiguriert werden, um ausgewählte Daten im Export zu berücksichtigen. Darüber hinaus ist hier ebenfalls eine Transformation der internen und der Export-METS-Datei via XSLT möglich und erlaubt so verschiedenste Einsatzszenarien.

## Übersicht

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_step\_exportPackage |
| Source code | [https://github.com/intranda/goobi-plugin-step-exportPackage](https://github.com/intranda/goobi-plugin-step-exportPackage) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.09 |
| Dokumentationsdatum | 03.10.2020 |

## Installation

Zur Installation des Plugins muss die folgende Datei installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_exportPackage.jar
```

Um zu konfigurieren, wie sich das Plugin verhalten soll, können verschiedene Werte in der Konfigurationsdatei angepasst werden. Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_exportPackage.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

```xml
<config_plugin>

<config>
    <!-- which projects to use for (can be more then one, otherwise use *) -->
    <project>*</project>
    <step>*</step>

    <!-- export path -->
    <target>/opt/digiverso/export/</target>

    <!-- which image folders to use (master|media|jpeg|source|...) -->
    <imagefolder>master</imagefolder>
    <imagefolder>media</imagefolder>

    <!-- which additional folders to use -->
    <ocr>false</ocr>
    <source>false</source>
    <import>false</import>
    <export>false</export>
    <itm>false</itm>
    <validation>false</validation>

    <!-- if the internal METS file shall get transformed into another file define the path of the xsl file here -->
    <transformMetaFile>true</transformMetaFile>
    <transformMetaFileXsl>/opt/digiverso/goobi/xslt/export_meta.xsl</transformMetaFileXsl>
    <transformMetaFileResultFileName>xslt_result_meta.xml</transformMetaFileResultFileName>

    <!-- if the METS file shall get transformed into another file define the path of the xsl file here -->
    <transformMetsFile>true</transformMetsFile>
    <transformMetsFileXsl>/opt/digiverso/goobi/xslt/export_mets.xsl</transformMetsFileXsl>
    <transformMetsFileResultFileName>xslt_result_mets.xml</transformMetsFileResultFileName>
</config>

</config_plugin>
```

Der Block `<config>` kann für verschiedene Projekte oder Arbeitsschritte wiederholt vorkommen, um innerhalb verschiedener Workflows unterschiedliche Aktionen durchführen zu können. Die weiteren Parameter innerhalb dieser Konfigurationsdatei haben folgende Bedeutungen:

| Wert | Beschreibung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `target` | Mit diesem Parameter wird der Hauptpfad definiert, wohin der Export des Vorgangs als Unterordner mit dem Vorgangsnamen exportiert werden soll. |
| `imagefolder` | Es können mehrere Verzeichnisse für die Bilder bzw. Digitalisate angegeben werden. Dies kann unter anderem z.B. die Master-Bilder sowie die Derivate umfassen. |
| `ocr` | Mit diesem Parameter wird angegeben, ob die OCR-Ergebnisse mit exportiert werden sollen. |
| `source` | Wenn die Inhalte des `source` Ordners mit berücksichtigt werden sollen, kann dies hier angegeben werden. |
| `import` | Wenn die Inhalte des `import` Ordners mit berücksichtigt werden sollen, kann dies hier definiert werden. |
| `export` | Wenn die Inhalte des `export` Ordners mit berücksichtigt werden sollen, kann dies hier ebenfalls angegeben werden. |
| `itm` | Sollen die Inhalte des TaskManager-Verzeichnisses `itm` mit exportiert werden, wird dies hier definiert. |
| `validation` | Mit diesem Parameter kann festgelegt werden, dass die Inhalte des Verzeichnisses `validation` ebenfalls exportiert werden sollen. |
| `transformMetaFile` | Mit diesem Parameter wird festgelegt, ob die interne METS-Datei von Goobi workflow in das Zielverzeichnis kopiert werden soll. |
| `transformMetaFileXsl` | Mit diesem Parameter kann festgelegt werden, ob die interne METS-Datei mittels der hier definierten XSLT-Transformationsdatei verarbeitet werden soll. |
| `transformMetaFileResultFileName` | Wenn eine Transformation der internen METS-Datei mittels XSLT erfolgen soll, kann hier festgelegt werden, wie der Name der zu generierenden Datei lauten soll. |
| `transformMetsFile` | Mit diesem Parameter wird festgelegt, ob die Export-METS-Datei von Goobi workflow in das Zielverzeichnis kopiert werden soll. |
| `transformMetsFileXsl` | Mit diesem Parameter kann festgelegt werden, ob die Export-METS-Datei mittels der hier definierten XSLT-Transformationsdatei verarbeitet werden soll. |
| `transformMetsFileResultFileName` | Wenn eine Transformation der Export-METS-Datei mittels XSLT erfolgen soll, kann hier festgelegt werden, wie der Name der zu generierenden Datei lauten soll. |

## Integration des Plugins in den Workflow

Zur Inbetriebnahme des Plugins muss dieses für einen oder mehrere gewünschte Aufgaben im Workflow aktiviert werden. Dies erfolgt wie im folgenden Screenshot aufgezeigt durch Auswahl des Plugins `intranda_step_exportPackage` aus der Liste der installierten Plugins.

![Zuweisung des Plugins zu einer bestimmten Aufgabe](../.gitbook/assets/intranda_step_exportPackage_de.png)

Da dieses Plugin üblicherweise automatisch ausgeführt werden soll, sollte der Arbeitsschritt im Workflow als automatisch konfiguriert werden.

## Arbeitsweise des Plugins

Nachdem das Plugin vollständig installiert und eingerichtet wurde, wird es üblicherweise automatisch innerhalb des Workflows ausgeführt, so dass keine manuelle Interaktion mit dem Nutzer erfolgt. Stattdessen erfolgt der Aufruf des Plugins durch den Workflow im Hintergrund und führt den konfigurierten Export in das Zielverzeichnis durch. Dabei werden die angegebenen Inhalte alle in ein Unterverzeichnes des definierten Export-Pfades kopiert.

Je nach Konfiguration kann dabei zusätzlich zu dem Export der Daten auch eine XSLT-Transformation der internen oder auch der Export-METS-Datei erfolgen, um diese in ein gewünschtes Format zu bringen. Abhängig von dieser Transformation sowie der Benennung der Transformationsdatei wird diese abschließend ebenfalls mit in dem Ordner des exportierten Vorganges gespeichert.
