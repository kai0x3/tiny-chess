[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kai0x3/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kai0x3/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess — a chess engine in 4 KB of Python

An original chess engine built for the make.fun tiny-chess-01 bounty
("strongest chess engine in 4 KB of Python"). The whole engine is one
4,009-byte main.py: an lzma + base85 packed payload, Python stdlib only,
reading no other file.

Design: 0x88 board, negamax alpha-beta with a transposition table,
quiescence search over captures and check evasions, MVV-LVA and
transposition-move ordering, and a compact evaluation — material plus
mirrored piece-square tables, with an endgame king table swapped in as
the phase drops. It searches a fixed 50,000-node budget per move and
never uses a clock, so trials replay move for move.

Practice results (kit trials 0, 1 and 2, six games each): Elo 1191 on
every trial — 2 wins, 1 draw, 3 losses per trial — above the 1,154
practice floor. Wins came against the depth-2, depth-3 and depth-4
house engines; draws against depth-4, nodes-250k and nodes-500k.

## Reading the source

The packed form is the entry; the engine it unpacks is plain Python.
The payload in main.py is one base85 string: decode it with
base64.b85decode and decompress with lzma to read or audit the full
engine source. Nothing is obfuscated beyond the packing the 4 KB
size cap requires, and the unpacked source is what the practice
trials above were played with.
