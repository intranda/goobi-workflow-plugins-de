---
description: >-
  Dieses Step Plugin skaliert Bilder auf maximale Ausmaße und rendert
  Wasserzeichen in die skalierten Bilder.
---

# Bildskalierung und Wasserzeichen rendern

## Einführung

Dieses Plugin erlaubt es, Bilder auf eine maximale Größe zu skalieren und Wasserzeichen in die zuvor skalierten Bilder zu rendern. Dabei ist die maximale Größe sowie das zu rendernde Wasserzeichen flexibel konfigurierbar. 

## Übersicht

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_step\_sudan-export-preparation |
| Source code | -noch kein öffentlicher source code verfügbar - |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.06 |
| Dokumentationsdatum | 31.08.2020 |

## Installation

Zur Installation des Plugins muss folgende Datei installiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_sudan-export-preparation.jar
```

Für die korrekte Ausführung des Plugins wird außerdem eine Konfigurationsdatei benötigt:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_sudan-export-preparation.xml
```

## Konfiguration des Plugins

Die Konfiguration erlaubt es festzulegen, auf welche Maximalgröße skaliert wird, sowie welches Wasserzeichen \(Bilder und auch Text-Wassserzeichen werden unterstützt\) gerendert wird. Auch die Positionierung des Wasserzeichens kann festgelegt werden. Dabei sind mehrere Konfigurationen möglich, die anhand des Projektes, des Schritt-Namens, der digitalen Kollektion sowie des Medien-Typs \(Metadatum im METS\) unterschieden werden. Bei der Ausführung des Plugins wird die erste Konfiguration verwendet, die zum aktuell bearbeiteten Vorgang passt. 

Eine Beispielkonfiguration sieht folgendermaßen aus:

```markup
<config_plugin>
    <gmPath>/usr/bin/gm</gmPath>
    <convertPath>/usr/bin/convert</convertPath>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
-->
    <config>
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- the source directory from which the images will be taken -->
        <sourceDir>cropped</sourceDir>
        <!-- the destination where the scaled and watermarked images will reside -->
        <destDir>media</destDir>
        
        <!-- only use this block with processes in collection "mycollection"
             and with mediaType "book" -->
        <imageConfig collection="mycollection" mediaType="book">
            <!-- The maximum size of the longest side of an image -->
            <resizeTo>1500</resizeTo>
            <!-- The watermark configuration -->
            <watermark>
                <!-- the image to use for watermarking -->
                <image>/opt/digiverso/goobi/scripts/watermark1.png</image>
                <!-- the location of the watermark. Possible values: north, 
                     northeast, east, southeast, south, southwest, 
                     west, northwest -->
                <location>southeast</location>
                <!-- these are the distances to the edges on the x- resp. y-axis -->
                <xDistance>100</xDistance>
                <yDistance>100</yDistance>
            </watermark>
        </imageConfig>
       
        <!-- use this block with processes in collection "myothercollection"
             and any mediaType --> 
        <imageConfig collection="myothercollection" mediaType="*">
            <resizeTo>1500</resizeTo>
            <watermark>
                <!-- you can also use a text-only watermark. -->
                <text>My watermark text</text>
                <location>southeast</location>
                <xDistance>600</xDistance>
                <yDistance>100</yDistance>
            </watermark>
        </imageConfig>
        
    </config>
</config_plugin>
```

## Integration des Plugins in den Workflow

Zur Inbetriebnahme des Plugins muss dieses für einen oder mehrere gewünschte Aufgaben im Workflow aktiviert werden. Dies erfolgt durch Auswahl des Plugins `intranda_step_sudan-export-preparation` aus der Liste der installierten Plugins.

