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

        <!-- Process property whose value shall be separated into parts, and it accepts two attributes:
              - @name: name of the process property
              - @separator: separator that shall be used to divide the value of the process property into smaller parts. OPTIONAL. DEFAULT "\n".
         -->
        <property name="short property" separator="," />

        <!-- Name of the step that shall be duplicated. OPTIONAL. If not configured, then the next step following the current one will be used as default. -->
        <stepToDuplicate>Metadata enrichment</stepToDuplicate>
    </config>

</config_plugin>
```

Der Block `<config>` kann für verschiedene Projekte oder Arbeitsschritte wiederholt vorkommen, um innerhalb verschiedener Workflows unterschiedliche Aktionen durchführen zu können.

| Wert | Beschreibung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `property` | Dieser Wert legt fest, welche Prozesseigenschaft zur Kontrolle der Duplizierung verwendet werden soll. Er akzeptiert zwei Attribute, wobei `@name` obligatorisch und `@separator` optional mit dem Standardwert `\n` ist. |
| `stepToDuplicate` | Dieser OPTIONALE Parameter kann verwendet werden, um den Namen der Aufgabe zu konfigurieren, die dupliziert werden soll. Wenn nicht konfiguriert, wird der nächste Schritt nach dem aktuellen Schritt standardmäßig verwendet. |

## Arbeitslogik dieses Plugins

1. Das Plugin holt sich den Wert der konfigurierten Prozesseigenschaft und teilt ihn unter Verwendung des eventuell konfigurierten Trennzeichens *(oder `\n`, falls nicht)* in Teile auf.
2. Für jeden Teil der ursprünglichen Eigenschaft wird der möglicherweise konfigurierte Schritt *(oder der nächste Schritt des aktuellen Schritts, wenn er nicht konfiguriert ist)* noch einmal dupliziert, und die Namen dieser duplizierten neuen Schritte sind der Name des ursprünglichen Schritts plus jeweilige Bestellnummer unter diesen Duplikaten.
3. Für jeden duplizierten neuen Schritt wird eine neue Prozesseigenschaft erstellt, die genau nach dem Namen dieses neuen Schritts benannt ist. Der Wert dieser neuen Prozesseigenschaft ist genau der Teil der ursprünglichen Eigenschaft, auf dessen Grundlage dieser Schritt dupliziert wurde.
4. Wenn Duplikate für jeden Teil der ursprünglichen Eigenschaft durchgeführt werden, wird der ursprüngliche Schritt zum Duplizieren deaktiviert.
