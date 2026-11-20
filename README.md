# tiny-chess-4k-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zehadfall69/tiny-chess-4k-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zehadfall69/tiny-chess-4k-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python, written for make.fun's
"Build the strongest chess engine in 4 KB of Python" bounty. `main.py` is
the whole engine: it talks to the competition env over HTTP
(`POST /game`, `POST /move`) and plays the house gauntlet.

Inside the byte budget:

- 0x88 board, full legal move generation (castling, en passant,
  promotions), incremental evaluation and Zobrist hashing
- Negamax alpha-beta search with PVS, quiescence (with check evasions),
  a transposition table, null-move pruning, late-move reductions and
  check extensions
- Move ordering: transposition-table move, MVV-LVA captures, killers,
  history heuristic
- Evaluation: material, piece-square tables and the bishop pair
- A fixed node budget — a per-game purse of nodes, spent faster in the
  opening and slower near the ply cap — never a clock, so every run
  plays exactly the same moves

The committed `main.py` is the readable source packed by the kit's
`makefun compress` (lzma + base85); the unpacker and payload together
stay under the 4,096-byte cap. Perft-verified against known node counts
(start position, Kiwipete and the standard test suite) and against
python-chess move lists.

## Run it

The engine plays through the env the harness serves it:

```sh
MAKEFUN_ENV_URL=http://localhost:PORT python3 main.py
```

To grade it the way the server does, use the competition kit:

```sh
makefun grade-local --kit . --bot <this repository> --driver docker
```

---

Entered on [make.fun](https://make.fun/?ref=1d1a57z4&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards
