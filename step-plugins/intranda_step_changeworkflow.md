---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin für das ändern des
  Workflows auf Grundlage von Vorgangseigenschaften
---

# Ändern des Workflows auf Grundlage von Vorgangseigenschaften

## Einführung

Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz eines Plugins zum automatischen Ändern von Workflows zur Laufzeit. Das Plugin kann \(je nach Konfiguration\) Schritte öffnen, schließen oder deaktivieren. Die Entscheidung, was geschehen soll, wird auf Grundlage von Vorgangseigenschaften getroffen.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_step\_changeWorkflow |
| Source code | - Quellcode noch nicht öffentlich verfügbar - |
| Kompatibilität | Goobi Workflow 3.0.0 |
| Dokumentationsdatum | 29.04.2019 |

## Voraussetzung

Voraussetzung für die Verwendung des Plugins ist der Einsatz von Goobi workflow in Version 3.0.0 oder höher, die korrekte Installation und Konfiguration des Plugins sowie die korrekte Einbindung des Plugins in die gewünschten Arbeitsschritte des Workflows.

## Installation und Konfiguration

Zur Nutzung des Plugins muss es an folgenden Ort kopiert werden:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_changeWorkflow.jar
```

Die Konfiguration des Plugins wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_changeWorkflow.xml
```

Es folgt eine kommentierte Beispielkonfiguration:

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
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>Register</project>
        <step>Check</step>
        <!-- name of the property to check -->
        <propertyName>TemplateID</propertyName>
        <!-- expected value -->
        <propertyValue>183</propertyValue>
        <!-- list of steps to open, if property value matches -->
        <steps type="open">
            <title>Box preparation</title>
        </steps>
        <!-- list of steps to deactivate -->
        <steps type="deactivate">
            <title>Image QA</title>
        </steps>
        <!-- list of steps to close -->
        <steps type="close">
            <title>Automatic LayoutWizzard Cropping</title>
            <title>LayoutWizzard: Manual confirmation</title>
        </steps>
        <!-- list of steps to lock -->
        <steps type="lock">
            <title>Automatic export to Islandora</title>
        </steps>
    </config>
   
    <config>
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <!-- name of the property to check -->
        <propertyName>upload to digitool</propertyName>
        <!-- expected value -->
        <propertyValue>No</propertyValue>
        <!-- list of steps to open, if property value matches -->
        <steps type="open">
            <title>Create derivates</title>
            <title>Jpeg 2000 generation and validation</title>
        </steps>
        <!-- list of steps to deactivate -->
        <steps type="deactivate">
            <title>Rename files</title>
        </steps>
        <!-- list of steps to close -->
        <steps type="close">
            <title>Upload raw tiffs to uploaddirectory Socrates</title>
            <title>Automatic pagination</title>
        </steps>
        <!-- list of steps to lock -->
        <steps type="lock">
            <title>Create METS file</title>
            <title>Ingest into DigiTool</title>
        </steps>
    </config>
    
</config_plugin>
```

Jeder `<config>`-Block ist hier für ein bestimmtes Projekt und einen bestimmten Schritt verantwortlich, wobei auch die Wildcard `*` und Mehrfachnennungen von Prozessen bzw. Schritten möglich sind. Wenn im Workflow also ein Schritt mit diesem Plugin ausgeführt wird, wird nach einem `<config>`-Block gesucht, der zum gerade geöffneten Schritt passt. Wenn zum Beispiel im Projekt "PDF Digitalisierung" der Schritt mit Titel "Workflow ändern nach PDF Extraktion" mit diesem Plugin konfiguriert und ausgeführt wird, sucht das Plugin einen `<config>`-Block der folgendermaßen aussieht:

```markup
<config>
    <project>PDF Digitalisierung</project>
    <step>Workflow ändern nach PDF Extraktion</step>
    [...]
</config>
```

In jedem `<config>`-Element wird dann konfiguriert, welche Prozesseigenschaft überprüft wird \(`<propertyName>`\) und welcher Wert erwartet wird \(`<propertyValue>`\).  Alle folgenden `<step>`-Elemente beschreiben dann eine Aktion, die ausgeführt wird, wenn die vorher konfigurierte Eigenschaft dem erwarteten Wert entspricht. Schritte können geöffnet `type="open"`, deaktiviert `type="deactivate"`, geschlossen `type="close"` und gesperrt `type="lock"` werden.

## Einstellungen in Goobi

Nachdem das Plugin installiert und konfiguriert wurde, kann es in der Nutzeroberfläche in einem Workflowschritt konfiguriert werden. Hierbei sollte darauf geachtet werden, dass der Schritt so heißt, wie in der Konfigurationsdatei. Außerdem sollte ein Haken bei `Automatische Aufgabe` gesetzt sein.

![Konfiguration des Workflowschritts](../.gitbook/assets/changeworkflow_step.png)

## Nutzung

Da das Plugin vollautomatisch laufen sollte, ist für die Nutzung nichts weiter zu beachten.

