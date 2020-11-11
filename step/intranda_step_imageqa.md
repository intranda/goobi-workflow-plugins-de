---
description: >-
  Mit dem Plugin für die Qualitätskontrolle von Bildern lassen sich Bilder im
  Detail auf Ihre Qualität prüfen
---

# Qualitätskontrolle von Bildern

## Einführung

Diese Plugin dient zur visuellen Prüfung der Qualität von Bildern. Es erlaubt verschiedene Sichten auf Bilder als Thumbnails, in großer Anzeige oder auch in einem Vollbildmodus. Darüber hinaus lassen sich parallel zur Bildanzeige ebenso der ggf. vorhandene Volltext einblenden und verschiedene Funktionen für einen Download oder zur Bildmanipulation aktivieren.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_imageQA |
| Source code | [https://github.com/intranda/goobi-plugin-step-imageqa](https://github.com/intranda/goobi-plugin-step-imageqa) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.03 |
| Dokumentationsdatum | 25.05.2020 |

## Installation

Zur Nutzung des Plugins müssen diese beiden Dateien an folgende Orte kopiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_imageQA.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_imageQA-GUI.jar
```

Die Konfiguration des Plugins findet innerhalb dessen Konfigurationsdatei `plugin_intranda_step_imageQA.xml` statt. Diese wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_imageQA.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

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
        <!-- define plugin type in which screen the plugin is displayed, allowed values are 'part', 'full' (default) or 'both' -->
        <guiType>full</guiType>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <!-- which images to use , possible values are master|main|jpeg|source|... -->
        <foldername>master</foldername>
        <!-- how to display the thumbnails -->
        <numberOfImagesPerPage>12</numberOfImagesPerPage>
        <thumbnailsize>200</thumbnailsize>
        <!-- number of thumbnails in preview area, default value is 8 -->
        <numberOfImagesInPartGUI>8</numberOfImagesInPartGUI>
        <!-- which image sizes to use for the big image -->
        <thumbnailFormat>jpg</thumbnailFormat>
        <mainImageFormat>jpg</mainImageFormat>
        <!-- use the high speed JavaScript fullscreen view -->
        <useJSFullscreen>false</useJSFullscreen>

        <imagesize>800</imagesize>
        <imagesize>1800</imagesize>
        <imagesize>3000</imagesize>
        <tileSize>256</tileSize>
        <scaleFactors>1</scaleFactors>
        <scaleFactors>32</scaleFactors>
        <useTiles>false</useTiles>
        <useTilesFullscreen>true</useTilesFullscreen>
        <noShortcutPrefix>false</noShortcutPrefix>
        <thumbnailsOnly>false</thumbnailsOnly>

        <!-- allow deletion of images -->
        <allowDeletion>false</allowDeletion>
        <deletion command="/opt/digiverso/goobi/scripts/deleteImage.sh|IMAGE_FOLDER|IMAGE_FILE"/>
        <!-- allow rotation of images -->
        <allowRotation>false</allowRotation>
        <rotationCommands>
            <left command="/usr/bin/mogrify|-rotate|-90|IMAGE_FILE" />
            <right command="/usr/bin/mogrify|-rotate|90|IMAGE_FILE" />
        </rotationCommands>
        <!-- allow renaming of images -->
        <allowRenaming>false</allowRenaming>
        <!-- allow selection of images -->
        <allowSelection>false</allowSelection>
        <allowSelectionAll>false</allowSelectionAll>
        <allowDownload>false</allowDownload>
        <allowDownloadAsPdf>false</allowDownloadAsPdf>
        <!-- allow the user to finish the task directly out of the plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
        <!-- configure button to display ocr -->
        <displayocr>true</displayocr>
    </config>

    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>My special project</project>
        <project>Archive_Project</project>
        <step>MasterQA</step>
        <step>DerivateQA</step>
        <!-- which images to use -->
        <foldername>master</foldername>
        <foldername>main</foldername>
        <!-- how to display the thumbnails -->
        <numberOfImagesPerPage>12</numberOfImagesPerPage>
        <thumbnailsize>200</thumbnailsize>
        <!-- which image sizes to use for the big image -->
        <imagesize>800</imagesize>
        <imagesize>3000</imagesize>
        <!-- allow deletion of images -->
        <allowDeletion>false</allowDeletion>
        <deletion command="/opt/digiverso/goobi/scripts/deleteImage.sh|IMAGE_FOLDER|IMAGE_FILE"/>
        <!-- allow rotation of images -->
        <allowRotation>false</allowRotation>
        <rotationCommands>
            <left command="/usr/bin/mogrify|-rotate|-90|IMAGE_FILE" />
            <right command="/usr/bin/mogrify|-rotate|90|IMAGE_FILE" />
        </rotationCommands>
        <!-- allow renaming of images -->
        <allowRenaming>false</allowRenaming>
        <!-- allow selection of images -->
        <allowSelection>false</allowSelection>
        <allowDownload>false</allowDownload>
        <allowDownloadAsPdf>false</allowDownloadAsPdf>
        <!-- allow the user to finish the task directly out of the plugin -->
        <allowTaskFinishButtons>false</allowTaskFinishButtons>
        <displayocr>true</displayocr>
    </config>

    <config>
        <project>*</project>
        <step>ImageQA</step>
        <foldername>master</foldername>
        <!-- how to display the thumbnails -->
        <numberOfImagesPerPage>12</numberOfImagesPerPage>
        <thumbnailsize>200</thumbnailsize>
        <!-- which image sizes to use for the big image -->
        <imagesize>800</imagesize>
        <imagesize>3000</imagesize>

        <!-- allow renaming of images -->
        <allowRenaming>false</allowRenaming>
        <renamingPattern>
            <field defaultValue="${process:Archive}"></field>
            <field defaultValue="${process:Object}"></field>
            <field defaultValue="1" number="true" format="0000"></field>
        </renamingPattern>
        <displayocr>true</displayocr>

    </config>

</config_plugin>
```

Die Parameter innerhalb dieser Konfigurationsdatei haben folgende Bedeutungen:

| Wert | Beschreibung |
| :--- | :--- |
| `guiType` | Mittels dieses Parameters läßt sich festlegen, wie sich die Nutzeroberfläche verhalten soll. Mögliche Werte hierfür sind `part`, `full` und `both`. |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `useOrigFolder` | Legen Sie hier fest, ob die Master-Bilder des Goobi-Vorgangs verwendet werden sollen oder stattdessen die vorliegenden Derivate. |
| `numberOfImagesPerPage` | Definieren Sie hier, wie viele Thumbnails in der regulären Anzeige gleichzeitig dargestellt werden sollen. |
| `thumbnailsize` | Hiermit läßt sich festlegen, in welcher Größe die Thumbnails angezeigt werden sollen. |
| `numberOfImagesInPartGUI` | Mit diesem Parameter läßt sich festlegen, wieviele Thumbnails gleich innerhalb der angenommenen Aufgabe angezeigt werden sollen. |
| `thumbnailFormat` | Definieren Sie hier das Dateiformat für die Darstellung der Thumbnails. |
| `mainImageFormat` | Definieren Sie hier das Dateiformat für die Darstellung des großen Bildes. |
| `imagesize` | Mit diesem wiederholt vorkommbaren Parameter können die einzelnen Zoomstufen der Bilder festgelegt werden. Je mehr Stufen definiert werden, um so öfter wird beim Zoomen des Bildes eine höher aufgelöste Fassung des Bildes erzeugt und geladen. |
| `tileSize` | Mit diesem Parameter läßt sich festlegen, welche Größe die Kacheln für eine gekachelte Darstellung der Bilder haben sollen. |
| `useTilesFullscreen` | Legen Sie hier fest, ob innerhalb der Vollbildanzeige ebenfalls eine Anzeige basierend auf Kacheln erfolgen soll. |
| `allowDeletion` | Mit diesem Parameter kann ein Löschen von einzelnen Bildern erlaubt werden. |
| `deletionCommand` | Hiermit wird das Kommando für das Löschen festgelegt. Dies kann unter anderem auch anstelle einer tatsächlichen Löschung ein Bewegen der Bilder in andere Verzeichnisse ermöglichen. |
| `allowRotation` | Wird diese Funktion aktiviert, so dürfen die angezeigten Bilder in 90-Grad-Schritten rotiert werden. |
| `rotationCommands` | Hiermit läßt sich festlegen, mit welchen Kommandozeilenaufrufen die Rotation der Bilder erfolgen soll. |
| `allowRenaming` | Mittels dieses Parameters läßt sich eine Funktionalität zur gezielten Benennung der Bilddateien aktivieren. |
| `allowSelection` | Mit diesem Parameter kann pro Bild eine Checkbox aktiviert werden, die eine einzelne Auswahl jedes Bildes erlaubt. |
| `allowSelectionAll` | Mit diesem Parameter läßt sich festlegen, ob ein Button zur Auswahl aller Bilder angezeigt werden soll. |
| `allowDownload` | Hiermit läßt sich festlegen, ob ein Download der ausgewählten Seiten innerhalb einer zip-Datei erlaubt sein soll. |
| `allowDownloadAsPdf` | Hiermit läßt sich festlegen, ob ein Download der ausgewählten Seiten als eine große PDF-Datei erlaubt sein soll. |
| `allowTaskFinishButtons` | Mit diesem Parameter kann ermöglicht werden, dass in der regulären Plugin-Oberfläche bereits Buttons zum Abschließen der Aufgabe angezeigt werden sollen, so dass das Plugin nicht zunächst verlassen werden muss. |
| `displayocr` | Hier kann festgelegt werden, ob der Button für die Anzeige von Volltextergebnissen aktiviert werden soll. |
| `useJSFullscreen` | Mit diesem Parameter kann festgelegt werden, dass die Vollbildanzeige allein mittels JavaScript erfolgen soll. Diese Anzeige ist deutlich performanter, erlaubt jedoch keine Anzeige des Volltextes parallel zum Bild. |

## Integration des Plugins in den Workflow

Zur Inbetriebnahme des Plugins muss dieses für einen oder mehrere gewünschte Aufgaben im Workflow aktiviert werden. Dies erfolgt wie im folgenden Screenshot aufgezeigt durch Auswahl des Plugins `intranda_step_imageQA` aus der Liste der installierten Plugins.

![Zuweisung des Plugins zu einer bestimmten Aufgabe](../.gitbook/assets/intranda_step_imageqa1.png)

![Integration des Plugins in eine Aufgabe innerhalb des Workflows](../.gitbook/assets/intranda_step_imageqa2.png)

## Arbeitsweise und Bedienung des Plugins

Nachdem das Plugin vollständig installiert und eingerichtet wurde, steht es für die Bearbeiter der entsprechenden Aufgaben zur Verfügung. Nach dem Betreten einer Aufgabe ist je nach Konfiguration gegebenenfalls sofort eine Anzeige einiger Bilder möglich.

![Anzeige einiger Bilder als Vorschau innerhalb der angenommenen Aufgabe](../.gitbook/assets/intranda_step_imageqa3.png)

Betritt man nun das Plugin durch Klick auf `Plugin: intranda image control` so gelangt man die volle Anzeige, bei der sowohl Thumbnails als auch eine große Bildanzeige erfolgt.

![Anzeige von Thumbnails und einer gro&#xDF;en Bildanzeige gleichzeitig](../.gitbook/assets/intranda_step_imageqa4.png)

Hier lassen sich gezielt die gewünschten Bilder auswählen, die in höherer Qualität betrachtet werden sollen. Das große Bild im rechten Bereich läßt sich zoomen und für die Anzeige rotieren. Eine Navigation zwischen den Bildern ist mit den gleichen Tastenkombinationen möglich, wie sie auch innerhalb des METS-Editors von Goobi workflow möglich ist.

Zur größeren Anzeige lassen sich Bilder auch in einer Vollbildanzeige darstellen. Sowohl in der regulären Bildanzeige als auch in der Vollbildanzeige läßt sich hierbei ebenfalls der zugehörige Volltext einblenden, sofern dieser denn im Vorfeld mittels OCR erzeugt wurde.

![Bildanzeige im Vollbildmodus mit aktivierter Volltextanzeige](../.gitbook/assets/intranda_step_imageqa5.png)

Neben der reinen Bildanzeige kann das Plugin ebenfalls mit anderen Objekttypen umgehen. So ist unter anderem auch eine Anzeige von 3D-Objekten möglich, die mittels zusätzlicher Navigationsbuttons auch zur Anzeige rotiert und vergrößert werden können.

Je nach individueller Konfiguration erlaubt das Plugin noch viele weitere Funktionen, die zumeist innerhalb der Thumbnaildarstellung sichtbar werden. Wurden diese Funktionen in der oben beschriebenen Konfigurationsdatei konfiguriert, so lassen sich diese zum Beispiel für einen Download von PDF-Dateien, Bilddateien, Rotationen, Löschungen und andere Operationen nutzen.

![Aktivierte Zusatzfunktionen innerhalb der Thumbnaildarstellung](../.gitbook/assets/intranda_step_imageqa6.png)
