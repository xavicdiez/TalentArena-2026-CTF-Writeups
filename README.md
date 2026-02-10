# 🚩 TalentArena-2026-CTF-Writeups
<p align="center"> <img src="https://img.shields.io/badge/CTF-2026-blueviolet?style=for-the-badge&logo=ctfd" alt="CTF Year"> <img src="https://img.shields.io/badge/Challenges-2-green?style=for-the-badge" alt="Challenges"> <img src="https://img.shields.io/badge/Platform-TalentArena-orange?style=for-the-badge" alt="Platform"> </p>

Write-up of the two challenges I completed on talentarena.ctfd.io. This repository documents the methodology, tools, and exploitation steps used to capture the flags in OSINT/Blockchain and Web categories.

---

## 📂 Project Structure
```bash
.
├── OSINT-Digital-Treasure-Hunt/
│   ├── README.md          # Full technical breakdown
│   └── screenshots/       # Blockchain explorer & Pastebin caps
└── Web-Companion-API/
    └── README.md          # Step-by-step exploitation
```
---

# 🔍 Challenge 1: The Digital Treasure Hunt
Category: OSINT / Blockchain | Difficulty: Medium

## 🛠️ Execution Flow

1. User Discovery: Started with username DevZero-99. Used usrlinks to map public profiles, identifying active GitHub and Pastebin accounts.

2. Commit Forensics: Scanned the GitHub commit history. Found a deleted comment in the 3rd commit ("Create utils.js").

    - The Leak: A personal note pointing to Pastebin ID qcSWnAj0.

3. On-Chain Investigation: The Pastebin contained migration notes to the Sepolia Testnet and a specific wallet address.

    - Wallet: 0x7c43829618ddda345f72ed9a4a5e6d290537eb10.

4. Data Decoding: Found a "Contract Creation" transaction on sepolia.etherscan.io. Extracted Hex data from the Input Data field.

    - The Command: echo "InputDataField" | xxd -r -p was used to convert Hex to plaintext.
    - The Pivot: Decoded a Base64 string from the output to get the final flag.

```bash
Flag: MWC{d3v_z3r0_n3v3r_d13s_0n_ch41n} 
```
---

# 🌐 Challenge 2: MWC Companion
Category: Web | Difficulty: Hard

## 🛠️ Exploitation Steps

- Step 1: Recon & Auth: Authenticated via /api/v1/auth/login as a demo user. Checked /api/v1/users/me to analyze internal identity metadata.
- Step 2: Fuzzing Tickets: Used ffuf to enumerate /api/v1/tickets/, discovering ranges 1000–1010 and 2000–2010.
- Step 3: Finding Fragments: * Fragment 1: Recovered from Ticket 2003 (UnjIIW-iyDlu).
  - Fragment 2: Accessed /api/v1/dev/panel (leaked in Ticket 2003) to find ma8_Gz6qnxyu.
- Step 4: JWT Forgery: Found the hardcoded secret at /api/v1/dev/jwt: dev-jwt-secret-do-not-use-in-production.
  - Forged a new token with user_id: admin and role: admin.
- Step 5: Admin Access: Accessed /api/v1/admin/console with the forged token to get Fragment 3 (uAXIPLe2u0RP).
- Step 6: Finalization: Submitted all fragments to /api/v1/finalize.

```bash
Flag: MWC{e52d96f605403b85b812dfe3e13d141e} 
```

# 🛠️ Tools Used
- usrlinks: OSINT social media mapping.
- ffuf: Web fuzzer for ticket enumeration.
- xxd: Hexadecimal to Plaintext conversion.
- Etherscan: Blockchain transaction analysis.
- CyberChef / Google Toolbox: Base64 decoding.

# 👨‍💻 Author
**Javier Cremades Diez**

Write-ups developed as part of my participation in the **Talent Arena 2026 CTF**.

