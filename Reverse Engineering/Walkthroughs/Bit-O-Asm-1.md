> Can you figure out what is in the `eax` register? Put your answer in the academy flag format: `academy{n}` where `n` is the contents of the `eax` register in the decimal number base. If the answer was `0x11` your flag would be `academy{17}`. Download the assembly dump [here](https://challenge-files.cylabacademy.net/library/f24a79de39123fb31928d2d4baf2de2385e587d089b95005083051f9afb90fa9/disassembler-dump0_a.txt).

---

## Challenge Access

| Field      | Value                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category   | Reverse Engineering                                                                                                                                                      |
| Difficulty | Easy                                                                                                                                                                     |
| Author     | LT 'syreal' Jones                                                                                                                                                        |
| Tags       | x86_64                                                                                                                                                                   |
| Files      | [`disassembler-dump0_a.txt`](https://challenge-files.cylabacademy.net/library/f24a79de39123fb31928d2d4baf2de2385e587d089b95005083051f9afb90fa9/disassembler-dump0_a.txt) |

---

## Commands Used

- `cat` → Read the assembly dump
- `printf` → Convert the hex value to decimal

---

## Solution

### Step 1 -- Understand the Problem

We aren't asked to run anything or crack anything, just to read a few lines of assembly and say what ends up in the `eax` register. The on-screen hint gives the one rule needed: `mov` copies the **second** operand into the **first**. That's Intel syntax, where the destination comes first and the source second, so `mov eax, 0x30` means "put `0x30` into `eax`".

The flag format adds one extra requirement: the value has to be submitted in **decimal**, not hex. Disassemblers print immediate values in hex, so there is a conversion at the end.

---

### Step 2 -- Read the Assembly Dump

```bash
$ cat disassembler-dump0_a.txt
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```

x86_64 functions start with a prologue (`push rbp` / `mov rbp, rsp`) and end with an epilogue (`pop rbp` / `ret`). None of those touch `eax`, so they can be skipped. The only line that matters is the `mov` whose first operand is `eax`:

```
mov    eax,0x30
```

Since `mov` writes the second operand into the first, `eax` holds `<HEX>`. Nothing after that line overwrites it, so that is the final value.

---

### Step 3 -- Convert the Value to Decimal

The challenge wants the decimal base, so `<HEX>` has to be converted. `printf` can do it directly, because it understands `0x` prefixes:

```bash
$ printf "%d\n" 0x30
<num>
```

```
academy{<num>}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Know the operand order of the syntax you're reading.** Intel syntax is `mov dest, src`. Reading a dump backwards gives you the wrong register contents.
- **Only instructions that write to the register matter.** Prologue and epilogue lines (`push`, `pop`, `ret`) can be skipped when the question is about a single register.
- **Hex is how assembly shows constants, not how answers are asked.** Always check the base the flag format requires before submitting.