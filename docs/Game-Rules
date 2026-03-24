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
