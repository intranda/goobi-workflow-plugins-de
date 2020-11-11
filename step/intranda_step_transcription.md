---
description: >-
  This step plugin allows you to create transcriptions of works.
  The transcriptions are recorded without word coordinates.
---

# Transcription of image content

## Introduction

The transcription plug-in allows the user to edit the txt-OCR results of a Goobi process. It always displays an image and a rich text editor in which the text can be entered.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_transcription |
| Source code | nicht verfügbar |
| Licence | GPL 2.0 or neuer |
| Compatibility | Goobi workflow 20.09 |
| Documentation date | 11.11.2020 |

## Installation

The plugin consists of two files, which are expected at the following paths:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_transcription.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_transcription-GUI.jar
```

## Configuration

The configuration is expected in the following path:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_transcription.xml
```

The configuration looks like this:

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
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

## Usage of this plugin

To use the plugin, it must be integrated into a workflow and is then available for users to edit.

If a user accepts a task with this plugin, he can enter its user interface. There it is then possible to browse between the image files. A rich text editor is displayed for the respective current page, which shows any existing transcription or, if available, the OCR results. In this editor the previous result can be corrected or a completely new text can be transcribed.

{% hint style="info" %}
Please note that this plugin only allows a simple transcription of page content. It is not possible to enter coordinates for paragraphs, lines or words.
{% endhint %}
