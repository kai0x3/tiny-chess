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
