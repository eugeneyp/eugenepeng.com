---
title: 'Building a 2000 ELO AlphaZero Chess AI for <$30'
description: 'How I built an AlphaZero-style Chess engine that reached 2000 ELO with less than $30 and in partnership with Claude Code.'
publishDate: '2026-03-10'
tags: ['AI', 'Chess', 'Machine Learning', 'Claude']
---

## Introduction
I set about to discover how easy it would be to build an AlphaZero-style Chess engine as a hobbyist and non-professional programmer. 

The result surpassed my expectations:
- Built and trained in **3 days**, in partnership with Claude Code.
- Trained on Google Cloud GPU in **21 hours** with a cost of **$27**.
- The engine reached expert level of **2000 ELO** (a rating considered **top 1%** among all chess players).

You can play it live here: https://chess-nanozero.onrender.com/
The code is open source on GitHub: https://github.com/eugeneyp/chess-nanozero. You can read the README for more technical details.

The project was not about building the next Chess AI grandmaster. But it proved to me how accessible it is for anyone, even non-professionals, to build cutting edge AI/ML applications.

Building a chess AI was a childhood dream for me. Years ago, I spent a high school summer making the attempt, ultimately failing, and delivering a Checkers AI instead. With Claude Code, I built the basic version (alpha-beta search) that reached 1600 ELO (intermediate level) in one day.

This is what is possible now, with an accelerated level of development that was unimaginable even several years ago.

## Path of Learning
I built up my knowledge by completing a sequence of ever more complex projects:
- Built a Connect 4 AI, driven by alpha-beta search, that reached **30th** on Kaggle's Connect-X leaderboard [[Github](https://github.com/eugeneyp/connectx-ai)]
- Built a Chess AI, driven by the same alpha-beta search, which reached **1600 ELO**. [[GitHub](https://github.com/eugeneyp/chess-ai) | [Play It Live](https://chess-ai-eu2i.onrender.com/)]
- Built a Connect 4 AI, using AlphaZero's reinforcement learning approach. It learned by playing **40,000 games** against itself. It reached **Top 10** on Kaggle's Connect-X leaderboard. [[GitHub](https://github.com/eugeneyp/connect4-alphazero) | [Play It Live](https://eugeneyp.github.io/connect4-alphazero/)]

It finally culminated in building the AlphaZero-style Chess AI.

I did not set about to build the best possible chess engine. I wanted to see what is possible as a hobbyist with constraints on time and budget. I also wanted to learn deeply the concepts and techniques behind the current state of the art of game-playing AI and how to train neural networks.

## What Sets AlphaZero Apart?
In 2017, DeepMind's AlphaZero represented a significant breakthrough for AIs that play zero-sum games like Chess, Go, and Shogi. The prior generations of chess AI won against human players by brute force speed to evaluate way more moves than a human can. However, the same technique for exhaustive search worked less well for Go, a game with many more move possibilities than Chess. Exhaustive search wastes too much time evaluating bad moves, whereas a human grandmaster can use their intuition and experience to quickly narrow down the best moves to evaluate. It's proven difficult to encode this human grandmaster-level intuition as rigid algorithmic rules into an AI engine. AlphaZero changed that. 

The "Zero" in AlphaZero represents starting with zero knowledge. Using a neural network architecture, and given no expert guidance about the game other than the basic rules, AlphaZero taught itself to play by generating millions of games against itself. Through random experimentation, it progressively learned on its own the value of different pieces, strong board positions, solid openings, etc. 

What is also special is that by starting from zero, it bypassed centuries of accumulated human knowledge about chess. It found new moves that never appeared before. It has a dynamic playing style that occasionally goes beyond convention. 

The results were astounding. With just **4 hours** of training, AlphaZero outperformed Stockfish, the world-best chess engine at the time. (Note: the 4-hour training happened on 5000 Google TPUs, which generated 20 million self-playing games within that 4 hour span. This is not a speed that can be duplicated by hobbyists). 

AlphaZero only evaluates **0.1%** of the moves that Stockfish evaluates, but still win. Rather than relying on brute force search, it is starting to approximate the intuitions that underlie human grandmaster play. You can get a sense of it for yourself playing against my live deployment of this project [here](https://chess-nanozero.onrender.com/). Set thinking time to **0**, which prevents the AI from searching ahead, and it still demonstrates expert play just by "intuition" and experience. 

## Why Neural Network?
A neural network is basically a "generalization machine". After having seen a set of data during training, it is fairly good at generalizing the prediction to a wider set of data. Some might argue this is approximating the human ability to extract the learning from one situation into a variety of new situations.

Of course there are limits. Even though the origin of neural network was inspired by theories of how human brains work, the fact is the scientific community still doesn't fully understand how human brains work. Modern neural networks work nothing like human brains, and it is nowhere near Artificial General Intelligence. But we won't get into that here. 

## My Training Method
Without the budget and infrastructure of Google DeepMind, I short-circuited the learning of my AlphaZero clone by training it with ~**200K** human elite-level games. 

Technically, this is called **Supervised Learning** and not the **Reinforcement Learning** approach of the original AlphaZero. Supervised learning is akin to studying under the tutelage of an experienced teacher (in this case, 200K games by elite human players). Reinforcement learning is self-experimentation and requires more time and compute. 

This project uses the same system design as the original AlphaZero—Residual Neural Network (ResNet) and Monte Carlos Tree Search (MCTS)—but took the faster and cheaper path of Supervised Learning. The neural network size is also smaller (3M parameters vs 24M of the original), so that the engine is playable on a laptop without GPUs.

I set up a Google Cloud VM with GPU (NVIDIA L4) at ~**$1.30 / hour**.

The training game set is downloaded from https://database.nikonoel.fr/ (specifically, the ~200K games from November 2025 between players rated 2500+ vs 2300+). 
- The **first** training run used a sample of ~50K games and took **5 hours**. The model already reached **1600 ELO **(strong intermediate play).
- The **second** training run used another ~150K games and took **15 hours**. The model reached **2000 ELO** (expert level). 

ELO rating is estimated from running a tournament between the model and Stockfish.

Total training time: **21 hours**. Total training cost: **$27.** 

## AI Coding Agent Workflow
Everyone seems to be experimenting with AI coding agent workflows. I'm not a professional programmer, but here is the best practice I found so far:

1. **Plan the Project with the Best Model:** Use the best thinking model (Claude Opus) to run deep research to generate a project plan based on my objectives and constraints. Iterate with AI on the project plan until you are satisfied. Get AI to generate a CLAUDE.md to guide the implementation.
2. **Phased Implementation with gate checks:** Delegate the implementation to Claude Code, with the context from CLAUDE.md, to build phase by phase, gated by test cases and acceptance criteria. To save on token cost, use a regular thinking model, Claude Sonnet, instead of Opus.
3. **Review of Critical Code:** This will vary depending on how much you are willing to trust with AI. 100% of code in my projects are generated by Claude Code. Earlier on, I reviewed critical sections of the code, not only to verify and to guide Claude in adding more text cases, but also to reinforce the chess AI theory I was learning. As I progressed and gained more trust in Claude Code, I stopped reviewing code, and relied more on the tests, benchmarks, and manual play to verify. If this is a production-critical application, I imagine a professional programmer would still want to review critical areas of the code.
4. **Accelerated Experimentation and Feedback Cycle**: With implementation no longer as a bottleneck, the cycle time for experimentation and feedback is vastly accelerated.

I imagine different people will come to their own varying levels of trust on the AI agent. I have seen instances of brilliance: Claude Code working through an issue for 45 minutes continuously, debugging an error, until it is resolved. I have also seen instances of common sense mistakes: writing code into a different repo. 

I expect the coding agents to continue to be better. A lot of the time I spent on configuring the experiments during training can all be automated as well. 

AI coding agent really is a game changer for software development. In a way, it is not so different from the increasing levels of abstractions that have occurred in computer programming: from punch cards, to assembly, to low level C, to higher abstraction languages, frameworks, and libraries. AI coding agent is just another level of abstraction that makes application development much faster and simpler. Each person will have to come to their own decision of how much to trust this level of abstraction. Regardless, what I do not recommend: give up human thinking and understanding of how things actually work. You do it at your own peril. AI has no real world understanding. It is a powerful tool for delegation. Maybe someday AI will do it all, but I would not depend on it yet.

## Lessons on Neural Network Training
Training a neural network costs time and money. So the trick is to find the most efficient way to get feedback that you are on the right track. It is a problem of finding the most optimum neural network weights within the constraints of time, cost, and training data set. You want a method to quickly narrow the search.

My lessons so far:
1. **Verify training pipeline using the smallest model**: before you conduct a full training run, use a small model to verify that the training pipeline works on your local laptop, without having to incur GPU expenses yet. This allows you to quickly fix any training script / pipeline issues.
2. **Optimize training speed early on using a quick run: **Use a medium or full-sized model to do a quick run on a GPU, measure the speed, then ask coding agent to optimize the speed. Experiment and repeat. This will save tons of time later. 
3. **Gain early and quick intuitions** by experimenting with smaller models before committing on the optimum approach.
4. **Monitor the training run to improve the next run**. This is especially helpful early on. Watch out for signs if performance / loss curve is plateauing, or if there are signs of overfitting / underfitting. It gives me opportunities to adjust hyper parameters or add more training data, before committing to more training time.

The overarching principle: use the smallest effort / cost / time to learn and gain feedback on whether you are on the right path. 

## Conclusions
Development of software applications, even cutting edge AI/ML models, have never been easier and faster with the abundance of community resources and AI coding agent. As a non-professional, I can build and train an expert-level Chess AI in 3 days and under $30. AI coding agents are expected to continue to improve, moving our work as humans to higher and higher levels of abstraction and coordination.

## References
**Blogs / Papers / Books**
- Silver et al. (2018). Blog. "AlphaZero: Shedding new light on chess, shogi, and Go".  https://deepmind.google/blog/alphazero-shedding-new-light-on-chess-shogi-and-go/
- Silver et al. (2018). Research Paper.  "Mastering Chess and Shogi by Self-Play with a General Reinforcement Learning Algorithm". https://arxiv.org/abs/1712.01815
- Klein, Dominik. (2023). Book. "Neural Network for Chess". https://github.com/asdfjkl/neural_network_chess
**Projects**
- [Leela Chess Zero (Lc0)](https://github.com/LeelaChessZero/lc0) — open-source AlphaZero for chess; architecture reference
**Data**
- [Lichess Elite Database](https://database.nikonoel.fr/) by Nikonoel — 2200+/2400+ rated games, free to download
- [UHO_4060_v4 opening book](https://github.com/official-stockfish/books) — balanced opening positions for engine testing













