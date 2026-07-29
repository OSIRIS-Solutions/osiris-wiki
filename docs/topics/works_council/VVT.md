# Verzeichnis von Verarbeitungstätigkeiten (VVT)


## Betriebsvereinbarung und VVT: zwei verschiedene Aufgaben

### Die Betriebs- oder Dienstvereinbarung

Die Betriebsvereinbarung ist eine kollektive Regelung zwischen Arbeitgeber und Betriebsrat. Sie kann insbesondere festlegen:

- für welche Zwecke OSIRIS eingesetzt werden darf;
- welche Nutzungen ausdrücklich ausgeschlossen sind;
- welche Beschäftigten OSIRIS nutzen müssen oder freiwillig nutzen können;
- welche Daten intern oder extern sichtbar sind;
- wer Daten prüfen, korrigieren, freigeben oder exportieren darf;
- welche Rechte Beschäftigte und Betriebsrat haben;
- wie Änderungen am System bewertet und abgestimmt werden;
- welche Folgen Verstöße haben.

Eine Kollektivvereinbarung kann nach § 26 Abs. 4 BDSG eine Grundlage für die Verarbeitung von Beschäftigtendaten sein. Sie muss dabei die Anforderungen der DSGVO einhalten, insbesondere Erforderlichkeit, Transparenz, Zweckbindung und angemessene Schutzmaßnahmen.

### Das Verzeichnis von Verarbeitungstätigkeiten

Das VVT ist die datenschutzrechtliche Dokumentation der tatsächlichen Verarbeitung. Es wird vom Verantwortlichen geführt und muss auf Anfrage der Aufsichtsbehörde bereitgestellt werden. Es ist weder ein Vertrag mit den Beschäftigten noch ein Ersatz für Mitbestimmung.

Ein VVT-Eintrag beantwortet insbesondere:

- Wer ist für die Verarbeitung verantwortlich?
- Welche Zwecke werden verfolgt?
- Welche Personengruppen sind betroffen?
- Welche Datenkategorien werden verarbeitet?
- An wen werden Daten übermittelt?
- Gibt es Übermittlungen in Drittländer?
- Welche Löschfristen sind vorgesehen?
- Welche technischen und organisatorischen Schutzmaßnahmen bestehen?

### Wie die Dokumente zusammenwirken

| Beispiel | Betriebsvereinbarung | VVT | Ergänzendes Konzept |
|---|---|---|---|
| Öffentliche Forschungsprofile | regelt Zulässigkeit, Freiwilligkeit und Grenzen | dokumentiert Zweck, Daten, Empfänger und Rechtsgrundlage | Sichtbarkeits- und Veröffentlichungskonzept |
| Berichte für Zuwendungsgebende | legt zulässige Berichtszwecke fest | dokumentiert Verarbeitung und Empfänger | Berichtskatalog |
| Rollen und Zugriffe | definiert Grundsätze und Zustimmungsschwellen | nennt Empfängerkategorien und Schutzmaßnahmen | detaillierte Berechtigungsmatrix |
| Ausscheiden einer Person | legt Schutz- und Verwendungsgrundsätze fest | dokumentiert Fristen und Kategorien | Lösch- und Archivierungskonzept |
| Neue Schnittstelle | regelt Änderungsverfahren | wird bei geänderter Verarbeitung aktualisiert | Schnittstellen- und Sicherheitsdokumentation |

!!! danger "Das VVT darf keine Hintertür sein"

    Eine mitbestimmungspflichtige Änderung wird nicht dadurch zulässig, dass sie lediglich im VVT dokumentiert wird. Dokumentation und Beteiligung sind zwei unterschiedliche Pflichten.






### Mögliche Struktur des OSIRIS-VVT

| VVT-Feld | Leitfragen für OSIRIS |
|---|---|
| Bezeichnung | Ist der Prozess auch ohne Produktnamen verständlich beschrieben? |
| Zwecke | Sind Dokumentation, Berichtswesen, Planung und Veröffentlichung getrennt beschrieben? |
| Betroffene | Beschäftigte, ehemalige Beschäftigte, Bewerbende, Gäste, externe Mitwirkende? |
| Stammdaten | Welche Daten kommen aus Identitäts- oder Personalsystemen? |
| Aktivitätsdaten | Welche Publikationen, Projekte und weiteren Aktivitäten werden verarbeitet? |
| Freiwillige Daten | Foto, Biografie, Identifier, Expertise, soziale Profile? |
| Verfahrensdaten | Status, Korrekturen, Freigaben, Protokolle? |
| Quellen | Selbsteingabe, Import, Verzeichnisdienst, externe Datenquellen? |
| Empfänger | interne Rollen, öffentliche Website, Zuwendungsgebende, Kooperationspartner? |
| Schnittstellen | Verzeichnisdienst, Website, Repositorium, Berichtssysteme, externe Register? |
| Rechtsgrundlagen | Welche Grundlage gilt für welchen Zweck und welche Personengruppe? |
| Aufbewahrung | Welche Frist oder welches Ereignis gilt je Datenkategorie und Zweck? |
| Schutzmaßnahmen | Rollen, Authentifizierung, Verschlüsselung, Protokollierung, Backup, Tests? |
| Drittlandbezug | Werden externe Dienste, Identifier oder Plattformen eingebunden? |
| Verantwortlichkeit | Wer hält Eintrag und zugehörige Konzepte aktuell? |

!!! warning "Rechtsgrundlagen nicht pauschal übernehmen"

    Art. 89 DSGVO enthält Schutzvorgaben und besondere Regeln für wissenschaftliche oder statistische Zwecke, ist aber nicht ohne Weiteres eine eigenständige Rechtsgrundlage. Auch Art. 6 Abs. 1 lit. f DSGVO passt nicht automatisch zu jeder Forschungseinrichtung oder jedem Zweck. Die Rechtsgrundlagen sollten je Verarbeitung und Rechtsform durch die Datenschutzbeauftragten beziehungsweise die Rechtsberatung geprüft werden.

### Wann muss das VVT aktualisiert werden?

Mindestens bei Änderungen an:

- Zwecken;
- Daten- oder Personenkategorien;
- Quellen und Schnittstellen;
- internen oder externen Empfängern;
- öffentlicher Sichtbarkeit;
- Rollen mit personenbezogenem Zugriff;
- Hosting oder Auftragsverarbeitung;
- Drittlandübermittlungen;
- Löschfristen;
- wesentlichen Schutzmaßnahmen.

Das VVT sollte außerdem regelmäßig gegen die tatsächliche Konfiguration geprüft werden. Ein formal aktuelles Dokument genügt nicht, wenn Rollen oder Datenflüsse technisch anders umgesetzt sind.

## Ergänzende Konzepte

### Rollen- und Berechtigungskonzept

Eine praktische Berechtigungsmatrix kann so aufgebaut sein:

| Funktion/Datenbereich | Eigene Daten | Beteiligte Aktivitäten | Organisationseinheit | Alle Daten | Export | Administration |
|---|---:|---:|---:|---:|---:|---:|
| Standardnutzung | definieren | definieren | definieren | nein | eigene Daten | nein |
| Fachliche Prüfung | nach Bedarf | nach Bedarf | nach Bedarf | nein | begrenzt | nein |
| Bibliographische Prüfung | nach Bedarf | Publikationen | gegebenenfalls | nur Publikationen | begrenzt | nein |
| Berichtswesen | soweit erforderlich | soweit erforderlich | definierte Berichte | nur bei Bedarf | definierte Exporte | nein |
| Fachadministration | Support nach Anlass | Support nach Anlass | nach Aufgabe | besonders begründen | besonders begründen | fachlich |
| Technische Administration | nur im Betriebsfall | nur im Betriebsfall | nur im Betriebsfall | technisch möglich, fachlich unzulässig | nein | technisch |

Jede Berechtigung sollte mit einem fachlichen Zweck begründet werden. „Wird vielleicht benötigt“ ist keine ausreichende Rollenbeschreibung.

### Lösch- und Aufbewahrungskonzept

Für jede Kategorie sollten folgende Fragen beantwortet werden:

| Frage | Beispielhafte Ausprägung |
|---|---|
| Was? | Stammdaten, freiwillige Profildaten, Aktivitäten, Projekte, Protokolle, Exporte |
| Warum aufbewahren? | Berichtspflicht, Nachweis, Veröffentlichung, Sicherheit |
| Ab wann läuft die Frist? | Erfassung, Projektende, Veröffentlichung, Ausscheiden |
| Wie lange? | konkrete Frist oder prüfbares Kriterium |
| Was geschieht danach? | löschen, anonymisieren, sperren oder archivieren |
| Wer entscheidet? | fachlich verantwortliche Stelle |
| Wer setzt um? | Fachadministration oder IT |
| Wie wird nachgewiesen? | Löschprotokoll oder regelmäßiger Kontrollbericht |
| Welche Kopien bestehen? | Backups, Exporte, Website, Drittsysteme |

!!! note "Backups mitdenken"

    Ein Löschkonzept sollte erklären, wie gelöschte Daten in Sicherungen behandelt werden. Üblich ist nicht zwingend die nachträgliche Änderung jedes Backups, sondern ein geschützter, zeitlich begrenzter Sicherungszyklus und die Regel, dass zurückgespielte Daten erneut dem Löschlauf unterzogen werden.

### Berichtskatalog

Neben regelmäßigen Berichten sollten auch spontane Auskünfte erfasst werden. „Anlassbezogene Berichte“ dürfen nicht zu einer unbegrenzten Generalklausel werden.

Prüffragen:

- Ist der Zweck bereits vereinbart?
- Werden personenbezogene Details wirklich benötigt?
- Kann aggregiert oder pseudonymisiert werden?
- Ist die Gruppe groß genug, um indirekte Identifizierung zu vermeiden?
- Wer darf den Bericht erstellen?
- Wer erhält ihn?
- Wie lange wird die Exportdatei gespeichert?
- Darf sie weitergegeben oder mit anderen Daten verbunden werden?

### Technisches Betriebskonzept

Mindestens zu dokumentieren:

- Verantwortlichkeit für Anwendung, Server und Datenbank;
- Produktiv-, Test- und Entwicklungsumgebungen;
- Authentifizierung und Anbindung an den Verzeichnisdienst;
- Rollenvergabe und Austrittsprozess;
- Verschlüsselung;
- Protokollierung;
- Backup-Zeitplan und Wiederherstellungstests;
- Sicherheitsupdates und Schwachstellenbehandlung;
- Monitoring ohne unzulässige Nutzungsanalyse;
- Supportzugriffe;
- Verfahren bei Störungen und Datenschutzvorfällen.

## Datenschutz-Folgenabschätzung prüfen

Ob eine Datenschutz-Folgenabschätzung erforderlich ist, muss anhand der konkreten Verarbeitung geprüft werden. Relevante Risikofaktoren können sein:

- umfangreiche Zusammenführung von Beschäftigten- und Leistungsdaten;
- systematische personenbezogene Auswertung;
- neue technische oder organisatorische Verknüpfungen;
- große Zahl betroffener Personen;
- besondere Kategorien personenbezogener Daten;
- schwer kontrollierbare öffentliche Veröffentlichung;
- Profilbildung oder automatisierte Bewertung.

!!! note "Prüfung dokumentieren"

    Auch wenn keine Datenschutz-Folgenabschätzung durchgeführt wird, sollte die Vorprüfung mit ihrer Begründung dokumentiert werden.

## Gute UX ist eine Schutzmaßnahme

Datenschutz und Mitbestimmung werden nicht nur durch Vertragstexte umgesetzt. Die Benutzeroberfläche entscheidet mit darüber, ob Beschäftigte ihre Rechte verstehen und wahrnehmen können.

Empfehlungen:

- Pflichtfelder und freiwillige Felder eindeutig kennzeichnen.
- Vor dem Speichern zeigen, wer einen Eintrag sehen kann.
- Öffentliche Sichtbarkeit nicht hinter allgemeinen Profileinstellungen verstecken.
- Eine Vorschau des öffentlichen Profils anbieten.
- Korrektur- und Freigabestatus verständlich erklären.
- Änderungen durch Prüfende sichtbar machen.
- Kontakt- und Beschwerdewege direkt im System verlinken.
- Keine manipulativen Voreinstellungen für freiwillige Angaben verwenden.
- Exporte mit Zweck und Empfänger beschriften.
- Bei gesperrten Daten einen verständlichen Korrekturweg anbieten.
