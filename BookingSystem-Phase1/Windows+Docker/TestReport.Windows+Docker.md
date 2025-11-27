# 1️⃣ Introduction

**Tester(s):**  
- Name:  Mari Taipale

**Purpose:**  
- -Identify vulnerabilities in registration and authentication flows of the BookingSystem application.

**Scope:**  
- Tested components:  http://localhost:8000
- Exclusions: API's, admin panel 
- Test approach: White-box

**Test environment & dates:**  
- Start:  25.11.2025
- End:  27.11.2025
- Test environment details (OS, runtime, DB, browsers): Windows+Docker, local browser (Chrome) via ZAP
  
**Assumptions & constraints:**  
- The test is based on automated ZAP-test
- Authentication was limited to the registration flow; login and password reset flows were not fully exercised.

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):**  
- ZAP-scan revealed several critical vulnerabilities, including SQL-injection and Path-Traversal on registeration form.
  There are missing protective HTTP-headers and CSRF tokens that expose the app to attacks. 

**Overall risk level:** (Low / Medium / High / Critical)

**Top 5 immediate actions:**  
1.  Fix SQL Injection by using parameterized queries and prevent dynamic string concatenation.
2.  Prevent Path Traversal by validating and canonicalizing file paths.
3.  Add Anti-CSRF tokens to all forms. 
4.  Enable Content Security Policy (CSP) and X-Frame-Options headers.
5.  Add X-Content-Type-Options: nosniff to all HTTP responses.

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
|      🔴 **High**     | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
|     🟠 **Medium**    | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                              | *Fix ASAP*                       |
|      🟡 **Low**      | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
| 🔵 **Info** | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |


---

# 4️⃣ Findings 


| ID | Severity | Finding | Description | Evidence / Proof |
|------|-----------|----------|--------------|------------------|
| F-01 | 🔴 High | SQL Injection in registration | Username field allows ' injection which causes 500 error | ZAP evidence: HTTP/1.1 500 Internal Server Error |
| F-02 | 🔴 High | Path Traversal in registration | User input in the username field can manipulate file paths | ZAP evidence: attack string ../ |
| F-03 | 🟠 Medium | Absence of Anti-CSRF Tokens | Registeration form is missing CSRF protection | ZAP evidence:without token |
| F-04 | 🟠 Medium | Missing security headers (CSP & X-Frame-Options) | Pages do not contain CSP or X-Frame-Options headers | ZAP evidence: response headers missing |
| F-05 | 🟡 Low | 	X-Content-Type-Options header missing | MIME sniffing possible, header missing from several resources | ZAP evidence: header x-content-type-options not set |
---


# 5️⃣ OWASP ZAP Test Report (Attachment)


> 📁 **[ZAP Report – Phase 1 (Windows + Docker)](BookingSystem-Phase1/Windows+Docker/ZAP-report-phase1-part1-WINDOWS+DOCKER.md)
**

---
