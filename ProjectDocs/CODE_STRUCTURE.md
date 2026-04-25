# Ludo Master: Code Logic & Structure

This document explains the technical architecture and how the Java code controls the Ludo game experience.

## 1. Application Entry (`SplashActivity.java`)
- **Purpose**: Displays the Ludo Master branding on startup.
- **Logic**: Uses a `Handler` with a `postDelayed` runnable to wait for 3 seconds before transitioning to the `HomeActivity`. It also handles full-screen window settings.

## 2. Dashboard & Setup (`HomeActivity.java`)
This class manages the user interface before the game starts.
- **Game Mode Selection**: Handles clicks for "Computer", "Pass n Play", and "Team Up".
- **Player Configuration**: Manages the number of players (2-4) and allows users to input their names using `EditText`.
- **Token Customization**: Allows switching between "Classic" and "Stylish" tokens, updating the `tokenNormal` boolean.
- **State Persistence**: Uses `SharedPreferences` to save user settings and statistics.

## 3. Game Engine (`MainActivity.java`)
The core logic resides here. It is responsible for the Ludo board's state machine.

### A. The Piece Class (Logic for each Token)
Each player has 4 tokens. The `Piece` class handles:
- **Movement**: Calculates coordinates based on the current block index.
- **Victory**: Detects if the piece has reached the center "Home" area.
- **Capture**: Checks if landing on a block occupied by another player (non-safe spot) should trigger a "kill".

### B. Dice Class
- **Randomization**: Generates a number between 1-6.
- **Visuals**: Triggers the rolling animation (`dice0001` to `dice0008`).
- **Logic**: Determines if a player gets an extra turn (if they roll a 6).

### C. Turn Management
- **Switching Players**: Logic to move the turn to the next active player.
- **Winner List**: Tracks who finishes 1st, 2nd, and 3rd, updating the victory screen accordingly.

## 4. UI Layouts (`res/layout`)
- `activity_main.xml`: A complex `ConstraintLayout` that positions the board, dice pads, and pieces using relative positioning.
- `activity_home.xml`: Contains multiple layers (ConstraintLayouts) for different menus like Settings, Statistics, and Game Mode selection.

## 5. Animations (`res/anim`)
- `blinkanimation.xml`: Makes pieces flash to indicate they can be moved.
- `rotateanimation.xml`: Provides smooth rotation for the "Ready to Pick" indicators.

---
*Created by Antigravity AI for the LudoMaster Project.*
