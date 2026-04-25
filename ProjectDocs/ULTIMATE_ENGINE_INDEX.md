# Ludo Master: The Ultimate Engine & Method Index

This is the most granular documentation available for Ludo Master. It lists every major functional method in the Java codebase, its parameters, and its exact role in the game engine.

---

## 1. MainActivity: The Game Core
`MainActivity` handles the 2D physics, turn logic, and board state.

| Method Name | Role / Description |
| :--- | :--- |
| `initViews()` | Connects all XML IDs to Java objects and sets the square aspect ratio. |
| `setFullScreen()` | Removes status bars and navigation bars for an immersive experience. |
| `createPieces()` | Dynamically spawns the 16 Ludo pieces based on the game mode (Bot vs Player). |
| `move(int n)` | Recursive animation loop that moves a piece step-by-step. |
| `checkDeath()` | Scans the board after a move to see if an opponent's piece was landed on. |
| `checkVictory()` | Checks if all 4 pieces of a color have reached the center home. |
| `switchPlayers()` | The state machine that passes the "Turn" from one color to the next. |
| `checkAdjustments()` | **Mathematical Logic**: If multiple pieces are on the same block, this method scales them down so they all fit visually. |
| `checkPantaAdjustments()` | Specifically handles scaling for pieces that have finished the game. |
| `showStep()` | Triggers the small visual "ripple" effect on the board when a piece lands. |
| `stopEverything()` | Stops all handlers and music when the game ends. |
| `showGameOverScreen()` | Triggers the Glide GIF animation and plays the victory sound. |
| `onBackPressed()` | Custom logic to handle the phone's back button. |

---

## 2. HomeActivity: Navigation & UI Hub
`HomeActivity` handles the pre-game setup and user preferences.

| Method Name | Role / Description |
| :--- | :--- |
| `onCreate()` | Initializes the dashboard, music, and checks if the user is a first-time player. |
| `initViews()` | Maps the hundreds of buttons and layouts for the various menu levels. |
| `unselectAllAndSelectThisP()` | Logic for the player count selector (2P, 3P, 4P, etc.). |
| `giveClickSound()` | Global method to play a consistent UI feedback sound. |
| `setProfilePic()` | Loads the user's selected avatar from local storage. |
| `showNoInternet()` | Triggers a popup if a cloud-only feature is clicked while offline. |
| `onBackPressed()` | Handles menu navigation depth (e.g., exiting settings vs exiting the app). |

---

## 3. Piece Class (Inner Class)
Every token has its own internal "brain."

| Method Name | Role / Description |
| :--- | :--- |
| `Piece()` | Constructor: Sets the color, starting coordinates, and style (Classic/Stylish). |
| `makeAlive()` | Animates the piece from the base to the board. |
| `die()` | Animates the piece back to the base (reverse movement). |
| `activeState()` | Shows the rotating circle indicator around the piece. |
| `inactiveState()` | Hides the indicator and locks the piece from interaction. |
| `check(diceValue)` | Logic to determine if a specific dice roll *can* move this piece. |

---

## 4. XML Layout Architecture
The app uses a **Layered Layout Strategy** in `activity_main.xml` and `activity_home.xml`.

1.  **Base Layer**: The background image (The board or dashboard).
2.  **Interaction Layer**: Invisible views (buttons) placed over the dice pads.
3.  **Entity Layer**: The 16 `Piece` layouts (ConstraintLayouts containing an ImageView and a Stand).
4.  **Overlay Layer**: The Turn Arrow, winner crowns, and status text.
5.  **Modal Layer**: The popups (Settings, Exit, Congratulations) which are hidden by default (`visibility="gone"`).

---

## 5. Build & Configuration Scripts
- **`build.gradle.kts`**: 
    - `minSdk = 24`: Ensures compatibility with Android 7.0 and up.
    - `implementation("com.github.bumptech.glide:glide:4.14.2")`: Imports the high-performance GIF engine.
- **`AndroidManifest.xml`**:
    - Defines `HomeActivity` as the "Main" and "Launcher" activity.
    - Defines `MainActivity` as a "SingleTask" to prevent duplicate game screens.

---
*The Ultimate Engine Guide by Antigravity AI.*
