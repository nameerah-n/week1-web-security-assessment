# Week 1 - WEB APPLICATION SECURITY ASSESSMENT 

## Application Tested
OWASP Juice Shop

## Tools Used
- Manual Testing
- OWASP ZAP


## SQL Injection (Authentication Bypass)

Payload:
' OR 1=1--

Result:
Login was successfully bypassed.


## Cross-Site Scripting (XSS)

Payload:
<iframe src="javascript:alert(`xss`)">

Result:
JavaScript alert executed successfully.


## OWASP ZAP Scan Results
An automated vulnerability scan was performed using OWASP ZAP against the deployed instance of OWASP Juice Shop.
The scan identified vulnerabilities across multiple risk levels.

## Key Findings
"HIGH RISK":

"SQL Injection": This is the most critical finding.


URL: https://juice-shop.herokuapp.com/rest/products/search?q=%27%28 was the target.

Parameter: The q parameter in the URL query string is likely vulnerable.

Evidence: The server returned an HTTP/1.1 500 Internal Server Error, and the application message contained a SQLITE_ERROR: near "(": syntax error. The error message itself indicates that the injected characters caused an error in the underlying SQL query (SELECT } FROM Products WHERE ((name LIKE '%'(%' OR description LIKE '%'(%') AND deletedAt IS NULL) ORDER BY name).

## "MEDIUM RISK":

"Missing Anti-clickjacking Header": 

The application responses lack the Content-Security-Policy with the 'frame-ancestors' directive or X-Frame-Options header, which protects against 'Clickjacking' attacks.

## "LOW RISK":

"Server Leaks Version Information":

The web/application server is revealing its version information via the "Server" HTTP response header field (specifically AmazonS3).

Access to such information may help attackers identify specific vulnerabilities associated with that software version.

This is generally classified as a Low risk but high-confidence finding.

## "INFORMATIONAL RISK":

"Information Disclosure Suspicious Comments":

Description: The application's response appears to contain suspicious comments that could assist an attacker in gathering information about the system or application logic.

For example, comments like "FIXME" that contain sensitive information (like cookie: root=true; Secure) can be an issue.

## Other Findings (Various Risks of all types)

1-Content Security Policy (CSP) Header Not Set: A medium-risk, high-confidence issue.

The application does not set a CSP header, which is an added layer of security that helps detect and mitigate attacks like Cross-Site Scripting (XSS) and data injection.

2- Missing Anti-clickjacking Header: A medium-risk, high-confidence issue.

The application responses lack the Content-Security-Policy with the 'frame-ancestors' directive or X-Frame-Options header, which protects against 'Clickjacking' attacks.

3- Vulnerable JS Library: A medium-risk issue.

The scan identified a JavaScript library with known vulnerabilities.

4- Private IP Disclosure: A low-risk issue.

5- Server Leaks Version Information via "Server": A low-risk issue.

6- Strict-Transport-Security Header Not Set (Systemic): A low-risk issue.

7- Timestamp Disclosure - Unix (Systemic): A low-risk issue.

8- X-Content-Type-Options Header Missing: A low-risk issue.

9- Session ID in URL Rewrite (Medium Risk):

A session ID was found being rewritten into the URL, which can expose sensitive information

10- Information Leakage (Low Risk):

The server is leaking version information via the "Server" HTTP response header field and through suspicious comments, which could aid an attacker in identifying specific vulnerabilities to exploit.

11- Missing Security Headers (Low Risk):

Other standard headers like Strict-Transport-Security and X-Content-Type-Options are not set, which weakens the overall security posture

12- Cross-Domain Misconfiguration (Medium Risk): 

This general category indicates potential issues with how resources are handled across different domains.

13- Session ID in URL Rewrite (Medium Risk):

A session ID was found being rewritten into the URL, which can expose sensitive information.

## Recommendations

SQL Injection
→ Use parameterized queries and prepared statements to prevent injection attacks.

Missing Anti-Clickjacking Header
→ Implement X-Frame-Options or Content-Security-Policy (frame-ancestors) to prevent clickjacking.

Vulnerable JavaScript Library
→ Update all third-party libraries to their latest secure versions.

Private IP Disclosure
→ Remove internal IP addresses from server responses and error messages.

Server Leaks Version Information
→ Disable or mask the Server HTTP response header to prevent technology fingerprinting.

Strict-Transport-Security Header Not Set
→ Enable HSTS to enforce secure HTTPS connections.

Timestamp Disclosure (Unix)
→ Avoid exposing system timestamps in server responses.

X-Content-Type-Options Header Missing
→ Add X-Content-Type-Options: nosniff to prevent MIME-type sniffing attacks.

Session ID in URL Rewrite
→ Use secure cookies instead of URL-based session identifiers.

Information Leakage via Comments
→ Remove sensitive or debug comments from production code.

Missing Security Headers (General)
→ Implement standard security headers to strengthen overall application security.

Cross-Domain Misconfiguration
→ Properly configure CORS policies to restrict unauthorized cross-origin access.

Information Disclosure (General)
→ Disable verbose error messages and restrict exposure of backend details.
