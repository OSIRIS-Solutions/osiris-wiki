---
tags:
  - Nutzerverwaltung
  - LDAP
  - Authentifizierung
  - OAUTH2
---


# Nutzerverwaltung

OSIRIS funktioniert zurzeit mit drei unterschiedlichen Möglichkeiten der Nutzerverwaltung, die über die `CONFIG.php` eingestellt werden können:


`LDAP` 

:   **LDAP**: über ein Active Directory werden die Nutzer zentral verwaltet und mittels [LDAP-Authentifizierung](https://www.redhat.com/de/topics/security/what-is-ldap-authentication) werden die Daten an OSIRIS weitergegeben. Falls es euch möglich ist, solltet ihr diese Einstellung verwenden, da sie einerseits Arbeit spart (Nutzer müssen nicht manuell angelegt werden) und andererseits auch mehr Sicherheit bietet. [Lies mehr zur LDAP-Schnittstelle](ldap.md).

`AUTH` 

:   **Auth-Addon**:
OSIRIS hat ein mitgeliefertes Addon zur Nutzer-Authentifizierung. Nutzer können sich dabei selbst einen Account erstellen. Dies kann aber in den Einstellungen (ab Version 1.5.0) deaktiviert werden, sodass Accounts nur noch zentral durch Admins angelegt werden können. [Lies mehr zum Auth-Addon](auth.md).


`OAUTH2` 

:   **Microsoft-SSO**:
OSIRIS kann auch mit Microsoft-SSO (Single Sign-On) verwendet werden. Dabei wird die Authentifizierung über Microsoft Azure Active Directory durchgeführt. Diese Option ist besonders nützlich, wenn ihr bereits Microsoft 365 oder andere Microsoft-Dienste nutzt und eine nahtlose Integration wünscht. In Zukunft wird es auch noch weitere OAuth-Provider geben, die OSIRIS unterstützen wird. [Lies mehr zur OAuth2-Schnittstelle](oauth2.md).

