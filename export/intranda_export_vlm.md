---
description: >-
  Dies ist eine technische Dokumentation für das VLM Export Plugin. Es ermöglicht, den Export in eine VLM Instanz.
---

# VLM Export

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des VLM-Export-Plugins in Goobi.

Mithilfe dieses Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes an den konfigurierten Ort für VLM exportiert werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.10 und neuer |
| Dokumentationsdatum | 15.11.2022 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_plugin_export_vlm_de.png)

Das Plugin muss zunächst in folgendes Verzeichnis kopiert werden:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_vlm.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_vlm.xml
```
## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_export_vlm.xml`. Die Konfiguration kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

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
		<!-- The name of the project. By default settings there are only two possibilities: Archive_Project, Manuscript_Project. -->
		<!-- To add other options a modification in the database is needed. -->
		<!-- For more details please check the codes around line 1070 in {GoobiDirectory}/db/start.sql. -->
		<!-- MANDATORY -->
		<project>Archive_Project</project>
		
		<!-- The name of the system, e.g. AlmaIDDigital, AlephIDDigital, CatalogIDDigital.  -->
		<!-- MANDATORY -->
		<identifier>CatalogIDDigital</identifier>
	    
		<!-- The name to be used to distinguish between different volumes of one book series. -->
		<!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
		<!-- Leave the default value unchanged if the book is a one-volume work. -->
		<!-- MANDATORY -->
		<volume>CurrentNoSorting</volume>
	    
		<!-- The place you would like to use for the export. -->
		<!-- Absolute path expected. No difference whether you append the directory separator '/' to the end or not. -->
		<!-- If left blank, then the default setting '/opt/digiverso/viewer/hotfolder' will be used. -->
		<path></path>
	    
		<!-- The prefix you would like to use for subfolders for different volumes. -->
		<!-- Leave it blank if no common prefix is needed. -->
		<subfolderPrefix>T_34_L_</subfolderPrefix>
		
		<!-- Choice of application to perform the checksum validation. -->
		<!-- Absolute path expected. -->
		<!-- Options are:
		1.) /usr/bin/sha1sum
		2.) /usr/bin/sha224sum
		3.) /usr/bin/sha256sum
		4.) /usr/bin/sha384sum
		5.) /usr/bin/sha512sum
		6.) /usr/bin/shasum
		7.) /usr/bin/md5sum
		 -->
		 <!-- If left blank, then 1.) will be used as the default setting. -->
		<checksumValidationCommand></checksumValidationCommand>
	</config>
	
	<config>
		<project>Manuscript_Project</project>		
		<identifier>CatalogIDDigital</identifier>		
		<volume>CurrentNoSorting</volume>	
		<!-- Setting up path using a goobi variable. -->
		<!-- No difference whether you add a '/' between '}' and '..' or not. -->		
		<path>{goobiFolder}../viewer/hotfolder/</path>
		<!-- No common prefix needed. -->
		<subfolderPrefix></subfolderPrefix>
		<checksumValidationCommand>/usr/bin/md5sum</checksumValidationCommand>
	</config>
	
	<config>
		<project>*</project>
		<identifier>CatalogIDDigital</identifier>
		<volume>CurrentNoSorting</volume>		
		<!-- Setting up path using an ABSOLUTE path. -->
		<path>/opt/digiverso/viewer/hotfolder</path>
		<!-- No common prefix needed. -->
		<subfolderPrefix></subfolderPrefix>
		<checksumValidationCommand>/usr/bin/sha1sum</checksumValidationCommand>
	</config>

</config_plugin>
```

| Parameter         | Erläuterung                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | Dieser Parameter legt fest, welches Metadatum als Ordnername verwendet werden soll. |
| `volume`          | Dieser Parameter steuert, mit dem Inhalt welchen Metadatums die Unterverzeichnisse für Bände benannt werden sollen. |
| `path`            | Dieser Parameter legt den Export-Pfad fest, wohin die Daten exportiert werden sollen. Erwartet wird ein absoluter Pfad. |
| `subfolderPrefix` | Dieser Parameter beschreibt den Präfix, der für jeden Band eines mehrbändigen Werkes in der Ornderbezeichnung vorangestellt werden soll. (Beispiel `T_34_L_`: Hier steht `T_34` für die Erkennung zur Erstellung eines Strukturknotens des Typs `Band` und das `L` gibt an, dass danach ein Text kommt.) |
| `checksumValidationCommand` | Dieser Parameter legt fest, welche Application zur Validation von Checksums der Dateien genutzt werden soll. |
