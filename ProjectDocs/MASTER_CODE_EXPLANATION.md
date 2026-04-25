# Ludo Master: Master Code Logic Breakdown (Exhaustive)

This document provides a massive, high-detail breakdown of the Ludo Master source code. It covers the logic, mathematics, and system architecture of the entire game.

---

## 1. The Mathematical Core: Coordinate Mapping
The game doesn't use a standard grid system. Instead, it uses **Coordinate Mapping** via two massive arrays: `dx` and `dy`.

```java
double[] dx = {53.32, 53.32, 53.32, 53.32, 53.32, 60.04, 66.7 ...};
double[] dy = {6.66, 13.32, 19.98, 26.64, 33.3, 40, 40 ...};
```

### How it works:
- **Percent-Based Positioning**: Each number represents a percentage of the total board width/height.
- **Conversion to Pixels**: In `onCreate`, these percentages are multiplied by the actual screen width (`pxWidth`) to get real pixel coordinates (`x[]` and `y[]`).
- **Dynamic Board**: This allows the game to look identical on a small phone and a large tablet.

---

## 2. The Piece Class: The Game Entity
Every token on the board is an instance of the `Piece` class. This is the most complex part of the code.

### A. Variables Breakdown:
- `isAlive`: If `false`, the piece is in the base (starting box). If `true`, it is on the path.
- `currBlock`: The current index (0 to 51) in the path array.
- `numberOfSteps`: Tracks how many total steps the piece has moved (used to trigger the "Home Stretch").
- `isReadyToEnterWinnerZone`: Becomes true when the piece reaches its specific entry point (e.g., Block 50 for red).

### B. Life Cycle of a Piece:
1.  **`makeAlive()`**: triggered by rolling a 6. It moves the piece from the base coordinates (`defX`, `defY`) to the starting block (`startPosition`).
2.  **`move(int n)`**: Uses a recursive-style `Handler` loop.
    - It plays a `stepSound`.
    - It triggers a **Pop-Up Animation** (`animatorSet`) to make the piece look like it's jumping.
    - After the last step, it calls `checkDeath()` to see if it landed on an opponent.
3.  **`die()`**: Triggered when an enemy lands on this piece. It reverses the movement with a fast animation back to the base.

---

## 3. The Dice Engine: Randomization & Visuals
The `Dice` class manages the interaction between the user and the random number generator.

### A. Animation Logic:
The `diceAnimationDrawable` is an `AnimationDrawable` created from 8 individual frames (`dice0001` to `dice0008`). It creates a high-speed "blur" effect to simulate rolling.

### B. Interaction Locking:
- `isRolling`: Prevents the user from clicking the dice while it's already rolling or while a piece is moving.
- `isDiceClickable`: A state flag that ensures turns are respected. You can't roll if it's not your turn or if you still need to move a piece.

---

## 4. Turn Management & AI (Bot Logic)
The `switchPlayers()` method handles the transition between the Red, Green, Blue, and Yellow players.

### A. Turn Sequence:
1.  The `currentPlayerIndex` increments.
2.  The code checks if the next player is still in the game (hasn't won yet).
3.  The "Turn Arrow" (`hintArrow`) moves to the new player's corner.
4.  **Bot Check**: If the new player `isBot` is true, the `roll()` method is called automatically after a 500ms delay.

### B. AI Decision Making:
The Bot chooses its move based on **Priority Scoring**:
1.  **Priority 1 (Killing)**: If a move will capture an enemy piece, the Bot takes it immediately.
2.  **Priority 2 (Safe Spots)**: If a move puts a piece on a "Safe Spot" (Star/Home entry), it is highly prioritized.
3.  **Priority 3 (Getting Home)**: The Bot prioritizes pieces that are closest to winning.
4.  **Priority 4 (Making Alive)**: If a 6 is rolled, the Bot always tries to bring a new piece onto the board.

---

## 5. UI and Screen Sizing (`initViews`)
Ludo Master uses **ConstraintLayout** for its versatility.
- **`pxWidth` and `pxHeight`**: These are calculated at startup using `DisplayMetrics`.
- **Layout Scaling**: The board is forced into a square aspect ratio (`pxWidth` x `pxWidth`) to ensure the pieces align perfectly with the background image.
- **Glassmorphism**: Popups use `Alpha` levels and gradients to create a modern "Glass" effect.

---

## 6. Media Management
- **`MediaPlayer`**: Used for sounds. The app manually manages the lifecycle of these players (`release()` and `prepare()`) to avoid memory leaks.
- **`Glide`**: Used for the heavy GIF files like `fireworksimg.gif`. Glide handles the frame-rate and memory buffering automatically.

---

## 7. Victory & Statistics
When a player finishes, the `currentWinnerPosition` tracks if they are 1st, 2nd, or 3rd.
- **Statistics**: The `SharedPreferences` saves the `botwins` and `botloses` so the player can see their progress over time in the statistics menu.

---
*This master guide covers the architectural foundation of Ludo Master.*
