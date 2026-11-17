<div align="center">

# 🎮 Underfell — Luzbel Battle

### A fan-made *Undertale / Underfell* battle game written entirely in **Jack**

*Built from scratch for the Nand2Tetris **Hack** platform — no engine, no sprites, just a 16-bit CPU and a framebuffer.*

<br/>

![Language](https://img.shields.io/badge/Language-Jack-blueviolet?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Hack%20%2F%20VM%20Emulator-black?style=for-the-badge)
![Course](https://img.shields.io/badge/Nand2Tetris-Project%2012-ff69b4?style=for-the-badge)
![License](https://img.shields.io/badge/Use-Educational-green?style=for-the-badge)

</div>

---

> [!WARNING]
> **Fan Project Disclaimer** — This is a **non-commercial fan project** inspired by *Undertale* (created by Toby Fox) and the *Underfell* AU. All original *Undertale* characters and concepts belong to their respective creators. The Jack source code here is my own work.

---

## 📑 Table of Contents

<table>
<tr>
<td valign="top" width="33%">

**📖 About**
- [Overview](#-overview)
- [Why Underfell?](#-why-underfell)
- [Tech Stack](#-tech-stack)

**📂 Codebase**
- [Project Structure](#-project-structure)
- [Module Roles](#-module-roles)

</td>
<td valign="top" width="33%">

**🎮 Playing**
- [How to Run](#-how-to-run)
- [Controls](#-controls)

**🧠 Mechanics**
- [Stats](#-stats)
- [Player Turn](#-player-turn--fight--act--item--mercy)
- [Defend Phase](#-defend-phase--bullet-hell)
- [Endings](#-endings)

</td>
<td valign="top" width="33%">

**🧩 Engineering**
- [Technical Highlights](#-technical-highlights)
- [What I Learned](#-what-i-learned)
- [Roadmap](#-roadmap)

**👤 Misc**
- [Author](#-author)
- [Acknowledgments](#-acknowledgments)

</td>
</tr>
</table>

---

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

## 🎨 Why Underfell?

The Hack platform is **extremely constrained**:

| Constraint | Detail |
| :--- | :--- |
| CPU | 16-bit |
| Framebuffer | 512 × 256, monochrome |
| Audio | None native |
| Sprites | None built-in |

The **Underfell aesthetic** — dark, red-black, high-contrast — maps naturally onto that. And a **turn-and-dodge** format fits the `Sys.wait` + `Keyboard.keyPressed` main loop much better than a free-roaming action game would.

---

## 🛠 Tech Stack

<table align="center">
<thead>
<tr>
<th>Item</th>
<th>Detail</th>
</tr>
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
