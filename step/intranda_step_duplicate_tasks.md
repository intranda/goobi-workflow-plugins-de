---
description: >-
  Dieses Step-Plugin ermöglicht es Ihnen, eine Aufgabe automatisch entsprechend dem Wert einer Prozesseigenschaft zu duplizieren.
---

# Duplikation des Schrittes

## Einführung

Dieses Plugin liest den Wert einer Prozesseigenschaft ein und entsprechend davon kann es eine Aufgabe automatisch duplizieren und neue Eigenschaften speichern, die auf diese Duplizierung verweisen.

## Übersicht

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_duplicate\_tasks |
| Source code | [https://github.com/intranda/goobi-plugin-step-duplicate-tasks](https://github.com/intranda/goobi-plugin-step-duplicate-tasks) |
| Lizenz | GPL 2.0 or newer |
| Kompatibilität | Goobi workflow 23.10 |
| Dokumentationsdatum | 16.11.2023 |

## Installation

Zur Installation des Plugins muss die folgende Datei installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_duplicate_tasks.jar
```

Die Konfigurationsdatei befindet sich üblicherweise hier:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_duplicate_tasks.xml
```

## Konfiguration

Der Inhalt dieser Konfigurationsdatei sieht beispielhaft wie folgt aus:

```xml
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
        
         <!-- Process property whose value shall be separated into parts, and it accepts four attributes:
              - @name: name of the process property that shall be splitted
              - @separator: separator that shall be used to split the value of the process property into smaller parts. OPTIONAL. DEFAULT "\n".
              - @target: configure with this attribute where and how to save the splitted parts. OPTIONAL.
                              - IF NOT configured, then all splitted parts will be saved as process properties, and the default property names depend on the configuration of @enabled of the tag <stepToDuplicate>:
                                If @enabled is true, then the default property name will be the step's name that is to be duplicated.
                                If @enabled is false, then the default property name will be the property's @name.
                              - IF configured without using a colon, then all splitted parts will be saved as process properties, and the configured @target will be the new properties' names.
                              - IF configured with a colon, then the part before that colon will control where the changes land, while the part after that colon will define the names of the splitted new parts:
                                Before the colon there are three options: property | metadata | person. For "metadata" and "person", changes will be saved into the METS file. For "property" changes will be saved as properties.
              - @useIndex: determines whether to use an index as suffix to each new process property / metadata entry to distinguish them between each other. OPTIONAL. DEFAULT true.
         -->
        <!-- ATTENTION: there can only be one such tag configured for each step, to split several properties, one has to do that in several steps. -->
        <property name="AssetUri" separator="," target="property:AssetUriSplitted" useIndex="true" />
        
        <!-- Name of the step that shall be duplicated. OPTIONAL. If not configured, then the next step following the current one will be used as default. It accepts an attribute:
              - @enabled: true if some step's duplication is needed, false otherwise. OPTIONAL. DEFAULT true.
         -->
        <stepToDuplicate enabled="true">Metadata enrichment</stepToDuplicate>
    </config>

</config_plugin>
```

Der Block `<config>` kann für verschiedene Projekte oder Arbeitsschritte wiederholt vorkommen, um innerhalb verschiedener Workflows unterschiedliche Aktionen durchführen zu können.

| Wert | Beschreibung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `property` | Dieser Wert legt fest, welche Prozesseigenschaft zur Kontrolle der Duplizierung verwendet werden soll. Er akzeptiert vier Attribute, wobei nur `@name` obligatorisch ist. Sehe die Konfiguration oben für mehr Details. |
| `stepToDuplicate` | Dieser OPTIONALE Parameter kann verwendet werden, um den Namen der Aufgabe zu konfigurieren, die dupliziert werden soll. Wenn nicht konfiguriert, wird der nächste Schritt nach dem aktuellen Schritt standardmäßig verwendet. Er akzeptiert auch ein OPTIONALES Attribut `@enabled` mit einem DEFAULTEN Wert `true`, das steuert ob es einen Arbeitsschritt zu duplizieren gibt. |

## Arbeitslogik dieses Plugins

### Mit Duplikation eines Schritts
1. Das Plugin holt sich den Wert der konfigurierten Prozesseigenschaft und teilt ihn unter Verwendung des eventuell konfigurierten Trennzeichens *(oder `\n`, falls nicht)* in Teile auf.
2. Für jeden Teil der ursprünglichen Eigenschaft wird der möglicherweise konfigurierte Schritt *(oder der nächste Schritt des aktuellen Schritts, wenn er nicht konfiguriert ist)* noch einmal dupliziert, und die Namen dieser duplizierten neuen Schritte sind der Name des ursprünglichen Schritts plus jeweilige Bestellnummer unter diesen Duplikaten.
3. Für jeden duplizierten neuen Schritt wird eine neue Prozesseigenschaft oder ein Metadatum erstellt, je nachdem wie das Attribut `@target` konfiguriert ist. Der Wert dieser neuen Prozesseigenschaft bzw. dieses neuen Metadatums ist genau der Teil der ursprünglichen Eigenschaft, auf dessen Grundlage dieser Schritt dupliziert wurde.
4. Wenn Duplikate für jeden Teil der ursprünglichen Eigenschaft durchgeführt werden, wird der ursprüngliche Schritt zum Duplizieren deaktiviert.

### Ohne Duplikation eines Schritts
1. Das Plugin holt sich den Wert der konfigurierten Prozesseigenschaft und teilt ihn unter Verwendung des eventuell konfigurierten Trennzeichens *(oder `\n`, falls nicht)* in Teile auf.
2. Für jeden Teil der ursprünglichen Eigenschaft wird eine neue Prozesseigenschaft oder ein Metadatum erstellt, je nachdem wie das Attribut `@target` konfiguriert ist. Der Wert dieser neuen Prozesseigenschaft bzw. dieses neuen Metadatums ist genau dieser Teil der ursprünglichen Eigenschaft.
