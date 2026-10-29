```
███████╗ ██████╗ ██████╗ ███████╗███╗   ██╗███████╗██╗ ██████╗███████╗
██╔════╝██╔═══██╗██╔══██╗██╔════╝████╗  ██║██╔════╝██║██╔════╝██╔════╝
█████╗  ██║   ██║██████╔╝█████╗  ██╔██╗ ██║███████╗██║██║     ███████╗
██╔══╝  ██║   ██║██╔══██╗██╔══╝  ██║╚██╗██║╚════██║██║██║     ╚════██║
██║     ╚██████╔╝██║  ██║███████╗██║ ╚████║███████║██║╚██████╗███████║
╚═╝      ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝╚══════╝╚═╝ ╚═════╝╚══════╝
```

---

## What is Forensics?

Forensics challenges hand you a file (an image, a document, a packet capture, a disk image) and ask you to recover something hidden inside it. The flag is never in plain sight, it's buried in metadata, appended after the file's real data ends, split across invisible markup, or hidden using actual steganographic techniques.

By working through these you get comfortable with metadata inspection (`exiftool`), raw text/byte scanning (`strings`, `xxd`, hex editors), and recognizing the "shape" of hidden data (base64, hex encoding, appended content) on sight.

---

## Challenges

| Challenge                                                    | Difficulty | Skills Introduced                                                            | Status |
| ------------------------------------------------------------ | ---------- | ---------------------------------------------------------------------------- | ------ |
| [Information](./Walkthroughs/Information.md)                 | Easy       | `exiftool`, spotting and decoding base64 in metadata fields                  | ✅      |
| [Glory of the Garden](./Walkthroughs/Glory_of_the_Garden.md) | Easy       | `file`, `strings` + `grep`, data appended after a file's real stream         | ✅      |
| [Enhance!](./Walkthroughs/Enhance!.md)                       | Medium     | `strings` on text-based (SVG) formats, invisible DOM text, CyberChef cleanup | ✅      |

---

## Structure

```
Forensics/
│
├── README.md               ← You are here
└── Walkthroughs
	├── Information.md
	├── Glory-of-the-Garden.md
	├── Enhance.md
	└── ...
```