# Ordner- und Dateioptionen

Hier können globale Einstellungen zur Handhabung der Dateien getätigt werden. Außerdem lässt sich hier der komplette LayoutWizzard Workflow zurücksetzen. Folgende Optionen sind immer verfügbar:

## **Seitenmodus**

Einstellung für die Einteilung der Seiten in rechte, linke, Doppel- oder Einzelseiten. Jeder Modus setzt die Ausrichtung für alle Bilder einmalig fest. Spätere Änderungen der Ausrichtung einzelner Seiten änder die Ausrichtung aller Folgeseiten wie vom Seitenmodus vorgegeben.  
Der interne Name in der Tabelle ist die in der LayoutWizzard-Konfigurationsdatei verwendete Bezeichnung für den Seitenmodus

| Seitenmodus | Beschreibung | interner Name |
| :--- | :--- | :--- |
| **Alternierend** | Die Bilder werden alternierend als rechte und linke Seiten behandelt. Die Ausrichtung der ersten Seite hängt von der Einstellung  `Schreibrichtung` ab. | `ALTERNATING` |
| **Nur linke Seiten** | Alle Bilder werden als linke Seiten behandelt. | `ALL_LEFT` |
| **Nur rechte Seiten** | Alle Bilder werden als rechte Seiten behandelt. | `ALL_RIGHT` |
| **Doppelseiten** | Alle Bilder werden als Doppelseiten behandelt. Das heißt, dass sie beim Speichern in jeweils zwei Einzelbilder zerlegt werden, wenn eine Falzlinie gesetzt wurde. | `DOUBLE_PAGES` |
| **Doppelseiten mit Einband** | Die Bilder werden wie im Modus `Doppelseiten` behandelt, außer der ersten und letzten Seite. Diese werden als Einzelblätter behandelt. | `DOUBLE_PAGES_WITH_COVERS` |
| **Einzelblätter** | Jede Seite wird als Einzelblatt behandelt. Das heißt es wird keine Falz geschnitten. | `SINGLE_PAGES` |
| **Unabhängig** | Alle Seiten behalten die aktuell eingestellte Ausrichtung. Änderungen der Orientierung von einzelnen Seiten haben keine Auswirkung auf die Folgeseiten. | `INDEPENDENT` |

## **Schreibrichtung**

Gibt die Richtung des Textflusses an. Ist die Schreibrichtung `Rechts-nach Links` ist beim alternierenden Seitenmodus die erste Seite nach dem Einband eine rechte Seite, sonst eine linke. Beim Doppelseitenmodus ist der rechte Teil einer Doppelseite in der Reihenfolge vor dem linken.

| **Schreibrichtung** | Beschreibung |
| :--- | :--- |
| **Links-nach-rechts** | Im alternierenden Seitenmodus ist die erste Seite nach dem Einband eine linke Seite. Im Doppelseiten Modus kommt die linke Seite einer Doppelseite vor der rechten in der Seitenreihenfolge. |
| **Rechts-nach-Links** | Im alternierenden Seitenmodus ist die erste Seite nach dem Einband eine rechte Seite. Im Doppelseiten Modus kommt die rechte Seite einer Doppelseite vor der linken in der Seitenreihenfolge. |

## **Verwerfen und neu Anfangen**

Hiermit können alle LayoutWizzard-Daten für diesen Vorgang gelöscht werden; das sind sowohl alle Analysedaten als auch die für dieen Vorgang spezifische Konfiguration. Beim Druck auf diesen Knopf öffnet sich ein Menü, um die Vorlage für neue Konfiguration auszuwählen. Druck auf `OK` verwirft alle bisherigen Daten und erstellt eine neue Konfiguration für den Vorgang aufgrund des ausgewählten Templates.

Die folgenden Optionen sind nur im erweiterten Modus verfügbar:

## Eingabe-Ordner

Der Ordner innerhalb des `images` Ordners des Goobi-Vorgangs, aus dem die Originalbilder gelesen werden.

#### **Ausgabe-Ordner**

Der Ordner innerhalb des `images` Ordners des Goobi-Vorgangs, in den die Derivate gespeichert werden.

#### **Komprimierung der Ausgabebilder**

Die Komprimierungsart der Derivate. Die Derivate werden immer im Tiff-Format gespeichert

| **Komprimierung** | Beschreibung |
| :--- | :--- |
| **Keine** | Unkomprimiertes Tiff |
| **JPEG** | Jpeg-komprimiertes Tiff |

## Ausreißer markieren für

Bilder, die in der Analyse auffällig andere Werte aufweisen als die maximal 12 Bilder vor und nach ihnen. Ebenso Bilder, deren Analyse aufgrund von Fehlern oder Zeitüberschreitungen nicht abgeschlossen werden konnte.  
Ausreißer werden in der Dateiliste rot dargestellt und in der Vorschauansicht von einem roten Rahmen umgeben.

Folgende Ausreißertypen werden unterschieden und können als zu markieren ausgewählt werden:

| Ausreißertyp | Beschreibung |
| :--- | :--- |
| **Rotation** | Winkel der Seitenausrichtung |
| **Seitengröße** | Größe des Zuschneiderahmens |
| **Falzposition** | Abstand der Falz vom Seitenrand |
| **Nicht bearbeitet** | Das Bild wurde konnte nicht vollständig analysiert werden. |
| **Alle** | Alle oben genannten Typen von Ausreißern werden markiert. |
| **Keine** | Ausreißer werden überhaupt nicht markiert. |

## Zuschneidebereiche anpassen

Wenn rechte und linke Bilder einzeln gescannt werden und sie dennoch in einer Doppelseitenansicht zusammenpassen sollen, versucht der LayoutWizzard Seiten so zuzuschneiden, dass der Inhalt jeweils auf gleicher Höhe beginnt, also der obere Buchrand auf allen Seiten bündig ist. Je nach Aufnahme ist es aber oft nicht möglich zwei Seiten ohne Verzerrungen komplett bündig zu schneiden. Für eine optimale Doppelseitendarstellung ist daher immer auch ein Doppelseitenscan anstelle dieser Option zu empfehlen.

| Anpassungsmodus | Beschreibung |
| :--- | :--- |
| **Keine Angleichung** | Angleich ist ausgeschaltet |
| **Angleichung gegenüberliegender Seiten, links nach rechts** | Jeweils gegenüberliegende Seiten werden bündig geschnitten. Als gegenüberliegende Seite einer linken Seiten gilt jeweils die folgende rechte Seite |
| **Angleichung gegenüberliegender Seiten, rechts nach links** | Jeweils gegenüberliegende Seiten werden bündig geschnitten. Als gegenüberliegende Seite einer rechten Seiten gilt jeweils die folgende linke Seite |
| **Angleichung über alle Seiten** | Alle Seiten werden möglichst bündig geschnitten. Dies verursacht oft große ungeschnittene Ränder, benötigt relativ viel Bearbeitungszeit und ist daher nicht empfehlenswert |



