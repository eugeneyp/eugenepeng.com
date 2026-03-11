---
title: "Chess NanoZero"
description: "AlphaZero-style Chess engine that reached 2000 ELO for under $30."
publishDate: "2026-03-05"
githubUrl: "https://github.com/eugeneyp/chess-nanozero"
---

## Overview

Chess NanoZero is an AlphaZero-style chess engine trained on Lichess Elite games. 

The engine uses Monte Carlo Tree Search (MCTS) guided by a dual-head ResNet neural network — no alpha-beta search, no hand-crafted evaluation. Trained entirely via supervised learning on elite human games. Reached ~**2000 ELO** after **21 hours** of GPU training on **~200K** games.

- Blog: [Building an AlphaZero Chess AI that reaches 2000 ELO](/blog/chess-alphazero)
- GitHub: https://github.com/eugeneyp/chess-nanozero
- Play it live: https://chess-nanozero.onrender.com/

## More Information

- Neural Network: 8 Residual Blocks, 128 Filters, ~3 million parameters.
- Built and trained in 3 days, in partnership with Claude Code.
- Trained on Google Cloud GPU in 21 hours with a cost of $27.
- The engine reached expert level of 2000 ELO (a rating considered top 1% among all chess players).

