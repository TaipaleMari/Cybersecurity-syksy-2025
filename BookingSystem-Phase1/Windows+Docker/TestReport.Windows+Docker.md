# 1️⃣ Introduction

**Tester(s):**  
- Name:  Mari Taipale

**Purpose:**  
- -Identify vulnerabilities in registration and authentication flows of the BookingSystem application.

**Scope:**  
- Tested components:  http://localhost:8000
- Exclusions: test includes only registeration 
- Test approach: White-box

**Test environment & dates:**  
- Start:  25.11.2025
- End:  27.11.2025
- Test environment details (OS, runtime, DB, browsers): Windows+Docker, local browser (Chrome) via ZAP, manual SQL-query via PowerShell
  
**Assumptions & constraints:**  
- Automated ZAP-test
- Manual SQL-query
- Authentication was limited to the registration flow; login and password reset flows were not fully exercised.

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):**  
- ZAP-scan revealed several critical vulnerabilities, including SQL-injection and Path-Traversal on registeration form.
  There are missing protective HTTP-headers and CSRF tokens that expose the app to attacks.
- Manual testing revealed vulnerabilities in password encryption and user roles.

**Overall risk level:** (Low / Medium / High / Critical)

**Top 5 immediate actions:**  
1.  Fix SQL Injection by using parameterized queries and prevent dynamic string concatenation.
2.  Enable secure password management/hashing.
3.  Prevent the creation of admin accounts based on customer input.
4.  Fix Path Traversal vulnerability by validating and canonicalizing file paths and restricting inputs to an accepted character set.
5.  Add CSRF protection to registration and other state-changing requests

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
| F-01 | 🔴 High | SQL Injection in registration | Username field allows ' injection which causes 500 error, password not hashed | ZAP evidence: HTTP/1.1 500 Internal Server Error |
| F-02 | 🔴 High | Passwords stored unhashed | Application stores user passwords in cleartext or reversible format. This exposes all users to credential theft and full compromise in case of database leak. | SQL query results show plaintext password column; no hashing or salting detected. |
| F-03 | 🔴 High | User can freely create an admin account | Registration endpoint accepts arbitrary role values (e.g. “admin”), allowing attackers to self-assign administrative privileges. | Registeration completes successfully if selected role = administrator.|
| F-04 | 🔴 High | Path Traversal in registration | User input in the username field can manipulate file paths | ZAP evidence: attack string ../ |
| F-05 | 🟠 Medium | Absence of Anti-CSRF Tokens | Registeration form is missing CSRF protection | ZAP evidence:without token |
| F-06 | 🟠 Medium | Missing security headers (CSP & X-Frame-Options) | Pages do not contain CSP or X-Frame-Options headers | ZAP evidence: response headers missing |
| F-07 | 🟡 Low | 	X-Content-Type-Options header missing | MIME sniffing possible, header missing from several resources | ZAP evidence: header x-content-type-options not set |
---


# 5️⃣ OWASP ZAP Test Report (Attachment)


> 📁 [ZAP Report – Phase 1 (Windows + Docker)](ZAP-report-phase1-part1-WINDOWS+DOCKER.md)

---
