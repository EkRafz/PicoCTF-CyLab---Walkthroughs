> Decrypt this message. Message: [message](https://challenge-files.cylabacademy.net/library/297f472a962ab45ee79af5ff4d616ba5206c85d5f4f5cfd41109602354d4121a/data.enc).

---

## Challenge Access

| Field    | Value                                                                                                                                                                  |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Category | Cryptography                                                                                                                                                           |
| Points   | Medium                                                                                                                                                                 |
| Author   | Sanjay C/Daniel Tunitis                                                                                                                                                |
| Files    | Decrypt this message. Message: [`message`](https://challenge-files.cylabacademy.net/library/297f472a962ab45ee79af5ff4d616ba5206c85d5f4f5cfd41109602354d4121a/data.enc) |

---

## Commands Used

- [dcode.fr Caesar Cipher solver](https://www.dcode.fr/caesar-cipher) → Automated brute-force / auto-detect of the shift

---

## Solution

### Step 1 -- Understand the Problem

A Caesar cipher shifts every letter in the plaintext forward by a fixed amount in the alphabet (wrapping around from `z` back to `a`), so `a` becomes `d`, `b` becomes `e`, and so on for a shift of 3. It's one of the oldest and weakest ciphers, since there are only 26 possible shifts total (including a shift of 0, which does nothing). That small a keyspace means there's no need to be clever about finding the right one, you can just try every single one and read off whichever result looks like English, or let a tool do that for you.

---

### Step 2 -- Download and Open the Message

The challenge provides the ciphertext as a downloadable file (Decrypt this message. Message: [`message`](https://challenge-files.cylabacademy.net/library/297f472a962ab45ee79af5ff4d616ba5206c85d5f4f5cfd41109602354d4121a/data.enc)) rather than inline text. Opening it gives a block of letters with no obvious structure, consistent with a substitution cipher rather than an encoding like base64.

---

### Step 3 -- Brute-Force the Shift with dcode.fr

Instead of scripting the brute force, paste the ciphertext directly into [dcode.fr's Caesar Cipher solver](https://www.dcode.fr/caesar-cipher). The tool tries all 26 possible shifts automatically, so it saves having to write a custom python script.

The correct shift decodes the message into the flag directly, recognizable by its `academy{...}` wrapper.

```
academy{...}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **A tiny keyspace means brute force beats cryptanalysis.** With only 26 possible shifts, there's no reason to try to deduce the key mathematically, a full brute force is cheap enough to just run.
- **A purpose-built tool beats writing your own script for a well-known cipher.** dcode.fr already implements the brute force, so there's no need to reinvent it for a standard Caesar cipher.
- **Recognizing English is itself the terminating condition.** There's no need to know the shift in advance, correctness is self-evident once the tool (or your own eyes) spot readable text among the candidates.