# Installation

Die Installation besteht aus insgesamt vier Programmbibliotheken, die im Apache Tomcat bzw. in Goobi erreichbar sein müssen:

| Datei | Speicherort |
| :--- | :--- |
| `layoutwizzard.jar` | Im lib-Ordner der Goobi webapp im Tomcat |
| `plugin_intranda_step_LayoutWizzard.jar` | Im Ordner plugins/step im Goobi-Arbeitsverzeichnis |
| `plugin_intranda_step_LayoutWizzard-GUI.jar` | Im Ordner plugins/GUI im Goobi-Arbeitsverzeichnis |

Neben diesen Programmdateien werden zwei Konfigurationsdateien benötigt, eine für das Goobi-Plugin, und eine für die zugrundeliegende LayoutWizzard-Bibliothek.

Die Konfigurationsdatei des Plugins `plugin_LayoutWizzardPlugin.xml` muss im Konfigurationsverzeichnis `config` innerhalb des Goobi-Arbeitsverzeichnisses liegen. Üblicherweise ist dies entsprechend dieser Pfad zur Datei:

```text
/opt/digiverso/goobi/config/plugin_LayoutWizzardPlugin.xml
```

Innerhalb dieser Datei wird der Pfad zur eigentlichen Konfigurationsdatei des LayoutWizzards angegeben. Der Aufbau dieser Datei sieht dabei folgendermaßen aus:

{% tabs %}
{% tab title="plugin\_LayoutWizzardPlugin.xml" %}
```markup
<config_plugin>
        <layout-wizzard-config-path>
                /opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
        </layout-wizzard-config-path>
</config_plugin>
```
{% endtab %}
{% endtabs %}

Die eigentliche Konfigurationsdatei gibt für den Vorgang der Layoutanalyse verschiedene Parameter vor. Diese Parameter sind beispielhaft in der folgenden Konfigurationsdatei aufgeführt. Sie befindet sich, wie in der Plugin-Konfigurationsdatei definiert unter folgendem Pfad:

```text
/opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
```

Beispielhaft hat diese Konfigurationsdatei den folgenden Inhalt:

{% tabs %}
{% tab title="layoutwizzard\_config.xml" %}
```markup
<!-- intranda Layout Wizzard configuration file -->
<config>
    <contentServerUrl>http://demo03.intranda.com/goobi/cs/cs</contentServerUrl>
    <defaultOutputFolderSuffix>media</defaultOutputFolderSuffix>
    <analysisImagesBasePath>/home/florian/LayoutWizzard/samples/</analysisImagesBasePath>
    <previews>
        <previewsPerPage>100</previewsPerPage>
        <maxPreviewsCached>10000</maxPreviewsCached>
        <previewWidth>700</previewWidth>
        <imageHeightLarge>800</imageHeightLarge>
    </previews>
    <outliers>
        <errorMultiplier>3.0</errorMultiplier>
        <weightExponent>2.0</weightExponent>
    </outliers>
    <saving>
        <defaultCompression quality="85">NONE</defaultCompression>
        <overwriteExistingImages>true</overwriteExistingImages>
    </saving>
    <analysis id="firstPageLeft">
        <firstPageOrientation>LEFT</firstPageOrientation>
    </analysis>
    <analysis id="firstPageRight">
        <firstPageOrientation>RIGHT</firstPageOrientation>
    </analysis>
    <analysis>
        <analysisStep name="PAGESKEW" type="edges" use="true">
            <saveAnalysisImages visibility="INVISIBLE" path="DESKEW">false</saveAnalysisImages>
            <deskewerMode visibility="VISIBLE">ALL_EDGES</deskewerMode>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode>GROUP_BY_DISTANCE</lineGroupingMode>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <analysisImageSize>300</analysisImageSize>
            <lowerCannyThreshold>70</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>10</minHoughLineLength>
            <houghLineThreshold>50</houghLineThreshold>
            <maxHoughLineGapSize>2</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>7</maxLineDistance>
            <rimAreaToIgnoreLines>0.0</rimAreaToIgnoreLines>
        </analysisStep>
        <analysisStep name="CONTENTAREA" use="true">
            <analysisImageSize>0</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="edgeDetection">false</saveAnalysisImages>
            <bitonalThreshold>150</bitonalThreshold>
            <bitonalInvert>false</bitonalInvert>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <contentPadding visibility="VISIBLE">0</contentPadding>
        </analysisStep>
        <analysisStep name="BOOKSPINE" use="true">
            <analysisImageSize>400</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="spineDetection">false</saveAnalysisImages>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode visibility="INVISIBLE">GROUP_BY_DISTANCE</lineGroupingMode>
            <croppingAggressiveness visibility="VISIBLE">BALANCED</croppingAggressiveness>
            <lowerCannyThreshold>40</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>20</minHoughLineLength>
            <houghLineThreshold>10</houghLineThreshold>
            <maxHoughLineGapSize>4</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>5</maxLineDistance>
            <featureSizeThreshold>0.1</featureSizeThreshold>
            <rimAreaToIgnoreLines>0.0125</rimAreaToIgnoreLines>
            <maxGroupAngleDeviation visibility="INVISIBLE">10</maxGroupAngleDeviation>
            <spineOffset visibility="VISIBLE">5</spineOffset>
        </analysisStep>
    </analysis>
</config>
```
{% endtab %}
{% endtabs %}

