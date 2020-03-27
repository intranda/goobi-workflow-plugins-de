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
| Version des Plugins | 1.2.0 |
| Identifier | intranda\_step\_pdf-extraction |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi Workflow 3.0.6 |
| Dokumentation vom | 09.04.2019 |

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

```markup
<config>
	<docType>
		<parent>Chapter</parent>
		<children>Chapter</children>
	</docType>
	
	<images>
		<resolution>300</resolution>
		<format>tif</format>
	</images>
	
	<properties>
		<fulltext>
			<name>OCRDone</name>
			<value exists="true">YES</value>
			<value exists="false">NO</value>
		</fulltext>
	</properties>
</config>
```

Über `<docType>` wird geregelt, welche Strukturtypen die aus dem PDF-Inhaltsverzeichnis extrahierten Einträge in der METS-Datei erhalten. Das `<parent>`-Element ist dabei das Hauptelement in dem alle anderen Inhaltsverzeichnis-Einträge landen. Wird es weggelassen, werden alle Einträge direkt in das Hauptelement der METS-Datei eingetragen.

Der `<images>`-tag regelt die Auflösung \(in DPI\) und das Ausgabeformat für die extrahierten Bilder.

Mit `<properties>` werden Vorgangseigenschaften je nach Ergebnis der Extraktion geschrieben. Die hier als Beispiel gegebene Konfiguration schreibt die Vorgangseigenschaft `OCRDone` mit Wert `YES`, wenn Volltext im PDF gefunden wurde und mit Wert `NO`, wenn es keinen Volltext in der PDF-Datei gab. Dies ist besonders hilfreich, wenn der Workflow im Nachhinein geändert werden soll, um zum Beispiel einen OCR-Schritt auszulassen, wenn schon Volltext existiert.

## Einstellungen in Goobi

Nachdem das Plugin installiert wurde, kann es in der Nutzeroberfläche in einem Workflowschritt konfiguriert werden. 

![](../.gitbook/assets/pdf_extraction_step%20%281%29.png)

## Nutzung

Für die Nutzung des Plugins muss zum Ausführungszeitpunkt im Master-Ordner des Vorgangs eine PDF-Datei liegen. Diese wird dann automatisch in Einzelseiten aufgeteilt. Außerdem wird \(falls vorhanden\) der Volltext extrahiert und das Inhaltsverzeichnis der PDF-Datei ausgelesen um dann als Strukturelemente in die METS-Datei eingetragen zu werden. 

Es ist also zu empfehlen, dem Workflowschritt mit diesem Plugin einen anderen Workflowschritt vorzulagern, in dem Dateien in den Master-Ordner geladen werden. Dies kann per Verlinken des Vorgangsordners in den Home-Ordner des Nutzers oder zum Beispiel im File-Upload-Plugin geschehen. 

