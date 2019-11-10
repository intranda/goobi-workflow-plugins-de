---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin LayoutWizzard zum
  automatisierten Croppen von Buchseitenscans.
---

# LayoutWizzard

## 1. Einführung

Der LayoutWizzard ist ein Werkzeug zur Analyse vom digitalisierten Buchseiten und ähnlicher Materialien, das die Position der physischen Seite im digitalisierten Bild erkennt und das Bild danach ausrichten und schneiden kann.

Die Analyse im LayoutWizzard findet halbautomatisch statt. Sie beginnt in der Regel mit einem automatischen Arbeitsschritt, in dem alle Bilder nach festen Algorithmen analysiert werden. Anschließend werden die Ergebnisse in einem manuellen Schritt überprüft und gegebenenfalls korrigiert. Abschließend werden in einem weiteren automatischen Schritt zugeschnittene Derivate der Ausgangsbilder erzeugt, üblicherweise innerhalb des Derivate-Ordners im entsprechenden Goobi-Vorgang.

Die automatischen Arbeitsschritte finden in aller Regel in einem TaskManager-Plugin statt, um andere Arbeiten in Goobi nicht zu blockieren. Aber auch eine Ausführung ohne TaskManager-Plugin ist möglich. 

Die manuelle Kontrolle der Analyseergebnisse wird in eines eigenen Goobi-Plugins als Arbeitsschritt durchgeführt.

Das Zusammenspiel der hier beschriebenen drei LayoutWizzard-Arbeitsschritte bildet einen eigenen kleinen Workflow innerhalb eines Goobi-Workflows.

| Details |  |
| :--- | :--- |
| Version des Plugins | 1.0.0 |
| Identifier | intranda\_step\_crop |
| Source code | - nicht öffentlich verfügbar - |
| Kompatibilität | Goobi workflow 3.0 |
| Dokumentation vom | 10.11.2019 |

