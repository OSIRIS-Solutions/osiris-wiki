---
title: Statistik und Exporte
---

# Initiativen und Objectives auswerten

<!-- md:version 3.0.0 -->

Diese Anleitung gilt ab **OSIRIS 3.0.0** (Beta). Für die Statistik benötigst du Zugang zu den Initiativen. Die sichtbaren Ergebnisse hängen zusätzlich von deinen Rechten für die verknüpften Inhalte ab.

## Inhalte finden

Auf den Detailseiten der Initiative oder des Objectives findest du die jeweils verknüpften Inhalte. In den erweiterten Suchen stehen Initiativen und Objectives als Filter und Tabellenspalten zur Verfügung. Bei Initiative-Zuordnungen zählen auch die automatisch aus Objectives abgeleiteten Beziehungen.

Für Dokumente und Berichte können die Beziehungen in den bestehenden Berichts- und Exportfunktionen verwendet werden. Wie ihr solche Ausgaben gestaltet, beschreibt der [Berichtsbereich](../reporting/index.md).

## Statistik öffnen

Klicke oben auf der Initiativen- oder Objective-Übersicht auf **Statistik**. Auf der Statistikseite kannst du zwischen **Initiativen** und **Objectives** wechseln. Der Objective-Tab erscheint nur, wenn Objectives angelegt sind.

Wähle ein Berichtsjahr, einen freien Zeitraum oder einen Stichtag und bestätige mit **Aktualisieren**. Zusätzlich kannst du einen einzelnen Eintrag auswählen und archivierte Einträge ein- oder ausschließen. Die angezeigte Statusspalte beschreibt den aktuellen Status.

## Diagramm und Tabellen

- Das Balkendiagramm zeigt für eine wählbare Inhaltsart bis zu 20 Initiativen beziehungsweise Objectives mit den meisten zugehörigen Inhalten.
- Die Übersichtstabelle enthält alle ausgewählten Einträge, ihre Laufzeit, den aktuellen Status und die Anzahl je Inhaltsart.
- Unter **Gezählte Inhalte** kannst du die einzelnen Ergebnisse zur Nachprüfung öffnen.

**Publikationen** und **Aktivitäten** werden getrennt gezählt; die Aktivitätsspalte enthält keine Publikationen. Ein Inhalt zählt pro Initiative oder Objective einmal. Bei Mehrfachzuordnungen kann er in mehreren Zeilen erscheinen. Addiere deshalb nicht alle Zeilen zu einer vermeintlich eindeutigen Gesamtzahl.

## Was bedeutet der Zeitraum?

Gezählt werden Inhalte, deren Datum oder Laufzeit den gewählten Zeitraum überschneidet. Bei Anträgen zählt das Einreichungsdatum. Bei Aktivitäten mit unvollständigem Datum gilt der bekannte Monat oder das bekannte Jahr. Inhalte ohne auswertbares Datum werden nicht mitgezählt; die Seite weist darauf hin. Berichtsausschlüsse werden berücksichtigt.

!!! info "Stichtag ist kein früherer Datenbankstand"
    Die Auswertung nutzt die **aktuell gespeicherten** Beziehungen und Daten. Eine heute nachgetragene Publikation kann daher auch in einer Auswertung für das Vorjahr erscheinen. Ein Stichtag filtert die Inhaltsdaten; er stellt nicht den damaligen Bearbeitungsstand wieder her. Die Laufzeiten der Initiativen beziehungsweise Objectives werden angezeigt, begrenzen aber nicht zusätzlich die Zählung bereits verknüpfter Inhalte.

## Herunterladen und weiterverwenden

Über die Exportfunktionen der Statistik-Tabellen kannst du die angezeigten Daten unter anderem für Excel herunterladen. **Auswertungsdaten herunterladen (CSV)** liefert die Anzahl je Initiative beziehungsweise Objective und Inhaltsart einschließlich IDs und Berichtszeitraum, etwa zur Weiterverarbeitung in einer Tabellenkalkulation oder einem BI-System.

Der CSV-Download enthält aggregierte Zahlen, nicht die vollständigen Inhaltsdatensätze. Für diese nutze die Tabelle **Gezählte Inhalte** beziehungsweise die [OSIRIS-API](../../api/initiatives.md). Planung und Plan-Ist-Vergleiche erfolgen außerhalb von OSIRIS.

[Zur Übersicht](index.md)
