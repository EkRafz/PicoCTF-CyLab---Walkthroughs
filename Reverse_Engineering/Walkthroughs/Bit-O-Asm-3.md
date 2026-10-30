> Can you figure out what is in the `eax` register? Put your answer in the academy flag format: `academy{n}` where `n` is the contents of the `eax` register in the decimal number base. If the answer was `0x11` your flag would be `academy{17}`. Download the assembly dump [here](https://challenge-files.cylabacademy.net/library/f16508f6e8ba4c520e7541081aee169bf4f5cab84d6852b697299b8cf4aff76a/disassembler-dump0_c.txt).

---

## Challenge Access

| Field      | Value                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category   | Reverse Engineering                                                                                                                                                      |
| Difficulty | Easy                                                                                                                                                                     |
| Author     | LT 'syreal' Jones                                                                                                                                                        |
| Tags       | x86_64                                                                                                                                                                   |
| Files      | [`disassembler-dump0_c.txt`](https://challenge-files.cylabacademy.net/library/f16508f6e8ba4c520e7541081aee169bf4f5cab84d6852b697299b8cf4aff76a/disassembler-dump0_c.txt) |

---

## Commands Used

- `cat` → Read the assembly dump
- `printf` → Do the arithmetic and convert the result to decimal

---

## Solution

### Step 1 -- Understand the Problem

Same question as the previous two levels, but now `eax` isn't just loaded with a constant. It goes through arithmetic (`imul` and `add`), so the final value has to be computed by following the instructions in order. The flag must be the result in **decimal**.

---

### Step 2 -- Read the Assembly Dump

```bash
$ cat disassembler-dump0_c.txt 
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a
<+22>:    mov    DWORD PTR [rbp-0x8],0x4
<+29>:    mov    eax,DWORD PTR [rbp-0xc]
<+32>:    imul   eax,DWORD PTR [rbp-0x8]
<+36>:    add    eax,0x1f5
<+41>:    mov    DWORD PTR [rbp-0x4],eax
<+44>:    mov    eax,DWORD PTR [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```

As before, `mov` copies the second operand into the first, and `[rbp-0x..]` are local variables on the stack. Reading it top to bottom and tracking `eax`:

1. `<+15>` and `<+22>` store two constants in memory: `[rbp-0xc] = 0x9fe1a` and `[rbp-0x8] = 0x4`.
2. `<+29>` loads the first one into `eax` → `eax = 0x9fe1a`.
3. `<+32>` `imul eax,[rbp-0x8]` multiplies `eax` by the second variable and stores the result back in `eax` → `eax = 0x9fe1a * 0x4 = 0x27f868`.
4. `<+36>` `add eax,0x1f5` → `eax = 0x27f868 + 0x1f5 = 0x27fa5d`.
5. `<+41>` saves `eax` into `[rbp-0x4]`, and `<+44>` loads it straight back, so `eax` is unchanged. This is the compiler storing the result in a variable and then reading it for the return.

The final value of `eax` is `0x27fa5d`.

---

### Step 3 -- Convert the Value to Decimal

`printf` understands `0x` prefixes and can do the arithmetic directly, which also lets you double-check the manual steps:

```bash
$ printf "%d\n" $((0x9fe1a * 0x4 + 0x1f5))
<num>
$ printf "%d\n" 0x27fa5d
<num>
```

Both commands must print the same number. Sanity check with the example from the prompt: `0x11` is `1*16 + 1 = 17`, which matches `academy{17}`.

```
academy{<num>}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Track `eax` line by line.** Registers get overwritten, so the answer is the value after the last instruction that writes to it, not the first constant you see.
- **Know what each instruction does to its first operand.** `mov` replaces it, `imul` multiplies it, `add` adds to it. In all three the result lands in the first operand.
- **Store-then-load pairs change nothing.** `mov [rbp-0x4],eax` followed by `mov eax,[rbp-0x4]` just moves the value through memory and back.
- **Verify by computing it twice.** Do the arithmetic step by step in hex, then let the shell compute the whole expression with `$(( ))`. If the two results differ, you misread an instruction.