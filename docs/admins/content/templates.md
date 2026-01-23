
# Template-Syntax für Zitationsstile in OSIRIS

Diese Dokumentation beschreibt die Template-Syntax, mit der in OSIRIS individuelle Zitationsstile definiert werden können.
Sie richtet sich an Personen, die den offiziellen Zitationsstil ihres Instituts (z. B. für Publikationen) möglichst genau nachbauen möchten.


## Grundprinzip

Ein Zitations-Template ist ein Text mit Platzhaltern, der zur Laufzeit mit den Daten einer Aktivität gefüllt wird.

**Beispiel:**

```
{authors}. {title}. %journal ({year})% {journal}, {year}.
```

OSIRIS ersetzt dabei:
- Feld-Platzhalter ({…}) durch konkrete Werte
- Konditionale Blöcke (%…%) nur dann, wenn bestimmte Felder existieren
- leere Klammern, doppelte Satzzeichen usw. automatisch

Bei neuen Aktivitäten wird ein Standard-Template verwendet, das aus Autoren, Titel und Jahr besteht. Dieses Template kann (und sollte) individuell angepasst werden. Besonders wenn deine Aktivitäten dir nur als leere Vorlage angezeigt werden, kannst du hier ansetzen.


## 1. Feld-Platzhalter {field}

Einfacher Feldzugriff

- `{title}`
- `{year}`
- `{journal}`

→ Wird durch den Inhalt des jeweiligen Feldes ersetzt.
- Existiert das Feld nicht oder ist leer → leerer String
- Arrays (z. B. Autorenlisten) werden automatisch mit ,  zusammengefügt
- MongoDB-Objekte werden intern korrekt aufgelöst

> Um herauszufinden, welche Felder zur Verfügung stehen, kann man den [Template Baukasten](tools.md) verwenden. Dort sind alle Felder mit Beispieldaten aufgelistet.
> 
> Für **benutzerdefinierte Felder** gilt: Der Feldname ist die ID, die du bei der Anlage des benutzerdefinierten Feldes vergeben hast.


## 2. Fallback / Priorität {field|fallback}

Mit `|` kann ein Fallback definiert werden.

### Literal als Fallback

```
{doi|"ohne DOI"}
```

- Wenn doi leer ist → Text ohne DOI
- Literale müssen in Anführungszeichen stehen (`"`)
- Wichtig: Dies gilt nur für das Fallback, nicht für konditionale Blöcke! Dort müssen Literale nicht in Anführungszeichen stehen und Felder müssen dafür in `{}` gesetzt werden. Der Grund für diese Inkonsistenz ist, dass eine verschachtelung mehrerer `{}` schwer lesbar und fehleranfällig wäre.

### Feld als Fallback

```
{doi|link}
```

- Wenn doi leer ist → Wert aus dem Feld link
- Wenn link kein bekanntes Feld wäre, würde es als Literal interpretiert und so ausgegeben werden.

### Regel zur Auflösung
1. Erstes Element wird immer als Feld interpretiert
2. Ist dieses leer:
   - Quoted → Literal
   - Unquoted + bekanntes Feld → Feldwert
   - sonst → Literal


## 3. Konditionale Blöcke %...%

Konditionale Blöcke werden nur ausgegeben, wenn bestimmte Felder existieren.

Syntax ist wie folgt:

```
%BEDINGUNG Text%
```



### Einzelnes Feld

```
%journal ({journal})%
```

→ Wird nur ausgegeben, wenn journal existiert und nicht leer ist. Dadurch werden leere Klammern vermieden. (Leere Klammern werden später automatisch entfernt, dies ist also nur ein Beispiel für die Verwendung von Konditionen.)


### UND-Bedingung (&)

```
%journal&year ({journal}, {year})%
```

→ Wird nur ausgegeben, wenn alle Felder existieren.

Typischer Use Case:
- Jahr nur, wenn auch Journal existiert
- Band/Heft nur, wenn Seiten existieren


### ODER-Bedingung (|)

```
%doi|link (Verfügbar unter: {doi|link})%
```

→ Wird ausgegeben, wenn mindestens eines der Felder existiert.


### Negation (!)

```
%!title (Kein Titel vorhanden)%
```

→ Wird nur ausgegeben, wenn title **nicht** existiert oder leer ist.


### Sonderfall: Platzhalterwert -

Ein Feld mit dem Wert `-` gilt als leer
→ Kondition schlägt fehl


## 4. Automatische Bereinigung

Nach der Ersetzung führt OSIRIS eine automatische Format-Bereinigung durch:

Entfernt werden u. a.:
- Leere Klammern: () oder []
- Überflüssige Leerzeichen
- Mehrfache Punkte oder Kommas
- Kommas vor Satzende
- Kommas nach `<br />` (Zeilenumbruch)

**Beispiel-Template:**
```
{authors}. {title}. ({journal}) {year}.
```

**Ohne Journal:** 
Müller J. Titel. 2024.

Kein händisches Abfangen nötig ✅


## 5. Die Sprache der Templates

Einige Felder können in mehreren Sprachen ausgegeben werden, z. B. der Monat oder die Art der Abschlussarbeit. Standardmäßig wird die Sprache der OSIRIS-Benutzeroberfläche verwendet, was allerdings bei unterschiedlichen Nutzern zu uneinheitlichen Zitaten führen kann. Deshalb empfehlen wirdt, die Sprache der Templates explizit festzulegen. Dies kannst du in den allgemeinen Einstellungen tun. 


## 6. Grenzen der aktuellen Syntax

Nicht unterstützt wird bisher:
- Verschachtelte Konditionen
- Mathematische Vergleiche
- Explizite else-Zweige

➡️ Ziel ist Nachvollziehbarkeit und Wartbarkeit, nicht Turing-Vollständigkeit.

