[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kai0x3/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kai0x3/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess — a chess engine in 4 KB of Python

An original chess engine built for the make.fun tiny-chess-01 bounty
("strongest chess engine in 4 KB of Python"). The whole engine is one
3,433-byte main.py: an lzma + base85 packed payload, Python stdlib only,
reading no other file.

Design: 0x88 board, negamax alpha-beta with a transposition table,
iterative deepening at a fixed 120,000-node budget per move, quiescence
search over captures and check evasions, late-move reductions, null-move
pruning, a check extension, MVV-LVA and transposition-move ordering, and
a compact
evaluation — material plus mirrored piece-square tables, a bishop-pair
bonus, with an endgame king table swapped in as the phase drops. It
never uses a clock, so trials replay move for move.

## Tuning (resubmission)

The first version searched 50,000 nodes per move and scored practice
Elo 1191 (above the 1,154 floor, below the ~1,365 house-baseline mean).
This tuned version raises the budget to 120,000 nodes and adds late-move
reductions, a check extension and a bishop-pair bonus, all inside the
same 4,096-byte cap (main.py was 3,345 bytes).

Practice results (kit trials, six games each): mean Elo 1493 over three
trials (1545 / 1654 / 1279) — above the ~1,365 house-baseline mean.
No crashes, illegal moves or time losses in 15 trials; slowest single
move 4,179 ms against the 10,000 ms limit.

A second tuning pass added null-move pruning on top of this version,
keeping the same 120,000-node budget: practice mean Elo 1557 over three
trials (1654 / 1654 / 1364), never worse than the tuned version on any
trial and strictly better on two. main.py is now 3,433 bytes; the
slowest single move across the second pass's trials was 4,135 ms.

## Verifying the engine

main.py is the kit's makefun-compress/v1 packing of the engine source:
one `#coding:l1` line, then `import lzma;exec(lzma.decompress(...))`.
To audit it, decode the string literal and decompress with lzma — the
result is plain Python (0x88 board, alpha-beta, transposition table).
A quick move-generation check: run the source's perft against the
standard positions (startpos depths 1–3 give 20 / 400 / 8902 nodes;
Kiwipete gives 48 / 2039 / 97862). The engine only plays when the
kit sets MAKEFUN_ENV_URL; without it, it exits silently.

## Reading the source

The packed form is the entry; the engine it unpacks is plain Python.
The payload in main.py is one base85 string: decode it with
base64.b85decode and decompress with lzma to read or audit the full
engine source. Nothing is obfuscated beyond the packing the 4 KB
size cap requires, and the unpacked source is what the practice
trials above were played with.
