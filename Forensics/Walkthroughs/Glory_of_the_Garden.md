> This file contains more than it seems. Get the flag from [garden.jpg](https://challenge-files.cylabacademy.net/library/3c60c5a7f7a8067606c51d4b4b3c7913fae709b11e2f2c7adbc0a0b4cda452ff/garden.jpg).

---

## Challenge Access

| Field      | Value                                                                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Category   | Forensics                                                                                                                                    |
| Difficulty | Easy                                                                                                                                         |
| Author     | jedavis/Danny                                                                                                                                |
| Files      | [`garden.jpg`](https://challenge-files.cylabacademy.net/library/3c60c5a7f7a8067606c51d4b4b3c7913fae709b11e2f2c7adbc0a0b4cda452ff/garden.jpg) |

---

## Commands Used

- `file` → Confirm the actual file type
- `strings` → Pull printable text out of the binary
- `grep` → Filter for the flag pattern

---

## Solution

### Step 1 -- Understand the Problem

The problem says the file "contains more than it seems," and the hint nudges toward a hex editor. That pattern (a normal-looking image, hint pointing at raw bytes) usually means one of two things: metadata hiding (covered by the "information" challenge) or data appended/hidden after the actual image stream ends. Since there's no obviously suspicious metadata field here, the second case is more likely: something was tacked onto the file after the JPEG's own data, which doesn't affect how the image renders.

---

### Step 2 -- Confirm the File Type

```bash
$ file garden.jpg
garden.jpg: JPEG image data, JFIF standard 1.01
```

Confirms it's a genuine JPEG and not a renamed archive or other container, so the "something extra" is appended to, or embedded within, valid JPEG data rather than the file being a different format wearing a `.jpg` extension.

---

### Step 3 -- Look for Appended Text

You could open this in a hex editor and scroll to the end looking for an `FF D9` (JPEG end-of-image marker) followed by plain text, but `strings` gets there faster: it scans the whole file for printable character sequences, images included, so if a flag was appended as plain text it shows up directly.

```bash
$ strings garden.jpg | grep pico
Here is a flag academy{...}
```

`strings` pulls every run of printable characters out of the binary; piping to `grep academy` filters that firehose down to the one line that matters, since `picoCTF{` is a predictable, greppable marker.

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Not every hidden flag needs structure-aware tool.** Where [StegoRSA](StegoRSA.md) needed `exiftool` to target a specific metadata field, this one is simpler: the flag is just plain text sitting in the file, so a blunt text scan finds it immediately.
- **`strings` + `grep` is a fast first pass.** Before reaching for a hex editor (useful for precise, byte-level inspection) try `strings <file> | grep <expected_pattern>`, it costs nothing and catches the common case of flags hidden as literal appended text.
- **A valid file header doesn't guarantee a "clean" file.** `file` confirming "JPEG image data" only validates the start of the file; data appended after the image stream ends is invisible to anything that renders the image but still sits there in the raw bytes.