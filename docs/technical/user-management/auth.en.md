---
title: Auth-Addon
tags:
  - User management
  - authentication
---


# User administration with Auth-Addon

<!-- md:version 1.0.0 -->
<!-- md:feature -->

To use OSIRIS' own user authentication, only the following setting needs to be adjusted in `CONFIG.php`:

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'AUTH');
```

User authentication now takes place via the OSIRIS add-on. LDAP configuration is therefore not required.


## Migration of the user administration

Both LDAP and the Auth add-on take care of the **authentication** of users. With LDAP, basic user data is also synchronised from the Active Directory; with Auth, this must be created by the user during registration. **However, the actual user data is always stored in OSIRIS.
This means that it is easy to migrate from Auth to LDAP at a later date. It is only necessary to ensure that the **user names are identical to those in LDAP**.
This means that the system still knows later which user belongs to which account.
