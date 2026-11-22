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

OSIRIS supports Microsoft Active Directory and OpenLDAP. LDAP verifies login credentials; profiles, roles and permissions are managed in OSIRIS. All settings below belong in `CONFIG.php`. These configurations do not require changes to `php/LDAPInterface.php`.

## Choose Active Directory or OpenLDAP

Keep `USER_MANAGEMENT` set to `LDAP` in both cases. `OPEN_LDAP` selects the user attributes:

| Setting | Login attribute | Unique identifier |
| --- | --- | --- |
| `OPEN_LDAP = false` (default) | `samaccountname` | `objectguid` |
| `OPEN_LDAP = true` | `uid` | `entryuuid` |

`OPEN_LDAP` also changes other profile attributes, for example from `description` to `department` for the organisational unit. It does not provide arbitrary configuration of an individual login attribute. For an OpenLDAP directory using `uid`, there is no need to replace `samaccountname` in the source code.

### Example: Active Directory

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'LDAP');
define('OPEN_LDAP', false);
define('ADMIN', 'musterfrau');

define('LDAP_IP', 'ldap.example.org');
define('LDAP_PORT', 389);
define('LDAP_USE_TLS', true);
define('LDAP_USER', 'osiris');
define('LDAP_PASSWORD', 'service_account_password');
define('LDAP_DOMAIN', '@example.org');
define('LDAP_BASEDN', 'DC=example,DC=org');
define('LDAP_BIND_METHOD', 'username');
```

`LDAP_USER` is a service account for searching and reading the directory. It does not need LDAP administrator rights. `ADMIN`, in contrast, is the username of the person administering OSIRIS. These accounts serve different purposes.

### Example: OpenLDAP with users in different subtrees

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'LDAP');
define('OPEN_LDAP', true);
define('ADMIN', 'musterfrau'); // uid of the OSIRIS administrator

define('LDAP_IP', 'ldap.example.org');
define('LDAP_PORT', 389);
define('LDAP_USE_TLS', true);
define('LDAP_USER', 'cn=osiris,ou=services,dc=example,dc=org');
define('LDAP_PASSWORD', 'service_account_password');
define('LDAP_DOMAIN', '');
define('LDAP_BASEDN', 'dc=example,dc=org');
define('LDAP_BIND_METHOD', 'dn');
```

OSIRIS searches for `(uid=username)` beneath `LDAP_BASEDN`, including all subtrees. This can find both `uid=musterfrau,ou=research,dc=example,dc=org` and `uid=mustermann,ou=administration,dc=example,dc=org`. Choose a common search base containing all required users. Usernames must be unique within that search base.

The examples use StartTLS on port 389. If your server uses LDAPS, set `LDAP_USE_SSL` to `true`, `LDAP_USE_TLS` to `false` and use the appropriate port, usually 636. The hostname and certificate trust must match your server.

## Search base, bind method and domain

Login takes three steps:

1. OSIRIS binds with `LDAP_USER` and `LDAP_PASSWORD` to search the directory.
2. It searches beneath `LDAP_BASEDN` for the entered username, using `samaccountname` or `uid` depending on `OPEN_LDAP`.
3. It verifies the entered password by binding again as the user found.

`LDAP_BIND_METHOD` selects the identifier for this second bind:

| Value | Identifier |
| --- | --- |
| `username` (default) | Entered username |
| `dn` | Full DN from the search result; suitable for different subtrees |
| `userPrincipalName` | `userPrincipalName` attribute from the search result; for Active Directory |

`LDAP_DOMAIN` is **not the search base**. The bind function applies it to both binds using these rules:

| Configuration or identifier | Behaviour |
| --- | --- |
| Identifier starts exactly with `cn=` | Used unchanged |
| `LDAP_DOMAIN` contains `%s` | Identifier is inserted into the template, e.g. `uid=%s,ou=people,dc=example,dc=org` |
| Otherwise | `LDAP_DOMAIN` is appended to the identifier, e.g. `musterfrau@example.org` |
| `LDAP_DOMAIN` is empty | Identifier remains unchanged |

!!! warning "Current limitation with full DNs"
    The bind function currently recognises only the lowercase prefix `cn=` as a full DN automatically. If a DN starts with `uid=` or `CN=`, a non-empty `LDAP_DOMAIN` may therefore be appended or applied as a template even with `LDAP_BIND_METHOD = 'dn'`. For the OpenLDAP example above, leave `LDAP_DOMAIN` empty and specify `LDAP_USER` as the full service account DN. The constant must still be defined.

`OPEN_LDAP = true` changes the attributes but does not automatically select DN-based login. For a branched OpenLDAP directory, also set `LDAP_BIND_METHOD = 'dn'` and configure the domain accordingly.

## Administrator rights and other roles

LDAP verifies credentials. **OSIRIS roles are not automatically imported from LDAP groups or role attributes.** New LDAP profiles initially have no additional roles beyond the general user role.

Configure the first administrator in `CONFIG.php`:

```php title="CONFIG.php"
define('ADMIN', 'musterfrau');
```

If `musterfrau` is the OSIRIS username of the LDAP account (`uid` or `samaccountname`), that person automatically receives the `admin` role after successful login. Match the exact spelling, including case. `ADMIN` does not create a local account or replace password verification.

Assign further roles and administrator rights in OSIRIS via **Roles & Rights** or the **Account** section of the user profile. These assignments are stored in OSIRIS and also apply to LDAP logins. See [Distribute roles](../../admins/user_roles/distribute_roles.md).

## Local guest accounts alongside LDAP

<!-- md:version 1.6.2 -->

With `USER_MANAGEMENT = 'LDAP'`, you can also use local accounts without an LDAP entry. Open **Guest accounts** in the admin area (`/admin/guest-account`) and select **Add guest account** to create an account with a username, local password and profile details. An optional validity end date limits access; leave it empty for an unlimited account.

Both account types use the same login form:

1. OSIRIS attempts LDAP login.
2. If that attempt returns a failed login, OSIRIS checks the local guest account, its password and any validity end date.

This is the existing login cascade **LDAP → guest account**. There is no additional fallback to the `AUTH` add-on; you do not need to switch user management. Use separate guest usernames that are not already assigned in LDAP.

Permissions for these accounts are also managed in OSIRIS. To create a local administrator, assign the additional `admin` role to the guest account or configure its username as `ADMIN`. The “guest account” label does not restrict additional role assignments. These login accounts are separate from [managing guest visits](../../users/people/guests.md).

!!! note "Not a guaranteed emergency login during an LDAP outage"
    Guest authentication follows an unsuccessful LDAP login. If LDAP connection setup or StartTLS throws an exception, the current flow does not reach the guest check. A guest account therefore does not guarantee emergency access during an LDAP outage.

## Filter users


<!-- md:version 1.3.6 -->

You can also specify which users should be synchronised from the LDAP server. To do this, you can set the `LDAP_FILTER` parameter in `CONFIG.php`. For example:

```php title="CONFIG.php"
define("LDAP_FILTER", "(objectClass=person)");
```

**This filter applies to fetching and synchronising user lists, not to the individual user search during login.** It therefore does not restrict login access. A filter using an LDAP attribute such as `role=employee` does not assign an OSIRIS role either.

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
30 2 * * * /usr/bin/php /path/to/osiris/jobs/synchronize_users.php >> /var/log/osiris_sync.log 2>&1
```

Note: Replace `/path/to/osiris/` with the directory in which you have installed OSIRIS.

You can also test the command manually:


```bash
php /path/to/osiris/jobs/synchronize_users.php
```

This allows you to check whether the synchronisation is running correctly before it is started automatically via CRON.

If the synchronisation was successful, you will find a timestamp with the last successful synchronisation exactly where you previously defined the fields.

