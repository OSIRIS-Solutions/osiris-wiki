---
title: Initiativen konfigurieren
---

# Initiativen konfigurieren

<!-- md:version 3.0.0 -->
<!-- md:feature -->

Diese Anleitung gilt ab **OSIRIS 3.0.0**, zunächst als Beta. „Initiativen“ kann in eurem Institut beispielsweise „Missionen“ oder „Arbeitsgruppen“ heißen. Die [Einordnung gegenüber Forschungsbereichen und Organisationseinheiten](../../topics/organizational-structure.md) erklärt die fachlichen Unterschiede. Die [Nutzeranleitung](../../users/initiatives/index.md) beschreibt die tägliche Arbeit.

## 1. Aktivieren und Bezeichnungen anpassen

1. Öffne im Admin-Bereich **System → Funktionen** (`/admin/features`). Aktiviere Initiativen und speichere.
2. Öffne **System → Sprachen** (`/admin/languages`). Über die Anpassung der jeweiligen Sprache kannst du die Begriffe für eure Einrichtung ändern.
3. Passe mindestens Singular und Plural an: `common.initiative` und `common.initiatives`. Die Begriffe für Objectives stehen unter `initiatives.objective` und `initiatives.objectives`. Wiederhole dies für die benötigten Oberflächensprachen.

Die Bezeichnungen werden über die Sprachverwaltung geändert; es gibt kein eigenes Namensfeld im Funktionsschalter. Neue Namen ändern nicht die Datenstruktur oder die Berechtigungs-IDs.

## 2. Rollen und Rechte vergeben

Öffne [Rollen und Rechte](../user_roles/roles_rights.md). Wähle eine bestehende Rolle oder lege eine Rolle für die Pflege der Initiativen an. Weise sie anschließend den zuständigen Personen zu; siehe [Rollen verteilen](../user_roles/distribute_roles.md).

| Recht | Wirkung |
| --- | --- |
| Initiativen ansehen (`initiatives.view`) | Zugang zu Übersichten, Detailseiten und Statistik der Initiativen und Objectives. |
| Initiativen bearbeiten (`initiatives.edit`) | Initiativen und Objectives anlegen, bearbeiten, archivieren und wiederherstellen; beinhaltet den Zugang zu ihren Seiten. |
| Außerhalb der Laufzeit zuordnen (`initiatives.select-outside-period`) | Bei Aktivitäten über **Alle anzeigen** auch Einträge außerhalb der regulären Datumsauswahl zuordnen. |

Die angezeigten Namen folgen eurer Übersetzung. Die Rechte gelten für Initiativen und Objectives gemeinsam. Das Bearbeitungsrecht ist nicht auf eigene Mitgliedschaften oder einzelne Initiativen beschränkt. Gib es deshalb gezielt an die Personen, die diese Daten pflegen sollen.

Die Konfiguration des Formulars benötigt Zugang zum Admin-Bereich (`admin.see`). Die Bearbeitung verknüpfter Aktivitäten oder Projekte richtet sich weiterhin nach den jeweiligen Inhaltsrechten. Mitgliedschaft allein erteilt kein Bearbeitungsrecht.

## 3. Formular der Initiativen konfigurieren

Öffne **Datenmodell & Inhalte → Datenmodell** (`/admin/data-model`). Dort findest du die Feldübersicht und den Link zum Form-Builder für Initiativen. Alternativ führt **Formular konfigurieren** auf der Initiativenübersicht direkt dorthin (`/admin/initiatives/fields`).

Im Form-Builder kannst du verfügbare Module und [Custom Fields](../custom_data/custom-fields.md) ergänzen, ihre Reihenfolge und Eigenschaften ändern und Überschriften zur Gliederung einsetzen. Das gespeicherte Formular gilt für alle Initiativen der Instanz und bestimmt auch die Angaben der Detailansicht.

**Name und Laufzeit sind Systemfelder.** Sie bleiben im Formular und können positioniert werden. Der Beginn der Initiative ist erforderlich; ein leeres Ende bedeutet eine offene Laufzeit. Für Name, Titel und Beschreibung stehen auch mehrsprachige Varianten zur Verfügung. Verwende jeweils die einfache oder die mehrsprachige Variante eines Feldes. Bei mehrsprachigen Pflichtfeldern genügt die Ausgangssprache.

### Personenmitgliedschaften

Wähle bei Bedarf eines dieser beiden Module:

| Modul | Verwendung |
| --- | --- |
| `persons` | Mitglieder ohne zusätzliche Rollenangabe. |
| `persons-role` | Mitglieder mit einer Rolle aus dem Vokabular **Personenrollen** (`persons-role`). |

Beide speichern dieselbe Personenzuordnung und sind Alternativen. Die Standardrollen sind **Member**, **Lead** und **Co-Lead**; ihre Beschriftung kann sprachabhängig abweichen. Über das [Vokabular](../custom_data/vocabulary.md) kannst du weitere Werte ergänzen. Explizite Mitgliedschaften können im Personenprofil unter den Zugehörigkeiten erscheinen. Aus der Mitwirkung an einer verknüpften Publikation wird keine Mitgliedschaft abgeleitet.

Wenn ihr keine direkten Mitgliedschaften benötigt, lasst das Personenmodul weg. Partner, potenzielle Förderer, regionale Bezüge und Strategieangaben ergänzt ihr über passende Module oder eigene Felder. Bereits gepflegte Organisationslisten sollten nicht gleichzeitig für zwei fachlich verschiedene Bedeutungen verwendet werden; nutzt dafür getrennte Custom Fields.

## 4. Objectives vorbereiten

Objectives benötigen keinen eigenen Form-Builder. Ihre festen Angaben sind Kennung, Titel, Beschreibung, Typ, Start- und Ablaufdatum. Nur der Titel ist als Texteingabe verpflichtend; die Kennung ist aktuell optional und wird nicht auf Eindeutigkeit geprüft. Vereinbart deshalb eine einheitliche Vergabe, beispielsweise `26-01`.

Der Typ wird über das Vokabular **Objective-Typen** (`objective-type`) gepflegt. Der Standardwert ist `objective`. Ergänzt bei Bedarf etwa wissenschaftliche, politische oder praktische Ziele. Jedes Objective gehört genau zu einer Initiative; Personen werden nur an der Initiative gepflegt.

## 5. Auswahl in anderen Formularen anbieten

Öffne den Form-Builder des gewünschten Aktivitätstyps, der Projektphase oder des anderen unterstützten Inhalts. Ergänze eines der folgenden Module und speichere:

| Modul | Bedeutung |
| --- | --- |
| `initiatives` | Direkte Zuordnung zu einer oder mehreren Initiativen. |
| `objectives` | Zuordnung zu einem oder mehreren Objectives. Deren Initiativen werden automatisch mitgespeichert. |

Die Module stehen für Aktivitäten, Projekte und Anträge, Events, Deadlines und Infrastrukturen bereit. Sie erscheinen nur in Formularen, in die ihr sie aufnehmt. Bei Projekten wird jede benötigte Phase separat konfiguriert. Ihr könnt die Auswahl als Pflicht- oder Kann-Feld definieren. Meist genügt eines der beiden Module; nur wenn beide Zuordnungsebenen gebraucht werden, nehmt ihr beide auf.

Eine Verknüpfung mit einem Projekt überträgt dessen Initiativen oder Objectives nicht automatisch auf eine Aktivität. Diese Zuordnung erfolgt im Aktivitätsformular selbst.

Bei Aktivitäten passt sich die reguläre Auswahl an deren Datum an. Das zusätzliche Recht für **Alle anzeigen** ermöglicht Ausnahmen. Die Datumseinschränkung ist derzeit auf Aktivitäten ausgelegt; sie gilt nicht entsprechend für die anderen Formulararten.

!!! tip "Pflichtauswahl mit begründeten Ausnahmen"
    Wenn jede relevante Aktivität bewusst zugeordnet werden soll, könnt ihr eine Auffanginitiative wie „Sonstiges“ anlegen. Bei reiner Objective-Auswahl braucht sie auch ein passendes Objective. Verzichtet für inhaltlich nicht betroffene Aktivitätstypen auf das Modul.

## 6. Konfiguration ausprobieren

Teste mit einer normalen Nutzerrolle und einer Pflegerolle: Initiative und zwei Objectives anlegen, eine Aktivität zuordnen, deren Datum ändern und ein vergangenes Ergebnis nachtragen. Prüfe anschließend Detailseiten, Personenmitgliedschaften, erweiterte Suche und [Statistik und Exporte](../../users/initiatives/statistics.md).

Erklärungen zum [Anlegen und Archivieren](../../users/initiatives/manage.md) sowie zur [Zuordnung von Inhalten](../../users/initiatives/linking.md) findest du im Nutzerbereich. Für externe Auswertungen gibt es die [OSIRIS-API](../../api/initiatives.md). Öffentliche Portfolio-Oberflächen sind eine getrennte Erweiterung.
