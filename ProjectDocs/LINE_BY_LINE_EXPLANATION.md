# Ludo Master: Line-by-Line Code Explanation

This document provides a detailed, line-by-line breakdown of the most critical sections of the Ludo Master source code. It is designed to help new developers understand how the game's core engine works.

---

## 1. Game Piece Movement (`MainActivity.java`)
The `move(int n)` method inside the `Piece` class is the heart of the movement logic.

```java
// Logic Block: Moving a piece 'n' steps
public void move(int n) {
    new Handler().postDelayed(new Runnable() { // 1. Use a handler to create a "step-by-step" animation effect
        int i = 0;
        @Override
        public void run() {
            if (i < n) { // 2. Loop until the piece has moved 'n' steps
                if (currBlock == -1) { // 3. Special case: Piece is starting from the home base
                    currBlock = startPosition; 
                } else {
                    currBlock = nextBlock(currBlock); // 4. Find the ID of the next block in the path
                }
                
                // 5. Update the visual position (X, Y) using the coordinate arrays
                piece.setTranslationX(x[currBlock]); 
                piece.setTranslationY(y[currBlock]);
                
                if (isSoundOn) { stepSound.start(); } // 6. Play "step" sound for each block moved
                
                i++;
                new Handler().postDelayed(this, 250); // 7. Wait 250ms before the next step (animation speed)
            } else {
                // 8. Final position reached: Check if we captured an enemy or reached "Home"
                checkDeath(this, currBlock);
                checkVictory(this);
            }
        }
    }, 0);
}
```

### Key Line Explanations:
- **Line 1 & 19**: We use a `Handler` instead of a simple loop. This prevents the UI from freezing and allows the user to see the piece "walking" block by block.
- **Line 4-8**: This handles the transition from the "Base" to the "Start" position on the board.
- **Line 11-12**: `x[]` and `y[]` are pre-calculated arrays that map every block index to actual screen pixels.

---

## 2. Capture (Kill) Logic (`MainActivity.java`)
This determines if a piece "kills" another player's piece.

```java
private boolean checkDeath(Piece killer, int targetBox) {
    // 1. Loop through all active players
    for(int i = 0; i < players.size(); i++) {
        String colour = players.get(i).getColor();
        
        // 2. Check if the player is an ENEMY (different color)
        if(!colour.equals(killer.colour)) {
            List<Piece> enemyPieces = getPiecesByColor(colour);
            
            for(Piece p : enemyPieces) {
                // 3. If an enemy piece is on the SAME block
                if(p.currBlock == targetBox) {
                    
                    // 4. Check if the block is a "Safe Spot" (Star/Home)
                    if(!safeSpots.contains(targetBox)) {
                        p.die(); // 5. Capture the piece (send it back to base)
                        if(isSoundOn) { deathSound.start(); }
                        return true; // 6. Return true to give the killer an extra turn
                    }
                }
            }
        }
    }
    return false;
}
```

---

## 3. Dice Rolling Engine (`MainActivity.java`)
How the random dice roll is generated and visualized.

```java
void roll() {
    isRolling = true; // 1. Lock the dice so user can't click twice
    mainDiceImageView.setImageDrawable(diceAnimationDrawable); // 2. Show the "rolling" animation
    diceAnimationDrawable.start();

    diceHandler.postDelayed(() -> {
        // 3. Generate a random number between 1 and 6
        int result = (int) Math.ceil(Math.random() * 6);
        
        // 4. Set the final static image (dice1.png to dice6.png)
        updateDiceVisual(result); 
        
        isRolling = false;
        currentPlayerDice = result; // 5. Store the value for the movement logic
        
        // 6. Automatically move if only one piece can move, else wait for click
        handleAutoMoveOrWait(result);
    }, 350); // 350ms is the duration of the rolling animation
}
```

---

## 4. Passing Data Between Screens (`HomeActivity.java`)
When you click "Play", the app sends the settings to the game screen.

```java
// Line-by-line of the Intent logic
Intent i = new Intent(HomeActivity.this, MainActivity.class); // 1. Create a link to the game screen
i.putExtra("nop", 4);              // 2. Pass the Number of Players (4)
i.putExtra("type", pnpgametype);   // 3. Pass the Game Mode (Classic/Team)
i.putExtra("normalPiece", tokenNormal); // 4. Pass the selected Token Style
startActivity(i);                 // 5. Launch the game!
overridePendingTransition(0, 0);  // 6. Smooth transition without flickering
```

---

## 5. Summary of logic flow
1.  **SplashActivity**: Wait 3 seconds -> Start Home.
2.  **HomeActivity**: User picks mode -> Data sent via `Intent` -> Start Main.
3.  **MainActivity**: 
    - `onCreate`: Load the board and piece coordinates.
    - `Dice.roll()`: Get a random number.
    - `Piece.move()`: Move the piece if it's "Alive".
    - `checkDeath()`: Check for collisions.
    - `switchPlayers()`: Move to the next color.

---
*Detailed line-by-line breakdown by Antigravity AI.*
