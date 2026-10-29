
[![CC BY 4.0][cc-by-shield]][cc-by]
 
[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg

---
```
 ██████╗██╗   ██╗██╗      █████╗ ██████╗ 
██╔════╝╚██╗ ██╔╝██║     ██╔══██╗██╔══██╗
██║      ╚████╔╝ ██║     ███████║██████╔╝
██║       ╚██╔╝  ██║     ██╔══██║██╔══██╗
╚██████╗   ██║   ███████╗██║  ██║██████╔╝
 ╚═════╝   ╚═╝   ╚══════╝╚═╝  ╚═╝╚═════╝ 

██████╗ ██╗ ██████╗ ██████╗  ██████╗████████╗███████╗
██╔══██╗██║██╔════╝██╔═══██╗██╔════╝╚══██╔══╝██╔════╝
██████╔╝██║██║     ██║   ██║██║        ██║   █████╗  
██╔═══╝ ██║██║     ██║   ██║██║        ██║   ██╔══╝  
██║     ██║╚██████╗╚██████╔╝╚██████╗   ██║   ██║     
╚═╝     ╚═╝ ╚═════╝ ╚═════╝  ╚═════╝   ╚═╝   ╚═╝
```

---

## What is CyLab Academy?

[CyLab Academy](https://learn.cylabacademy.org/) is a security training platform built around **capture-the-flag style challenges**, each one hands you a file, a service, or a scenario and asks you to find a hidden **flag**. The platform was formerly branded **picoCTF**; it's the same platform and the same challenge set under a new name, so this repo treats "picoCTF" and "CyLab" challenges as one continuous set.

Challenges are split by category, each exercising a different corner of security.

---

## How to Use This Repo

The structure of this repo is very similar to [OverTheWire's repo](https://github.com/EkRafz/OverTheWire---Walkthroughs). Each category has its own folder containing an index and one file per challenge. Every writeup follows the same structure: challenge access details, commands used, a step-by-step solution with real terminal output and explanations, and a Key Takeaways section.

**Try the challenge yourself before opening the writeup.**

> **Flags are never published here.** The writeups explain _how_ to find the flag, not what it is.

---

## Categories Index

| Category                                               | Topic                                                       | Status         |
| ------------------------------------------------------ | ----------------------------------------------------------- | -------------- |
| [Cryptography](./Cryptography/README.md)               | RSA, classical ciphers, key recovery, encoding schemes      | 🟡 In Progress |
| [Forensics](./Forensics/README.md)                     | Metadata analysis, file carving, steganography, hidden data | 🟡 In Progress |
| Web Exploitation                                       | Injection, auth bypass, client-side vulnerabilities         | 🔴 Coming Soon |
| Reverse Engineering                                    | Disassembly, decompilation, binary analysis                 | 🔴 Coming Soon |
| [Binary Exploitation](./Reverse_Engineering/README.md) | Memory corruption, stack/heap attacks                       | 🟡 In Progress |
| General Skills                                         | Linux fundamentals, scripting, tool literacy                | 🔴 Coming Soon |
| Blockchain                                             | Smart contract vulnerabilities, chain analysis              | 🔴 Coming Soon |
| Artificial Intelligence                                | AI/ML security, adversarial ML, model exploitation          | 🔴 Coming Soon |

---

## Repository Structure

```
picoctf-cylab-ctfs/
│
├── README.md                      ← You are here
│
├── Cryptography/
│   ├── README.md                  ← Cryptography index and challenge table
│   ├── StegoRSA.md
|	└── ...
│
└── Forensics/
    ├── README.md                  ← Forensics index and challenge table
    ├── Information.md
    ├── Glory-of-the-Garden.md
    ├── Enhance.md
    └── ...
...
...
...
```

New categories will each get their own top-level folder following the same layout.

---

## Useful Resources

| Resource                                         | What it's for                                       |
| ------------------------------------------------ | --------------------------------------------------- |
| [explainshell.com](https://explainshell.com/)    | Break down any shell command flag by flag           |
| [CyberChef](https://gchq.github.io/CyberChef/)   | Encoding, decoding, and data transformation recipes |
| [tldr.sh](https://tldr.sh/)                      | Practical, example-driven man pages                 |
| [CyLab Academy](https://learn.cylabacademy.org/) | Challenge descriptions and hints                    |
| [ShellDex](https://shelldex.ibra.codes/)         | A definitive compendium of terminal commands        |

---

## Licence

[![CC BY 4.0][cc-by-shield]][cc-by]

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

© 2026 [EkRafz](https://github.com/EkRafz). All walkthroughs, explanations, and content in this repository are original work by EkRafz.

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg