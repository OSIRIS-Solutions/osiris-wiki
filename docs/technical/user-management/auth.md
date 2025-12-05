---
title: Auth-Addon
tags:
  - Nutzerverwaltung
  - Authentifizierung
---


# Nutzerverwaltung mit Auth-Addon

<!-- md:version 1.0.0 --> 
<!-- md:feature -->

Um die OSIRIS-eigene Nutzerauthentifizierung zu nutzen, muss nur folgende Einstellung in der `CONFIG.php` angepasst werden:

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'AUTH');
```

Die Nutzer-Authentifizierung findet jetzt über das OSIRIS-Addon statt. LDAP-Konfiguration muss demnach nicht vorgenommen werden.


## Migration der Nutzerverwaltung

Sowohl LDAP als auch das Auth-Addon kümmern sich um die **Authentifizierung** der Nutzer. Bei LDAP werden grundlegende Nutzerdaten ebenfalls aus dem Active Directory synchronisiert, beim Auth müssen diese vom Nutzer bei der Registrierung angelegt werden. **Die eigentlichen Nutzerdaten werden aber immer in OSIRIS gespeichert.**
Das führt dazu, dass sich im Nachhinein leicht von Auth auf LDAP migriert werden kann. Dabei muss nur beachtet werden, dass die **Nutzernamen mit denen im LDAP identisch sind**.
Dadurch weiß das System später noch, welcher Nutzer zu welchem Account gehört.
