```
 ██████╗██████╗ ██╗   ██╗██████╗ ████████╗ ██████╗
██╔════╝██╔══██╗╚██╗ ██╔╝██╔══██╗╚══██╔══╝██╔═══██╗
██║     ██████╔╝ ╚████╔╝ ██████╔╝   ██║   ██║   ██║
██║     ██╔══██╗  ╚██╔╝  ██╔═══╝    ██║   ██║   ██║
╚██████╗██║  ██║   ██║   ██║        ██║   ╚██████╔╝
 ╚═════╝╚═╝  ╚═╝   ╚═╝   ╚═╝        ╚═╝    ╚═════╝
```

---

## What is Cryptography?

Cryptography challenges center on encryption schemes, encoding, and key material, recovering a lost key, breaking a weak cipher, or decrypting a message once you've found what you need to decrypt it with. Unlike Forensics, the "finding" step is often the easy part; the real work is understanding the underlying crypto well enough to exploit a mistake in how it was used.

By working through these you'll get comfortable with asymmetric crypto fundamentals (RSA key structure and validation), classical and modern encoding schemes, and the `openssl` command-line toolkit.

---

## Challenges

| Challenge                              | Difficulty | Skills Introduced                                                                   | Status |
| -------------------------------------- | ---------- | ----------------------------------------------------------------------------------- | ------ |
| [StegoRSA](./Walkthroughs/StegoRSA.md) | EASY       | `exiftool`, hex-to-ASCII decoding, RSA key validation and decryption with `openssl` | ✅      |

---

## Structure

```
Cryptography/
│
├── README.md               ← You are here
└── Walkthroughs	
	├── StegoRSA.md
	└── ...
```