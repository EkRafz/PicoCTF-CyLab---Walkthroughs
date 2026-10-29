> Can you get the real meaning from this file. Download the file [here](https://challenge-files.cylabacademy.net/library/0fbf6ae2bc00090132a51cd3a1fdf2be7871a6bb7b54bf8f2b06dfada963e014/enc_flag).

---

## Challenge Access

| Field    | Value                                                                                                                                    |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Category | Cryptography                                                                                                                             |
| Points   | Easy                                                                                                                                     |
| Author   | NGIRIMANA Schadrack                                                                                                                      |
| Tags     | base64, browser_webshell_solvable, caesar                                                                                                |
| Files    | [`enc_flag`](https://challenge-files.cylabacademy.net/library/0fbf6ae2bc00090132a51cd3a1fdf2be7871a6bb7b54bf8f2b06dfada963e014/enc_flag) |

---

## Commands Used

- `cat` → View the encoded contents
- `base64 -d` → Decode each base64 layer
- A Caesar brute-force (dcode.fr, CyberChef, or a quick script) → Recover the final shift

---

## Solution

### Step 1 -- Understand the Problem

The hint, "engaging in various decoding processes is of utmost importance," plus the `base64` and `caesar` tags, is telling us directly that this isn't one layer of obfuscation, it's several stacked on top of each other. The approach is mechanical: decode one layer, look at what comes out, and if it still isn't the flag, figure out what the next layer is and decode that too. Repeat until the flag format appears.

---

### Step 2 -- Decode the First Base64 Layer

```bash
$ cat enc_flag
YidhR3BvYTJ4MFpudHFhR3g2YUhsZmF6TnFlVGwzWVROclh6bHJiVEJxYld4b2ZRPT0nCg==
```

The content of `enc_flag` is one base64-looking string. Decoding it:

```bash
$ echo "<contents of enc_flag>" | base64 -d
b'aGpoa2x0ZntqaGx6aHlfazNqeTl3YTNrXzlrbTBqbWxofQ=='
```

The output is still not the flag, and it's wrapped in `b'...'`, the literal characters of a Python bytes-object representation that got embedded as part of the challenge's obfuscation rather than anything meaningful on its own. Stripping that `b'` prefix and the trailing `'` leaves another clean base64 string underneath.

---

### Step 3 -- Decode the Second Base64 Layer

```bash
$ echo "aGpoa2x0ZntqaGx6aHlfazNqeTl3YTNrXzlrbTBqbWxofQ==" | base64 -d
hjhkltf{jhlzhy_k3jy9wa3k_9km0jmlh}
```

This result has the right _shape_ for a flag, braces, underscores, and a plausible length, but the letters themselves don't spell anything. That's the signature of a Caesar cipher underneath the encoding layers, not a third round of base64: the structure is already correct, only the letters are wrong.

---

### Step 4 -- Brute-Force the Caesar Shift

Running all 26 possible shifts against `hjhkltf{jhlzhy_k3jy9wa3k_9km0jmlh}` (by hand, with a script, or with [dcode.fr's Caesar Cipher solver](https://www.dcode.fr/caesar-cipher)), a shift of 7 is the only one that turns the scrambled prefix into readable text:

```
academy{...}
```


> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Stacked obfuscation is solved one layer at a time.** Don't try to find a single tool or recipe that undoes everything at once, decode, look at the result, and let its shape tell you what the next layer is.
- **Not every leftover character is part of the next layer.** The `b'...'` wrapper here was a literal artifact baked into the obfuscation, not valid base64 itself, so it had to be stripped by hand before the next decode would work.
- **Base64 and substitution ciphers leave different fingerprints.** Base64 output uses the `A-Za-z0-9+/=` alphabet and padding; a Caesar-shifted string keeps the original text's structure (length, punctuation, word/field boundaries) but scrambles the letters themselves. Recognizing which one you're looking at tells you which tool to reach for next.
- **A visible structure is a free hint.** Once braces, underscores, and spacing already look correct, you know you're down to a letter-substitution problem, not more encoding, which narrows the next step immediately.