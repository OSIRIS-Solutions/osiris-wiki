---
tags:
  - Nutzerverwaltung
  - Authentifizierung
  - OAUTH
title: OAuth
---

# Nutzerverwaltung mit OAuth

OSIRIS unterstützt die Nutzerverwaltung über OAuth, zurzeit allerdings mit Fokus auf Microsoft-SSO (Single Sign-On). Diese Methode ermöglicht eine nahtlose Integration mit Microsoft Azure Active Directory und bietet eine einfache Möglichkeit, Nutzer zu authentifizieren, ohne dass sie separate Anmeldedaten für OSIRIS benötigen. Weitere OAuth-Provider müssen noch implementiert werden, aber die Grundstruktur ist bereits vorhanden.

## Nutzerverwaltung mit Microsoft-SSO

OSIRIS kann auch mit Microsoft-SSO (Single Sign-On) verwendet werden. Dabei wird die Authentifizierung über Microsoft Azure Active Directory durchgeführt. Das wird wie folgt konfiguriert:

```php
define('USER_MANAGEMENT', 'OAUTH');

// OAUTH user management:
define('OAUTH', 'Microsoft');
define('CLIENT_ID', 'DEINE_CLIENT_ID');
define('CLIENT_SECRET', 'DEIN_CLIENT_SECRET');
define('REDIRECT_URI', 'http://localhost/login-callback.php');
define('AUTHORITY', 'https://login.microsoftonline.com/common');
define('SCOPES', 'openid profile email User.Read');
```

