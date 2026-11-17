Underfell — Luzbel Battle
A fan-made Undertale inspired RPG battle game written entirely in Jack for the Nand2Tetris Hack platform.

📖 About
A compact overworld + turn-based bullet-hell battle that runs on the Hack computer. You walk through a small room, meet Luzbel — an aggressive Underfell-style boss — and fight him with the classic Undertale loop: FIGHT / ACT / ITEM / MERCY, followed by a dodging phase where you steer the red Heart through bullet patterns inside a battle box.

Everything is implemented from scratch in Jack — rendering, animation, input, collision detection, randomness, UI — using only the built-in Jack OS (Screen, Keyboard, Output, Sys, Array, Math). No external assets, no engine.

Why Underfell? The Hack platform is extremely constrained (16-bit CPU, 512×256 monochrome framebuffer, no native audio). The Underfell aesthetic — dark, red-black, high-contrast — maps naturally onto that, and a turn-and-dodge format fits the Sys.wait + Keyboard.keyPressed main loop much better than a free-roaming action game would.

🛠 Tech Stack
Item	Detail
Language	Jack
Target	Hack / VM Emulator
Toolchain	Nand2Tetris Jack Compiler + VM Emulator
OS Dependencies	Screen, Keyboard, Output, Sys, Array, Memory
Course Context	Nand2Tetris Part II, Project 12
No Sprite, Math, or helper libraries are used beyond the built-in OS classes.

📂 Project Structure
text
Underfell-game/
├── Main.jack      // Entry point, title menu, overworld, battle loop, ending logic
├── Room.jack      // Overworld room: layout, drawing, collision rectangles, trigger zones
├── Walker.jack    // Player character sprite in the overworld (33×29 px, sub-pixel scroll)
├── Heart.jack     // The red Heart (8×8 px) — player's soul in the battle box
├── Bullets.jack   // Fixed bullet pool (12), 5 attack patterns, sub-pixel sprites, collision
├── Box.jack       // Battle box boundaries + drawing; provides limits for the Heart
└── Ui.jack        // HP bars, menu labels, typewriter text, boss sprites (small & big), flash
Module roles:

Main.jack — Static state (hp, bossHp, items, talk, turn, ending, seed), title screen, HOW TO PLAY, overworld walk + interact, playerTurn(), defend(), ending resolution.

Room.jack — Array-based collision rectangles for bed, TV stand, plant, sign, walls; hits(), nearSign(), nearBoss(); procedural drawing of the room (bricks, windows, ladder/stairs, furniture).

Walker.jack — The player sprite with 16 sub-pixel shifts and saved-pixel restoration (draw saves what was underneath, erase restores it exactly) — no flicker, no partial erases.

Heart.jack — Player's heart. Same sub-pixel trick; moves within Box limits; setLimits() receives them from Box.jack.

Bullets.jack — Pool of 12 bullets with 8×8 pixel-perfect sprites (16 shifts); 5 patterns keyed by turn; difficulty scales with turn number; collision hits reduce HP.

Box.jack — Defines the playfield (words 7..24, rows 100..180) and draws the four border bars.

Ui.jack — HP bar for player (3 px / HP), HP bar for boss (4 px / HP), turn label, menu labels (FIGHT ACT ITEM MERCY), typewriter text (say/sayAt), small boss sprite for title/room, big boss sprite for battle, flashBig() for rage flashes, clearBossBig().

🚀 How to Run
Get the Nand2Tetris software suite from nand2tetris.org.

Clone this repo:

bash
git clone https://github.com/Dannyntt/Underfell-game.git
Open the Jack Compiler (in tools/), load the folder with all .jack files, compile. It produces .vm files.

Open the VM Emulator, load the folder with the .vm files.

Set speed to Fast or Maximum — Normal is too slow for real-time gameplay on Hack.

Click Run.

Tip: if the compiler complains about missing OS classes, point it at the standard OS/ folder from the toolchain, or use the built-in implementation bundled with the VM Emulator.

🎮 Controls
Action	Key	Jack code
Move (Heart / Walker)	← ↑ → ↓	130 / 131 / 132 / 133
Confirm / Interact	Z or Enter	90 / 128
Cancel / Back	X or Esc	88 / 140
Menu (back to title)	Q	81
On the title screen: arrows to pick START / HOW TO PLAY / QUIT, then Z or Enter.
In the overworld: arrows to walk, Z near the sign to read it, Q to go back to the title.
In battle: arrows to move the heart menu cursor, Z to confirm; during the dodging phase, arrows move the heart inside the box.

🧠 Game Mechanics
Stats
Player HP: 20

Luzbel HP: 30 (ATK 4, DEF 0)

Items: Cake × 2 (each restores +10 HP, capped at 20)

Turn counter starts at 1 and drives both difficulty and pattern selection

Player turn — FIGHT / ACT / ITEM / MERCY
FIGHT — A vertical bar slides across a slot; press Z as close to the center as possible. Damage scales with distance from center: 12 / 9 / 6 / 3 / 1, or 0 if you miss entirely. Reduces bossHp.

ACT → Check — shows "LUZBEL ATK 4 DEF 0".

ACT → Talk — increments talk up to 3. This is the pacifist path.

ITEM — eat a cake (+10 HP). If you have none, the turn is not consumed.

MERCY → Spare — only succeeds if talk > 2 (talked 3 times). Otherwise the turn is consumed and the battle continues.

Q — quit the battle immediately.

If you cancel a submenu (X / Esc) the turn is not consumed and you can pick again.

Defend phase — bullet hell
Duration: 150 + 30 × level frames, where level = turn (capped at 5).

The pattern for turn N is pattern = turn − 1 (also capped at 4). The 5 patterns are:

Turn	Pattern	Behavior
1	0 — Rain	Vertical bullets dropping on random lanes
2	1 — Aimed horizontals	Bullets enter from left/right at the heart's row, alternating sides
3	2 — Sweeping curtain	A vertical stream that sweeps lane-by-lane, bouncing at the borders
4	3 — Aimed alternated	Alternates vertical (aimed at the heart's column) and horizontal (aimed at its row)
5+	4 — Chaos	Random mix of vertical and horizontal bullets
Difficulty scaling: setLevel(1..5) changes the fire interval (thr = 14 − 2·level) and bullet speed (v = 1 + level/2 + rnd). Higher turns → more bullets, faster, more aimed attacks.

Collision: the heart's hurtbox is a 6×6 rectangle inside the 8×8 sprite (x+2..x+7, y+1..y+6); each hit costs 1 HP.

Rage flash: if turn > 2 and bossHp < 30, the big boss sprite is inverted 4 times (Ui.flashBig). Purely cosmetic.

Endings
ending = 1 — Defeat: boss HP reaches 0. Shows "* You defeated Luzbel."

ending = 2 — Spare: you selected MERCY after 3 Talks. Shows "* You spared Luzbel." / "* Peaceful ending."

ending = 3 — Game Over: player HP reaches 0. Shows "GAME OVER".

Q at any time exits the battle (goes back to the title screen without an ending).

Overworld
A short exploration segment: brick-walled room with a bed, a nightstand with a lamp, a TV stand, a plant, a floor lamp, and a sign.

The Walker (player) has a hitbox for its feet (x+5..x+28, y+20..y+28) which is checked against Room collision rectangles — so you can walk in front of furniture visually but not through it.

Reading the sign (Z while near it): "* The sign reads: / Only faith will keep you from the fire."

Walking into the trigger zone at the top of the stairs (around the door frame) starts the battle with "* Luzbel blocks your path."

Seeding
seed is set to the total number of Sys.wait-free loops spent on the title screen — i.e. how long you take to press Start becomes the RNG seed. Neat touch for replay variety.

🧩 Technical Highlights
These are the parts I'm most proud of, all hand-rolled in Jack:

Framebuffer access by hand. Every sprite is a bitpacked array written directly to Screen memory at 16384 + row*32 + word. No Screen.drawPixel calls in the hot path — the sprites are OR'd/masked into the framebuffer word by word.

16-offset sub-pixel scrolling. Sprites (Heart, Bullets, Walker) store 16 precomputed copies, one per horizontal sub-pixel shift, so moveLeft/moveRight can shift by 1 px without recomputing the bit pattern.

Save/restore for the Walker. Because the walker overlaps the room's drawn furniture, draw() saves the 87 words it covers into a saved array, and erase() writes them back exactly. No full-screen redraws, no flicker.

Pool-based bullet system. Fixed pool of 12 bullets; freeSlot() finds an inactive slot on each spawn. Zero allocations per frame — important because Jack has no garbage collection.

Procedural bullet patterns. Five attack patterns (0..4) with tunable spawn interval and velocity. Pattern 3 aims at the heart's current column/row; pattern 2 sweeps lanes with bounce logic; pattern 4 mixes randomly.

Typewriter text. Ui.say / Ui.sayAt reveal each character after Sys.wait(18) for the Undertale feel, and dispose() the String afterwards (Jack leaks otherwise).

Timing-based attack minigame. Main.attack() slides a 4-px bar at 6 px / tick and maps the final offset to a damage tier — the whole "hit the center" feel, in ~40 lines of Jack.

Turn = difficulty. The boss's bullet-hell phase gets harder and switches patterns each turn, so the fight escalates naturally without a separate difficulty setting.

📝 What I Learned
Jack has no generics, no exceptions, and no GC. Every String passed to Output.printString must be .dispose()d, every array must be .dispose()d, and every class must .dispose() itself in its own dispose(). Getting this right across Main → Room / Walker / Heart / Box / Bullets / Ui was a real lesson in manual memory management.

The Hack screen is a real framebuffer. Drawing via Screen.drawRectangle is convenient but slow; writing bit patterns directly into 16384 + row*32 + word is orders of magnitude faster and is what makes the sprites animate smoothly in the VM Emulator.

Modular design pays off. The separation between Heart (movement + sub-pixel state), Bullets (pool + patterns), Box (limits + border), and Main (turn logic + endings) meant each piece could be tested and tuned in isolation. Bullets.setPattern() and Bullets.setLevel() were especially clean to tune independently.

Rendering order matters. The Walker is drawn after the room, and saves/restores the pixels it covers — otherwise moving across a piece of furniture left holes. This one bit me more than once.

Constrained platforms force good design. No sprite engine, no fonts, no audio — just Screen, Keyboard, Output, and a 16-bit CPU. Every visual decision was a bit-level one, and every loop had to fit the frame budget of the VM Emulator at "Fast".

🔭 Possible Improvements
□ More bullet patterns (spirals, gaps, walls) and boss attack phases.
□ Sound — the Hack platform has no native audio, but the VM Emulator could be extended.
□ Save the high score / spare-run flag across sessions (not possible without adding persistence; would be a good extension exercise).
□ Reduce per-frame allocations further to keep the VM Emulator smooth at "Normal" speed.
□ Add a fourth ending (e.g. attack → spare) — currently the fight is strictly kill-or-spare.
