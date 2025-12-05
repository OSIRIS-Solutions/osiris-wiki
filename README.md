# OSIRIS Documentation

Dies ist die offizielle Dokumentation für [OSIRIS](https://osiris-app.de), dem Open Source Forschungsinformationssystem.


## 🛠 Mitmachen – Beiträge zur OSIRIS-Dokumentation

Du möchtest helfen, die OSIRIS-Dokumentation zu verbessern? Super!  
Ob kleinere Korrekturen oder ganz neue Inhalte – jeder Beitrag ist willkommen. So funktioniert’s:

### ✏️ Bestehende Seiten bearbeiten

1. Klicke auf eine Markdown-Datei im Ordner `docs/`, z. B. [`docs/users/index.md`](docs/users/index.md)
2. Klicke oben rechts auf das ✏️ Stiftsymbol („Edit this file“)
3. GitHub erstellt automatisch eine Kopie des Projekts (*Fork*) in deinem Account
   - Falls du noch kein GitHub-Konto hast, musst du dich zuerst registrieren
   - Du kannst auch direkt auf `Fork` klicken, um ein neues Repository zu erstellen
4. Mach deine Änderungen direkt im Editor
5. Scrolle nach unten und klicke auf `Commit changes`
   - Gib falls nötig eine kurze Beschreibung deiner Änderung ein
   - Klicke auf `Propose changes`
   - Du solltest jetzt auf die Seite `Comparing changes` weitergeleitet werden, wo du noch einmal deine Änderungen überprüfen kannst
6. Klicke auf `Create pull request`, um deine Änderung vorzuschlagen
   - Die Änderungen werden nicht sofort übernommen, sondern müssen von den Maintainer:innen geprüft werden
7. Fertig! Du kannst den Fortschritt deines Pull Requests auf GitHub verfolgen

🧠 **Hinweis für Einsteiger:innen:**  
Ein *Fork* ist eine eigene Kopie dieses Repositories in deinem GitHub-Konto. Änderungen werden dort gemacht und anschließend als *Pull Request* zurück zum Hauptprojekt vorgeschlagen. Du brauchst dafür keinen technischen Hintergrund – GitHub führt dich durch die Schritte.


### 🆕 Neue Seiten hinzufügen

1. Erstelle in deinem Fork (siehe oben) eine neue Datei im passenden Unterordner in `docs/`, z. B. `docs/admins/reporting.md`
2. Schreibe deinen Inhalt im Markdown-Format (`.md`)
3. Ergänze die Datei in der `mkdocs.yml` unter dem richtigen Abschnitt, z. B.:

```yaml
  - Admins:
      - Berichte erstellen: admins/reporting.md
```
4. Committe die Änderung (z. B. per Pull Request)



### 💡 Tipps für Beiträge
- Nutze Überschriften (##), Aufzählungen und Codeblöcke für bessere Lesbarkeit. Es sollte nur eine Überschrift der Ebene 1 (`#`) pro Seite geben, die den Titel der Seite angibt.
- Für Bilder: Speichere sie im Ordner docs/images/ und verwende relative Pfade, z. B.

```markdown
![Screenshot](/assets/images/setup.png)
```
- Verlinke auf andere Seiten im Wiki mit `[Linktext](ziel.md)`
- Bitte halte dich an den bestehenden Stil und die Struktur der Dokumentation
- Schreibe möglichst klar, knapp und verständlich – die Zielgruppe sind auch Nicht-Techniker:innen

### 📚 Markdown-Grundlagen

Markdown ist eine einfache Auszeichnungssprache, die für alle Dokumente in diesem Wiki verwendet wird. Hier sind einige nützliche Links, um Markdown zu lernen (auf englisch):
- [Grundlegender Markdown Guide](https://www.markdownguide.org/basic-syntax/)
- [MkDocs Material Reference für weitere Elemente wie Icons und Hinweisblöcke](https://squidfunk.github.io/mkdocs-material/reference/)


### 🙏 Danke!

Mit deinem Beitrag hilfst du, OSIRIS für alle besser nutzbar zu machen.
Bei Fragen oder Ideen melde dich gerne direkt bei uns: contact@osiris-solutions.de



## 💻 Das Wiki lokal installieren

Du kannst das Wiki auch lokal auf deinem Rechner installieren, um Änderungen direkt zu testen. Hier ist eine kurze Anleitung:

Zuerst musst du die Abhängigkeiten für die Grafikbibliotheken installieren, die von MkDocs Material benötigt werden. Finde die Anleitungen [hier](https://squidfunk.github.io/mkdocs-material/plugins/requirements/image-processing/#cairo-graphics-linux).


Danach lädst du dir das Repository herunter und installierst die Abhängigkeiten:

```bash
git clone https://github.com/OSIRIS-Solutions/osiris-wiki.git osiris-wiki

cd osiris-wiki

pip install mkdocs mkdocs-material mkdocs-git-revision-date-localized-plugin mkdocs-git-authors-plugin "mkdocs-material[imaging]"
pip install mkdocs-nav-weight
```

Du startest den lokalen Server mit:

```bash
mkdocs serve
```