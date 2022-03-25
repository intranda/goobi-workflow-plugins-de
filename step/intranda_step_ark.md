---
description: >-
  Goobi Step Plugin für die Erstellung von Archival Resource Keys (ARK) mit DataCite Metadaten.
---

# Katalogabfrage


## Einführung
Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Step Plugins für die Generierng von ARK-Identifiern in Goobi workflow.

| Details |  |
| :--- | :--- |
| Identifier | intranda_step_ark |
| Source code | [https://gitea.intranda.com/goobi-workflow/goobi-plugin-step-ark](https://gitea.intranda.com/goobi-workflow/goobi-plugin-step-ark) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.03 |
| Dokumentationsdatum | 25.03.2022 |


## Arbeitsweise des Plugins
Das Plugin wird üblicherweise vollautomatisch innerhalb des Workflows ausgeführt. Es ermittelt zunächst, ob bereits ein Archival Resource Key vorhanden ist. Sollte noch kein ARK vorhanden sein, wird ein neuer ARK registriert. Falls schon ein ARK in den Metadaten vorhanden ist, wird versucht die Metadaten des ARKs zu aktualisieren.


## Bedienung des Plugins
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_catalogue_request_de.png)


## Installation
Das Plugin besteht aus der folgenden Datei:

```text
plugin_intranda_step_ark.jar
```

Diese Datei muss in dem richtigen Verzeichnis installiert werden, so dass diese nach der Installation an folgendem Pfad vorliegt:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_ark.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_catalogue_ark.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_step_ark.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
	<!-- order of configuration is: 1.) project name and step name matches 2.)
		step name matches and project is * 3.) project name matches and step name
		is * 4.) project name and step name are * -->

	<config>
		<!-- which projects to use for (can be more then one, otherwise use *) -->
		<project>*</project>
		<step>*</step>

		<!-- URI of the ARK API, must use https -->
		<uri>https://www.arketype.ch/</uri>

		<!-- Name Assigning Number Authority -->
		<naan>99999</naan>

		<!-- name of the API user -->
		<apiUser>apiUser</apiUser>

		<!-- password of the API user -->
		<apiPassword></apiPassword>

		<!-- shoulder on which new ARKs shall be minted -->
		<shoulder>fgt</shoulder>

		<!-- Datacite Metadata fields -->

		<!-- metadata field datacite.creator -->
		<metadataCreator>{meta.CreatorsAllOrigin}</metadataCreator>

		<!-- metadata field datacite.title -->
		<metadataTitle>{meta.TitleDocMain}</metadataTitle>

		<!-- metadata field datacite.publisher -->
		<metadataPublisher>{meta.PublisherName}</metadataPublisher>

		<!-- metadata field datacite.publicationyear -->
		<metadataPublicationYear>{meta.PublicationYear}
		</metadataPublicationYear>

		<!-- metadata field datacite.resourcetype can only contain following values:
			Audiovisual, Collection, Dataset, Event, Image ,InteractiveResource, Model,
			PhysicalObject, Service, Software, Sound, Text, Workflow, Other. For more
			information consult the API-documentation https://www.arketype.ch/doc/api -->
		<metadataResourceType>Text</metadataResourceType>

		<!--target url ark will forward to -->
		<publicationUrl>https://viewer.example.org/{meta.CatalogIDDigital}
		</publicationUrl>

		<!--metadatatype in METS-File -->
		<metadataType>ARK</metadataType>

	</config>
</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `uri` | In diesem Parameter muss die URL der API hinterlegt werden. In der Regel kann der Standardeintrag `https://www.arketype.ch` übernommen werden.  |
| `naan` | NAAN ist ein Akronym für Name Assigning Number Authority. Es handelt sich um einen eindeutigen Bezeichner, der welcher der Account zugeordnet ist. |
| `apiUser` |  Name des API Nutzers |
| `apiPassword` | Passwort des API Nutzers |
| `shoulder` | Name des Unternamensraumes in dem die neuen ARKs erzeugt werden sollen |
| `metadataCreator` | Entspricht dem `datacite.creator` Feld und sollte die Forscher benennen, die die Daten erzeugt haben. In der Regel kann der vorgegebene Wert `{meta.CreatorsAllOrigin}` beibehalten werden.  |
| `metadataTitle` | Entspricht dem `datacite.title` Feld und sollte den Namen unter dem die Veröffentlichung bekannt ist beinhalten. In der Regel kann der vorgegebene Wert `{meta.TitleDocMain}` beibehalten werden. |
| `metadataPublisher` | Entspricht dem `datacite.publisher` Feld. In der Regel kann der vorgegebene Wert `{meta.PublisherName}` beibehalten werden. |
| `metadataResourceType` | Entspricht dem `datacite.publicationyear` Feld .In der Regel kann der vorgegebene Wert `{meta.PublicationYear}` beibehalten werden. |
| `metadataResourceType`   | Entspricht dem `datacite.resourcetype` Feld. Es sind nur die Werte Audiovisual, Collection, Dataset, Event, Image ,InteractiveResource, Model, PhysicalObject, Service, Software, Sound, Text, Workflow, und Other zulässig. Zusätzlich können noch spezifische Untertypen angegeben werden. Ein Beispiel wäre Image/`Photo`. Der Untertyp, also der Teil hinter dem `/`, unterliegt dabei keiner Einschränkung.|
| `publicationUrl`   | URL unter der Sie das digitalisierte Werk in Zukunft zur Verfügung stellen. In der Regel wird die Veröffentlichungs-URL einem Muster folgen, z.B. `https://viewer.example.org/{meta.CatalogIDDigital}`. In diesem Fall wird davon ausgegangen, dass die Werke in Zukunft unter einer URL veröffentlicht werden, die das Metadatum `Identifier` enthält. |
| `metadaType`  | Gibt den Metadatentyp an unter dem die URN erfasst werden soll. Hier sollte die Vorgabe nicht verändert werden.  |
