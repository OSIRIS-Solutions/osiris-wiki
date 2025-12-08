---
title: LDAP
tags:
  - User administration
  - LDAP
  - authentication
---

# User administration with LDAP

<!-- md:version 1.0.0 -->
<!-- md:feature -->

OSIRIS currently works preferably with [LDAP authentication](https://www.redhat.com/de/topics/security/what-is-ldap-authentication), which is used by most institutions for user administration.

The LDAP settings must also be specified in the configuration. If you do not know which settings you need to make yourself, it is best to ask your IT department.

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'LDAP');

define("LDAP_IP", "100.10.100.0");
define("LDAP_PORT", 389);
define("LDAP_USER", "osiris");
define("LDAP_DOMAIN", "@domain.local");
define("LDAP_PASSWORD", "ldap_password");
define("LDAP_BASEDN", "OU=Users,OU=DSMZ,DC=dsmz,DC=local");
```

## Filter users:inside


<!-- md:version 1.3.6 -->

You can also specify which users should be synchronised from the LDAP server. To do this, you can set the `LDAP_FILTER` parameter in `CONFIG.php`. For example:

```php title="CONFIG.php"
define("LDAP_FILTER", "(objectClass=person)");
```

This filter ensures that only users with the object class `person` are synchronised. You can customise the filter to use other criteria that apply to your users.

Here are a few examples of frequently used filters:

```php title="CONFIG.php"
// Only users with the role "Employee"
define("LDAP_FILTER", "(role=employee)");
// Only users who work in the "Research" department
define("LDAP_FILTER", "(department=research)");
// Only users who are in the "Employees" OU
define("LDAP_FILTER", "(ou=employees)");
// Only users who are members of a specific group
define("LDAP_FILTER", "(memberOf=CN=Research,OU=Groups,DC=example,DC=com)");
```

You can also combine several filters to create more complex queries. For example:

```php title="CONFIG.php"
define("LDAP_FILTER", "(&(objectClass=person)(role=employee)(department=research))");
```

You can find more information on LDAP filters in the [LDAP documentation](https://ldap.com/ldap-filters/).


## Possible pitfalls

Problems can occur if the LDAP password contains a character that is interpreted as code. This is the case, for example, if the password contains a dollar sign ($). In this case, the character must be escaped with a backslash (\) or the string must be set with single inverted commas. The password would then look like this: `define("LDAP_PASSWORD", "\$uperSecret");` or `define("LDAP_PASSWORD", '$uperSecret');`.

For the LDAP domain, either a domain with `@` or a base DN must be specified, for which the user name must then be specified with `%s`. This then looks like this:
`define("LDAP_DOMAIN", "uid=%s,ou=people,dc=strong,dc=com");`


## Synchronisation of the user attributes

<!-- md:version 1.4.1 -->

This feature is only available for synchronisation with LDAP.

In the admin area, you can define attributes that are to be synchronised via LDAP in the general settings under the "Employees" tab. Here you enter the exact name for each field that is stored in the LDAP server. If you click on "Save and show preview", OSIRIS will show you a table with all users and you can check whether the attributes are being read correctly.

<!-- md:version 1.5.0 -->

From version 1.5.0 you will find the settings for synchronisation in the new section "Contents" &#8594 "People" &#8594 "LDAP attributes". The settings are the same as before, except that they can now be found in a separate area.

Important: These attributes can then no longer be edited in OSIRIS itself. To activate synchronisation, you must also activate the CRON job below. This is important so that the attributes are updated regularly.

To ensure that OSIRIS updates the LDAP attributes regularly, you must set up a CRON job that executes the corresponding script on a daily basis.

To do this, open the crontab of your system:

```bash
crontab -e
```

Then add the following line to run the synchronisation daily at 2:30 am:

```cron
30 2 * * * /usr/bin/php /path/to/osiris/jobs/synchronise_users.php >> /var/log/osiris_sync.log 2>&1
```

Note: Replace `/path/to/osiris/` with the directory in which you have installed OSIRIS.

You can also test the command manually:


```bash
php /path/to/osiris/jobs/synchronise_users.php
```

This allows you to check whether the synchronisation is running correctly before it is started automatically via CRON.

If the synchronisation was successful, you will find a timestamp with the last successful synchronisation exactly where you previously defined the fields.

