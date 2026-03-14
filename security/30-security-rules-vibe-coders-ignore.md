# 30 Security Rules Every VIBE CODER Ignores (Until They Get BURNT 🔥)

## 1. Never store sensitive data in localStorage. Use httpOnly cookies.
**Detail:** `localStorage` is accessible via JavaScript (i.e., `window.localStorage`), which means any Cross-Site Scripting (XSS) vulnerability in your application or third-party scripts can easily steal authentication tokens, API keys, or personal data. Instead, use `httpOnly` secure cookies for session tokens. `httpOnly` prevents JavaScript access to the cookie, significantly mitigating the impact of XSS attacks.

## 2. Disable directory listing on your server. Never expose file structure.
**Detail:** When a user requests a directory that lacks a default index file (like `index.html`), many web servers default to displaying a list of all files in that directory. This exposes your file structure and might leak sensitive files (e.g., config files, `.env` files, backups). Always configure your server (Nginx, Apache, or cloud bucket) to deny directory listing.

## 3. Always regenerate session IDs after login.
**Detail:** Session fixation attacks occur when an attacker forces a user's browser to use a known session ID, waits for them to log in, and then uses that same ID to hijack their authenticated session. By regenerating the session identifier upon successful authentication, you ensure that any pre-existing session ID becomes invalid, destroying the attacker's foothold.

## 4. Use Content Security Policy (CSP) headers on every page.
**Detail:** CSP is an added layer of security that helps detect and mitigate certain types of attacks, including XSS and data injection attacks. By defining via HTTP headers which dynamic resources are allowed to load and execute (like scripts, styles, and images), you severely restrict an attacker's ability to run unauthorized code on your users' browsers.

## 5. Never trust client-side validation alone. Always re-validate server-side.
**Detail:** Client-side form validation provides a better user experience by catching errors early, but it is trivial to bypass using browser developer tools, cURL, or Postman. The backend must independently verify all incoming data (type checking, length validation, sanitization) to guarantee data integrity and prevent injection or logic-based exploits.

## 6. Set X-Frame-Options to DENY. Prevents clickjacking attacks.
**Detail:** Clickjacking tricks users into clicking on actionable elements on a hidden or disguised webpage (often loaded in an `iframe`). Setting the `X-Frame-Options` header to `DENY` or `SAMEORIGIN` prevents other domains from embedding your site inside an iframe, neutralizing this threat entirely.

## 7. Strip metadata from every user-uploaded file before storing.
**Detail:** Files like images, PDFs, and videos often contain embedded metadata (EXIF data) that can reveal the creator's location, device model, software versions, and potentially sensitive personal information. Stripping this metadata before storing or serving user uploads protects user privacy and reduces data leakage.

## 8. Never expose stack traces or error details in production responses.
**Detail:** Verbose error messages and stack traces provide attackers with a roadmap of your application's architecture, revealing file paths, database structure, and library versions. Your application should only log these details securely on the server and return generic, unhelpful error messages (e.g., "An unexpected error occurred!") to end users.

## 9. Use short-lived presigned URLs for private file access. Never public bucket URLs.
**Detail:** If you're storing user-specific or private files (like in an AWS S3 bucket), making the bucket publicly readable is a catastrophic risk leading to massive data leaks. Instead, keep the bucket private and generate expiring, short-lived presigned URLs dynamically whenever an authorized user needs access.

## 10. Implement CSRF tokens on every state-changing form or request.
**Detail:** Cross-Site Request Forgery (CSRF) tricks a victim's browser into executing unwanted actions on a web application where they are currently authenticated. Including a unique, unpredictable, and tightly scoped CSRF token with every state-changing request (POST, PUT, DELETE) ensures that the request intentionally came from your own application interface.

## 11. Disable autocomplete on sensitive form fields (passwords, card numbers).
**Detail:** Browsers aggressively try to save user inputs, including Credit Card details, Social Security Numbers, and Passwords. Adding `autocomplete="off"` to sensitive fields helps prevent local data exposure if a device is compromised, shared, or left unlocked (though note that some password managers ignore this for logins).

## 12. Always hash passwords with bcrypt minimum cost factor of 12.
**Detail:** Using outdated algorithms like MD5 or even plain SHA-256 makes passwords trivial to crack using precomputed rainbow tables or modern GPU brute-forcing. Algorithms like bcrypt, Argon2, or scrypt are designed to be intentionally slow (key stretching). A cost factor of 12 or higher ensures that cracking even a moderate number of passwords becomes computationally unfeasible.

## 13. Keep dependency list minimal. Every extra package is an attack surface.
**Detail:** The npm, PyPI, and RubyGems ecosystems are notoriously prone to supply chain attacks (e.g., typosquatting, socially engineered takeovers). Every third-party library you install expands your application's attack surface. Rely on native features when possible and aggressively audit/prune dependency trees.

## 14. Use Subresource Integrity (SRI) for every external script you load.
**Detail:** When you load scripts from a CDN (Content Delivery Network), you trust that the CDN hasn't been compromised. If it is, attackers could swap the legitimate script with malicious code. SRI provides a cryptographic hash of the script you expect to load; if the CDN serves a modified file, the browser will refuse to execute it.

## 15. Never log user passwords, tokens, or PII, even by accident.
**Detail:** Ensure that your application logging is configured to automatically mask, redact, or drop sensitive fields (like `password`, `authorization`, `credit_card`) before they hit your centralized logging system. Exposing Personally Identifiable Information (PII) or credentials in plain-text logs means anyone with log access can compromise user accounts.

## 16. Enforce HTTPS everywhere. Redirect all HTTP to HTTPS at server level.
**Detail:** Unencrypted HTTP traffic is subject to eavesdropping and Man-in-the-Middle (MitM) attacks, allowing attackers on the same network (like a public Wi-Fi) to hijack sessions or alter data. Force HTTPS at the infrastructure level (load balancer, reverse proxy) and use HSTS (HTTP Strict Transport Security) to ensure browsers never attempt unencrypted connections.

## 17. Use separate DB credentials per environment. Never share prod creds.
**Detail:** It's dangerously easy to accidentally run a destructive migration script or leak a connection string if your local, staging, and production environments share database credentials. Always use separate roles, permissions, and passwords for each environment. Furthermore, deploy the principle of least privilege: the API should only have CRUD rights, not table-dropping capabilities.

## 18. Implement account lockout after 5 failed login attempts.
**Detail:** Without rate limiting and account lockouts, attackers can use credential stuffing tools to infinitely test compromised passwords against your login endpoints. Implement a progressive timeout or hard lockout (accompanied by an email to the user) after consecutive failures to stop automated brute forcing.

## 19. Validate content-type headers on every API request.
**Detail:** Attackers often abuse content-type handling to bypass input filters and inject unexpected data payloads (like sending XML to an endpoint expecting JSON to trigger an XXE vulnerability). Strictly validate the `Content-Type` header and instantly reject requests lacking the expected MIME type to shut down parser-based attacks.

## 20. Never use MD5 or SHA1 for anything security-related.
**Detail:** MD5 and SHA1 are cryptographically broken algorithms susceptible to rapid collision attacks. While they are acceptable for simple non-secure checksums (like checking file integrity against random corruption), they should under no circumstances be used to hash passwords, generate tokens, or sign certificates.

## 21. Scope OAuth tokens to minimum required permissions only.
**Detail:** When issuing or requesting OAuth tokens, it is tempting to request sweeping scopes (like `repo` or `full_access`). If those tokens are leaked, the attacker gains massive control. Always follow the Principle of Least Privilege by restricting token scopes strictly to exactly what the integration needs to minimally function.

## 22. Use nonces for every inline script in your CSP policy.
**Detail:** If you must use inline `<script>` tags, doing so safely require CSP nonces. A nonce is a dynamically generated, single-use random string injected into both the CSP header and the secure script tag. The browser will only execute inline scripts carrying the matching nonce, stopping attackers from running their own injected inline XSS.

## 23. Monitor for dependency vulnerabilities with Snyk or similar weekly.
**Detail:** New vulnerabilities (CVEs) are discovered in open-source components daily. Automating dependency scanning with tools like Snyk, Dependabot, or NPM Audit during your CI/CD pipeline guarantees you don't accidentally ship known, critical vulnerabilities in your release artifacts.

## 24. Disable HTTP methods you don't use (PUT, DELETE, TRACE, OPTIONS).
**Detail:** If a specific endpoint only requires `GET` and `POST`, allowing other methods (like `PUT` or `DELETE`) opens the door to unintended state changes or information disclosure. Explicitly disabling unused HTTP methods hardens your API against exploration and unintended exploitation.

## 25. Implement proper logout: invalidate server-side sessions, not just clear cookies.
**Detail:** A common "vibe-coder" mistake is handling a logout simply by deleting the JWT or session cookie on the client side. If an attacker has already stolen that token, they can still use it. A true logout must invalidate the session on the server-side (e.g., removing the session from Redis or adding the JWT to a server-maintained blacklist).

## 26. Use constant-time string comparison for token validation. Prevents timing attacks.
**Detail:** Standard string compilation (e.g., `a === b`) exits as soon as it finds a non-matching character. Attackers can measure the micro-seconds it takes for your server to reject an API key or HMAC signature, slowly guessing the correct string character by character. Use a constant-time comparison library or built-in crypto function (like `crypto.timingSafeEqual`) to validate sensitive strings.

## 27. Never cache sensitive API responses. Set Cache-Control: no-store.
**Detail:** Browsers, intermediate proxies, and CDNs aggressively cache responses to speed up the web. If an endpoint returns sensitive user data, a shared cache might accidentally serve User A's data to User B. Enforcing `Cache-Control: no-store` guarantees sensitive data only lives as long as the immediate request.

## 28. Set Referrer-Policy to strict-origin. Stop leaking URLs to third parties.
**Detail:** When a user clicks a link exiting your site, the browser sends the current page's URL in the `Referer` header. If your URLs contain sensitive parameters (e.g., password reset tokens, session keys), you could leak them to external analytics or linked sites. Setting `Referrer-Policy: strict-origin` restricts what information is passed along.

## 29. Enforce password complexity server-side. Not just a regex on the frontend.
**Detail:** A sleek UI validator forcing special characters and numbers doesn't stop attackers from using an API tool to post a payload with the password "123". Like everything else, password complexity and length requirements must be strictly checked and enforced on the server.

## 30. Scan your Docker images for vulnerabilities before every deployment.
**Detail:** A secure application running on a vulnerable Base OS is still vulnerable. Outdated base images like an old Alpine or Ubuntu release might contain vulnerable system packages (like unpatched OpenSSL or bash). Run tools like Trivy or internal cloud scanners in your pipeline to guarantee your container sandbox isn't already compromised from within.
