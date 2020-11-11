---
description: >-
  Dieses Schritte-Plugin erlaubt es, Transkriptionen von Werken zu erstellen.
  Die Transkriptionen werden ohne Wortkoordinaten erfasst.
---

# Transkription von Bildinhalten

## Einführung

Das Transkriptions-Plugin erlaubt es dem Benutzer, die txt-OCR Ergebnisse eines Goobi-Vorgangs zu bearbeiten. Dabei wird immer ein Bild und ein RichText-Editor angezeigt, in dem der Text erfasst werden kann.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_transcription |
| Source code | nicht verfügbar |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 20.09 |
| Dokumentationsdatum | 11.11.2020 |

## Installation

Das Plugin besteht aus zwei Dateien, die an den folgenden Pfaden erwartet werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_transcription.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_transcription-GUI.jar
```

## Konfiguration

Die Konfiguration wird in folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_transcription.xml
```

Die Konfiguration sieht so aus:

```markup
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
-->
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>


</config_plugin>
```

## Benutzung

Zur Benutzung des Plugins muss dieses in einen Workflow integriert werden und steht dann für die Nutzer zur Bearbeitung zur Verfügung.

Nimmt ein Nutzer eine Aufgabe mit diesem Plugin entgegen, so kann er dessen Nutzeroberfläche betreten. Dort kann anschließend zwischen den Bilddateien geblättert werden. Zur jeweilig aktuellen Seite wird dabei ein Richtext-Editor angezeigt, der die eventuell bereits vorhandene Transkription anzeigt oder falls vorhanden die OCR-Ergebnisse. In diesem Editor kann nun das bisherige Ergebnis korrigiert oder auch ein ganz neuer Text transkribiert werden.

{% hint style="info" %}
Bitte beachten Sie, dass dieses Plugin lediglich eine einfache Transkription von Seiteninhalten erlaubt. Hierbei ist keine Erfassung von Koordinaten für Absätze, Zeilen oder Wörter möglich.
{% endhint %}

