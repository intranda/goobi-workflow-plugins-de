---
description: >-
  Dieses Workflow-Plugin erlaubt den Massenimport von Daten aus JSON-Dateien
---

# Generisches Import Plugin für JSON-Dateien

## Einführung

Dieses Workflow-Plugin wurde implementiert, um Metadaten aus mehreren JSON Dateien auszulesen und in einen Workflow zu importieren.

## Übersicht

| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_import\_json |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-import-json](https://github.com/intranda/goobi-plugin-workflow-import-json) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 23.07 |
| Dokumentationsdatum | 01.09.2023 |

## Installation

* Für die Installation braucht man `plugin_intranda_workflow_import_json.jar` und `plugin_intranda_workflow_import_json-GUI.jar`
```bash
{PATH_TO_GOOBI}/plugins/workflow/plugin_intranda_workflow_import_json.jar
{PATH_TO_GOOBI}/plugins/GUI/plugin_intranda_workflow_import_json-GUI.jar
```
* Die Konfigurationsdatei `plugin_intranda_workflow_import_json.xml` muss kopiert zu
```bash
{PATH_TO_GOOBI}/config/plugin_intranda_workflow_import_json.xml
```
Es können verschiedene Werte in der Konfigurationsdatei angepasst werden. Ein Beispiel sieht wie folgt aus:

```markup
<config_plugin>
	<!-- folder that contains the json files -->
	<jsonFolder>/opt/digiverso/g2g/workspace/demo_content/</jsonFolder>

	<!-- folder used to hold the downloaded images temporarily -->
	<importFolder>/opt/digiverso/g2g/workspace/workflow/tmp/</importFolder>

	<!-- which workflow to use -->
	<workflow>KHM_Giza_Import_Workflow</workflow>

	<!-- which publication type to use -->
	<publicationType>GizaDocument</publicationType>

	<!-- URL types that hold downloadable resources, could be none or multiple -->
	<downloadableUrl>GizaPhotosMain</downloadableUrl>
	<downloadableUrl>GizaPrimaryDisplayMain</downloadableUrl>

	<!-- whether or not to save the partner url, DEFAULT false -->
	<partnerUrl save="true">
		<!-- at most one urlBase should be configured, DEFAULT "" -->
		<urlBase>giza.fas.harvard.edu</urlBase>
		<!-- there could be multiple urlParts-->
		<urlPart>$._type</urlPart>
		<urlPart>$._id</urlPart>
		<!-- at most one urlTail should be configured, DEFAULT "" -->
		<urlTail>full</urlTail>
		<!-- MetadataType that is to hold the value of the partner url -->
		<urlMetadata>GizaPartnerUrl</urlMetadata>
	</partnerUrl>

	<!-- same source could be used multiple times for different MetadataTypes -->
	<metadata source="$._source.id" target="TitleDocMain" />
	<metadata source="$._source.id" target="CatalogIDDigital" />
	<!-- source that does not start with $ or @ will be regarded as value -->
	<metadata source="Giza" target="singleDigCollection" />
	<!-- source that ends with [:] is an array -->
	<metadata source="$._source.allnumbers[:]" target="GizaAllNumbers" />

	<!-- group is for MetadataGroups, there could be multiple -->
	<!-- no need to add the [:] to the end of @source -->
	<!-- @type is the name of the MetadataGroupType -->
	<group source="$._source.sitedates" type="GizaSiteDatesGroup">
		<!-- source that starts with @ will be regarded as relative json path -->
		<metadata source="@.type" target="GizaSiteDateType" />
		<metadata source="@.date" target="GizaSiteDate" />
	</group>

	<!-- @altType is the name of an alternative MetadataGroupType, which will be used to create another MetadataGroup for all items that are filtered out. OPTIONAL. -->
	<!-- @key is the filtering key that is used to retrieve the value from the JSONObject. OPTIONAL. -->
	<!-- @value is used to compare with the value retrieved from the JSONObject. OPTIONAL. DEFAULT "". -->
	<!-- @method defines the logic for comparing values. Options are is, not, startsWith, endsWith, contains. OPTIONAL. DEFAULT 'is'. -->
	<group source="$._source.relateditems.modernpeople" type="ModernPeopleGroup" altType="_ModernPeopleGroupHidden" key="role" value="Excavator" method="is">
		<metadata source="@.role" target="GizaModernPeopleRole" />
		<!-- same source could be used multiple times for different MetadataTypes -->
		<metadata source="@.id" target="GizaModernPeopleId" />
		<metadata source="@.id" target="GizaModernPeopleId2" />
	</group>

	<!-- if no @altType is configured, then all items that are filtered out will not be imported. -->
	<group source="$._source.relateditems.ancientpeople" type="AncientPeopleGroup" key="role" value="Tomb Owner">
		<metadata source="@.role" target="GizaAncientPeopleRole" />
		<metadata source="@.id" target="GizaAncientPeopleId" />
	</group>

	<!-- child is for child DocStructs, there could be multiple -->
	<!-- no need to add the [:] to the end of @source-->
	<!-- the attributes @key, @value, @method are the same as in group -->
	<!-- there is NO @altType attribute for child, all items that are filtered out will not be imported -->
	<child source="$._source.relateditems.photos" type="GizaPhoto" key="number" value="KHM_AEOS_" method="startsWith">
		<!-- same source could be used multiple times for different MetadataTypes -->
		<metadata source="@.drs_id" target="GizaPhotosDrsId" />
		<metadata source="@.drs_id" target="CatalogIDDigital" />
	</child>

</config_plugin>
```

## Allgemeine Konfiguration des Plugins

| Wert | Beschreibung |
| :--- | :--- |
| `jsonFolder` | Pfad zur Verzeichnis wo sich die JSON Dateien befinden. |
| `importFolder` | Pfad zur Verzeichnis wo die herunterzuladenden Bilder landen, bevor sie importiert werden. |
| `workflow` | Name des Workflows als Template. |
| `publicationType` | Publikationstyp des neuen Prozesses. |
| `downloadableUrl` | Name des Typs der URL Metadaten, woraus Bilder herunterzuladen sind. Man kann hier auch mehrere Fälle konfigurieren. |
| `metadata` | Aus jedem Tag wird schließlich ein Metadata Objekt erzeugt. Das Attribut `@source` bezieht sich auf einen JSON-Pfad, wenn es mit `$` startet. Aus diesem Pfad wird der Wert des Metadatums ausgelesen. In diesem Falle bezieht sich das Attribut auf eine Liste wenn es zusätzlich noch mit `[:]` endet. Wenn es weder mit `$` noch mit `@` startet, dann wird es selbst als Wert des Metadatums benutzt. Das Attribut `@target` konfiguriert den Namen des MetadataTypes. |
| `group` | Hier werden MetadataGroups konfiguriert. Es gibt sechs Attribute, siehe unten für Details. Unter einem `group` Tag können mehrere `metadata` Tags konfiguriert werden, deren `@source` Attribute mit `@` starten sollen.  |
| `child` | Hier werden Kinder DocStructs konfiguriert. Es gibt fünf Attribute, siehe unten für Details. Unter einem `child` Tag können mehrere `metadata` Tags konfiguriert werden, deren `@source` Attribute mit `@` starten sollen. |

## Konfiguration der Attribute eines MetadataGroups

Die folgenden Attribute können in einem `group` Tag konfiguriert werden:

| Wert | Beschreibung |
| :--- | :--- |
| `@source` | JSON-Pfad zum Eltern-Tag einer Liste. Startet mit einem `$`. |
| `@type` | Name des MetadataGroupTypes. |
| `@altType` | Name eines alternativen MetadataGroupTypes. Wenn der alternative Typ richtig konfiguriert wird, wird dann aus allen ausgefilterten Einträgen eine MetadataGroup von diesem Typ erzeugt. Wenn das Attribut nicht konfiguriert ist, oder wenn der konfigurierte Typ nicht zu finden ist, wird dann alle ausgefilterten Einträge nicht importiert. OPTIONAL. |
| `@key` | Schlüssel der JSONObjekten fürs Filtern. OPTIONAL. |
| `@value` | Wert zum Vergleich beim Filtern. OPTIONAL. |
| `@method` | Logik zu nutzen beim Filtern. Möglichkeiten sind `is`, `not`, `startsWith`, `endsWith` und `contains`. OPTIONAL. DEFAULT `is`. |

## Konfiguration der Attribute eines DocStructs als Kind

Die folgenden Attribute können in einem `child` Tag konfiguriert werden:

| Wert | Beschreibung |
| :--- | :--- |
| `@source` | JSON-Pfad zum Eltern-Tag einer Liste. Startet mit einem `$`. |
| `@type` | Name des DocStructTypes. |
| `@key` | Schlüssel der JSONObjekten fürs Filtern. OPTIONAL. |
| `@value` | Wert zum Vergleich beim Filtern. OPTIONAL. |
| `@method` | Logik zu nutzen beim Filtern. Möglichkeiten sind `is`, `not`, `startsWith`, `endsWith` und `contains`. OPTIONAL. DEFAULT `is`. |

## Konfiguration des Metadatums für den URL des Partners

Es gibt eine Möglichkeit, ein Metadatum für den URL des Partners zu erzeugen und zu speichern. Der zu erzeugende URL wird formuliert als `{urlBase}/{urlPart_1}/{urlPart_2}/.../{urlPart_k}/{urlTail}/`.

| Wert | Beschreibung |
| :--- | :--- |
| `partnerUrl` | Das Attribut `save` konfiguriert ob ein Metadatum für den URL des Partners erzeugt werden soll. DEFAULT `false`. |
| `urlBase` | Hier konfiguriert man den URL zum Server des Partners. OPTIONAL. MAXIMAL ein Treffer. |
| `urlPart` | Hier konfiguriert man die Teile, deren Werte aus den JSONObjekten an die `urlBase` nacheinander anhängen werden. OPTIONAL. Mehrere Treffer möglich. |
| `urlTail` | Hier konfiguriert man den letzten Teil des URLs. OPTIONAL. MAXIMAL ein Treffer. |
| `urlMetadata` | Name des MetadataTypes für dieses Metadatum. |
