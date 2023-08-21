---
description: >-
  Dies ist eine technische Dokumentation für das Import Plugin für die Reichskrone. Es ermöglicht den Import einer hierarchisch organisierten Exceldatei.
---

# Reichskrone Import

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Importplugins für die Reichskrone in Goobi.

Mithilfe dieses Plugins kann eine Exceldatei importiert werden. Die einzelnen Zeilen werden zu Goobi-Vorgängen konvertiert, es können Bilder importiert werden und es wird eine hierarchische EAD-Tektonik erstellt.

| Details |  |
| :--- | :--- |
| Identifier | intranda_import_crown |
| Source code | [https://gitea.intranda.com/goobi-workflow/goobi-plugin-import-crown](https://gitea.intranda.com/goobi-workflow/goobi-plugin-import-crown) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 23.05 und neuer |
| Dokumentationsdatum | 21.08.2023 |

## Installation

Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```text
/opt/digiverso/goobi/plugins/import/plugin_intranda_import_crown.jar
/opt/digiverso/goobi/config/plugin_intranda_import_crown.xml
```


Außerdem muss die XML-Datenbank BaseX im Hintergrund laufen und eingerichtet sein. Die Installation wird [hier](https://docs.goobi.io/goobi-workflow-plugins-de/administration/intranda_administration_archive_management/installation_for_productive_use) beschrieben.



## Überblick und Funktionsweise

Die zu importierende Exceldatei muss folgende Struktur beinhalten:

|  |  |          |                       |                                |                                                        |
|------|-------------|----------|-----------------------|--------------------------------|--------------------------------------------------------|
| **CR_1** | <font color="grey">Reichskrone</font> |          |                       |                                |                                                        |
|      | **CR_1_A-H**    | <font color="grey">Kronreif</font> |                       |                                |                                                        |
|      |             | **CR_1_A**   | <font color="grey">Platte A, Stirnplatte</font> |                                |                                                        |
|      |             |          | **CR_1_A_GrPl**           | <font color="grey">Grundplatte</font>                    |                                                        |
|      |             |          |                       | **CR_1_A_GrPl_1**                  | <font color="grey">Riss in Grundplatte (?)</font>                                |
|      |             |          |                       | **CR_1_A_GrPl_2**                  | <font color="grey">Riss in Grundplatte und Grundplattenperldrahtumsäumung</font> |
|      |             |          |                       | **CR_1_A_GrPl_3**                  | <font color="grey">Riss in Grundplatte</font>                                    |
|      |             |          |                       | **CR_1_A_GrPl_4**                  | <font color="grey">Riss in Grundplatte und Grundplattenperldrahtumsäumung</font> |
|      |             |          |                       | **CR_1_A_GrPl_5**                  | <font color="grey">Deformierung von Grundplatte</font>                           |
|      |             |          |                       | **CR_1_A_GrPl_6**                  | <font color="grey">Steg durch Öffnung in Grundplatte hinter Fa_4</font>          |
|      |             |          |                       | **CR_1_A_GrPl_7**                  | <font color="grey">4 Löcher in Grundplatte</font>                                |
|      |             |          |                       | **CR_1_A_GrPl_8**                  | <font color="grey">Löcher in Grundplatte</font>                                  |
|      |             |          |                       | **CR_1_A_GrPl_9**                  | <font color="grey">4 Löcher in Grundplatte</font>                                |
|      |             |          |                       | **CR_1_A_GrPl_10**                 | <font color="grey">angelöteter Span auf Grundplatte</font>                       |
|      |             |          | **CR_1_A_SchS**           | <font color="grey">Scharnierstift</font>                 |                                                        |
|      |             |          | CR_1_A_SchR           | <font color="grey">Scharnierrohre</font>                 |                                                        |
|      |             |          |                       | **CR_1_A_SchR_1**                  | <font color="grey">Scharnierrohr</font>                                          |
|      |             |          |                       | **CR_1_A_SchR_2**                  | <font color="grey">Scharnierrohr</font>                                          |
|      |             |          |                       | **CR_1_A_SchR_3**                  | <font color="grey">Scharnierrohr</font>                                          |
|      |             |          | **CR_1_A_GrUm**           | <font color="grey">Grundplattenperldrahtumsäumung</font> |                                                        |
|      |             |          |                       | **CR_1_A_GrUm_1**                  | <font color="grey">Grundplattenperldrahtumsäumung</font>                         |
|      |             |          |                       | **CR_1_A_GrUm_2**                  | <font color="grey">Grundplattenperldrahtumsäumung</font>                         |
|      |             |          | **CR_1_A_GrFi**           | <font color="grey">Grundplattenfiliigrandekor</font>     |                                                        |
|      |             |          | CR_1_A_RoeG           | <font color="grey">Röhrchen mit Granalien</font>         |                                                        |
|      |             |          |                       | **CR_1_A_RoeG_1**                  | <font color="grey">Röhrchen mit Kugelpyramide</font>                             |
|      |             |          |                       | **CR_1_A_RoeG_2**                  | <font color="grey">Röhrchen mit Kugelpyramide</font>                             |



Die Datei wird zeilenweise eingelesen und analysiert. Dabei wird zuerst geprüft, wie weit die aktuelle Zeile eingerückt wurde. Ist keine Einrückung vorhanden, liegt das root-Element der Tektonik vor. Ansonsten handelt es sich um Unterelemente. Das übergeordnete Element einer jeden Zeile ist dabei jeweils das letzte Element mit einer geringeren Einrückung.

Als nächstes werden die beiden Zellen mit Inhalt gelesen. Die erste Information ist dabei die Inventarnummer/der Identifier des Datensatzes und die zweite ist eine Beschreibung. Wenn die erste Information **fett** ist, dann wird für diese Zeile auch ein Vorgang erstellt und nach Bildern gesucht. Der Titel wird aus beiden Informationen zusammengesetzt.

Die Bilder werden innerhalb eines konfigurierten Ordners in Unterordnern erwartet, die nach der Inventarnummer benannt sind. Diese können entweder flach in einer Ordnerliste organisiert sein oder der gleichen hierarchischen Struktur folgen wie die Tektonik.

Wird ein Ordner gefunden, werden alle darin enthaltenen Dateien aufgelistet und nach folgenden Regeln geprüft:

1. ignoriere alle Daten, die kein tif, jpg oder wmv sind
2. ignoriere alle Dateien, die das Wort "komprimiert" enthalten
3. wenn eine Datei ohne den suffix "_bearbeitet" gefunden wurde, prüfe, ob es eine Datei mit dem gleichen Namen und dem suffix "_bearbeitet" gibt. Falls ja, ignoriere die aktuelle Datei un nutze die Version mit "_bearbeitet"
4. wenn eine jpg Datei gefunden wurde, prüfe, ob es ein tif mit dem gleichen Namen gibt, falls ja, ignoriere die jpg Datei und nutze das tif


## Konfiguration

Die Konfiguration erfolgt in der Datei plugin_intranda_import_crown.xml:

```xml
<config_plugin>
	<config>
		<!-- which workflow template shall be used -->
		<template>*</template>

		<!-- define if import shall use GoobiScript to run in the background -->
		<runAsGoobiScript>true</runAsGoobiScript>
		
		<!-- first data row in excel file -->
		<startRow>7</startRow>

		<!-- basex database name and file name -->
        <basex>
            <database>basexdb</database>
            <filename>crown.xml</filename>
        </basex>

        <images>/opt/digiverso/import/crown/</images>

		<!-- metadata --> 
        <metadata>
            <doctype>Other</doctype>
            <title>TitleDocMain</title>
            <identifier>CatalogIDDigital</identifier>
            <description>ContentDescription</description>
        </metadata>

	</config>
</config_plugin>
```

Im Feld `<template>` wird definiert, für welche Produktionsvorlage die vorliegende Konfiguration angewendet werden soll, da das `<config>` wiederholbar ist, sind unterschiedliche Konfigurationen für verschiedene Produktionsvorlagen möglich. So kann es zum Beispiel für die Reichskrone eine andere Konfiguration geben als für den Reichsapfel.

Das Feld `<runAsGoobiScript>` steuert, ob der Import direkt in der Nutzersession oder im Hintergrund als GoobiScript ausgeführt wird. Bei größeren Exceldateien empfielt sich die Nutzung von GoobiScript.

`<startRow>` legt fest, welche Zeile die erste Datenzeile der Exceldatei ist. Damit können oberhalb weitere Informationen wie Header, Beschreibungen oder Hilfetexte angegeben werden, die dann vom Import ignoriert werden.

Der Bereich `<basex>` legt fest, wo die EAD-Tektonik gespeichert wird. Das Unterelement `<database>` enthält den Namen der BaseX-Datenbank, diese muss bereits existieren. In `<filename>` wird der Name der EAD-Datei festgelegt. Wenn dieser Name bereits verwendet wird, werden vorhandene Daten überschrieben.

Der root-Ordner der Bilder wird im `<images>` Element festgelegt und `<metadata>` enthält die zu verwendenden Metadaten. Mittels `<doctype>` wird der Strukturtyp definiert und die Felder `<title>`, `<identifier>` und `<description>` enthalten die Namen der Metadaten für Titel, Inventarnummer und Beschreibungstext.
