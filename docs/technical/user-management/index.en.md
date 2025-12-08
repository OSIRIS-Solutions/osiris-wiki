---
tags:
  - User administration
  - LDAP
  - authentication
  - OAUTH2
---


# User administration

OSIRIS currently works with three different options for user administration, which can be set via `CONFIG.php`:


`LDAP`

:   **LDAP**: The users are managed centrally via an Active Directory and the data is forwarded to OSIRIS using [LDAP authentication](https://www.redhat.com/de/topics/security/what-is-ldap-authentication). If you can, you should use this setting as it saves work (users do not have to be created manually) and also offers more security. [Read more about the LDAP interface](ldap.md).

AUTH

:   **Auth-Addon**:
OSIRIS has a supplied add-on for user authentication. Users can create an account themselves. However, this can be deactivated in the settings (from version 1.5.0) so that accounts can only be created centrally by admins. [Read more about the Auth-Addon](auth.md).


`OAUTH2`

:   **Microsoft-SSO**:
OSIRIS can also be used with Microsoft SSO (Single Sign-On). Authentication is carried out via Microsoft Azure Active Directory. This option is particularly useful if you already use Microsoft 365 or other Microsoft services and want seamless integration. In the future, there will also be other OAuth providers that OSIRIS will support. [Read more about the OAuth2 interface](oauth2.md).

