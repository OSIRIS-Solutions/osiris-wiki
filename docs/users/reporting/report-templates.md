---
status: false
---

# Berichte-Vorlagen

Die Funktion der Berichte-Vorlagen ermöglicht es dir, individuelle Berichte passend für die Bedürfnisse deines Institus zu erstellen. Wähle hierfür im Admin-Bereich den **Berichte-Vorlagen** Button, gebe deiner Vorlage einen Namen und klicke auf **erstellen**. Du befindest dich jetzt im **Berichtseditor** der es dir ermöglicht, deinen Bericht durch verschiedene Bausteine aufzubauen. Du kannst deinem Bericht hier noch eine Beschreibung geben, den Startmonat und die Dauer in Monaten angeben. Dies kann später durch die Person, die den Bericht erstellt, angepasst werden. Alle Änderungen müssen **gespeichert** werden, damit sie für die fertige Vorschau (oben rechts) übernommen werden.

![Berichtseditor](screenshots/Berichtseditor_start.png)
///caption
Der Berichtseditor mit deinem gewählten Titel und Platz für eine Beschreibung
///

## Hinzufügen von Bausteinen

Jedes Element in deinem Bericht ist ein individueller Baustein, das heißt, für jede Überschrift, jeden Zwischentext und jede Auflistung von Aktivitäten muss ein Baustein hinzugefügt werden. Es gibt verschiedene Arten von Bausteinen, die durch einen Klick auf **Hinzufügen** ausgewählt werden können. Die einzelnen Bausteine können nach dem Erstellen in der Reihenfolge verschoben werden. Du kannst dir mit einem Klick auf den Button **Vorschau** oben rechts immer ansehen, wie dein Bericht aussehen würde. Gehe im Browser einfach zurück, um wieder zum Editor zu gelangen. 

### Text-Bausteine

Text-Bausteine benötigst du um Überschriften und Textelemente in deinem Bericht zu erstellen. Für jede Überschrift und Textabschnitt musst du einen individuellen Baustein erstellen, da die Schrift für den gesamten Text im Feld festgelegt ist. Du erstellst also beispielsweise einen Text-Baustein mit der Schrift *Überschrift 1* und einen mit *Absatz*.

![Text-Bausteine](screenshots/textbaustein.png)
///caption
Erstelle zwei Text-Bausteine um deinem Bericht unterschiedliche Arten von Text hinzuzufügen.
///

![Vorschau Text-Bausteine](screenshots/textbaustein_vorschau.png)
///caption
So sieht dein Bericht aktuell aus.
///

### Aktivitäten-Bausteine

Um alle Aktivitäten, die im oben gewählten Zeitraum in OSIRIS hinzugefügt wurden, im Bericht aufzulisten, fügst du einen Aktivitäten-Baustein hinzu. Hier kannst du mithilfe der IDs der Aktivitäten die aufzulistenden Beiträge filtern, indem du folgenden MongoDB Befehl in das Feld eingibst:

```bash
{"$and":[{"type":"publication"}]}
```

Durch diesen Befehl würdest du zum Beispiel alle Aktivitäten, die mit der ID *publication* versehen sind, auflisten.  
Möchtest du zudem alle Poster auflisten, erstellst du einen neuen Aktivitäts-Baustein und gibst folgenden Befehl ein:

```bash
{"$and":[{"type":"poster"}]}
```

Wenn du komplexere Filteroptionen nutzen möchtest, nimmst du am besten die [erweiterte Suche](https://wiki.osiris-app.de/users/activities/advanced-search/) zur Hilfe. Hier kannst du verschiedene Filter anwenden und dir den entsprechenden MongoDB Befehl mit einem Klick auf **Zeige Filter** rüberkopieren. Als Beispiel werden wir hier nach dem Vornamen *Martinique* filtern.

```bash
{"$and":[{"authors.first":"Martinique"}]}
```
### Tabellen-Bausteine

Du kannst eine Tabelle in deinen Bericht einfügen, die Werte über bestimmte Kriterien aggregiert. Auch hier kannst du die Aktivitäten filtern, als Beispiel nehmen wir den gleichen wie bei den Aktivitäten.  
Nehmen wir an wir möchten eine Tabelle mit der Anzahl der unterschiedlichen Aktivitäten über die letzten Jahre aufgelistet haben. Dafür wählen wir als **Erste Aggregation** (y-Achse) *type* und als **Zweite Aggregation** (x-Achse) *year*.

![table type year](screenshots/table_type_year_code.png)

---

![table type year outcome](screenshots/table_type_year.png)

///caption
Die Einstellungen für einen Tabellen-Baustein aggregiert nach "type" über Jahre und das Ergebnis
///

Man kann hier auch auswählen, dass die Angaben auf den oben gewählten Zeitraum beschränkt werden. Hier bietet sich beispielsweise eine Aggregation über die Monate an.

![table type month](screenshots/table_type_month.png)

---

![table type month outcome](screenshots/table_type_month_outcome.png)

///caption
Die Einstellungen für einen Tabellen-Baustein aggregiert nach "type" über Monate für den gewählten Zeitraum und das Ergebnis
///

### Linie-Bausteine

Um die unterschiedlichen Einträge deutlicher in deinem Bericht voneinander zu trennen, kannst du mit dem Linien-Bausteine eine Trennlinie hinzufügen.

![Mehrere Bausteine](screenshots/report_complete.png)
///caption
Hier siehst du die Anordnung unterschiedlicher Bausteine für deinen Bericht
///

![Mehrere Bausteine Vorschau](screenshots/report_complete_outcome.png)
///caption
So sieht dein Bericht mit den vielen Bausteinen aus
///

Wenn du die Vorlage so speicherst, kann in Zukunft jeder Nutzende, der dazu berechtigt ist, einen Bericht dieser Art erstellen. Unter Export&Import &#8594 Berichte sieht deine Vorlage  so aus:

![Bericht-Vorlage](screenshots/report_useransicht.png)
///caption
Berichte-Vorlage zum Ausfüllen für den Nutzenden
///

Hier können Start-Jahr, -Monat und die Dauer in Monaten, die der Bericht umfassen soll, eingestellt werden. Der fertige Bericht, der deiner Vorschau im Editor ähneln sollte, kann entweder als MS Word Dokument oder als HTML exportiert werden.
