# 🕵️‍♂️ OSINT Challenge – Talent Arena CTFd 2026
```
Category: OSINT
Event: Talent Arena CTFd 2026
Difficulty: 🟡 Medium
Goal: Find the final flag using open-source intelligence and a bit of blockchain digging.
```
## 📌 Challenge Description

The challenge starts with a single piece of information:
```bash
Username: DevZero-99
```

From here, the objective is to pivot across publicly available platforms, correlate clues, and ultimately extract the flag.

## 🔍 Step 1 – Username Enumeration

The first step was to enumerate the username DevZero-99 across multiple platforms.

To do this, I used the usrlinks tool, which gathers publicly accessible profiles related to a username.

### ✅ Valid results found:

- GitHub
- Pastebin

The Pastebin profile itself didn’t contain any visible information, but the GitHub profile turned out to be a goldmine.

## 🧠 Step 2 – GitHub Recon & Commit History

I cloned the GitHub repository and reviewed the source code.

At first glance, nothing useful appeared—just generic functions and placeholder data.


### 🔁 Commit History Analysis

While reviewing the commit history, I noticed something interesting:

- The third commit, titled “Create utils.js”, contained sensitive information.
- This information was later removed in the sixth commit, meaning it no longer existed in the current version of the code.

Inside that third commit, the last two lines read:
```
// PERSONAL NOTE: My new notes are in a more... pasteable place ;)
// ID: qcSWnAj0 (in case I forget)
```

🚩 That comment was the key pivot.

## 📋 Step 3 – Pastebin Discovery

Since we already knew the user had a Pastebin account, the next move was obvious.

I appended the ID to the Pastebin URL:
```
https://pastebin.com/qcSWnAj0
```
### 🎯 Bingo.

This Pastebin contained crucial information that allowed the investigation to continue, including:

- The blockchain network
- A wallet address involved in a transaction

## ⛓️ Step 4 – Blockchain Analysis (Ethereum Sepolia)

With the wallet address identified, I headed straight to:
- Etherscan (Sepolia testnet)

On the wallet page, I found three transactions, one of which was marked as:
```
Outgoing – Contract Creation
```
That transaction was particularly interesting.

## 🧪 Step 5 – Decoding the Input Data

Inside the transaction details, there was a field called Input Data, containing a long hexadecimal string.

After some research, I discovered that the Linux tool xxd can convert hex data back into plaintext.

## 🖥️ Command Used (Kali Linux):
```
echo "InputDataField" | xxd -r -p
- r → reverse (hex → plaintext)
- p → input is raw hexadecimal
```
## 🧩 Step 6 – Final Decoding

The decoded output was mostly unreadable, except for the final line, which stood out:
```
m3dsolc3Some secrets are hidden in plain sight. 
The journey is the reward.
TVdDe2Qzdl96M3IwX24zdjNyX2QxM3NfMG5fY2g0MW59
```

The last part clearly looked like Base64.

I copied it and decoded it using Google Toolbox → Base64 Decode.

## 🏁 Final Flag

### 🎉 After decoding, the final flag was revealed:
```
MWC{d3v_z3r0_n3v3r_d13s_0n_ch41n}
```
## ✅ Conclusion

This challenge was a great example of:

- OSINT pivoting 🔄
- GitHub commit history abuse 🧠
- Metadata that “shouldn’t matter” actually mattering 😏
- Blockchain transparency being a double-edged sword ⛓️
```
Some secrets are hidden in plain sight.
And yeah… DevZero never really dies 😉
```
