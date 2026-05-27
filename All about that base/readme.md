# All About That Base — ctf.cyberleague.at

## Description

The challenge provided a `.txt` file containing a binary string grouped into space-separated bytes.

## Solution

1. **Binary → Octal**  
   The file contained bits grouped into bytes (`00110000 00110110...`). Converting from binary produced digits in the range `0–7` — the **octal** number system.

2. **Octal → Hex**  
   Converting from octal produced what looked like hex data, but CyberChef threw an error when trying to parse it as a byte array:  
   `Data is not a valid byteArray: [-1,-1,190,-1,-1,-1,-1...]`  
   Using **From Hex** resolved the issue.

3. **Magic Tool → Base32 → Base64 → Flag**  
   CyberChef's **Magic** tool automatically detected the remaining encoding layers — first **Base32**, then **Base64** — and revealed the flag.

## Decoding Chain

```
Binary → Octal → Hex → Base32 → Base64 → FLAG
```

![Screen from cyberchef](https://github.com/0273574/ctfs-writeups/blob/main/All%20about%20that%20base/Its%20all%20about%20that%20base.png?raw=true)
