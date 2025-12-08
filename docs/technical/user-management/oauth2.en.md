---
tags:
  - User administration
  - authentication
  - OAUTH
title: OAuth
---

# User administration with OAuth

OSIRIS supports user management via OAuth, but currently focusses on Microsoft SSO (Single Sign-On). This method enables seamless integration with Microsoft Azure Active Directory and provides an easy way to authenticate users without requiring separate credentials for OSIRIS. Further OAuth providers still need to be implemented, but the basic structure is already in place.

## User management with Microsoft SSO

OSIRIS can also be used with Microsoft SSO (Single Sign-On). Authentication is carried out via Microsoft Azure Active Directory. This is configured as follows:

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

