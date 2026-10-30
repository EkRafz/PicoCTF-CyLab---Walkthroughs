> Can you figure out what is in the `eax` register? Put your answer in the academy flag format: `academy{n}` where `n` is the contents of the `eax` register in the decimal number base. If the answer was `0x11` your flag would be `academy{17}`. Download the assembly dump [here](https://challenge-files.cylabacademy.net/library/f24a79de39123fb31928d2d4baf2de2385e587d089b95005083051f9afb90fa9/disassembler-dump0_a.txt).

---

## Challenge Access

| Field      | Value                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category   | Reverse Engineering                                                                                                                                                      |
| Difficulty | Easy                                                                                                                                                                     |
| Author     | LT 'syreal' Jones                                                                                                                                                        |
| Tags       | x86_64                                                                                                                                                                   |
| Files      | [`disassembler-dump0_d.txt`](https://challenge-files.cylabacademy.net/library/f24a79de39123fb31928d2d4baf2de2385e587d089b95005083051f9afb90fa9/disassembler-dump0_a.txt) |

---

## Commands Used

- `cat` → Read the assembly dump
- `printf` → Convert the hex value to decimal

---

## Solution

### Step 1 -- Understand the Problem

Same question as the previous levels, but this dump contains a **branch**: a comparison followed by a conditional jump. Only one of the two possible paths actually runs, so the work is deciding which one, and then following only that path. The flag must be the value of `eax` in **decimal**.

---

### Step 2 -- Read the Assembly Dump

```bash
$ cat disassembler-dump0_d.txt
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710
<+29>:    jle    0x55555555514e <main+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65
<+35>:    jmp    0x555555555152 <main+41>
<+37>:    add    DWORD PTR [rbp-0x4],0x65
<+41>:    mov    eax,DWORD PTR [rbp-0x4]
<+44>:    pop    rbp
<+45>:    ret
```

The first five lines are the function prologue and `pop rbp` / `ret` are the epilogue. None of them touch the value we care about. Starting from `<+15>`:

1. `<+15>` stores `0x9fe1a` in the local variable `[rbp-0x4]`.
2. `<+22>` `cmp [rbp-0x4],0x2710` compares that variable with `0x2710`. `cmp` doesn't change either operand, it only sets the CPU flags.
3. `<+29>` `jle` means "jump if less than or equal". It jumps to `<+37>` only if `[rbp-0x4] <= 0x2710`. Here `0x9fe1a` is far larger than `0x2710`, so the jump **is not taken** and execution continues to the next line.
4. `<+31>` `sub [rbp-0x4],0x65` → `0x9fe1a - 0x65 = 0x9fdb5`.
5. `<+35>` `jmp` skips over the `add` at `<+37>` and lands on `<+41>`. This is why the `add` path never runs.
6. `<+41>` loads `[rbp-0x4]` into `eax` → `eax = 0x9fdb5`.

The final value of `eax` is `0x9fdb5`. Had the comparison gone the other way, `jle` would have jumped to `<+37>` and `0x65` would have been **added** instead.

---

### Step 3 -- Convert the Value to Decimal

```bash
$ printf "%d\n" 0x9fdb5
<num>
```

As a cross-check, let the shell redo the whole branch arithmetic from the original constant:

```bash
$ printf "%d\n" $((0x9fe1a - 0x65))
<num>
```

Both commands must print the same number.

```
academy{<num>}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Evaluate the condition before reading further.** With a `cmp` + conditional jump, decide whether the jump is taken, then follow only that path. The instructions on the other path never execute.
- **`cmp` changes nothing but flags.** It doesn't modify its operands. The jump that follows reads those flags.
- **Know your condition codes.** `jle` jumps if the first operand is less than or equal to the second. The mnemonic after the `j` is the comparison, in the order the `cmp` operands were written.
- **An unconditional `jmp` skips code.** The `jmp` at `<+35>` exists to step over the `add` branch, so the "else" path must not run after the "if" path finishes.
- **Verify by computing it twice.** Trace the branch by hand in hex, then let the shell compute the expression with `$(( ))` and compare.