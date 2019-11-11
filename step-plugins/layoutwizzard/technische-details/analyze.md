# Funktionsweise des LayoutWizzards

Der LayoutWizzard arbeitet in mehreren Phasen, um ausgehend von dem originalen Bild zu dem gewünschten Bildderivat zu gelangen. Hierzu durchläuft jedes Bild eine Bildanalyse, in der die Seiten ausgerichtet, das Objekt erkannt und die Buchfalz ermittelt wird. Die Bildanalyse besteht entsprechend aus den folgenden Phasen:

![Analysephasen des LayoutWizzards](../../../.gitbook/assets/layoutwizzard_diagramm_de.png)



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
        <img src="../../../.gitbook/assets/layoutwizzard_phases_1.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>Start</b>
        </p>
        <p></p>
        <p>Das originale Bild (Master-Scan) ist oft etwas verdreht und enth&#xE4;lt
          h&#xE4;ufig einen sichtbarem Bereich der gegen&#xFC;berliegenden Buchseite.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../../.gitbook/assets/layoutwizzard_phases_2.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>1. Phase: Seite ausrichten</b>
        </p>
        <p></p>
        <p>In der ersten Phase erfolgt die Erkennung der Ausrichtung der Seite und
          deren Rotation gegen&#xFC;ber einer waagerechten Leserichtung. Diese Rotation
          wird beim Speichern herausgerechnet, so dass die Seite idealerweise waagerecht
          ausgerichtet ist. Diese Phase wird h&#xE4;ufig auch als Deskewing bezeichnet.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../../.gitbook/assets/layoutwizzard_phases_3.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>2. Phase: Seite zuschneiden</b>
        </p>
        <p></p>
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
        <img src="../../../.gitbook/assets/layoutwizzard_phases_4.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>3. Phase: Falz erkennen</b>
        </p>
        <p></p>
        <p>In der dritten Phase erfolgt die Ermittlung der Buchfalz. Diese ist nur
          notwendig, wenn die Buchfalz tats&#xE4;chlich teil des Bildes ist und an
          dieses Bild entlang der Falz geschnitten werden soll, um den Teil der gegen&#xFC;berliegenden
          Seite zu verbergen. Diese Analysephase ist hochgradig abh&#xE4;ngig von
          der ermittelten Ausrichtung der Seite, also ob es sich hierbei um eine
          rechte oder linke Seite im Buch oder um eine Doppelseite handelt. In letzterem
          Fall wird das Bild beim Speichern in zwei Einzelbilder unterteilt, je eines
          rechts und links der ermittelten Falz.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../../.gitbook/assets/layoutwizzard_phases_5.png" alt/>
      </td>
      <td style="text-align:left">
        <p><b>Ende</b>
        </p>
        <p></p>
        <p>Der Vorschlag des LayoutWizzards nach dem Durchlaufen aller Analysephasen
          wird durch den Nutzer gepr&#xFC;ft und ggf. angepasst. Nach dem Anwenden
          dieser Werte speichert der LayoutWizzard das zugeschnittene Ergebnis als
          Derivat in einem separaten Verzeichnis des Goobi-Vorgangs.</p>
      </td>
    </tr>
  </tbody>
</table>