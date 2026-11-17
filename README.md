<div align="center">

# 🎮 Underfell

### A fan-made *Undertale* battle game written entirely in **Jack**

*Built from scratch for the Nand2Tetris **Hack** platform — no engine, no sprites, just a 16-bit CPU and a framebuffer.*

<br/>

![Language](https://img.shields.io/badge/Language-Jack-blueviolet?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Hack%20%2F%20VM%20Emulator-black?style=for-the-badge)
![Course](https://img.shields.io/badge/Nand2Tetris-Project%2012-ff69b4?style=for-the-badge)
![License](https://img.shields.io/badge/Use-Educational-green?style=for-the-badge)

</div>

---

> [!WARNING]
> **Fan Project Disclaimer** — This is a **non-commercial fan project** inspired by *Undertale* (created by Toby Fox) All original *Undertale* characters and concepts belong to their respective creators. The Jack source code here is my own work.

---

<a id="toc"></a>

## 📑 Table of Contents

**📖 About** · [Overview](#overview) · [Why Underfell](#why-underfell) · [Tech Stack](#tech-stack)

**📂 Codebase** · [Project Structure](#project-structure) · [Module Roles](#module-roles)

**🎮 Playing** · [How to Run](#how-to-run) · [Controls](#controls)

**🧠 Mechanics** · [Stats](#stats) · [Player Turn](#player-turn) · [Defend Phase](#defend-phase) · [Endings](#endings) · [Overworld](#overworld) · [Seeding](#seeding)

**🧩 Engineering** · [Technical Highlights](#technical-highlights) · [What I Learned](#what-i-learned) · [Roadmap](#roadmap)

**👤 Misc** · [Author](#author) · [Acknowledgments](#acknowledgments)

---

<a id="overview"></a>

## 📖 Overview

A compact **overworld + turn-based bullet-hell battle** that runs on the Hack computer. You walk through a small room, meet **Luzbel** — an aggressive Underfell-style boss — and fight him with the classic Undertale loop:

<div align="center">

**`FIGHT`** · **`ACT`** · **`ITEM`** · **`MERCY`**  
→ followed by a **dodging phase** where you steer the red **Heart** through bullet patterns inside a battle box.

</div>

Everything is implemented **from scratch in Jack** — rendering, animation, input, collision detection, randomness, UI — using only the built-in Jack OS:

<div align="center">

`Screen` · `Keyboard` · `Output` · `Sys` · `Array` · `Math`

</div>

**No external assets. No engine.**

---

<a id="why-underfell"></a>

## 🎨 Why Underfell

The Hack platform is **extremely constrained**:

| Constraint | Detail |
| :--- | :--- |
| CPU | 16-bit |
| Framebuffer | 512 × 256, monochrome |
| Audio | None native |
| Sprites | None built-in |

The **Underfell aesthetic** — dark, red-black, high-contrast — maps naturally onto that. And a **turn-and-dodge** format fits the `Sys.wait` + `Keyboard.keyPressed` main loop much better than a free-roaming action game would.

---

<a id="tech-stack"></a>

## 🛠 Tech Stack

<table align="center">
<thead>
<tr><th>Item</th><th>Detail</th></tr>
</thead>
<tbody>
<tr><td><b>Language</b></td><td><code>Jack</code></td></tr>
<tr><td><b>Target</b></td><td>Hack / VM Emulator</td></tr>
<tr><td><b>Toolchain</b></td><td>Nand2Tetris <i>Jack Compiler</i> + <i>VM Emulator</i></td></tr>
<tr><td><b>OS Dependencies</b></td><td><code>Screen</code>, <code>Keyboard</code>, <code>Output</code>, <code>Sys</code>, <code>Array</code>, <code>Memory</code></td></tr>
<tr><td><b>Course Context</b></td><td>Nand2Tetris Part II — Project 12</td></tr>
</tbody>
</table>

> No `Sprite`, `Math`, or helper libraries are used beyond the built-in OS classes.

---

<a id="project-structure"></a>

## 📂 Project Structure

```text
Underfell-game/
├── Main.jack      // Entry point, title menu, overworld, battle loop, ending logic
├── Room.jack      // Overworld room: layout, drawing, collision rectangles, trigger zones
├── Walker.jack    // Player character sprite in the overworld (33×29 px, sub-pixel scroll)
├── Heart.jack     // The red Heart (8×8 px) — player's soul in the battle box
├── Bullets.jack   // Fixed bullet pool (12), 5 attack patterns, sub-pixel sprites, collision
├── Box.jack       // Battle box boundaries + drawing; provides limits for the Heart
└── Ui.jack        // HP bars, menu labels, typewriter text, boss sprites, flash
<a id="module-roles"></a>
```
## 🔍 Module Roles
<details> <summary><b>Main.jack</b> — Game controller &amp; battle loop</summary>
Static state (hp, bossHp, items, talk, turn, ending, seed), title screen, HOW TO PLAY, overworld walk + interact, playerTurn(), defend(), ending resolution.

</details><details> <summary><b>Room.jack</b> — Overworld environment</summary>
Array-based collision rectangles for bed, TV stand, plant, sign, walls; hits(), nearSign(), nearBoss(); procedural drawing of the room (bricks, windows, ladder/stairs, furniture).

</details><details> <summary><b>Walker.jack</b> — Player sprite in the overworld</summary>
The player sprite with 16 sub-pixel shifts and saved-pixel restoration (draw() saves what was underneath, erase() restores it exactly) — no flicker, no partial erases.

</details><details> <summary><b>Heart.jack</b> — The red soul</summary>
Player's heart. Same sub-pixel trick; moves within Box limits; setLimits() receives them from Box.jack.

</details><details> <summary><b>Bullets.jack</b> — Bullet pool &amp; attack patterns</summary>
Pool of 12 bullets with 8×8 pixel-perfect sprites (16 shifts); 5 patterns keyed by turn; difficulty scales with turn number; collision hits reduce HP.

</details><details> <summary><b>Box.jack</b> — Battle box</summary>
Defines the playfield (words 7..24, rows 100..180) and draws the four border bars.

</details><details> <summary><b>Ui.jack</b> — HUD &amp; sprites</summary>
HP bar for player (3 px/HP), HP bar for boss (4 px/HP), turn label, menu labels (FIGHT ACT ITEM MERCY), typewriter text (say/sayAt), small boss sprite for title/room, big boss sprite for battle, flashBig() for rage flashes, clearBossBig().

</details>
<a id="how-to-run"></a>

## 🚀 How to Run
<table> <tr> <td width="40" align="center" valign="middle">
🥇 Step 1
Get the toolchain

</td> <td valign="middle">
Download the Nand2Tetris software suite from nand2tetris.org.

</td> </tr> <tr> <td align="center" valign="middle">
🥈 Step 2
Clone

</td> <td valign="middle">
bash
git clone https://github.com/Dannyntt/Underfell-game.git
</td> </tr> <tr> <td align="center" valign="middle">
🥉 Step 3
Compile

</td> <td valign="middle">
Open the Jack Compiler (in tools/), load the folder with all .jack files, compile → produces .vm files.

</td> </tr> <tr> <td align="center" valign="middle">
🏃 Step 4
Run

</td> <td valign="middle">
Open the VM Emulator, load the folder with the .vm files, set speed to Fast or Maximum, click Run.

</td> </tr> </table>
[!TIP]
If the compiler complains about missing OS classes, point it at the standard OS/ folder from the toolchain, or use the built-in implementation bundled with the VM Emulator.

[!IMPORTANT]
The VM Emulator at Normal speed is too slow for real-time gameplay on Hack. Always use Fast or Maximum when playing.

<a id="controls"></a>

## 🎮 Controls
Action	Key	Jack code
Move (Heart / Walker)	<kbd>←</kbd> <kbd>↑</kbd> <kbd>→</kbd> <kbd>↓</kbd>	130 / 131 / 132 / 133
Confirm / Interact	<kbd>Z</kbd> or <kbd>Enter</kbd>	90 / 128
Cancel / Back	<kbd>X</kbd> or <kbd>Esc</kbd>	88 / 140
Menu (back to title)	<kbd>Q</kbd>	81
Contextual behavior
<table> <tr> <td width="25%" align="center"><b>Title Screen</b></td> <td>Arrows to pick <b>START / HOW TO PLAY / QUIT</b>, then <kbd>Z</kbd> or <kbd>Enter</kbd>.</td> </tr> <tr> <td align="center"><b>Overworld</b></td> <td>Arrows to walk. <kbd>Z</kbd> near the sign to read it. <kbd>Q</kbd> to go back to the title.</td> </tr> <tr> <td align="center"><b>Battle Menu</b></td> <td>Arrows to move the heart cursor between options. <kbd>Z</kbd> to confirm.</td> </tr> <tr> <td align="center"><b>Dodging Phase</b></td> <td>Arrows move the heart inside the battle box.</td> </tr> </table>
<a id="stats"></a>

## 🧠 Game Mechanics
⚔️ Stats
Actor	HP	Notes
🫀 Player	20	Can be healed by Cake
👹 Luzbel	30	ATK 4 · DEF 0
🍰 Items	Cake × 2	Each restores +10 HP (capped at 20)
The turn counter starts at 1 and drives both difficulty and pattern selection.

<a id="player-turn"></a>

## 🎯 Player Turn — FIGHT / ACT / ITEM / MERCY
<details> <summary><b>⚔️ FIGHT</b> — Timing-based attack minigame</summary>
A vertical bar slides across a slot. Press <kbd>Z</kbd> as close to the center as possible.

Distance from center	Damage
< 8 px	12
< 24 px	9
< 48 px	6
< 96 px	3
≥ 96 px	1
Missed entirely	0
Reduces bossHp.

</details><details> <summary><b>💬 ACT</b> — Check / Talk</summary>
Check → shows "LUZBEL ATK 4 DEF 0".

Talk → increments talk up to 3. This is the pacifist path.

</details><details> <summary><b>🍰 ITEM</b> — Eat a cake</summary>
Restores +10 HP. If you have none, the turn is not consumed.

</details><details> <summary><b>🕊️ MERCY → Spare</b> — Pacifist ending</summary>
Only succeeds if talk > 2 (i.e. you talked 3 times). Otherwise the turn is consumed and the battle continues.

</details>
[!NOTE]
Pressing <kbd>Q</kbd> quits the battle immediately.
Cancelling a submenu with <kbd>X</kbd> / <kbd>Esc</kbd> does not consume the turn — you can pick again.

<a id="defend-phase"></a>

## 💥 Defend Phase — Bullet Hell
Duration: 150 + 30 × level frames, where level = turn (capped at 5).
Pattern for turn N: pattern = turn − 1 (also capped at 4).

<table> <thead> <tr><th align="center">Turn</th><th align="left">Pattern</th><th align="left">Behavior</th></tr> </thead> <tbody> <tr><td align="center"><b>1</b></td><td>🌧️ <b>0 — Rain</b></td><td>Vertical bullets dropping on random lanes</td></tr> <tr><td align="center"><b>2</b></td><td>➡️ <b>1 — Aimed horizontals</b></td><td>Bullets enter from left/right at the heart's row, alternating sides</td></tr> <tr><td align="center"><b>3</b></td><td>🌊 <b>2 — Sweeping curtain</b></td><td>A vertical stream that sweeps lane-by-lane, bouncing at the borders</td></tr> <tr><td align="center"><b>4</b></td><td>🎯 <b>3 — Aimed alternated</b></td><td>Alternates vertical (aimed at the heart's column) and horizontal (aimed at its row)</td></tr> <tr><td align="center"><b>5+</b></td><td>🌀 <b>4 — Chaos</b></td><td>Random mix of vertical and horizontal bullets</td></tr> </tbody> </table>

## 📈 Difficulty Scaling
setLevel(1..5) changes the fire interval and bullet speed:

text
fire interval  =  14 − 2·level
bullet speed   =  1 + level/2 + rnd
Higher turns → more bullets, faster, more aimed attacks.

🎯 Collision
The heart's hurtbox is a 6×6 rectangle inside the 8×8 sprite (x+2..x+7, y+1..y+6).
Each hit costs 1 HP.

😡 Rage Flash
If turn > 2 and bossHp < 30, the big boss sprite is inverted 4 times (Ui.flashBig). Purely cosmetic.

<a id="endings"></a>

## 🎬 Endings
<table> <thead> <tr><th align="center">ID</th><th align="left">Ending</th><th align="left">Trigger</th></tr> </thead> <tbody> <tr><td align="center"><code>1</code></td><td>💀 <b>Defeat</b></td><td>Boss HP reaches 0 → <i>"You defeated Luzbel."</i></td></tr> <tr><td align="center"><code>2</code></td><td>🕊️ <b>Spare</b></td><td>You selected MERCY after 3 Talks → <i>"You spared Luzbel. / Peaceful ending."</i></td></tr> <tr><td align="center"><code>3</code></td><td>☠️ <b>Game Over</b></td><td>Player HP reaches 0 → <i>"GAME OVER"</i></td></tr> </tbody> </table>
[!TIP]
Pressing <kbd>Q</kbd> at any time exits the battle and returns to the title screen without an ending.

<a id="overworld"></a>

## 🏠 Overworld
A short exploration segment: brick-walled room with a bed, a nightstand with a lamp, a TV stand, a plant, a floor lamp, and a sign.

Element	Behavior
Walker hitbox	Feet only — x+5..x+28, y+20..y+28
Collision	Checked against Room rectangles → you can walk in front of furniture, not through it
Sign (read with Z)	"The sign reads: / Only faith will keep you from the fire."
Battle trigger	Walking into the stairs/door frame → "Luzbel blocks your path."
<a id="seeding"></a>

## 🎲 Seeding
seed is set to the total number of Sys.wait-free loops spent on the title screen.

Translation: how long you take to press Start becomes the RNG seed.
Neat touch for replay variety.

<a id="technical-highlights"></a>

## 🧩 Technical Highlights
<div align="center">
The parts I'm most proud of — all hand-rolled in Jack.

</div><table> <tr> <td width="50%" valign="top">
 
## 🖼️ Framebuffer by Hand
Every sprite is a bitpacked array written directly to Screen memory at 16384 + row*32 + word. No Screen.drawPixel calls in the hot path — sprites are OR'd/masked into the framebuffer word by word.

## 🎞️ 16-Offset Sub-Pixel Scrolling
Sprites (Heart, Bullets, Walker) store 16 precomputed copies, one per horizontal sub-pixel shift, so moveLeft / moveRight can shift by 1 px without recomputing the bit pattern.

## 💾 Save / Restore for the Walker
Because the walker overlaps the room's drawn furniture, draw() saves the 87 words it covers into a saved array, and erase() writes them back exactly. No full-screen redraws, no flicker.

## 🎯 Pool-Based Bullet System
Fixed pool of 12 bullets; freeSlot() finds an inactive slot on each spawn. Zero allocations per frame — important because Jack has no garbage collection.

</td> <td width="50%" valign="top">
 
## 🌀 Procedural Bullet Patterns
Five attack patterns (0..4) with tunable spawn interval and velocity.

Pattern 3 aims at the heart's current column/row.

Pattern 2 sweeps lanes with bounce logic.

Pattern 4 mixes randomly.

## ⌨️ Typewriter Text
Ui.say / Ui.sayAt reveal each character after Sys.wait(18) for the Undertale feel, and dispose() the String afterwards — Jack leaks otherwise.

## ⚔️ Timing-Based Attack Minigame
Main.attack() slides a 4-px bar at 6 px/tick and maps the final offset to a damage tier — the whole "hit the center" feel in ~40 lines of Jack.

## 📈 Turn = Difficulty
The boss's bullet-hell phase gets harder and switches patterns each turn, so the fight escalates naturally without a separate difficulty setting.

</td> </tr> </table>
<a id="what-i-learned"></a>

## 📝 What I Learned

<table> <tr> <td width="50%" valign="top">
 
## 🧠 Manual Memory Management
Jack has no generics, no exceptions, no GC. Every String passed to Output.printString must be .dispose()d, every array must be .dispose()d, and every class must .dispose() itself in its own dispose(). Getting this right across Main → Room / Walker / Heart / Box / Bullets / Ui was a real lesson.

## 🖥️ The Screen Is a Real Framebuffer
Drawing via Screen.drawRectangle is convenient but slow. Writing bit patterns directly into 16384 + row*32 + word is orders of magnitude faster and is what makes the sprites animate smoothly in the VM Emulator.

## 🧩 Modular Design Pays Off
The separation between Heart (movement + sub-pixel state), Bullets (pool + patterns), Box (limits + border), and Main (turn logic + endings) meant each piece could be tested and tuned in isolation. Bullets.setPattern() and Bullets.setLevel() were especially clean to tune independently.

</td> <td width="50%" valign="top">
 
## 🎨 Rendering Order Matters
The Walker is drawn after the room and saves/restores the pixels it covers — otherwise moving across a piece of furniture left holes. This one bit me more than once.

 ## 🔒 Constraints Force Good Design
No sprite engine, no fonts, no audio — just Screen, Keyboard, Output, and a 16-bit CPU. Every visual decision was a bit-level one, and every loop had to fit the frame budget of the VM Emulator at Fast.

</td> </tr> </table>
<a id="roadmap"></a>

## 🔭 Roadmap

□ More bullet patterns — spirals, gaps, walls, boss attack phases
□ Sound — Hack has no native audio, but the VM Emulator could be extended
□ Persistence — save a high score / spare-run flag across sessions
□ Performance — reduce per-frame allocations further to stay smooth at Normal speed
□ Fourth ending — e.g. attack → spare (currently strictly kill-or-spare)

<a id="author"></a>

## 👤 Author
@Dannyntt
@vantaflex
@

</div>
📜 License
Source code shared for learning purposes.
Undertale and Underfell belong to their respective creators. No commercial use is intended.

<a id="acknowledgments"></a>

## 🙏 Acknowledgments
<div align="center">
Shimon Schocken & Noam Nisan
for Nand2Tetris and the Jack language

Toby Fox
creator of Undertale, the foundation of this fan project


</div>
<div align="center">
⭐ If you liked this project, drop a star!
Made with ❤️ and a lot of Screen.drawRectangle calls.

</div> ```
