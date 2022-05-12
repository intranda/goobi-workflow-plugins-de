---
description: >-
  Dies ist eine technische Dokumentation für das Goobi Export-Plugin configurable. Es ermöglicht, den Exportort mithilfe der Projektkonfiguration anzupassen. Durch die Verwendung von Exportprojekten ist es auch möglich in verschiedene Zielorte in einem Durchlauf zu exportieren
---

# Export Configurable

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Export-Plugins in Goobi.

Mithilfe dieses Export-Plugins für Goobi können die Goobi-Vorgänge innerhalb eines Arbeitsschrittes gleichzeitig an mehrere Orte exportiert werden.

| Details |  |
| :--- | :--- |
| Identifier | HaabExport |
| Source code | [https://github.com/intranda/goobi-plugin-export-configurable](https://github.com/intranda/goobi-plugin-export-configurable) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.03 und neuer |
| Dokumentationsdatum | 12.05.2022 |

## Installation

Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/plugin_intranda_export_configurable-step.png)

Das Plugin muss zunächst in folgendes Verzeichnis kopiert werden:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_configurable.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_configurable.xml
```
## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_export_configurable.jar` und über die Projekteinstellungen. Die Konfiguration kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
	<!--
        order of configuration is:
          1.) project name name matches
          2.) project is *
	-->
	<config>
		<project>testocr</project>
		<folder>
			<includeMedia>true</includeMedia>
			<includeMaster>true</includeMaster>
			<includeSource>false</includeSource>
			<includeImport>false</includeImport>
			<includeExort>false</includeExort>
			<includeITM>false</includeITM>
			<includeOcr>true</includeOcr>
            <includeValidation>false</includeValidation>
			<ocr>
				<suffix>alto</suffix>
			</ocr>
		</folder>
	</config>


	<config>
		<project>*</project>
		<target key="{meta.ViewerInstance}" value="eivfaanddigihub" projectName="eivfExportProject"/>
		<target key="{meta.ViewerInstance}" value="eivfaanddigihub" projectName="gihubExportProject"/>
		<target key="{meta.ViewerInstance}" value="" projectName=""/>
		<target key="{meta.URN}" value="" projectName=""/>
        <includeMarcXml>false<includeMarcXml>
		<folder>
            <!-- as configured in goobi_config.properties -->
            <!--genericFolder>thumbs</genericFolder-->
			<includeMedia>true</includeMedia>
			<includeMaster>true</includeMaster>
			<includeOcr>false</includeOcr>
			<includeSource>false</includeSource>
			<includeImport>false</includeImport>
			<includeExort>false</includeExort>
			<includeITM>false</includeITM>
			<includeValidation>false</includeValidation>
		</folder>
	</config>

</config_plugin>

```

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Der `<config>`-Block mit dem `project` `*` wird immer verwendet, wenn kein anderer Block auf den Projektnamen passt.  
| `target` | Dieser Parameter hat 3 obligatorische Attribute. Im Parameter `key` sollte eine Goobi Variable der Form {meta.Metadatenname} verwendet werden. Im Attribut `value` kann dann der geünschte Wert angegeben werden. Setzt man `value=""` So schlägt die Bedingung an, wenn das Metadatum leer oder nicht gesetzt ist. Im Attribut `projectName` sollte der Name des Exportprojektes, mit dessen Einstellungen der Export stattfinden soll, angegeben werden. Wird dem Attribut ein leerer String zugewiesen `projectName=""`, so werden die Einstellungen des Projektes des Vorgangs zum Export verwendet. Wenn keine target condition gesetzt ist, wird ein normaler Export durchgeführt. Für jede target Bedingung, die zutrifft, wird ein Export angestoßen.  |
|`includeMarcXml`| Dieser Parameter legt fest, ob evtl. vorhandene MARC-XML Daten in die exportierte Metsdatei eingebettet werden sollen. Der Defaultwert ist false.|

Der Block `<config>` ist wiederholbar und kann so in unterschiedlichen Projekten verschiedene Metadaten definieren. Der Block mit <project>*</project> wird angewendet, wenn kein Block mit der Projektbezeichnung des Projektes existiert.

### Der folder-Block

Der folder Block befindet sich in jedem `config`-Element.

| Parameter | Erläuterung |
| :--- | :--- |
| `includeMedia` | Hier kann definiert werden, ob der media-Ordner exportiert werden soll.|
| `includeMaster` | Hier kann definiert werden, ob der master-Ordner exportiert werden soll. |
| `includeOcr` | Hier kann definiert werden, ob der ocr-Ordner exportiert werden soll. |
| `includeSource` | Hier kann definiert werden, ob der source-Ordner exportiert werden soll. |
| `includeImport` | Hier kann definiert werden, ob der import-Ordner exportiert werden soll. |
| `includeExort` | Hier kann definiert werden, ob der export-Ordner exportiert werden soll. |
| `includeITM` | Hier kann definiert werden, ob der ITM-Ordner exportiert werden soll. |
| `includeValidation` | Hier kann definiert werden, ob der validation-Ordner exportiert werden soll. |
| `ocr` | Das ocr Element wird benötigt, wenn man OCR-Ordner mit verschiedenen Sufixen verwendet. Das konkrete Suffix kann dann im Unterlement `suffix` angegeben werden. |

Der Defaultwert für jeden dieser Parameter außer ocr ist false. Wird der jeweilige Parameter nicht in der Konfiguration erwähnt, findet kein Export des entsprechenden Ordners statt.

Die Konfiguration des Zielordners kann in den Projekteinstellungen vorgenommen werden. Wenn der Haken bei `Erzeuge Vorgangsverzeichnis` gesetzt ist, wird der Vorgang in einen Unterordner mit seinem Titel als Namen im Zielverzeichnis abgelegt.

![](../.gitbook/assets/plugin_intranda_export_configurable-project.png)
