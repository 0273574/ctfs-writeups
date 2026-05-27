# Flags 2: Intermediate - ctf.cyberleague.at

## Description

The challenge provided a `.gif` file containing an animated stick figure waving flags in various positions.

---

## Solution

### Step 1 — Identifying the Encoding

Looking at the GIF, the stick figure holds two red-and-yellow flags and moves them into different positions across multiple frames. After some research, the poses were identified as **flag semaphore** — a system of signaling that encodes letters by the position of the arms holding flags.

![Flag semaphore GIF](https://github.com/user-attachments/assets/24e26dd2-d5dd-45d2-a485-0c6226233b64)

### Step 2 — Decoding

Each frame of the GIF corresponds to one letter. The arm positions were fed into an online flag semaphore decoder, which produced the following output:

![Semaphore decoder output showing PEARLHARBOR](https://github.com/user-attachments/assets/db142756-39bb-4060-9d67-b6267f0e03cb)

Decoded message: `PEARLHARBOR`

---

## Flag

```
KDCTF{pearlharbor}
```
