# Secret Letter - ctf.cyberleague.at  

**Platform:** 
**Category:** Steganography  
**Difficulty:** Easy  

---

## Challenge Description

We received a PDF file containing what appeared to be an ordinary letter. Hidden somewhere inside was an encrypted flag.

---

## Solution

### Step 1 — Analyzing the PDF

Upon opening the PDF, the letter looked normal at first glance. However, on closer inspection, certain characters stood out as unusual — they didn't quite fit the rest of the text visually.

![PDF with suspicious characters](https://github.com/user-attachments/assets/95716a8d-243f-4a96-976d-18a0e15e5a3b)

> **Key observation:** Some characters embedded in the letter looked out of place. This is a classic steganography technique — hiding data inside seemingly innocent text.

---

### Step 2 — Extracting the Hidden Data

The suspicious characters were extracted and collected. They turned out to form a **Base64-encoded string**.

---

### Step 3 — Decoding with CyberChef

The extracted Base64 string was pasted into [CyberChef](https://gchq.github.io/CyberChef/) and decoded using the **"From Base64"** operation.

The output revealed the flag directly.

![CyberChef decoding the Base64 string to reveal the flag](https://github.com/user-attachments/assets/cc1180f9-fc52-4aec-a9ea-b0ad5f50775c)

