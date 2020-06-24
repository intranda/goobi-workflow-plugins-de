---
description: >-
  Administration Plugins für eine Migration von einem Goobi worklow System zu einem anderen Goobi workflow System
---

# Goobi-to-Goobi

## 1. Einführung

Mit den beiden hier beschriebenen Plugins ist ein Datentransfer von einem Goobi workflow System zu einem anderem Goobi workflow System (`Goobi-to-Goobi`) möglich. Diese Dokumentation erläutert die Installation, Konfiguration sowie die Verwendung der zugehörigen Plugins.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda_administration_goobi2goobi_export <br/> intranda_administration_goobi2goobi_import_infrastructure <br/> intranda_administration_goobi2goobi_import_data |
| Source code | [https://github.com/intranda/goobi-plugin-administration-dataexport](https://github.com/intranda/goobi-plugin-administration-dataexport) <br/>[https://github.com/intranda/goobi-plugin-administration-dataimport](https://github.com/intranda/goobi-plugin-administration-dataimport) |
| Licence | GPL 2.0 oder neuer |
| Kompatibilität | 2020.06 |
| Dokumentationsdatum | 16.06.2020 |

## 1. Installation und Konfiguration

Bevor die Verwendung des Export- und Import-Mechanismus erfolgen kann, müssen verschiedene Installations- und Konfigurationsschritte durchlaufen werden. Diese sind hier im Detail beschrieben:

[Installation und Konfiguration](installation.md)

## 2. Arbeitsweise

Der Mechanismus für einen Datentransfer von einem Goobi workflow System zu einem anderem Goobi workflow System (`Goobi-to-Goobi`) ist in drei große Arbeitsschritte aufgeteilt.

![Funktionsweise des Goobi-to-Goobi Datenaustausches](images/goobi-to-goobi-description_de.png)

Diese drei Arbeitsschritte gestalten sich folgendermaßen:

### a) Erzeugung der Export-Verzeichnisse

Im ersten Arbeitsschritt erfolgt auf dem Ausgangssystem eine Anreicherung der Daten innerhalb des Dateisystems mit denjenigen Informationen, die Goobi intern in der Datenbank für jeden Vorgang gespeichert hat. Mit der Ausführung dieses Arbeitsschrittes wird somit in den Ordner eines jeden Goobi-Vorgangs eine zusätzliche xml-Datei geschrieben, die die Datenbankinformationen über den Workflow und einige weitere notwendigen Daten enthält.

[Erläuterung der Erzeugung der Exportverzeichnisse](step_1_export.md)

### b) Transfer der Export-Verzeichnisse

Nach der vollständigen Erzeugung und Anreicherung der Export-Verzeichnisse auf dem Ausgangssystem können diese auf den Server des Zielsystems transferiert werden. Dies kann auf verschiedene Arten erfolgen. Aufgrund der Datenmengen hat sich hierfür vorrangig ein Transfer mittels `rsync` bewährt.

[Erläuterung des Transfers der Export-Verzeichnisse](step_2_transfer.md)

### c) Einspielen der Export-Verzeichnisse

Nachdem die Export-Verzeichnisse erfolgreich auf das Zielsystem transferiert wurden, können die Daten dort eingespielt werden. Hierzu müssen die Daten an der richtigen Stelle im System abgelegt werden und auch einige weitere Vorkehrungen hinsichtlich der Infrastruktur vorbereitet sein.

[Erläuterung des Einspielens der Export-Verzeichnisse](step_3_import.md)
