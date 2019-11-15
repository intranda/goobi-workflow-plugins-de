# Verwendung des Plugins

Das Goobi-LayoutWizzard-Plugin ist der wichtigste Interaktionspunkt von Goobi-Nutzern mit dem LayoutWizzard. Es ermöglicht die komplette Konfiguration und Durchführung der automatischen Analyse und das Speichern der Derivate. Seine Hauptaufgabe ist jedoch die Kontrolle und Korrektur der Analyseergebnisse. Dazu besitzt das LayoutWizzard-Plugin zwei Ansichten; die `Einzelseitenansicht` zur detaillierten Bearbeitung einzelner Bilder und die `Vorschauansicht` zur Sichtung und direkten Korrektur aller Bilder eines Vorganges. 

Alle allgemeinen Einstellungen zur Konfiguration und dem Analyseworkflow sind in der `Einzelseitenansicht` untergebracht, weshalb sie als Startseite für das Plugin fungiert. Die eigentliche Kontrolle und Korrektur wird in den meisten Fällen jedoch ausschließlich in der `Vorschauansicht` stattfinden.

## Konfiguration

Die Konfigurationsdatei des Plugins muss im `config` Verzeichnis der Goobi-Installation liegen und heißt in aktuellen Goobi-Versionen `plugin_intranda_step_LayoutWizzard.xml`.

Üblicherweise lautet der vollständige Pfad wie folgt:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_LayoutWizzard.xml
```

Der Inhalt dieser Konfigurationsdatei ist folgendermaßen aufgebaut:

```markup
<config_plugin>
  <config>
    <project>*</project>
    <step>*</step>
    <layout-wizzard-config-path>
        /opt/digiverso/LayoutWizzard/layoutwizzard_config.xml
    </layout-wizzard-config-path>
    <startPage>PREVIEW</startPage>
	<!--<startPage>SINGLEVIEW</startPage>-->
    <previewMode>ALTERNATING</previewMode>
    <contentBorder>
        <width>2</width>
        <color>#00fa9a</color>
        <opacity>1</opacity>
    </contentBorder>
    <spineMarker>
        <outer>
            <linewidth>20</linewidth>
            <linecolor>#D73946</linecolor>
            <opacity>0.1</opacity>
        </outer>
        <inner>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <opacity>1</opacity>
        </inner>
    </spineMarker>
	<preview>
		<cropFrame>
			<linewidth>2</linewidth>
	        <linecolor>#44ffdd</linecolor>
	        <fillcolor>#f1f2f3</fillcolor>
	        <clickradius>20</clickradius>
		</cropFrame>
		<spineMarker>
			<linewidth>2</linewidth>
	        <linecolor>#ff3344</linecolor>
	        <clickradius>20</clickradius>
		</spineMarker>
	</preview>
    <previewCroppingOptions>
        <show>true</show>
    </previewCroppingOptions>
    <previewOrientationSelect>
        <show>false</show>
    </previewOrientationSelect>
    <globalCroppingOptions>
        <show>true</show>
        <unit>mm</unit>
        <keyboardControls use="true">
            <moveMaskKey>SHIFT</moveMaskKey>
            <moveMaskKey>CTRL</moveMaskKey>
            <resizeMaskKey>SHIFT</resizeMaskKey>
            <stepSize>0.1</stepSize>
        </keyboardControls>
    </globalCroppingOptions>
    <info show="true">
        <spine>
            <format>Falz: {f}{u}</format>
        </spine>
    </info>
  </config>
</config_plugin>
```

{% hint style="warning" %}
Bitte beachten Sie, dass hier der korrekte Pfad zur Konfigurationsdatei des LayoutWizzards  im innerhalb des Elements`<layout-wizzard-config-path>` angegeben ist.
{% endhint %}

