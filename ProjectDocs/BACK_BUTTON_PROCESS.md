# Ludo Master: Back Button & Navigation Process

This document explains exactly what happens when a user clicks the "Back" button on their phone or the "Back" icon in the app. It covers the logic, the code implementation, and the user experience.

---

## 1. Overview of the "Back" Flow
In Ludo Master, navigation is designed to be safe and interactive. Instead of simply closing the app, the back process handles:
1.  **Confirmation Dialogs**: Preventing accidental game exits.
2.  **Smooth Transitions**: Animating layouts when moving between menus.
3.  **State Management**: Pausing music or clearing handlers when leaving a screen.

---

## 2. Code Implementation: The System Back Button
Both `MainActivity` and `HomeActivity` override the default Android back behavior to provide a custom experience.

### A. In HomeActivity (Main Menu)
When you are on the home screen and press the phone's back button:

```java
@Override
public void onBackPressed() {
    // 1. If the "Exit Confirmation" popup is ALREADY open
    if(exitAppLayout.getVisibility() == View.VISIBLE) {
        exitNoBtn.callOnClick(); // Close the popup (Cancel exit)
    } else {
        // 2. If no popup is visible, trigger the "Exit Game" button
        exitgamebtn.callOnClick(); // This opens the confirmation popup
    }
}
```

**What happens next?**
- `exitgamebtn.callOnClick()` triggers an **Overshoot Animation** that scales the exit dialog from `0.0` to `1.0`.
- If you then click **Yes**, the code runs `finishAndRemoveTask()`, which completely clears the app from the phone's memory.

### B. In MainActivity (During Gameplay)
During a live game, pressing back is even more critical to avoid losing progress.

```java
@Override
public void onBackPressed() {
    // 1. Check if the "Quit Game" layout is visible
    if(quitgamelayout.getVisibility() == View.VISIBLE) {
        ingamenobtn.callOnClick(); // If visible, hide it (Return to game)
    } else {
        // 2. Otherwise, trigger the exit sequence
        menuexitbtn.callOnClick(); // Shows the "Are you sure you want to quit?" popup
    }
}
```

---

## 3. Manual "Back" Icons (UI Buttons)
The app features several dedicated "Back" icons (e.g., `pnpbackbtn` in the multiplayer setup). These do not use `onBackPressed` but follow a custom logic:

```java
pnpbackbtn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        // 1. Play the "Anticipate" animation (shrinks the layout to 0)
        passnplaylayout.animate()
            .scaleX(0.0f)
            .scaleY(0.0f)
            .setInterpolator(new AnticipateInterpolator())
            .setDuration(200)
            .setListener(new AnimatorListenerAdapter() {
                @Override
                public void onAnimationEnd(Animator animation) {
                    super.onAnimationEnd(animation);
                    // 2. Hide the layout completely once the animation ends
                    passnplaylayout.setVisibility(View.GONE);
                }
            }).start();
    }
});
```

---

## 4. Behind the Scenes: The Cleanup Process
When the app finally "exits" or moves back to a previous activity, the following system processes occur:

1.  **onPause()**:
    - The background music (`MediaPlayer m`) is paused to save battery.
    - `m.pause();`
2.  **onDestroy() / finish()**:
    - Handlers (like the dice roll delay or "No Internet" check) are removed to prevent memory leaks.
    - `nointernethandler.removeCallbacks(nointernetrunnable);`
3.  **Memory Management**:
    - Glide (the image library) clears its cache for the current session to free up RAM for other apps.

---

### Summary Table: Back Process
| Trigger | Action Taken | Animation Used |
| :--- | :--- | :--- |
| Phone Back Button | Opens/Closes confirmation popups | `Overshoot` (Bounce effect) |
| UI Back Icon | Returns to previous menu level | `Anticipate` (Zoom out) |
| Exit "Yes" | `finishAndRemoveTask()` | Default System Exit |

---
*Process documentation by Antigravity AI.*
