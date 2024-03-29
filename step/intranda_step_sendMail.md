---
description: >-
  Goobi Step Plugin für das Versenden von E-Mails innerhalb einer Aufgabe.
---

# Versenden von E-Mails


## Einführung
Die vorliegende Dokumentation beschreibt die Installation, die Konfiguration und den Einsatz des Step Plugins für versenden von Mails innerhalb einer Aufgabe in Goobi workflow. Die Liste der Empfänger und der Text lassen sich für verschiedene Arbeitsschritte individuell konfigurieren. Dabei stehen auch alle Felder aus dem VariablenReplacer zur Verfügung. Somit kann auch auf Metadaten oder Informationen zum Vorgang, Schritt oder Projekt zurückgegriffen werden.

| Details |  |
| :--- | :--- |
| Identifier | intranda_step_sendMail |
| Source code | [https://github.com/intranda/goobi-plugin-step-sendMail](https://github.com/intranda/goobi-plugin-step-sendMail) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.03 |
| Dokumentationsdatum | 30.03.2022 |


## Arbeitsweise des Plugins
Das Plugin wird üblicherweise vollautomatisch innerhalb des Workflows ausgeführt. Es ermittelt zunächst, ob sich innerhalb der Konfigurationsdatei ein Block befindet, der für den aktuellen Workflow bzgl. des Projektnamens und Arbeitsschrittes konfiguriert wurde. Wenn dies der Fall ist, wird die Mail generiert und an die konfigurierten Empfänger versendet.


## Bedienung des Plugins
Dieses Plugin wird in den Workflow so integriert, dass es automatisch ausgeführt wird. Eine manuelle Interaktion mit dem Plugin ist nicht notwendig. Zur Verwendung innerhalb eines Arbeitsschrittes des Workflows sollte es wie im nachfolgenden Screenshot konfiguriert werden.

![Integration des Plugins in den Workflow](../.gitbook/assets/intranda_step_sendMail_de.png)


## Installation
Das Plugin besteht insgesamt aus den folgenden zu installierenden Dateien:

```text
plugin_intranda_step_sendMail.jar
plugin_intranda_step_sendMail.xml
```

Die erste Datei muss in dem folgenden Verzeichnis installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_sendMail.jar
```

Daneben gibt es eine Konfigurationsdatei, die an folgender Stelle liegen muss:

```bash
/opt/digiverso/goobi/plugins/config/plugin_intranda_step_sendMail.xml
```

## Konfiguration

Die Konfiguration des Plugins erfolgt über die Konfigurationsdatei `plugin_intranda_step_sendMail.xml` und kann im laufenden Betrieb angepasst werden. Im folgenden ist eine beispielhafte Konfigurationsdatei aufgeführt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
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
        <!-- mail account -->
        <smtpUser>test@example.com</smtpUser>
        <smtpPassword>password</smtpPassword>

        <!-- server configuration -->
        <smtpServer>example.com</smtpServer>
        <smtpUseStartTls>false</smtpUseStartTls>
        <smtpUseSsl>true</smtpUseSsl>

        <!-- displayed sender address -->
        <smtpSenderAddress>do-not-reply@example.com</smtpSenderAddress>
        <!-- receiver, can be repeated -->
        <receiver>user@example.com</receiver>
        <receiver>second-user@example.com</receiver>

        <!-- message -->
        <messageSubject>subject text</messageSubject>
        <messageBody>body &lt;br /&gt; &lt;h1&gt;with html&lt;/h1&gt;</messageBody>
    </config>
</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. |
| `<smtpServer>` | Dieser Parameter legt den SMTP-Server fest. |
| `<smtpUseStartTls>` | Mit diesem Parameter wird gesteuert, ob der Zugriff wie TLS laufen soll. |
| `<smtpUseSsl>` | Hiermit wird festgelegt, ob die Kommunikation via SSL verschlüsselt sein soll. |
| `<smtpUser>` | Dieser Parameter legt den Nutzernamen fest. |
| `<smtpPassword>` | Hiermit wird das zu verwendende Passwort definiert. |
| `<smtpSenderAddress>` | Das Feld `<smtpSenderAddress>` definiert den angezeigten Absender, der sich auch vom Nutzernamen unterscheiden kann. |
| `<receiver>` | Das Feld `<receiver>` kann mehrfach genutzt werden und enthält die Email-Adressen der Empfänger. |
| `<messageSubject>` | Dieser Parameter erlaubt die Festlegung des Subjects. Eine Verwendung von Variablen ist hier möglich. |
| `<messageBody>` | In `<messageBody>` wird die Mail selbst definiert. Hier kann PlainText oder auch ein HTML formatierter Text geschrieben werden. Zusätzlich ist hier der Zugriff auf das Variablensystem von Goobi möglich, damit können auch Informationen zum Vorgang, Projekt, Eigenschaften oder Metadaten in der Mail genutzt werden. |
