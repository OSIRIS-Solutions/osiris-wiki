# Windows

## Installation unter Windows (for Dummies)

> Diese Anleitung ist nicht offiziell getestet und könnte in Zukunft nicht mehr funktionieren. Sie dient lediglich als Hilfestellung für die Installation von OSIRIS unter Windows.

Vielen Dank **Robin Lange** vom IWH, der uns folgende Installationsanleitung für Windows zur Verfügung gestellt hat. Sie ist nach seiner eigenen Aussage "for Dummies", also für jemanden geschrieben, der vorher noch nie mit Webservern zu tun hatte.



## Benötigte Programme

* [XAMPP](https://www.apachefriends.org/download.html)
* [MongoDB Datenbank](https://www.mongodb.com/try/download/community)
* [MongoDB PHP Driver](https://pecl.php.net/package/mongodb)
* [Git for Windows](https://git-scm.com/download/win)
* [Composer](https://getcomposer.org/download/)



## Anleitung


1. [XAMPP](https://www.apachefriends.org/download.html) downloaden und installieren. Hierbei ist wichtig, dass man eine Version mit PHP 8.1.x wählt (PHP 8.2 wird noch nicht von dem Mongo PHP Driver für Windows unterstützt). Wenn man möchte, dass die Webseite später für alle im Netzwerk/Wlan erreichbar ist, muss man am Ende der Installation den Zugriff aufs Netzwerk bei der Windows Firewall erlauben.


Wenn XAMPP installiert ist, das Programm starten und bei Apache auf „Start“ klicken. Anschließend im Browser [localhost](http://localhost) eingeben. Jetzt sollte die Standardseite von Apache angezeigt werden.
2. Die [MongoDB Datenbank](https://www.mongodb.com/try/download/community) als .msi runterladen und installieren. Die aktuellste Version (6.0) ist kompatibel mit OSIRIS. Im Installationsmenü alle Voreinstellungen so lassen.
3. Den [MongoDB PHP Driver](https://pecl.php.net/package/mongodb) runterladen. Dazu in der Liste zur Version 1.13.0 scrollen und auf das blaue Windows Kästchen mit „DLL“ klicken. Unten in der Liste die Version PHP 8.1 Thread Safe für euer System auswählen (in der Regel x64). Die DLL Datei muss anschließend in den folgenden Ordner in eurem Xampp Verzeichnis kopiert werden: …\xampp\php\ext


Wenn das geglückt ist, muss man den Treiber noch aktivieren. Dazu sucht man in dem Verzeichnis …\xampp\php nach der Datei php.ini (Achtung: wenn man sich unter Windows die Dateiendungen nicht anzeigen lässt, heißt die Datei evtl. einfach nur „php“. Da es mehrere solcher Dateien gibt, macht es Sinn, sich die Dateiendungen anzeigen zu lassen).
Die Datei php.ini mit dem Editor der Wahl öffnen (zum Beispiel Notepad++) und die Zeile „extension=php\_mongodb.dll“ hinzufügen. Der Übersicht halber sollte man die Zeile dort einfügen, wo auch die anderen extensions gelistet werden (ungefähr Zeile 950). Das ist aber nicht zwingend notwendig.
4. [Git for Windows](https://git-scm.com/download/win) installieren. Im Installationsmenü alle Voreinstellungen so lassen. Anschließend den Ordner …\xampp\htdocs öffnen.


In diesen Ordner wird die Webseite geladen. Dazu erst einmal alle Dateien, die sich in dem Ordner befinden, löschen. Dann Rechtsklick in das leere Verzeichnis und auf „Git Bash Here“ klicken. Es öffnet sich ein Kommandofenster. Da gibt man den folgenden Befehl ein:
`git clone https://github.com/JKoblitz/osiris.git`
Aufpassen! Den Code kann man hier nicht mit Strg+V einfügen. Wenn man das trotzdem versucht, bildet sich ein „unsichtbares Zeichen“ und der anschließende Code funktioniert nicht mehr. Man kann aber mit Linksklick den Code einfügen.
Jetzt sollten alle Dateien aus dem Git Repository in das Verzeichnis kopiert werden.
5. Zu guter Letzt installiert man [Composer](https://getcomposer.org/download/) mit der .exe Datei. Alle Voreinstellungen können so belassen werden. Anschließend die Windows Eingabeaufforderung (cmd) öffnen, indem man zum Beispiel in die Suchzeile „cmd“ eingibt.


In dem Terminal muss man jetzt in das Xampp Verzeichnis der Webseite navigieren. Dies tut man mit dem Befehl `cd`. Wenn Xampp standardmäßig installiert ist lautet der Befehl
`cd C:\xampp\htdocs`
Wenn man sich jetzt im Verzeichnis befindet, gibt man `Composer update` ein. Wenn alles funktioniert, lädt Composer die fehlenden libraries nach.
6. Wenn nicht bereits geschehen, XAMPP starten und bei Apache auf „Start“ bzw. „Restart“ klicken. In den Browser [localhost](http://localhost) eingeben und die Webseite sollte erscheinen.
7. In der Config Datei können noch weitere Einstellungen vorgenommen werden (wie Administrator, ldap, Auth, …). Siehe dazu die den Abschnitt [OSIRIS konfigurieren](#config).


