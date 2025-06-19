
## Installieren als Docker-Container

> Vielen Dank an **Paul C. Gaida**, der eine Docker-Umgebung für OSIRIS erstellt hat.
 
## Voraussetzungen

Um OSIRIS in einem Docker-Container zu installieren, benötigt ihr Docker auf dem Host-System. Außerdem müssen die OSIRIS-Quellcodes im aktuellen Verzeichnis liegen. Hier solltet ihr auch wie [weiter unten in Grundlegende Einstellungen beschrieben](../configure/index.md) die `CONFIG.php`-Datei anpassen.
 


## Docker-Container erstellen und starten

Um OSIRIS in einem Docker-Container zu installieren, müsst ihr die Docker-Umgebung mit dem folgenden Befehl erstellen:
 


**Für eine Entwicklungsumgebung:**

```bash
docker-compose -f docker-compose.dev.yml up -d
```

Danach könnt ihr OSIRIS im Browser unter `http://localhost:8080` erreichen (oder unter der Adresse des Servers, auf dem Docker läuft).
 

**Für eine Produktionsumgebung**

```bash
docker-compose -f docker-compose.prod.yml up -d
```

