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

### 🔦 Q3: What hint text can be discovered in the final file?

**Answer:** `Red Pixel`

**Method:**  
Analyzed `Dalgona.png` using **Stegsolve**:

```bash
java -jar stegsolve.jar
```

Steps:

- Opened the file
- Scrolled through layers (Gray Bits layer revealed "Red Pixel" as a message)

This clue led to the next layer of hidden content.

---

### 🧬 Q4: What is the final flag?

**Answer:** `SBT{S4v3_My4nm4r_Guy5}`

**Method:**

Used **PixSpy** and **dCode.fr** to extract and decode RGB values:

1. Opened `Dalgona.png` on [PixSpy](https://29a.ch/photo-forensics/#forensic-magnifier)
2. Zoomed into a vertical line of red pixels near the map image
3. Manually extracted red values in order:

```
123, 102, 124, 173, 123, 64, 166, 63, 137, 115, 171, 64,
156, 155, 64, 162, 137, 107, 165, 171, 65, 175
```

4. Used [dCode ASCII Converter](https://www.dcode.fr/ascii-code) to convert these values

The resulting string revealed the final flag.

---
