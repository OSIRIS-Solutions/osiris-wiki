---
tags:
  - Configuration
  - Proxy
  - Network
---

# Configure an outgoing proxy

<!-- md:version 2.1.5 -->

If your OSIRIS server can only access the internet through a proxy, you can optionally configure it in `CONFIG.php`. This applies to connections **from the server to external services**. Users' browser proxy settings do not configure these connections.

!!! info "Availability and scope"
    This configuration requires an OSIRIS version containing the shared HTTP client (`php/HttpClient.php`). Older installations must first be updated to a version with this support.

    The setting currently applies to **downloading the country list** and **retrieving journal metrics when creating a journal**. Other external requests, including journal searches and separate metrics updates, are not covered yet.

## Add the proxy to CONFIG.php

Add your proxy's address and port within the PHP section of `CONFIG.php`:

```php
define('PROXY_URL', 'http://proxy.example.org:3128');
```

Replace the example address with the details provided by your IT team. Specify the protocol of the **proxy itself**: an HTTP proxy can also forward connections to HTTPS destinations using a CONNECT tunnel. The destination connection remains TLS-encrypted unless the proxy performs TLS inspection.

If the proxy requires authentication, also add:

```php
define('PROXY_USERNAME', 'username');
define('PROXY_PASSWORD', 'password');
```

The authentication method is negotiated with the proxy. Available methods depend on the server's libcurl installation, for example Basic, Digest or NTLM. Credentials are only applied when `PROXY_URL` is configured. Protect `CONFIG.php` accordingly and keep real credentials out of version control.

## Operation without an explicit proxy configuration

All three settings are optional. If `PROXY_URL` is unset, `null` or an empty string, OSIRIS does not force a proxy. Existing libcurl proxy environment variables such as `https_proxy`, `http_proxy`, `ALL_PROXY` and `NO_PROXY` still apply. They must be available in the PHP service's environment; settings from an interactive shell are not necessarily inherited.

A nonempty `PROXY_URL` takes precedence over environment proxies and also over `NO_PROXY` for the two supported operations. An empty value therefore does not disable a proxy already configured through the environment.

## Connections and certificates

The server needs access to these HTTPS destinations (port 443) through the proxy:

| Operation | Destination |
| --- | --- |
| Downloading the country list | `raw.githubusercontent.com` |
| Retrieving metrics when creating a journal | `osiris-app.de` |

Normal country selection reads from the local database. Internet access is needed to import or update the countries.

The PHP **cURL** extension must be installed and enabled for the PHP service. Certificate validation remains enabled. If your proxy performs TLS inspection, your IT team must install the relevant CA certificate in the trust store used by PHP/cURL. A custom CA bundle path can be configured with `curl.cainfo` in the PHP configuration. Do not disable certificate validation.

## Error handling

The connection timeout is five seconds. The total request timeout is ten seconds, or 30 seconds for the country download. Redirects are limited to three and to HTTP(S).

- If the metrics request fails or returns invalid JSON, the journal is saved without external metrics. After such a failure, further ISSN lookups are stopped for that save. A generic message is written to the PHP error log.
- If the country download fails or returns invalid data, an error is displayed. Existing countries are not replaced in this case.

## Verify the setup

Ask your IT team to check access from the same PHP runtime that runs OSIRIS. A successful request in a browser or server shell alone does not confirm this configuration. PHP error log messages help distinguish connection, proxy authentication, HTTP and certificate problems.

Then test [updating the country list](../../admins/custom_data/countries.md) and creating a journal with an ISSN. Saving a journal alone does not confirm a successful external request: saving is also possible when the metrics lookup fails.
