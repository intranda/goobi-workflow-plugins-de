---
description: >-
  Mit dem Plugin für die Auswahl von Bildern lassen sich Bilder aus einer Menge von Bildern auswählen.
---

# Auswahl von Bildern

## Einführung

Diese Plugin dient zur visuellen Auswahl von Bildern. Es erlaubt Auswahl, Abwahl und Sortierung der Ausgewählten per Drag & Drop.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_image\_selection |
| Source code | [https://github.com/intranda/goobi-plugin-step-image-selection](https://github.com/intranda/goobi-plugin-step-image-selection) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2023.03 |
| Dokumentationsdatum | 21.03.2023 |

## Installation

Zur Nutzung des Plugins müssen diese beiden Dateien an folgende Orte kopiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_image_selection.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_image_selection-GUI.jar
```

Die Konfiguration des Plugins findet innerhalb dessen Konfigurationsdatei `plugin_intranda_step_image_selection.xml` statt. Diese wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_image_selection.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

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
        
        <!-- define how many images shall be loaded in the beginning, DEFAULT 20 -->
        <defaultNumberToLoad>20</defaultNumberToLoad>
        
        <!-- define how many more images shall be loaded when the window is scrolled to the bottom, DEFAULT 10 -->
        <defaultNumberToAdd>10</defaultNumberToAdd>
        
        <!-- define which folder shall be used to display the thumbnails, possible values are master | main | jpeg | source | ... -->
        <folder>main</folder>
        
        <!-- define how many images shall be selected as maximum, DEFAULT 5 -->
        <!-- if smaller than min, then no selection is permitted -->
        <max>5</max>
        
        <!-- define how many images shall be selected as minimum, DEFAULT 1 -->
        <!-- if greater than max, then no selection is permitted -->
        <min>0</min>
        
        <!-- display button to finish the task directly from within the entered plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

Die Parameter innerhalb dieser Konfigurationsdatei haben folgende Bedeutungen:

| Wert | Beschreibung |
| :--- | :--- |
| `defaultNumberToLoad` | Mit diesem Parameter können Sie festlegen, wie viele Bilder zu Beginn geladen werden sollen. Default 20. |
| `defaultNumberToAdd` | Mit diesem Parameter können Sie festlegen, wie viele weitere Bilder beim Scrollen nach unten geladen werden sollen. Default 10. |
| `folder` | Geben Sie hier den konfigurierten Namen des Ordners an, aus dem die Bilder angezeigt werden sollen. Mögliche Werte sind `master`, `main`, `jpeg`, `source`... solange sie gut konfiguriert sind. |
| `max` | Hier können Sie die maximale Anzahl von Miniaturbildern festlegen, die ausgewählt werden können. |
| `min` | Hier können Sie die Mindestanzahl von Miniaturbildern festlegen, die ausgewählt werden sollen, um als Prozesseigenschaft gespeichert zu werden. |
| `allowTaskFinishButtons` | Mit diesem Parameter können Sie festlegen, ob diese Schaltfläche aktiviert werden soll oder nicht. |

## Integration des Plugins in den Workflow

Zur Inbetriebnahme des Plugins muss dieses für einen oder mehrere gewünschte Aufgaben im Workflow aktiviert werden. Dies erfolgt wie im folgenden Screenshot aufgezeigt durch Auswahl des Plugins `intranda_step_image_selection` aus der Liste der installierten Plugins.

![Zuweisung des Plugins zu einer bestimmten Aufgabe](../.gitbook/assets/intranda_step_image_selection1.png)

![Integration des Plugins in eine Aufgabe innerhalb des Workflows](../.gitbook/assets/intranda_step_image_selection2.png)

## Arbeitsweise und Bedienung des Plugins

1. Das Plugin zeigt einige Bilder aus dem konfigurierten Ordner im linken Feld an, und wenn das Fenster nach unten gescrollt wird, werden weitere Bilder angezeigt, solange noch welche vorhanden sind.
2. Wenn sich der Mauszeiger über einem Bild befindet, wird eine vergrößerte Ansicht des Bildes angezeigt, mit der Sie die Details des Bildes überprüfen können.
3. Die ausgewählten Bilder werden in der rechten Box angezeigt, wobei das oberste Bild größer als die anderen ist.
4. Neue Bilder können per Drag & Drop ausgewählt werden. Wenn die relative Position zum Ablegen korrekt erfasst wird, wird das neu ausgewählte Bild dort eingefügt, andernfalls wird es am Ende angehängt.
5. Wenn das konfigurierte `max` bereits erreicht ist, oder wenn das auszuwählende Bild bereits einmal ausgewählt wurde, dann funktioniert die Auswahl nicht.
6. Ausgewählte Bilder können per Drag & Drop abgewählt werden. Ziehen Sie einfach das Bild aus der rechten Box und legen Sie es in der linken Box ab.
7. Ausgewählte Bilder können per Drag & Drop neu sortiert werden, mit einer Einschränkung bezüglich des letzten Bildes: die letzte Position wird noch nicht für das Ablegen unterstützt, wenn man also ein anderes Bild die letzte Position einnehmen lassen will, muss man einige Vertauschungen vornehmen.
8. Vergessen Sie nicht, auf Schaltfläche "Save Property" zu klicken, um die Informationen des ausgewählten Bildes in der Prozesseigenschaft zu speichern. Andernfalls werden die Änderungen verworfen und können beim nächsten Start des Plugins nicht wiederhergestellt werden.
9. Man kann das Plugin auch verlassen, ohne die Änderungen zu speichern, indem man auf die Schaltfläche neben "Save Property" klickt.

