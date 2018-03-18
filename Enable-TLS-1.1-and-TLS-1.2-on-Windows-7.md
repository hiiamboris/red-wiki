Some websites require TLS1.1 or TLS1.2 support on client, but WinHTTP use TLS 1.0 by default on Windows 7.

If you cannot `read` a **https** site in red, please update WinHTTP on the system. 

1. Install the [update](http://www.catalog.update.microsoft.com/search.aspx?q=kb3140245).
2. Install the [easy fix](https://support.microsoft.com/en-us/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in#easy).

Reference: https://support.microsoft.com/en-us/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in