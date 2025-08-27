# E-Mail-Einstellungen

<!-- md:version 1.5.0 -->

OSIRIS bietet eine einfache Möglichkeit, E-Mails für bestimmte Ereignisse zu versenden, wie z.B. Benachrichtigungen über neue Projekte oder Projektänderungen. 

!!! info "Hinweis"
    Die E-Mail-Funktionalität ist noch relativ neu und wird kontinuierlich erweitert. Im Moment ist sie auf die Benachrichtigung zu Projekten beschränkt. Weitere Funktionen sind in Planung, zum Beispiel ein Weekly/Monthly Digest, der alle Benachrichtigungen der letzten Woche/Monat zusammenfasst oder eine Erinnerung an das quartalsweise Reporting.

Es gibt grundsätzlich zwei Möglichkeiten, E-Mails zu versenden: über einen SMTP-Server oder über den lokalen Mailserver. Wir empfehlen die Verwendung eines SMTP-Servers, da dieser in der Regel zuverlässiger ist und mehr Funktionen bietet. Für Testzwecke könnt ihr aber auch den lokalen Mailserver verwenden, der in der Regel bereits auf dem Server installiert ist.

## Lokaler Mailserver

Falls ihr den lokalen Mailserver verwenden möchtet, solltet ihr zuerst sicherstellen, dass dieser korrekt installiert und konfiguriert ist. 

Ihr könnt dazu zum Beispiel Postfix oder Exim verwenden. Diese Mailserver sind in der Regel bereits auf den meisten Linux-Distributionen vorinstalliert und können einfach konfiguriert werden. Mehr Informationen zur Konfiguration findet ihr in der Dokumentation eurer Linux-Distribution oder auf den jeweiligen Webseiten der Mailserver.

Für einen lokalen Mailserver müsst ihr im Admin-Bereich unter Einstellungen &#8594 E-Mail keine weiteren Einstellungen vornehmen. OSIRIS verwendet dann automatisch den lokalen Mailserver, um E-Mails zu versenden. Ihr könntet lediglich die Absenderadresse anpassen, die für alle E-Mails verwendet wird. Diese ist standardmäßig auf `noreply@osiris-app.de` gesetzt. 

## SMTP-Server

Die E-Mail-Einstellungen findet ihr im Admin-Bereich unter Einstellungen &#8594 E-Mail. Hier könnt ihr die folgenden Einstellungen vornehmen:

- **SMTP-Server**: Der Hostname oder die IP-Adresse des SMTP-Servers, der für den Versand von E-Mails verwendet wird.
- **Port**: Der Port, der für die Verbindung zum SMTP-Server verwendet wird.
- **Benutzername**: Der Benutzername für die Authentifizierung am SMTP-Server.
- **Passwort**: Das Passwort für die Authentifizierung am SMTP-Server.
- **Absenderadresse**: Die E-Mail-Adresse, die als Absender für die Benachrichtigungen verwendet wird.
- **Sicherheitsprotokoll**: Das Sicherheitsprotokoll, das für die Verbindung zum SMTP-Server verwendet wird. Hier könnt ihr zwischen "TLS", "SSL" und "Keine" wählen.

## Email-Versand testen

Weiter unten im E-Mail-Einstellungsbereich findet ihr die Möglichkeit, eine Test-E-Mail zu versenden. Hier könnt ihr überprüfen, ob die E-Mail-Einstellungen korrekt sind und ob der Versand von E-Mails funktioniert. Dazu gebt ihr einfach eine E-Mail-Adresse ein, an die die Test-E-Mail gesendet werden soll, und klickt auf "Test-E-Mail senden". Wenn alles korrekt konfiguriert ist, sollte die E-Mail innerhalb weniger Minuten in eurem Posteingang landen.