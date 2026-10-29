```
██████╗ ███████╗██╗   ██╗███████╗██████╗ ███████╗███████╗
██╔══██╗██╔════╝██║   ██║██╔════╝██╔══██╗██╔════╝██╔════╝
██████╔╝█████╗  ██║   ██║█████╗  ██████╔╝███████╗█████╗  
██╔══██╗██╔══╝  ╚██╗ ██╔╝██╔══╝  ██╔══██╗╚════██║██╔══╝  
██║  ██║███████╗ ╚████╔╝ ███████╗██║  ██║███████║███████╗
╚═╝  ╚═╝╚══════╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝╚══════╝╚══════╝
```

---

## What is Reverse Engineering?

Reverse Engineering challenges hand you a compiled artifact (a binary, a disassembly dump, a bytecode file) and ask you to work out what it does without having the source code. The flag is a consequence of the program's behavior: a value left in a register, a string built at runtime, or a check you have to satisfy.

By working through these you get comfortable with reading assembly (instruction syntax, registers, operand order), converting between number bases, and following what a program does by reading it instead of running it.

---

## Challenges

| Challenge                                    | Difficulty | Skills Introduced                                                       | Status |
| -------------------------------------------- | ---------- | ----------------------------------------------------------------------- | ------ |
| [Bit-O-Asm-1](./Walkthroughs/Bit-O-Asm-1.md) | Easy       | x86_64 assembly, Intel syntax `mov` (destination first), hex-to-decimal | ✅      |

---

## Structure

```
Reverse-Engineering/
│
├── README.md               ← You are here
└── Walkthroughs
	├── Bit-O-Asm-1.md
	└── ...
```