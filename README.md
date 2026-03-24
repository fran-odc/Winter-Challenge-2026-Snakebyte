# 🐍 Winter Challenge 2026 - Exotec Snakebots

# 🎯 Goal
Collect energy to grow your robot snakes and have the longest ones at game end.

[📂 Official Contest Website](https://www.codingame.com/contests/winter-challenge-2026-exotec) 

[📂 Game Rules](https://www.codingame.com/contests/winter-challenge-2026-exotec) 

# 🏗️ Grid & Platforms

✅ Side-view grid with impassable platforms  

✅ Cells: empty | platform | snakebot body | energy

✅ Gravity: snakebot falls if no solid support

✅ Solid surfaces: platforms + energy + other snakebots


# 🐍 Snakebots

✅ Multi-adjacent segments (head + body)

✅ Simultaneous movement for all snakebots

✅ Initial direction: UP

✅ Continue previous direction by default


# ↪️ Movement Mechanics

- **Case 1**: Head collision

   - **Platform or body** → head destroyed (≥3 segments)

  - **< 3 segments** → snakebot eliminated

- **Case 2**: Energy

  ✅ Snakebot grows (new tail segment)

  ✅ Energy disappears (cell = empty after)

  ✅ Multiple heads = all eat (shared energy!)

  
# Phase Gravity

- **After collisions** → all fall until solid support
- **Out of bounds** = elimination
  

# 🎮 Commands

| Commands | Stdout |
| --------- | ------ |
| **Directions** | `id UP \| DOWN \| LEFT \| RIGHT` |
| **Debug** | `id RIGHT Debug text` |
| **Marker** | `MARK x y` (max 4/turn) |
| **Pause** | `WAIT` |
| **Separator** | `;` |

Directions: UP(0,-1) \| DOWN(0,1) \| LEFT(-1,0) \| RIGHT(1,0)


# ⛔  Game End

❌ All snakebots eliminated

❌ No energy left

❌ 200 turns max


# 🏆 Victory/Defeat

✅ **VICTORY**: Most total segments

❌ **DEFEAT**: Timeout \| invalid commands


# 🔧 Debugging

🔸 MARK x y (viewer)

🔸 Hover grid (cell info)

🔸 ⚙️ Viewer options

🔸 Keyboard: SPACE (pause) \| Arrow keys (frame by frame)

## 🧠 Solution Approach

- Iterative improvement using insights from Vertex AI, Claude, and ChatGPT.
- Identified key weaknesses through arena losses, implemented targeted fixes via 3 PRs directly on GitHub web interface.

## 🛠️ Tech Stack

Python 3.11
- No external dependencies
- Grid simulation + BFS pathfinding

## 🚀 How to Run & Test

**Arena deployment:**
- Copy code to official arena → battle other players → analyze losses
- Paste losing code into GitHub web editor → reproduce scenarios → fix weaknesses

**Testing:**
- Arena validation: Replayed 10 lost matches, confirmed win rate improvement
- Integration: Multi-snake coordination

**Validation:** 
- Arena: Replayed 10 lost matches → confirmed through win rate improvement
- Integration: Multi-snake coordination validated via scenario replay

## ⚖️ Design Trade-offs

- **Correctness > Speed**: Full validation before moves (safer)
- **Simple opponent model**: Predicts 1-2 moves ahead (fast, good enough for most races)
- **Risky moves allowed**: Scores high-reward positions despite temporary gravity risk
- **Team coordination**: Greedy team scoring (scales better than full combinatorial search)
- **No runtime ML**: Development used AI tools, but gameplay runs pure algorithms (fits per-turn time limits)
- **Web-only workflow**: All development via GitHub web editor (no local setup required)
