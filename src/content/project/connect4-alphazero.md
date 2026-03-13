---
title: 'Connect4 AlphaZero'
description: "Connect 4 AI using AlphaZero's reinforcement learning approach."
publishDate: '2023-10-15'
githubUrl: 'https://github.com/eugeneyp/connect4-alphazero'
---

## Overview

A reinforcement learning agent that masters the game of Connect 4 using the AlphaZero algorithm.

Combines Monte Carlo Tree Search (MCTS) with a dual-headed residual neural network. The large model (5 ResBlocks, 128 filters) beats Minimax depth-11 at 100% and placed **top 10** on the Kaggle Connect X leaderboard using only ~50 MCTS simulations per move within Kaggle's 2-second time limit.

- GitHub: https://github.com/eugeneyp/connect4-alphazero
- Play It Live: https://eugeneyp.github.io/connect4-alphazero/

## Features

- Neural network: 5 Residual Blocks, 128 Filters, ~1.6 million parameters.
- Training: 40,000 self-played games
