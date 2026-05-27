# CTF Writeup — 3 Ways to Stego

## Description

The challenge description stated that the flag is split into 3 parts, all hidden inside a single `.jpg` file.

---

## Solution

### Part 1 — EXIF Metadata

The image was uploaded to [Aperi'Solve](https://www.aperisolve.com/), which runs ExifTool automatically. Inside the EXIF data, the `XP Comment` field contained a Base64-encoded string:

```
S0RDVEZ7NXRlZzBf
```

Decoding it from Base64 gives the first part of the flag:

```
KDCTF{5teg0_
```

---

### Part 2 — Strings

Aperi'Solve also runs a `strings` analysis on the file. Among the output, the following line appeared:

```
FLAG:-------YzRuX2Iz--------
```

Decoding `YzRuX2Iz` from Base64 gives the second part:

```
c4n_b3
```

---

### Part 3 — Hidden ZIP Archive

Running `binwalk` on the image revealed a ZIP archive embedded inside the JPEG:

```
$ binwalk flag.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
21820         0x553C          Zip archive data, compressed size: 6892, name: flag.odt
28856         0x70B8          End of Zip archive, footer length: 22
```

Extracting it:

```bash
binwalk -e flag.jpg
```

This produced a `flag.odt` file. Since ODT files are ZIP archives internally, the contents were extracted and `content.xml` was inspected:

```bash
unzip -p flag.odt content.xml | sed 's/<[^>]*>//g'
```

Inside the XML, the third and final part of the flag was found:

```
pr3tty_fun}
```

---

## Flag

Combining all three parts:

```
KDCTF{5teg0_c4n_b3_pr3tty_fun}
```

---
