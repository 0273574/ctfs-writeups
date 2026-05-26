# BSidesSF 2026 CTF - Ads

**Category:** Forensics / Steganography  
**Author:** kacper 

---

## Overview

We were given a single `.mp4` file named `ads.mp4`. The goal was to find a hidden flag concealed inside the video file.

---

## Reconnaissance

The first step was to inspect the file's stream metadata using `ffprobe`:

```bash
ffprobe -v quiet -print_format json -show_streams ads.mp4
```

The output revealed that the file contained more than one video stream — specifically a hidden **AV1** track embedded alongside the main video. This was the key indicator that something was concealed inside.

---

## Extraction

With the hidden stream identified, `ffmpeg` was used to extract it as a separate video file:

```bash
ffmpeg -i ads.mp4 -map 0:1 -c copy hidden_track.mp4
```

The extracted track was then split into individual frames:

```bash
mkdir hidden_frames
ffmpeg -i hidden_track.mp4 hidden_frames/frame_%04d.png
```

Opening the first frame revealed the flag:

```bash
open hidden_frames/frame_0001.png
```

![Flag frame](https://github.com/0273574/ctfs-writeups/blob/main/ads/ads.png?raw=true)

---

## Summary

| Step | Tool | Action | Result |
|---|---|---|---|
| 1 | `ffprobe` | Inspect stream metadata | Discovered hidden AV1 video stream |
| 2 | `ffmpeg -map 0:1` | Extract secondary stream | Isolated hidden video track |
| 3 | `ffmpeg` frame dump | Split video into frames | Flag visible in `frame_0001.png` |

---

## Vulnerabilities / Techniques Covered

- **Steganography via multiple video streams** — a secondary stream can be embedded in a container format (`.mp4`) and is invisible to a standard media player
- **AV1 codec** — used as the carrier for the hidden content; not always decoded by default players, making it easy to overlook

---

## Tools Used

- [ffprobe](https://ffmpeg.org/ffprobe.html) — stream metadata inspection
- [ffmpeg](https://ffmpeg.org) — stream extraction and frame dumping

---
