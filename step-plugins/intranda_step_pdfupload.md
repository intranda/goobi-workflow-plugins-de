---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin zur Integration von
  elektronischen Publikationen.
---

# Elektronische Publikationen

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zum Upload von Dokumenten in Goobi. Mit Hilfe dieses Plugins können born-digital-Dokumente wie beispielsweise PDF-Dateien direkt in Goobi hochgeladen und verwaltet werden.

| Details |  |
| :--- | :--- |
| Version des Plugins | 1.0.0 |
| Identifier | intranda\_step\_pdfUpload |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi workflow 2.1 |
| Dokumentation vom | 04.02.2016 |

## Voraussetzung

Voraussetzung für die Verwendung des Plugins ist der Einsatz der Goobi Version 2.1, die Nutzung der neuen Oberfläche von Goobi, die anschließende korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte der Workflows.

## Installation und Konfiguration

Zur Nutzung des Plugins müssen folgende drei Dateien installiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_pdfUpload.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_pdfUpload-GUI.jar
/opt/digiverso/goobi/config/plugin_PdfUploadPlugin.xml
```

Die erste Datei enthält die eigentliche Programmlogik, die zweite Datei stellt die grafische Oberfläche für die Anzeige zur Verfügung und die dritte Datei dient zur Konfiguration des Plugins.

Der Inhalt der Konfigurationsdatei `plugin_PdfUploadPlugin.xml` muss folgendermaßen aufgebaut sein: 

{% code-tabs %}
{% code-tabs-item title="plugin\_PdfUploadPlugin.xml" %}
```markup
<config_plugin>
 <!-- allowed values: master, source, derivate -->
    <folder>derivate</folder>
    <extensions>
        <extension>pdf</extension>
        <extension>PDF</extension>
    </extensions>
</config_plugin>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In `<folder>` wird festgelegt, in welchem Ordner die hochgeladenen Dateien gespeichert werden. Dabei können die Werte  `master`, `source` und `derivate` verwendet werden.

Anschließend müssen noch die erlaubten Dateiendungen konfiguriert werden. Hierbei muss darauf geachtet werden, dass die Überprüfung case sensitive ist. Will man Groß- und Kleinschreibung erlauben, müssen beide Schreibweisen definiert werden.

## Einstellungen in Goobi

Nachdem das Plugin installiert und konfiguriert wurde, kann es innerhalb der neuen Benutzeroberfläche von Goobi genutzt werden.

Dazu muss innerhalb der gewünschten Aufgabe das Plugin für den Arbeitsschritt `intranda_step_pdfUpload` eingetragen werden.

![Abbildung 1: Task-Details](../.gitbook/assets/intranda_step_pdfupload_01.png)

## Nutzung

Wurde der entsprechende Arbeitsschritt durch den jeweiligen Nutzer geöffnet, innerhalb dem das Plugin konfiguriert wurde, steht ein neuer Bereich zum Hochladen von Dateien zur Verfügung.

![Abbildung 2: Upload-Plugin](../.gitbook/assets/intranda_step_pdfupload_02.png)

Mittels des Buttons `Datei auswählen` kann nun eine Datei gewählt werden, die anschließend mit dem Button `Datei hochladen` zum Server übertragen wird. Dabei kann optional ein Kommentar angegeben werden.

Die hochgeladene Datei wird anschließend mit einigen Informationen aufgelistet. Dort gibt es auch die Möglichkeit, die Datei wieder zu löschen.

![Abbildung 3: Hochgeladene Dateien](../.gitbook/assets/intranda_step_pdfupload_03.png)

Es werden nur Dateien akzeptiert, deren Dateiendung zuvor in der Liste der erlaubten Endungen eingetragen wurde. Außerdem darf der Dateiname nur aus den Zeichen `A-Za-z0-9äöüß-_` bestehen. `Leerzeichen` werden automatisch durch `Unterstriche` ersetzt.

Wird eine Datei hochgeladen, werden die Informationen dazu in der METS Datei gespeichert.

```markup
<mets:fileGrp USE="LOCAL">
    <mets:file ID="FILE_0001" MIMETYPE="">
        <mets:FLocat LOCTYPE="URL"
 xlink:href="/opt/digiverso/goobi/metadata/6212/images/myFolder/AC03719978.pdf"
 xmlns:xlink="http://www.w3.org/1999/xlink" />
    </mets:file>
</mets:fileGrp>

[...]

<mets:structMap TYPE="PHYSICAL">
    <mets:div DMDID="DMDPHYS_0000" ID="PHYS_0000" TYPE="BoundBook">
        <mets:div ID="PHYS_0001" ORDER="1" TYPE="page" ORDERLABEL="Kommentar">
            <mets:fptr FILEID="FILE_0001" />
        </mets:div>
    </mets:div>
</mets:structMap>
```

Durch das Übernehmen in die METS-Datei ist sichergestellt, dass die Reihenfolge der hochgeladenen Dateien persistent gespeichert wird.

Wird die Aufgabe anschließend erneut geöffnet, werden die Informationen zu den hochgeladenen Dateien aus der METS-Datei entnommen und ebenfalls angezeigt.



