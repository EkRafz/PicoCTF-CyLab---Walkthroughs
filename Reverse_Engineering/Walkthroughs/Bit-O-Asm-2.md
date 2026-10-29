> Can you figure out what is in the `eax` register? Put your answer in the academy flag format: `academy{n}` where `n` is the contents of the `eax` register in the decimal number base. If the answer was `0x11` your flag would be `academy{17}`. Download the assembly dump [here](https://challenge-files.cylabacademy.net/library/0325d59cb3b29a78bb7872b19f37856d51973b33085362dd4fe5f4cbe7aef80c/disassembler-dump0_b.txt).

---

## Challenge Access

| Field      | Value                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category   | Reverse Engineering                                                                                                                                                      |
| Difficulty | Easy                                                                                                                                                                     |
| Author     | LT 'syreal' Jones                                                                                                                                                        |
| Tags       | x86_64                                                                                                                                                                   |
| Files      | [`disassembler-dump0_b.txt`](https://challenge-files.cylabacademy.net/library/0325d59cb3b29a78bb7872b19f37856d51973b33085362dd4fe5f4cbe7aef80c/disassembler-dump0_b.txt) |

---

## Commands Used

- `cat` → Read the assembly dump
- `printf` → Convert the hex value to decimal

---

## Solution

### Step 1 -- Understand the Problem

We aren't asked to run anything, just to read the assembly and say what ends up in the `eax` register. The flag has to be the value in **decimal**, while disassemblers print constants in hex, so there is a conversion at the end.

The difference from Bit-O-Asm-1 is that the value doesn't go into `eax` directly. It is stored in memory first and loaded from there, so the dump has to be read as two connected instructions.

---

### Step 2 -- Read the Assembly Dump

```bash
$ cat disassembler-dump0_b.txt 
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret
```

Most of the dump is noise (function prologue, epilogue, `pop`/`ret`), and only these two lines touch `eax` or the value it ends up with. `mov` copies the **second** operand into the **first** (Intel syntax), so:

1. `mov DWORD PTR [rbp-0x4],0x9fe1a` → writes the constant `0x9fe1a` into the 4-byte (`DWORD`) memory slot at `rbp-0x4`. `PTR` is a pointer: it means "the value stored at this memory address", not the address itself.
2. `mov eax,DWORD PTR [rbp-0x4]` → reads that same slot back into `eax`.

Both lines use `[rbp-0x4]`, which is how we know the value read in the second line is the one written in the first. So `eax` ends up holding `0x9fe1a`, and nothing after that overwrites it.

---

### Step 3 -- Convert the Value to Decimal

`printf` understands the `0x` prefix, so it can do the conversion directly:

```bash
$ printf "%d\n" 0x9fe1a
<num>
```

Sanity check with the example from the prompt: `0x11` is `1*16 + 1 = 17`, which matches `academy{17}`.

```
academy{<num>}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **A value can reach a register through memory.** Here the constant is first stored at `[rbp-0x4]` and then loaded into `eax`. When the source operand of a `mov` is a memory reference, find the earlier instruction that wrote to that same address.
- **`PTR` means "the contents at this address".** `DWORD PTR [rbp-0x4]` is the 4-byte value stored at `rbp-0x4`, not the address.
- **Match the address to follow the data.** Two instructions using the same `[rbp-0x4]` operand are reading and writing the same variable.
- **Hex is how assembly shows constants, not how answers are asked.** Check the base the flag format requires before submitting, and test your conversion on the prompt's `0x11 → 17` example.