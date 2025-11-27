# 1️⃣ Introduction

**Tester:**  
- Name:  Mari Taipale

**Purpose:**  
- Identify vulnerabilities in registration and authentication flows of the BookingSystem application.

**Scope:**  
- Tested components: http:/localhost:8000. `/register`, `/login`, static resources (`/static/footer.html`, `/static/*.js`, `/static/*.css`, `/static/logo.png`)
- Exclusions:  Admin interfaces, APIs not exposed via web UI
- Test approach: White-box

**Test environment & dates:**  
- Start: 19/11/2025 
- End: 19/11/2025 
- Test environment details (OS, runtime, DB, browsers): Kali Linux VM + Docker, local browser via ZAP

**Assumptions & constraints:**  
- Default credentials provided for test environment  
- Focused only on web UI, not backend DB hardening

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):**  
All parts of testing were not successful, for example manual registeration testing didn't work due to browser (Firefox) password protection. Automated The ZAP scan revealed several medium-severity issues. 

**Overall risk level:** Medium

**Top 5 immediate actions:**  
1. Implement Anti-CSRF tokens in all forms, especially `/register`.  
2. Configure a Content Security Policy (CSP) header site-wide.  
3. Add X-Frame-Options or CSP `frame-ancestors` to prevent clickjacking.  
4. Set `X-Content-Type-Options: nosniff` header on all responses.  
5. Review and harden other HTTP security headers (e.g., HSTS, Referrer-Policy).
   
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
| F-01 | 🟠 Medium | Missing Anti-CSRF Tokens | Registration form has no CSRF protection, allowing forged requests. | `/register` form without CSRF token |
| F-02 | 🟠 Medium | CSP Header Not Set | No Content-Security-Policy header, increasing risk of XSS and data injection. | Responses from `/`, `/register`, `/static/footer.html` |
| F-03 | 🟠 Medium | Missing Anti-clickjacking | No X-Frame-Options or CSP `frame-ancestors`, vulnerable to clickjacking. | Responses from `/`, `/register`, `/static/footer.html` |
| F-04 | 🟡 Low    | X-Content-Type-Options Missing | MIME-sniffing possible, browser may misinterpret file types. | 7 instances across `/`, `/register`, `/static/...` |


---

# 5️⃣ OWASP ZAP Test Report (Attachment)

> 📁 [ZAP-raportti (osa 1)](ZAP_report-phase1-part1.VM-Kali+Docker.md)

---
