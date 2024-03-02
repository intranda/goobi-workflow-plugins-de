---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin für das
  automatische Auslesen von Informationen aus PDF-Dateien
---

# PDFs aufsplitten, Volltext extrahieren und Inhaltsverzeichnis auslesen

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zum Extrahieren von Bildern, Volltexten und dem Inhaltsverzeichnis aus PDF-Dateien. Das Plugin extrahiert immer nur, was in der PDF vorhanden ist und schreibt keine Fehlermeldung wenn kein Volltext oder Inhaltsverzeichnis gefunden werden kann.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_pdf-extraction |
| Source code | [https://github.com/intranda/goobi-plugin-step-pdf-extraction](https://github.com/intranda/goobi-plugin-step-pdf-extraction) |
| Lizenz | GPL 2.0 oder neuer |
| Dokumentationsdatum | 09.04.2019 |

## Voraussetzung

Voraussetzung für die Verwendung des Plugins ist der Einsatz von Goobi workflow in Version 3.0.6 oder höher, die korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte des Workflows.

Außerdem wird das Kommandozeilenprogramm Ghostscript benötigt. Es kann über die Paketquellen des Systems installiert werden.

## Installation und Konfiguration <a id="installation-und-konfiguration"></a>

Zur Nutzung des Plugins muss es an folgenden Ort kopiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_pdf-extraction.jar
```

Die Konfiguration des Plugins wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_pdf-extraction.xml
```

Eine Beispielkonfiguration könnte folgendermaßen aussehen:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configs>

	<config>
		<!--
        order of configuration is:
        1.) project name and step name matches
        2.) step name matches and project is *
        3.) project name matches and step name is *
        4.) project name and step name are *
        -->
		<project>*</project>
		<step>*</step>

		<validation>
			<failOnMissingPDF>true</failOnMissingPDF>
		</validation>

		<overwriteExistingData>true</overwriteExistingData>

		<docType>
			<parent>Chapter</parent>
			<children>Chapter</children>
		</docType>

		<mets>
			<write>true</write>
			<failOnError>false</failOnError>
		</mets>

		<images>
			<write>true</write>
			<failOnError>true</failOnError>
			<resolution>300</resolution>
			<format>tif</format>
			<generator>pdftoppm</generator>
			<generatorParameter>-cropbox</generatorParameter>
		</images>

		<plaintext>
			<write>true</write>
			<failOnError>false</failOnError>
		</plaintext>

		<alto>
			<write>true</write>
			<failOnError>false</failOnError>
		</alto>

		<pagePdfs>
			<write>true</write>
			<failOnError>false</failOnError>
		</pagePdfs>

		<properties>
			<fulltext>
				<name>OCRDone</name>
				<value exists="true">YES</value>
				<value exists="false">NO</value>
			</fulltext>
		</properties>

	</config>

	<!-- more configurations can be added for projects/steps
	<config>
	</config>
	-->

</configs>
```

## Einstellungen in der Konfigurationsdatei

In der Konfigurationsdatei können beliebig viele Konfigurationen für Projekte oder Arbeitsschritte mit bestimmten Namen definiert werden. Dazu können verschiedene `<config>`-Blöcke untereinander verwendet werden, wobei in jedem die Eigenschaften `<project>` und `<step>` anzugeben sind. Die `<config>`-Blöcke werden in der folgenden Reihenfolge auf einen bestimmten Arbeitsschritt angewandt:

1) `<project>` und `<step>` entsprechen dem aktuellen Projekt und Arbeitsschritt
2) `<step>` entspricht dem aktuellen Arbeitsschritt und `<project>` ist auf `*` gesetzt
3) `<project>` entspricht dem aktuellen Projekt und `<step>` ist auf  `*` gesetzt
4) `<project>` und `<step>` sind auf `*` gesetzt

Das `<failOnMissingPDF>`-Tag innerhalb des `<validation>`-Tags kann auf `true` gesetzt werden, um eine Warnung auszugeben, wenn keine PDF-Dateien gefunden werden konnten. Warnungen werden dann in das Journal und die server-internen Log-Dateien geschrieben. Wird diese Option mit `false` deaktiviert, so wird der Fall ignoriert, dass eventuell keine PDF-Dateien existieren.

Mit dem `<overwriteExistingData>`-Tag kann global für dieses Plugin eingestellt werden, ob existierende PDF-Dateien überschrieben werden dürfen.

Mittels `<docType>` wird geregelt, welche Strukturtypen die aus dem PDF-Inhaltsverzeichnis extrahierten Einträge in der METS-Datei erhalten. Das `<parent>`-Element ist dabei das Hauptelement in dem alle anderen Inhaltsverzeichnis-Einträge landen. Wird es weggelassen, werden alle Einträge direkt in das Hauptelement der METS-Datei eingetragen. Mit dem `<children>` Element wird angegeben, welchen Strukturtyp die Unterelemente des aus dem PDF-Inhaltsverzeichnis extrahierten Eintrags bekommen sollen.

Die Elemente `<pagePdfs>`, `<alto>`, `<plaintext>`, `<images>` und `<mets>` haben jeweils eine Eigenschaft `<write>` und `<failOnError>`. Damit kann entsprechend des XML-Elements für PDF-Dateien, ALTO-Dateien, TXT-Dateien, allgemeine Bilddateien und die METS-Datei eingestellt werden, ob Dateien dieser Typen jeweils geschrieben oder überschrieben werden sollen und ob eine Fehlermeldung ausgegeben und die weitere Ausführung abgebrochen werden soll, wenn diese nicht geschrieben werden konnten.

Im `<images>` Tag sind einige weitere Einstellungen für Bilddateien möglich. Mit den Werten in `<resolution>` und `<format>` können die Bildauflösung \(in DPI\) und das Ausgabe-Dateiformat für die extrahierten Bilder festgelegt werden.

Das Unterelement `<generator>` innerhalb von `<images>` gibt an, welches ausführbare Programm auf dem Server verwendet werden soll, um die Bilder zu extrahieren. Gültige Werte sind in der Regel `pdftoppm` und `ghostscript`. Das Element `<generatorParameter>` kann mehrfach verwendet werden und beinhaltet jeweils einen Kommandozeilenparameter für das in `<generator>` angegebene Programm.

Mit `<properties>` werden Vorgangseigenschaften je nach Ergebnis der Extraktion geschrieben. Die hier als Beispiel gegebene Konfiguration schreibt die Vorgangseigenschaft `OCRDone` mit Wert `YES`, wenn Volltext im PDF gefunden wurde und mit Wert `NO`, wenn es keinen Volltext in der PDF-Datei gab. Dies ist besonders hilfreich, wenn der Workflow im Nachhinein geändert werden soll, um zum Beispiel einen OCR-Schritt auszulassen, wenn schon Volltext existiert.

## Einstellungen in Goobi

Nachdem das Plugin installiert wurde, kann es in der Nutzeroberfläche in einem Workflowschritt konfiguriert werden.

![](../.gitbook/assets/intranda_step_pdf_extraction.png)

## Nutzung

Für die Nutzung des Plugins muss zum Ausführungszeitpunkt im Master-Ordner des Vorgangs eine PDF-Datei liegen. Diese wird dann automatisch in Einzelseiten aufgeteilt. Außerdem wird \(falls vorhanden\) der Volltext extrahiert und das Inhaltsverzeichnis der PDF-Datei ausgelesen um dann als Strukturelemente in die METS-Datei eingetragen zu werden.

Es ist also zu empfehlen, dem Workflowschritt mit diesem Plugin einen anderen Workflowschritt vorzulagern, in dem Dateien in den Master-Ordner geladen werden. Dies kann per Verlinken des Vorgangsordners in den Home-Ordner des Nutzers oder zum Beispiel im File-Upload-Plugin geschehen.

