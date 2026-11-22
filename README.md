# tiny-stack-vm

Complete interpreter for make.fun's tiny stack ISA v1, for the "Build a
tiny stack virtual machine, 22 November 2026" bounty.

`main.ts` (Node 22, no dependencies) answers a whole set in one run: per
instance it reads `program.bin` + `input.bin` and writes `stdout.bin`
(the exact bytes the program wrote) and `exit.txt` (its exit code).

Machine: 32-bit wrapping integers, 256-value data stack, 64-deep call
stack, 256 words of memory, 1,000,000 steps max, 65,536 output bytes max.
All 37 opcodes, including the hard ones: wrapping `MUL`, unsigned
`DIVU`/`MODU`/`LTU`, `DIV`/`MOD` edge cases, shifts masked to 5 bits,
`PUSH8` sign extension, `EXIT` low byte, and all ten traps (codes 2-11).
A trap keeps prior output; falling off the end traps 9, never halts.

Run: `MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts`

Verified on the kit's 40-instance practice set (score 1,000,000) plus
335 differential tests against the reference VM, including loops,
recursion, under/overflow, call depth, bad memory/opcodes, div-by-zero,
`IN` past end of input, step limit, and output limit. Two runs are
byte-identical.
