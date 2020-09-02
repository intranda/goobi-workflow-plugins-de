---
description: >-
  Dieses Workflow Plugin erlaubt die Abarbeitung von mehreren LayoutWizzard
  Vorgängen in einem Arbeitsschritt.
---

# LayoutWizzard workflow plugin

## Einführung

Dieses Plugin erlaubt es, mehrere LayoutWizzard-Vorgänge an einer Stelle zu bearbeiten. Dazu werden alle Vorgänge mit dem richtigen offenen Schritt gesucht und eine LayoutWizzard Korrektur für alle diese Vorgänge angeboten. 

## Übersicht

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_workflow\_barcode\_generator |
| Source code | Source code noch nicht öffentlich verfügbar |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 20.08 |
| Dokumentationsdatum | 02.09.2020 |

## Installation

Zur Installation des Plugins müssen folgende beiden Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_crop.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_crop-GUI.jar
```

Um zu konfigurieren, wie sich das Plugin verhalten soll, können verschiedene Werte in der Konfigurationsdatei angepasst werden. Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_crop.xml
```

Der Inhalt dieser Konfigurationsdatei sieht wie folgt aus:

```markup
<config>
    <!-- this step must be open in a process for the process to appear in the plugin-->
    <allowed-step>
        LayoutWizzard manuelle Kontrolle
    </allowed-step>
    <singleImage>
        <cropFrame>
            <linewidth>2</linewidth>
            <linecolor>#00fa9a</linecolor>
            <fillcolor>#ffffff</fillcolor>
            <clickradius>20</clickradius>
            <fillcolor>#ffffff</fillcolor>
        </cropFrame>
        <spineMarker>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <fillcolor>#ffffff</fillcolor>
            <clickradius>20</clickradius>
        </spineMarker>
    </singleImage>
    <!-- Config for appearance of images in preview mode -->
    <preview>
        <cropFrame>
            <linewidth>2</linewidth>
            <linecolor>#368EE0</linecolor>
            <fillcolor>#f1f2f3</fillcolor>
            <clickradius>20</clickradius>
            <fillcolor>#f1f2f3</fillcolor>
        </cropFrame>
        <spineMarker>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <fillcolor>#f1f2f3</fillcolor>
            <clickradius>10</clickradius>
        </spineMarker>
    </preview>
    
    <previewCroppingOptions>
        <show>true</show>
    </previewCroppingOptions>
</config>
```

## Bedienung des Plugins

Wenn das Plugin korrekt installiert und konfiguriert wurde, ist es innerhalb des Menüpunkts Workflow zu finden. Nach dem Betreten des Plugins wird dann eine LayoutWizzard-Preview Ansicht geöffnet, ganz ähnlich zu [der im step-plugin](https://docs.goobi.io/goobi-workflow-plugins-de/step/layoutwizzard/01_use/01_preview). 

![Preview-Ansicht im LayoutWizzard-Workflow-Plugin](../.gitbook/assets/lw_workflow_01.png)

Die Bedienung ist zu der im Preview-Modus auch identisch, einziger Unterschied ist, dass die einzelnen Vorgänge visuell voneinander getrennt sind und durch einen einfachen Klick abgeschlossen werden. Die Ansicht springt danach zum nächsten Vorgang. 

![Vorgang abschlie&#xDF;en](../.gitbook/assets/lw_workflow_02.png)

