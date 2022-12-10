---
description: >-
  Dies ist eine technische Dokumentation für das ZOP Export Plugin. Es ermöglicht den Export in die ZOP Instanz der ZB Zürich.
---

# ZOP Export

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des ZOP-Export-Plugins in Goobi.

Mithilfe dieses Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes an den konfigurierten Ort für ZOP exportiert werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_zop |
| Source code | [https://github.com/intranda/goobi-plugin-export-zop](https://github.com/intranda/goobi-plugin-export-zop) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.10 und neuer |
| Dokumentationsdatum | 16.11.2022 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_export_zop_de.png)

Das Plugin muss zunächst in folgendes Verzeichnis kopiert werden:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_zop.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_zop.xml
```
## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_export_zop.xml`. Die Konfiguration kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
	<!--
	Order of configuration is:
	1.) project name matches
	2.) project is *
	-->

	<!-- There could be multiple config blocks. -->
	<!-- Please make sure that the project names of different config blocks are also different. -->
	<!-- Given two config blocks with the same project name, the settings of the first one will be taken. -->
	<config>
		<!-- The name of the project. -->
		<!-- MANDATORY -->
		<project>Archive_Project</project>

		<!-- The name of the item identifier, e.g. CatalogIDDigital.  -->
		<!-- For one-volume works, the value of this Metadata field will be used as the folder's as well as the .ctl file's name. -->
		<!-- For multi-volume works, the value of this Metadata field will be used as the name's first part. -->
		<!-- MANDATORY -->
		<identifier>CatalogIDDigital</identifier>

		<!-- The name to be used to distinguish between different volumes of one book series. -->
		<!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
		<!-- For one-volume works, the value of this Metadata field will not be used. BUT do NOT leave it blank anyway. -->
		<!-- For multi-volume works, the value of this Metadata field will be used as the second part of the folder's and the .ctl file's name. -->
		<!-- MANDATORY -->
		<volume>CurrentNoSorting</volume>

		<!-- The place you would like to use for the export. -->
		<!-- Absolute path expected. No difference whether you append the directory separator '/' to the end or not. -->
		<!-- If left blank, then the default setting '/opt/digiverso/viewer/hotfolder' will be used. -->
		<path></path>
	</config>

	<config>
		<project>Manuscript_Project</project>
		<identifier>CatalogIDDigital</identifier>
		<volume>CurrentNoSorting</volume>
		<!-- Setting up path using a goobi variable. -->
		<!-- No difference whether you add a '/' between '}' and '..' or not. -->		
		<path>{goobiFolder}../viewer/hotfolder/</path>
	</config>

	<config>
		<project>*</project>
		<identifier>CatalogIDDigital</identifier>
		<volume>CurrentNoSorting</volume>
		<!-- Setting up path using an ABSOLUTE path. -->
		<path>/opt/digiverso/viewer/hotfolder</path>
	</config>

</config_plugin>
```

| Parameter         | Erläuterung                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | Dieser Parameter legt fest, welches Metadatum als Ordnername verwendet werden soll. |
| `volume`          | Dieser Parameter steuert, mit dem Inhalt welchen Metadatums die Unterverzeichnisse für Bände benannt werden sollen. |
| `path`            | Dieser Parameter legt den Export-Pfad fest, wohin die Daten exportiert werden sollen. Erwartet wird ein absoluter Pfad. |
