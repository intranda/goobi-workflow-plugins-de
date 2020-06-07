---
description: >-
  Dieses Workflow Plugin erlaubt die Generierung von verschiedenen Barcodes für
  die Verwendung auch außerhalb von Goobi selbst.
---

# Barcode Generator

## Einführung

Dieses Workflow-Plugin dient zur flexiblen Generierung von Barcodes in einer mehrseitigen PDF-Datei. Hierzu kann der Nutzer in einer Nutzeroberfläche verschiedene Werte konfigurieren und festlegen, welche Anzahl an Barcodes mit welchem Präfix und welchem Zähler erzeugt werden sollen. Zugleich kann festgelegt werden, welche xsl-Datei für de Generierung der PDF-Datei verwendet werden soll, so dass ein großes Maß an Freiheit hinsichtlich der optischen Gestaltung besteht.

## Übersicht

| Details |  |
| :--- | :--- |
| Version des Plugins | 1.0.0 |
| Identifier | goobi-plugin-workflow-barcode-generator |
| Source code | - Source code not yet publicly available - |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.02 |
| Dokumentation vom | 15.02.2020 |

## Installation

Zur Installation des Plugins müssen folgende beiden Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_barcode-generator.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_barcode-generator-GUI.jar
```

Um zu konfigurieren, wie sich das Plugin verhalten soll, können verschiedene Werte in der Konfigurationsdatei angepasst werden. Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_barcode-generator.xml
```

Der Inhalt dieser Konfigurationsdatei sieht wie folgt aus:

```markup
<config_plugin>

	<!--  default value for the number format -->
	<format>00000</format>

	<!--  default value for the amount of barcodes to be generated -->
	<amount>200</amount>

	<!--  default value the first barcode number -->
	<start>1</start>

	<!--  default value the first barcode number -->
	<prefix></prefix>

	<!-- path to xslt file for barcode generation,
	this value can exist multiple times and gets displayed as dropdown list -->
	<xslt-path>/opt/digiverso/goobi/xslt/barcodes.xsl</xslt-path>

</config_plugin>
```

## Allgemeine Konfiguration des Plugins

Einige Konfigurationen des Plugins gelten allgemein für alle zu importierenden Datensätze. Diese lauten wie folgt:

| Wert | Beschreibung |
| :--- | :--- |
| `format` | Dieser Parameter legt fest, ob die Zähler mit führenden Nullen aufgefüllt werden sollen. Der Wert `00000` beispielsweise legt fest, dass alle Zahlen zumindest mit fünf Stellen angezeigt werden. |
| `amount` | Dieser Parameter legt fest, wieviele Barcodes festgelegt werden. |
| `start` | Soll der Zähler für die Barcodes bei einem bestimmten Startwert beginnen, so kann dieser hier festgelegt werden. |
| `prefix` | Dieser Parameter definiert einen Präfix, der dem Zähler mit einem Unterstrich `_` vorangestellt wird. |
| `xslt-path` | Der Parameter `xslt-path` erlaubt die Definition beliebig vieler xsl-Dateien. Die hier konfigurierten Dateien werden dem Nutzer anschließend innerhalb der Nutzeroberfläche zur Auswahl angeboten. |

##
