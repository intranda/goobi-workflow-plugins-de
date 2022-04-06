---
description: >-
  Goobi Administration Plugin für die Wiederherstellung von Bildordnern von externem Storage
---

# Wiederherstellung von archivierten Bildordnern

## Einführung
Dieses Plugin für Goobi workflow stellt Bildordner wieder her, die zuvor mit dem Plugin `goobi-plugin-step-archiveimagefolder` archiviert wurden.


| Details |  |
| :--- | :--- |
| Identifier | intranda_administration_restorearchivedimagefolders |
| Source code | [https://github.com/intranda/goobi-plugin-administration-restorearchivedimagefolders](https://github.com/intranda/goobi-plugin-administration-restorearchivedimagefolders) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2022.03 |
| Dokumentationsdatum | 29.03.2023 |


## Installation
Das Plugin besteht insgesamt aus den folgenden zu installierenden Dateien:

```text
goobi_plugin_administration_restorearchivedimagefolders.jar
goobi_plugin_administration_restorearchivedimagefolders-GUI.jar
plugin_intranda_administration_restorearchivedimagefolders.xml
```

Diese Dateien müssen in den richtigen Verzeichnissen installiert werden, so dass diese nach der Installation in folgenden Pfaden vorliegen:

```bash
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_restorearchivedimagefolders.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_restorearchivedimagefolders-GUI.jar
/opt/digiverso/goobi/config/plugin_intranda_administration_restorearchivedimagefolders.xml
```


## Konfiguration
Die Konfigurationsdatei ist im Moment leer, muss aber trotzdem vorliegen.

```xml
<config_plugin>
  <!-- currently no config needed -->
</config_plugin>
```

Die Information, woher die Daten geholt werden sollen, sind im jeweiligen Vorgangsordner in einer XML-Datei vom Archivierungs-Plugin hinterlegt worden.

Für die Authentifizierung an ssh-Servern wird an den üblichen Stellen (`$USER_HOME/.ssh`) nach public keys gesucht. Andere Authentifizierungsmethoden wie username/password sind nicht vorgesehen.  

Für eine Nutzung dieses Plugins muss der Nutzer über die korrekte Rollenberechtigung verfügen. Bitte weisen Sie daher der Benutzergruppe die Rolle `Plugin_administration_restorearchivedimagefolders` zu.

![Korrekt zugewiesene Rolle für die Nutzer](../.gitbook/assets/intranda_administration_restorearchivedimagefolders1_de.png)

## Bedienung des Plugins
Das Plugin bietet eine grafische Oberfläche an, die über das Menü `Administration` geöffnet werden kann. Dort kann dann ein Suchfilter verwendet werden, wie er auch an anderen Stellen von Goobi workflow (z.B. für die Vorgangsliste) verwendet wird. Mit einem Klick auf `Plugin ausführen`, werden dann die für die über den eingegebenen Filter gefundenen Vorgänge die Bilder wieder hergestellt. Die Nutzeroberfläche aktualisiert sich automatisch.

![User interface of the plugin](../.gitbook/assets/intranda_administration_restorearchivedimagefolders2_de.png)
