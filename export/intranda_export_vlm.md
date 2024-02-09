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
| Dokumentationsdatum | 12.01.2023 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_export_vlm_de.png)

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
		<!-- The name of the project -->
		<!-- MANDATORY -->
		<project>Archive_Project</project>
		
		<!-- The field to use as identifier e.g. CatalogIDDigital.  -->
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
		
		<!-- Whether or not use SFTP for the export. -->
		<!-- If true then use SFTP. If false then perform local export. -->
		<!-- If left blank, then the default setting 'false' will be used. -->
		<sftp>true</sftp>
		
		<!-- Absolute path to the location of the file 'known_hosts'. -->
		<!-- If left blank, then the default setting '{user.home}/.ssh/known_hosts' will be used. -->
		<knownHosts></knownHosts>
		
		<!-- User name at the remote host. -->
		<!-- MANDATORY if sftp is set to be true. -->
		<username>CHANGE_ME</username>
		
		<!-- Name of the remote host. -->
		<!-- MANDATORY if sftp is set to be true. -->
		<hostname>CHANGE_ME</hostname>
		
		<!-- Password to log into the remote host 'username'@'hostname'. -->
		<password>CHANGE_ME</password>
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
		
		<sftp>false</sftp>
		<!-- Use the default setting '{user.home}/.ssh/known_hosts'. -->
		<knownHosts></knownHosts>
		
		<username></username>
		<hostname></hostname>
		<password></password>
	</config>

	<!-- Apply this configuration only under the condition that the `singleDigCollection` in the metadata is a 20 digit number -->
	<config>
		<project>Manuscript_Project</project>		
		<identifier>CatalogIDDigital</identifier>		
		<volume>CurrentNoSorting</volume>		
		<path>/tmp/somewhere</path>
		<subfolderPrefix></subfolderPrefix>
		
		<condition>
            <type>variablematcher</type>
            <field>{meta.singleDigCollection}</field>
            <matches>\d{20}</matches>
        </condition>
		
		<sftp>false</sftp>
		<knownHosts></knownHosts>
		
		<username></username>
		<hostname></hostname>
		<password></password>
	</config>
	
	<config>
		<project>*</project>
		<identifier>CatalogIDDigital</identifier>
		<volume>CurrentNoSorting</volume>		
		<!-- Setting up path using an ABSOLUTE path. -->
		<path>/opt/digiverso/viewer/hotfolder</path>
		<!-- No common prefix needed. -->
		<subfolderPrefix></subfolderPrefix>
		
		<!-- Use the default setting 'false'. -->
		<sftp></sftp>
		<!-- Use the default setting '{user.home}/.ssh/known_hosts'. -->
		<knownHosts></knownHosts>
		
		<username></username>
		<hostname></hostname>
		<password></password>
	</config>

</config_plugin>
```

| Parameter         | Erläuterung                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | Dieser Parameter legt fest, welches Metadatum als Ordnername verwendet werden soll. |
| `volume`          | Dieser Parameter steuert, mit dem Inhalt welchen Metadatums die Unterverzeichnisse für Bände benannt werden sollen. |
| `path`            | Dieser Parameter legt den Export-Pfad fest, wohin die Daten exportiert werden sollen. Erwartet wird ein absoluter Pfad. |
| `condition`       | Dieses Element ist optional und kann mehrfach auftreten um Bedingungen zu definieren, unter welchen dieser Konfigurationsabschnitt verwendet werden kann. Das Format den `condition` Elements ist weiter unten beschrieben. Ein Konfigurationsabschnitt kann verwendet werden, wenn alle Bedingungen erfüllt sind. Wenn es mehrere Konfigurationsabschnitte gibt und mehr als eine verwendet werden könnte wird der Konfigurationsabschnitt mit den meisten Bedingungen ausgewählt (je spezieller die Bedingungen werden, desto höher ist die Priorität). Sollte das nicht eindeutig sein, wird ein beliebiger Konfigurationsabschnitt gewählt. In diesem Fall wird dem Benutzer eine Fehlermeldung angezeigt. |
| `subfolderPrefix` | Dieser Parameter beschreibt den Präfix, der für jeden Band eines mehrbändigen Werkes in der Ornderbezeichnung vorangestellt werden soll. (Beispiel `T_34_L_`: Hier steht `T_34` für die Erkennung zur Erstellung eines Strukturknotens des Typs `Band` und das `L` gibt an, dass danach ein Text kommt.) |
| `sftp`            | Dieser Parameter legt fest, ob der Export mittels SFTP stattfinden soll. |
| `knownHosts`      | Dieser Parameter legt fest, wo die Datei namens `known_hosts` ist. Wenn keine Datei angegeben wurde, dann wird der Pfad `{user.home}/.ssh/known_hosts` genutzt. Sonst wird hier ein absoluter Pfad erwartet. |
| `username`        | Dieser Parameter legt fest, welcher Nutzername für die Anmeldung bei dem Remote-Host verwendet werden soll. |
| `hostname`        | Dieser Parameter legt fest, wie der Remote-Host heißt. |
| `password`        | Dieser Parameter definiert das Passwort, das für die Anmldung mittels `username`@`hostname` verwendet werden soll. |


### Format der Bedingungen
Aktuell gibt es nur eine mögliche Art von Bedingungen, die `variablematcher` Bedingung. Diese Art der Bedingung prüft eine beliebige Variable, welche im Feld `field` definiert wird, gegen einen regulären Ausdruck, welcher im Feld `matches` definiert wird.

Eine Beispiel `condition` ist hier zu sehen:
```
<condition>
    <type>variablematcher</type>
    <field>{meta.singleDigCollection}</field>
    <matches>\d{20}</matches>
</condition>
```

Diese Bedingung hat den Typen `variablematcher`. Sie prüft das Feld `{meta.singleDigCollection}`, welches dem Metadatum `singleDigCollection` entspricht. Die Bedingung prüft, ob der Wert des Metadatums dem regulären Ausdruck `\d{20}` entspricht, also ob der Wert aus 20 Ziffern besteht.