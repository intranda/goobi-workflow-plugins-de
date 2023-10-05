---
Beschreibung: >-
    Dieses Plugin wurde ursprünglich implementiert, um mit dem ALMA-System zu kommunizieren und zurückgegebene Antworten zu verarbeiten. Dank seines allgemeinen Designs kann es aber auch für die Anbindung mit anderen Systemen über REST verwendet werden.
---

# ALMA API Plugin

## Einführung

Dieses Plugin wird verwendet, um Anfragen an ein Dienstsystem, z.B. ALMA, zu senden und die zurückgegebenen Antworten zu verarbeiten. Mehrere Befehle können konfiguriert werden, um eine komplizierte Aufgabe zusammenzustellen, und das Plugin führt diese Befehle nacheinander in derselben Reihenfolge aus.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_alma\_api |
| Source code | [https://github.com/intranda/goobi-plugin-step-alma-api](https://github.com/intranda/goobi-plugin-step-alma-api) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2023.09 |
| Dokumentationsdatum | 05.10.2023 |

## Installation

Zur Nutzung des Plugins muss die folgende JAR Datei ans folgende Ort kopiert werden:

```text
{GOOBI_INSTALLATION_PFAD}/plugins/step/plugin_intranda_step_alma_api.jar
```

Die Konfigurationsdatei `plugin_intranda_step_alma_api.xml` wird unter folgendem Pfad erwartet:

```text
{GOOBI_INSTALLATION_PFAD}/config/plugin_intranda_step_alma_api.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

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

        <!-- Base URL -->
        <url>https://api-eu.hosted.exlibrisgroup.com</url>

        <!-- API key -->
        <api-key>CHANGE_ME</api-key>

        <!-- Variables that can be used for following commands.
              @name: name of the variable, e.g. VARIABLE. To use this variable's value, one can simply use {$VARIABLE}.
              @value: value to initialize this variable. It can be plain string value, or a Goobi variable, e.g. {meta.NAME} for a Metadata named NAME.
         -->
        <variable name="MMS_ID" value="{meta.RezenssionsZssDBID}" />
        <!-- There can be multiple variables defined before all commands. -->
        <variable name="SIGNATURE" value="{meta.shelfmarksource}" />

        <!-- A command is a step to perform. There can be several commands configured, and if so, they will be run one by one in the same order as they are defined.
              @method: get | put | post | patch
              @endpoint: raw endpoint string without replacing the placeholders enclosed by {}. For every placeholder say {PLACEHOLDER}, one has to configure it in a
                         sub-tag, and in our example it would be <PLACEHOLDER>. Options for values of these sub-tags are:
                         - plain text value
                         - any variable defined by a <variable> tag before all <command> blocks
                         - any variable defined by a <target> sub-tag of any previous <command> block
         -->     
        <command method="get" endpoint="/almaws/v1/bibs/{mms_id}/holdings/ALL/items">
        	<!-- define the value of the placeholder {mms_id} using the variable named MMS_ID -->
        	<mms_id>{$MMS_ID}</mms_id>

        	<!-- Filter condition that is to be applied on the REST call response. OPTIONAL.
        	      @key: JSON path from which the values are to be fetched for the filtering process
        	      @fallback: JSON path that shall be used when the JSON path configured by @key contains no value. OPTIONAL.
        	      @value: value to be compared with, which can be a plain text value, or a variable defined previously in the format {$VARIABLE}
        	      @alt: alternative option if there is no match found. Options are: all | first | last | random | none. OPTIONAL.
                      - all: filter nothing out, search the whole JSONObject for the following targets
                      - first: get the first JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - last: get the last JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - random: get a random JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - none: filter everything out and stop searching. DEFAULT.
        	 -->
        	<filter key="item.item_data.alternative_call_number" fallback="item.holding_data.permanent_call_number" value="{$SIGNATURE}" alt="all" />

        	<!-- Target values that is to be retrieved from the REST call response and saved as a variable. OPTIONAL.
        	      @var: name of the variable that is to be used to save the target values retrieved.
                      If the variable was already defined before, then its value will be updated. Otherwise a new variable under this name will be created.
        	      @path: JSON path from where values are to be retrieved.
        	 -->
        	<target var="ITEM_PID" path="item.item_data.pid" />

        	<!-- There can be multiple targets configured. -->
        	<!-- In this example, the new variable will be named HOLDING_ID and it can be reused in the following steps using {$HOLDING_ID}. -->
        	<target var="HOLDING_ID" path="item.holding_data.holding_id" />
        </command>

        <command method ="post" endpoint="/almaws/v1/bibs/{mms_id}/holdings/{holding_id}/items/{item_pid}">
        	<!-- define the value of the placeholder {mms_id} using the variable named MMS_ID -->
        	<mms_id>{$MMS_ID}</mms_id>
        	<!-- define the value of the placeholder {holding_id} using the variable named HOLDING_ID -->
        	<holding_id>{$HOLDING_ID}</holding_id>
        	<!-- define the value of the placeholder {item_pid} using the variable named ITEM_PID -->
        	<item_pid>{$ITEM_PID}</item_pid>

          <!-- A parameter tag can be used to define parameters that shall be sent by the REST request. There can be multiple parameters configured.
                  @name: parameter name
                  @value: parameter value, which can only be plain text values here
            -->
        	<parameter name="op" value="scan" />
        	<parameter name="library" value="" />
        	<parameter name="department" value="InDigiZ_Dep" />
        	<parameter name="work_order_type" value="InDigiZ" />
        	<parameter name="done" value="false" />
        </command>

        <!-- A property tag is used to define a process property that is to be saved after running all previous commands. OPTIONAL.
              @name: name of the new process property
              @value: value of the new process property, possible values are
                      - a plain text value
                      - a variable defined before all <command> blocks via a <variable> tag
                      - a variable defined within some <command> block via a <target> tag
              @choice: indicates how many items should be saved into this new property, OPTIONS are first | last | all | random.
                       - first: save only the first one among all retrieved values
                       - last: save only the last one among all retrieved values
                       - random: save a random one from all retrieved values
                       - all: join all retrieved values to formulate a single string separated by commas and save it. DEFAULT.
              @overwrite: true if the old property named so should be reused, false if a new property should be created, DEFAULT false.
        -->
        <property name="holding_id" value="{$HOLDING_ID}" choice="all" overwrite="true" />
        <!-- There can be multiple property tags configured. -->
        <property name="item_pid" value="{$ITEM_PID}" choice="all" />

    </config>

</config_plugin>
```

### Allgemeine Konfigurationen

| Wert | Beschreibung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block <config> gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro <config> Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block <config> gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro <config> Block vorkommen. |
| `url` | Hier wird die Basis-URL des Systems angegeben. |
| `api-key` | Hier wird der API-Schlüssel für die Verbindung zum System konfiguriert. |
| `variable` | Mit diesem Tag kann man eine Variable definieren, die von allen nachfolgenden Befehlen verwendet werden kann. Dieses Tag hat zwei Attribute, wobei `@name` den Namen und `@value` den Wert definiert. `@value` erwartet einen einfachen Textwert oder eine Goobi-Variable. |
| `command` | Ein Befehlsblock definiert einen Befehl, der im Auftrag ausgeführt werden soll. Er hat selbst zwei Attribute, wobei `@method` die zu verwendende Methode angibt und `@endpoint` den rohen Endpunktpfad, deren Platzhalter nicht ersetzt werden. Weitere Einzelheiten finden Sie in der nachstehenden Tabelle und in der obigen Beispielkonfiguration. |
| `property` | Ein optionales Property-Tag definiert eine Prozesseigenschaft, die nach der Ausführung aller Befehle gespeichert werden soll. Es hat zwei obligatorische Attribute, wobei `@name` den Namen der Prozesseigenschaft definiert und `@value` den Wert der Eigenschaft bestimmt, der ein einfacher Textwert oder eine zuvor definierte Variable sein kann. Es hat auch zwei optionale Attribute, wobei `@choice` angibt, welcher Wert gespeichert werden soll, wenn mehrere gefunden werden, und `@overwrite` bestimmt, ob eine zuvor erstellte Prozesseigenschaft mit demselben Namen wiederverwendet werden soll oder nicht. |

### Konfigurationen innerhalb von Befehlsblöcken

| Wert | Beschreibung |
| :--- | :--- |
| `filter` | Hier wird angegeben, welche Teile der JSON-Antwort für die Suche nach den "target"-Werten verwendet werden sollen. Es hat vier Attribute, wobei `@key` und `@value` obligatorisch sind, während `@fallback` und `@alt` optional sind. Weitere Einzelheiten finden Sie in den Kommentaren in der Beispielkonfiguration. |
| `target` | Hier gibt man an, welche Werte als Variablen zur späteren Verwendung gespeichert werden sollen. Sie hat zwei Attribute, wobei `@var` den Variablennamen und `@path` den JSON-Pfad zum Abrufen der Werte angibt. |
| `parameter` | Hier wird ein Parameter angegeben, der zusammen mit einer Anfrage an das System gesendet werden soll. Er hat zwei Attribute, nämlich `@name` für den Parameternamen und `@value` für den Parameterwert, der NUR aus reinen Textwerten bestehen kann. |
