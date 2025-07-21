# 🎮 BTLO: Squid Game – Blue Team Challenge Walkthrough

**Platform:** [Blue Team Labs Online (BTLO)](https://blueteamlabs.online)  
**Challenge:** Squid Game  
**Category:** CTF-style / Steganography / OSINT  
**Difficulty:** Beginner-Intermediate  
**Date Completed:** July 2025

---

## 🧠 Challenge Overview

A mysterious Squid Game invitation contains clues leading to a hidden message and ultimately a flag. This challenge involves:

- OSINT lookup
- Steganography extraction
- Image analysis using pixel and channel manipulation
- Cipher decoding

---

## 🧩 Questions and Steps

### 🔍 Q1: What is the phone number on the invitation card in Squid Game?

**Answer:** `86504006`

**Method:**  
Basic OSINT search (Google image/text reference). This was a warm-up flag.

---

### 🔎 Q2: Can you extract something from the invitation card file? What is the name of the file?

**Answer:** `Dalgona.png`

**Method:**

1. Tried `exiftool` — no useful metadata found.
2. Used `strings`, `binwalk`, and `xxd` — still no result.
3. Success came with `steghide`:

```bash
steghide extract -sf "Invitation Card.jpg"
# Passphrase used: 86504006
```

The extracted file: `Dalgona.png`.

---
