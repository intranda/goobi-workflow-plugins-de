---
description: >-
  Goobi Administration Plugin für das Kopieren einer Anchor-Datei zu allen
  zugehörigen Bänden
---

# Copy Master-Anchor

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Administration Plugins für die automatisierte Übernahme einer zentralen Anchor-Datei eines Bandes \(z.B. von Zeitschriften oder Mehrbändigen Werken\) zu anderen Bänden innerhalb von Goobi workflow.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_administration\_copymasteranchor |
| Source code | [https://github.com/intranda/goobi-plugin-administration-copyanchor](https://github.com/intranda/goobi-plugin-administration-copyanchor) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.05 |
| Dokumentationsdatum | 05.06.2020 |

## Installation

Das Plugin besteht insgesamt aus den folgenden zu installierenden Dateien

```text
plugin_intranda_administration_copyanchor.jar
plugin_intranda_administration_copyanchor-GUI.jar
```

Diese Dateien müssen in den richtigen Verzeichnissen installiert werden, so dass diese nach der Installation in folgenden Pfaden vorliegen:

```bash
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_copyanchor.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_copyanchor-GUI.jar
```

Es existiert derzeit keine Konfigurationsdatei für dieses Plugin.

## Konfiguration

Das Plugin verfügt nicht über eine eigene Konfigurationsdatei. Dennoch ist eine Anpassung des verwendeten Regelsatzes zwingende Voraussetzung für den Betrieb des Plugins. Beispielhaft soll dies an einem Regelsatz aufgezeigt werden, der sich beispielsweise unter folgendem Pfad findet:

```bash
/opt/digiverso/goobi/rulesets/ruleset.xml
```

Innerhalb des Regelsatzes muss das Metadatum `InternalNote` definiert werden:

```markup
<MetadataType>
  <Name>InternalNote</Name>
  <language name="de">Interne Goobi-Anmerkung</language>
  <language name="en">Internal Note for Goobi</language>
</MetadataType>
```

Dieses Metadatum muss nun innerhalb der Definition der Bände erlaubt werden. Anhand eines Zeitschriftenbandes erfolgt dies beispielhaft so:

```markup
<DocStrctType topStruct="true">
  <Name>PeriodicalVolume</Name>
  <language name="de">Zeitschriftenband</language>
  <language name="en">Periodical volume</language>
  <!-- Definitions of other metadata and structure elemtents skipped here -->
  <metadata num="*">InternalNote</metadata>
</DocStrctType>
```

Mittels dieser Anpassung am Regelsatz sind die Vorbereitungen für die Verwendung des Plugins bereits abgeschlossen.

## Nutzung des Plugins innerhalb von Goobi

### Definition eines Master-Anchors

Nach der vollständigen Konfiguration des Plugins kann dieses verwendet werden. Dazu wird zunächst innerhalb desjenigen Bandes, der als Master-Anchor markiert werden soll, das neu definierte Metadatum `InternalNote` hinzugefügt und als Wert `AnchorMaster` eingetragen. Im folgenden Screenshot wird dies einmal verdeutlicht:

![Dem Zeitschriftenband wurde Metadatum InternalNote mit dem Wert AnchorMaster zugewiesen](../.gitbook/assets/intranda_administration_copy_anchor_01.png)

Der somit angepasste Zeitschriftenband wurde mit dieser Änderung als Master definiert. Von nun an dienen die dort verwendeten Metadaten des übergeordneten Werkes \(z.B. der Zeitschrift\) als Vorgabe für alle anderen zugehörigen Bände. Änderungen, die für alle Bände innerhalb der Anchor-Dateien vorgenommen werden sollen, erfolgen daher von nun an innerhalb dieses Datensatzes.

![Anpassungen an diesem Anchor k&#xF6;nnen mit dem Plugin von nun an f&#xFC;r alle zugeh&#xF6;rigen B&#xE4;nde &#xFC;bernommen werden](../.gitbook/assets/intranda_administration_copy_anchor_02.png)

### Übernahme der Metadaten für alle zugehörigen Bände

Sowie innerhalb eines Goobi-Vorgangs ein Band als Master festgelegt wurde, kann das Plugin dazu genutzt werden, alle Metadaten des Masters auf alle zugehörigen Bände zu übertragen. Gehen Sie dazu folgendermaßen vor:

Öffnen Sie zunächst das Plugin mittels des Menüs `Administration` und darin des Menüpunktes `Kopieren von Master-Anchor Daten`.

![&#xD6;ffnen des Plugins &#xFC;ber das Men&#xFC; Administration](../.gitbook/assets/intranda_administration_copy_anchor_03.png)

Geben Sie in dem Inputfeld des Plugins den Katalog-Identifier des übergeordneten Werkes ein \(z.B. die ID der Zeitschrift\) und klicken sie anschließend auf den Butten `Kopiervorgang starten`. Hiermit wird der Kopiervorgang aufgerufen, der die Metadaten des Master-Anchor-Datensatzes automatisch in alle zugehörigen Bände \(z.B. alle Bände der Zeitschrift\) übernimmt.

![Ausf&#xFC;hren des Kopiervorgangs](../.gitbook/assets/intranda_administration_copy_anchor_04.png)

