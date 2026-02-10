# 🌐 WEB Challenge – Talent Arena CTFd 2026

Category: WEB
Event: Talent Arena CTFd 2026
Difficulty: 🔴 Hard
Goal: Collect three cryptographic fragments through web exploitation and privilege escalation.


## 📌 Challenge Description

The WEB challenge was a multi-stage web exploitation task involving:

- API discovery 🔍
- Information leakage 🩸
- Privilege escalation via JWT forgery 🔑

The objective was to collect three cryptographic fragments hidden across different privilege tiers and submit them to retrieve the final flag.

## 🔍 Phase I – Reconnaissance & Entry

The attack surface was explored using provided low-privilege credentials.

### 🔐 Authentication

Authenticated via the following endpoint to obtain a demo user JWT:
```
/api/v1/auth/login
```

### 🧬 Information Leakage

Using the obtained token, I queried:
```
/api/v1/users/me
```

This endpoint leaked internal metadata about the user object, including role structure and identifiers, establishing a baseline understanding of the authorization model.

## 🧭 Phase II – API Enumeration & Discovery

With initial access confirmed, the next step was API enumeration.

### ⚙️ Fuzzing with ffuf

A fuzzing campaign was launched against the ticket system:
```
/api/v1/tickets/FUZZ
```

This revealed two valid ID ranges:

- 1000–1010
- 2000–2010


### 🎟️ Ticket Analysis

Most tickets returned generic data, except for one.

Ticket **2003** leaked:
```
Fragment 1: UnjIIW-iyDlu
Hidden endpoint: /api/v1/dev/panel
```

This ticket acted as the primary pivot point for further escalation.

## 🧠 Phase III – Developer Access & Escalation

### 🛠️ Developer Panel

Accessing the leaked endpoint:
```
/api/v1/dev/panel
```

Returned the second fragment:
```
Fragment 2: ma8_Gz6qnxyu
```

### 🔓 Secret Exfiltration

Further enumeration uncovered a critical vulnerability at:
```
/api/v1/dev/jwt
```

This endpoint exposed a hardcoded JWT signing secret:
```
dev-jwt-secret-do-not-use-in-production
```

At this stage, the integrity of the authentication system was fully compromised.

## 🚀 Phase IV – Privilege Escalation & Final Collection

### 🧬 JWT Forgery

Using the leaked signing secret, I forged a new JWT with elevated privileges:
```
user_id: admin
role: admin
```

### 👑 Admin Console Access

The forged token granted access to the admin endpoint:
```
/api/v1/admin/console
```

Inside the admin panel, the final fragment was recovered:
```
Fragment 3: uAXIPLe2u0RP
```

## 🧩 Phase V – Finalization

All three fragments were submitted via a POST request to:
```
/api/v1/finalize
```

Fragments submitted:
```
UnjIIW-iyDlu
ma8_Gz6qnxyu
uAXIPLe2u0RP
```

## 🏁 Final Flag
```
MWC{e52d96f605403b85b812dfe3e13d141e}
```

## ✅ Conclusion

This challenge demonstrated multiple critical web security failures:

- Overexposed APIs 🔍
- Improper access control 🧱
- Hardcoded secrets 💀
- JWT trust abuse 🔑

Never trust the client.
Never hardcode secrets.
Never assume dev endpoints are hidden.
