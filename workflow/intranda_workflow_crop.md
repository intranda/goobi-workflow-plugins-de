---
description: >-
  Dieses Workflow Plugin erlaubt die Abarbeitung von Bildern aus mehreren Goobi
  Vorgängen innerhalb der LayoutWizzard Oberfläche.
---

# LayoutWizzard workflow plugin

## Einführung

Dieses Workflow-Plugin erlaubt es, mehrere Bilder, die aus verschiedenen Goobi-Vorgängen stammen in einer gemeinsamen LayoutWizzard-Oberfläche zu bearbeiten. Dazu werden alle Vorgänge ermittelt, deren Workflow sich aktuell auf dem konfigurieren offenen Arbeitsschritt befinden, um für diese eine LayoutWizzard-Korrektur anzubieten.

## Übersicht

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_workflow\_crop |
| Source code | Source code nicht öffentlich verfügbar |
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
        LayoutWizzard
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

Wenn das Plugin korrekt installiert und konfiguriert wurde, ist es innerhalb des Menüpunkts `Workflow` zu finden. Nach dem Betreten des Plugins wird dann eine LayoutWizzard-Preview-Ansicht geöffnet, die sehr ähnlich zu derjenigen des [Step-Plugins für den Layoutwizzard](https://docs.goobi.io/goobi-workflow-plugins-de/step/layoutwizzard/01_use/01_preview) gestaltet ist.

![Preview-Ansicht im LayoutWizzard-Workflow-Plugin](../.gitbook/assets/intranda_workflow_crop_01.png)

Die Bedienung dieses Plugins ist mit derjenigen des regulären LayoutWizzards innerhalb des Preview-Modus weitestgehend identisch. Der einzig nennenswerte Unterschied betriff hierbei lediglich die Aufführung der einzelnen Vorgänge, die jeweils visuell voneinander abgetrennt sind und durch einen einfachen Klick auf den zugehörigen Butten abgeschlossen werden können. Die Anzeige der Bilder aktualisiert sich daraufhin und zeigt anschließend den jeweils nächsten Vorgang an.

![Abschlie&#xDF;en aller Bilder eines Vorgangs](../.gitbook/assets/intranda_workflow_crop_02.png)

