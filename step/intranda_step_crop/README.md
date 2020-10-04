---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin LayoutWizzard zum
  automatisierten Croppen von Buchseitenscans.
---

# LayoutWizzard

## 1. Einführung

Der LayoutWizzard ist ein Werkzeug zur Analyse vom digitalisierten Buchseiten und ähnlicher Materialien, das die Position der physischen Seite im digitalisierten Bild erkennt und das Bild entsprechend ausrichten und schneiden kann.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_step\_crop |
| Source code | [https://gitea.intranda.com/goobi-workflow/goobi-plugin-step-crop](https://gitea.intranda.com/goobi-workflow/goobi-plugin-step-crop) |
| Lizenz | kommerzielle Lizenz |
| Kompatibilität | Goobi workflow 3.0 |
| Dokumentationsdatum | 10.11.2019 |

## 2. Arbeitsweise

Die Analyse im LayoutWizzard findet halbautomatisch statt. Sie beginnt in der Regel mit einem automatischen Arbeitsschritt, in dem alle Bilder nach festen Algorithmen analysiert werden. Anschließend werden die Ergebnisse in einem manuellen Schritt überprüft und gegebenenfalls korrigiert. Abschließend werden in einem weiteren automatischen Schritt zugeschnittene Derivate der Ausgangsbilder erzeugt, üblicherweise innerhalb des Derivate-Ordners im entsprechenden Goobi-Vorgang.

![Arbeitsweise des LayoutWizzards innerhalb des Goobi Workflows](../../.gitbook/assets/intranda_step_crop_workflow_de.png)

Die automatischen Arbeitsschritte \(`Automatische Bildanalyse` und `Automatisches Croppen`\) finden in aller Regel ausgelagert in einem TaskManager-Plugin statt, um andere Arbeiten innerhalb von Goobi nicht durch eine hohe Rechenlast auf dem Goobi-Server einzuschränken. Aber auch eine Ausführung ohne TaskManager-Plugin ist möglich, so dass diese automatische Arbeitsschritte innerhalb von Goobi Step Plugins ohne eigenen Nutzeroberfläche unmittelbar innerhalb von Goobi workflow erfolgen.

Die manuelle Kontrolle der Analyseergebnisse mit dem Vorschlag für das Croppen erfolgt innerhalb eines eigenständigen Goobi Step Plugins mit Nutzeroberläche, so dass dessen Bedienung vollständig in Goobi workflow integriert ist.

Je nach individueller Installation von Goobi workflow und der jeweiligen Workflows können die einzelnen Arbeitsschritte natürlich individuell benannt werden. Im folgenden Screenshot wurden für die drei nacheinander folgenden Arbeitsschritte beispielsweise andere Namen vergeben:

![Individuelle Benennung der einzelnen Arbeitsschritte, die zum LayoutWizzard geh&#xF6;ren](../../.gitbook/assets/intranda_step_crop_goobi_workflow%20%281%29.png)

## 3. Details zur Bildanalyse

Die Bildanalyse des LayoutWizzard arbeitet in mehreren Phasen, um ausgehend von dem originalen Bild zu dem gewünschten Bildderivat zu gelangen. Hierzu durchläuft jedes Bild eine Bildanalyse, in der die Seiten ausgerichtet, das Objekt erkannt und die Buchfalz ermittelt wird. Die Bildanalyse besteht entsprechend zumeist aus den folgenden Phasen:

![Analysephasen des LayoutWizzards](../../.gitbook/assets/intranda_step_crop_diagramm_de.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Beispiel</th>
      <th style="text-align:left">Phase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/intranda_step_crop_phases_1.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>Start</b>
        </p>
        <p>Das originale Bild (Master-Scan) ist oft etwas verdreht und enth&#xE4;lt
          h&#xE4;ufig einen sichtbarem Bereich der gegen&#xFC;berliegenden Buchseite.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/intranda_step_crop_phases_2.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>1. Phase: Seite ausrichten</b>
        </p>
        <p>In der ersten Phase erfolgt die Erkennung der Ausrichtung der Seite und
          deren Rotation gegen&#xFC;ber einer waagerechten Leserichtung. Diese Rotation
          wird beim Speichern herausgerechnet, so dass die Seite idealerweise waagerecht
          ausgerichtet ist. Diese Phase wird h&#xE4;ufig auch als Deskewing bezeichnet.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/intranda_step_crop_phases_3.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>2. Phase: Seite zuschneiden</b>
        </p>
        <p>In der zweiten Phase findet die Erkennung des erfassten Objektes statt,
          &#xFC;blicherweise z.B. das Buch. Um das ermittelte Objekt wird hierbei
          ein rechteckiger Zuschneiderahmen berechnet, der das Objekt vollst&#xE4;ndig
          mit m&#xF6;glichst wenig zus&#xE4;tzlichem Rand enth&#xE4;lt. Damit dieser
          Rahmen m&#xF6;glichst genau auf ein rechteckiges Objekt wie ein Buch passt,
          sollte die Ausrichtung der Seite (Phase 1) zuvor angewandt werden. Diese
          Phase wird h&#xE4;ufig auch als das Abschneiden von R&#xE4;ndern bezeichnet,
          da die zumeist dunklen Bildr&#xE4;nder hierbei entfernt werden.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/intranda_step_crop_phases_4.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>3. Phase: Falz erkennen</b>
        </p>
        <p>In der dritten Phase erfolgt die Ermittlung der Buchfalz. Diese ist nur
          notwendig, wenn die Buchfalz tats&#xE4;chlich teil des Bildes ist und an
          diesem Bild entlang der Falz geschnitten werden soll, um den Teil der gegen&#xFC;berliegenden
          Seite zu verbergen. Diese Analysephase ist hochgradig abh&#xE4;ngig von
          der ermittelten Ausrichtung der Seite, also ob es sich hierbei um eine
          rechte oder linke Seite im Buch oder um eine Doppelseite handelt. In letzterem
          Fall wird das Bild beim Speichern in zwei Einzelbilder unterteilt, je eines
          rechts und links der ermittelten Falz.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/intranda_step_crop_phases_5.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>Ende</b>
        </p>
        <p>Der Vorschlag des LayoutWizzards wird dem Nutzer nach dem Durchlaufen
          aller Analysephasen zur Pr&#xFC;fung &#xFC;bergeben und dort ggf. angepasst.</p>
      </td>
    </tr>
  </tbody>
</table>

